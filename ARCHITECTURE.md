🏛️ Documentación de Arquitectura — AuraPOS v1.0 Enterprise
Este documento detalla los principios de diseño, la separación de capas, los protocolos de seguridad y el modelo de sincronización híbrida implementados en AuraPOS v1.0 Enterprise.
1. El Paradigma Offline-First
AuraPOS está construido bajo el principio de resiliencia operacional absoluta (Offline-First). En el retail tradicional latinoamericano (minimarkets, botillerías y almacenes de barrio), la conectividad a internet suele ser inestable.
El sistema opera al 100% sin conexión a internet durante las operaciones críticas de caja, lectura de códigos de barra, cobros e impresión térmica.
Las consultas y escrituras se ejecutan de manera local a velocidad sub-milisegundo, eliminando la dependencia de servidores externos para las ventas del día a día.
2. Separación Estricta de Capas (Separation of Concerns)
Para mantener un código mantenible, testeable y escalable, el sistema divide sus responsabilidades en tres capas inmutables:
A. Capa de Acceso a Datos (DAL) — database.py
Es el único módulo autorizado para ejecutar sentencias SQL y comunicarse con la base de datos local SQLite3.
Configuración del Motor SQLite:
Modo WAL (Write-Ahead Logging) activado para permitir lecturas y escrituras concurrentes de alta velocidad.
PRAGMA synchronous = NORMAL para balancear rendimiento y seguridad transaccional.
Claves foráneas activadas (PRAGMA foreign_keys = ON).
Transacciones estrictas con BEGIN IMMEDIATE; en operaciones críticas (ventas y abonos) para evitar bloqueos en red local.
B. Capa de Lógica de Negocio (BLL) — inventario.py
Concentra todas las operaciones matemáticas, financieras y de cumplimiento normativo legal chileno:
Ley de Redondeo (Ley N° 20.956): Algoritmo condicional estricto para pagos en efectivo (redondeo a la decena anterior o posterior según el dígito final de 1 a 9).
Decodificador Balanza EAN-13 (Prefijo 20): Extracción matemática de SKU y peso en kilogramos a partir de códigos de barras de balanzas comerciales.
Márgenes Comerciales: Cálculo de rentabilidad sobre precio de venta considerando el 19% de IVA chileno.
C. Capa de Presentación (UI) — gui.py & login_gui.py
Construido con CustomTkinter bajo una grilla rígida de tres columnas para evitar colapsos de diseño (minsize).
Optimización de Vistas (Caché en Memoria): Las vistas principales (Bodega, Fiados, Tienda, Ajustes) se pre-inicializan una sola vez al arrancar la app y se gestionan mediante grid() y grid_remove(), logrando tiempos de transición instantáneos (0 ms de delay).
3. Seguridad, Criptografía e Identidad
Firma de Hardware (HWID): El sistema extrae el número de serie de la placa madre y el UUID del procesador mediante subprocesos seguros del sistema operativo (wmic), generando una huella criptográfica SHA-256 única por terminal.
Autenticación Criptográfica (auth.py):
Las contraseñas se almacenan empleando PBKDF2-HMAC-SHA256 con 250.000 iteraciones y sales aleatorias de 16 bytes.
Mitigación de ataques de tiempo (Timing Attacks) utilizando hmac.compare_digest.
Retardos artificiales ante intentos fallidos para neutralizar ataques de fuerza bruta.
4. Ciclo de Vida SaaS y Licenciamiento (Kill-Switch)
El motor de licencias (licensing.py) opera bajo tres estados comerciales estrictos:
prueba_gratis: 15 días desde la primera instalación. Operación silenciosa del día 1 al 9; advertencia regresiva del día 10 al 15. Bloqueo automático al día 16 si no se adquiere suscripción.
pago: Operación offline continua. Exige una validación de red el día del mes configurado (dia_pago_obligatorio) para revalidar la suscripción en la nube.
no_pagado: Bloqueo total del sistema con despliegue de pantalla de desbloqueo manual por folio de soporte y semilla maestra (100 rondas SHA-256).
5. Sincronización Cloud Híbrida (Supabase Storage)
Para garantizar simplicidad y evitar migraciones de esquemas relacionales complejos en la nube, la telemetría y el respaldo de datos operan mediante paquetes estructurados en formato JSON:
Un hilo demonio en segundo plano (cloud_sync.py) detecta la conectividad a internet de forma pasiva.
Tras consolidar la base de datos local (db_forzar_checkpoint()), empaqueta el inventario completo de productos y las ventas pendientes con su detalle de ítems.
Sube el archivo JSON directamente a Supabase Storage bajo un bucket dedicado (aurapos-terminales), organizando los respaldos en carpetas virtuales legibles basadas en el nombre comercial del local y el HWID del terminal (/{slug_local}_{hwid_corto}/sync_{timestamp}.json).
6. Auditoría y Trazabilidad Inalterable (audit_logs)
El sistema cuenta con una tabla de auditoría ciega que registra de forma automática y detallada:
Creación y cierre de ventas (con detalle de ítems y medios de pago).
Movimientos de caja (retiros, ingresos de sencillo, devoluciones).
Bajas de inventario por mermas y ajustes de stock.
Los registros incluyen marca de tiempo local, usuario responsable, tipo de evento, monto y observaciones, permitiendo una trazabilidad fiscal y gerencial completa.
(Fin del documento técnico de arquitectura)