<div align="center">
  <h1 align="center">⚡ AuraPOS v1.0 Enterprise</h1>
  <p align="center">
    <b>El Sistema de Punto de Venta (POS) Offline-First definitivo para el retail tradicional y almacenes de barrio en Chile.</b>
  </p>

  <p align="center">
    <img src="https://img.shields.io/badge/Python-3.11%2B-blue.svg?style=flat-square&logo=python&logoColor=white" alt="Python">
    <img src="https://img.shields.io/badge/UI-CustomTkinter-4f46e5.svg?style=flat-square" alt="CustomTkinter">
    <img src="https://img.shields.io/badge/Database-SQLite%20WAL-003b57.svg?style=flat-square&logo=sqlite&logoColor=white" alt="SQLite">
    <img src="https://img.shields.io/badge/Cloud-Supabase-10b981.svg?style=flat-square&logo=supabase&logoColor=white" alt="Supabase">
    <img src="https://img.shields.io/badge/Platform-Windows-0078d7.svg?style=flat-square&logo=windows&logoColor=white" alt="Windows">
    <img src="https://img.shields.io/badge/License-Proprietary-red.svg?style=flat-square" alt="License">
  </p>
</div>

---

## 💡 ¿Qué es AuraPOS?
**AuraPOS** es un software de escritorio comercializado como **SaaS B2B**, diseñado específicamente para minimarkets, almacenes de barrio, panaderías y botillerías en Chile. A diferencia de los POS tradicionales basados 100% en la nube (que colapsan y dejan de vender cuando falla la conexión a internet), AuraPOS opera bajo una arquitectura **Offline-First indestructible**.

---

## 📊 Comparativa de Mercado

| Criterio | AuraPOS v1.0 Enterprise | Sistemas Tradicionales en la Nube (e.g., Bsale, Fudo) | Máquinas "Smart POS" (e.g., Redelcom, Transbank) |
| :--- | :--- | :--- | :--- |
| **Dependencia de Red** | **Ninguna para vender.** Operación 100% local. Sincroniza en background al detectar red. | **Alta.** Si el internet falla o se satura (VTR/Entel), la caja se detiene por completo. | **Alta.** Dependen de conectividad móvil (Chip/Wi-Fi) para procesar transacciones. |
| **Velocidad de Caja** | **Instantánea.** Ejecución local en SQLite3 WAL, cero latencia de red. | **Media / Variable.** Sujeta a la velocidad y latencia del proveedor de internet. | **Media.** Limitada por la potencia del hardware físico del dispositivo. |
| **Herramientas de Almacén** | **Avanzadas y Locales.** Multiplicador de supermercado, balanzas EAN-13, libreta de fiados y mermas. | **Muy avanzadas.** Control multi-sucursal y reportes en la nube. | **Básicas / Nulas.** Enfoque exclusivo en cobro con tarjeta, sin gestión logística. |
| **Cumplimiento Legal (Chile)** | **Nativo.** Ley de Redondeo (Ley 20.956) e IVA del 19% integrados. | **Alto.** Emisión electrónica de documentos tributarios. | **Básico.** Emisión de boleta electrónica estándar del operador. |
| **Costo Operativo** | **SaaS Predictivo.** Sin cobros ocultos por transacción o comisiones por venta. | **Medio - Alto.** Suscripciones mensuales fijas recurrentes. | **Comisión por venta** + arriendo o compra mensual de equipo. |

---

## 🔎 Capacidades y Módulos de la v1.0

### 🛒 1. Mesón de Venta e Interfaz (POS)
* **Búsqueda Dual y Multiplicador:** Permite escaneo directo o uso de multiplicador de supermercado (ej: `3*CODIGO`).
* **Pago Mixto Inteligente:** Fraccionamiento de cuentas entre efectivo y tarjetas, aplicando la *Ley de Redondeo (Ley N° 20.956)* estrictamente a la fracción en efectivo.
* **Libreta de Fiados Flexible:** Gestión de cuentas corrientes de vecinos con opción de abono directo o **Fiado Express** de única vez con notas personalizadas.
* **Impresión Térmica ESC/POS:** Conexión nativa con impresoras USB de 58mm/80mm y respaldo dual automático en archivos `.json` estructurados y `.txt` dentro de la carpeta local `Boletas/`.

### 🛡️ 2. Seguridad y Control Interno
* **Jerarquía Estricta de Roles:** Cuentas segregadas entre *Vendedor* (operación de caja), *Admin* (dueño del local con control de bodega, precios y cajeros) y *SuperAdmin*.
* **Cambio Rápido por PIN:** Cambio de operador al vuelo mediante PINs seguros de 4 dígitos.
* **Auditoría Ciega (`audit_logs`):** Registro inalterable y filtrable en tiempo real de cada venta, retiro de caja, gasto o merma.
* **Criptografía Robusta:** Hashing de credenciales con **PBKDF2-HMAC-SHA256** (250.000 iteraciones) y protección contra *Timing Attacks*.

### 🇨🇱 3. Cumplimiento Normativo (Chile)
* **Ley N° 20.956 de Redondeo:** Ajuste automático de centavos en pagos de efectivo (terminaciones 1-5 a la baja, 6-9 al alza).
* **Soporte Balanzas EAN-13 (Prefijo 20):** Lectura e interpretación de etiquetas de peso en gramos para fiambrerías y panaderías.
* **Márgenes Comerciales:** Control de rentabilidad y calculadora de precios bonitos (.990) con desglose de IVA (19%).

---

## ☁️ Arquitectura Híbrida y Cloud Sync
* **Offline-First Absoluto:** El motor SQLite opera en modo `WAL` con transacciones atómicas `BEGIN IMMEDIATE`.
* **Sincronización Supabase Storage:** Un hilo demonio en segundo plano empaqueta los estados del inventario y las ventas pendientes en formato JSON y los sube a Supabase Storage organizándolos limpiamente en carpetas virtuales por local (`/{slug_local}_{hwid_corto}/sync_timestamp.json`).
* **Motor de Licencias (Kill-Switch):** Bloqueo por firma única de hardware (**HWID**), periodo de prueba de 15 días y revalidación mensual asíncrona.

---

## 🗺️ Roadmap & Futuras Actualizaciones (v1.1)
- [ ] **Sincronización Multicaja:** Interconexión en red local entre múltiples terminales de cobro.
- [ ] **Pasarela de Pagos Automática (Aura Store):** Compra y activación en caliente de add-ons mediante Webhooks (Flow / Mercado Pago).
- [ ] **Integración Transbank POS (Serial/USB):** Envío automático de montos directos al pinpad.
- [ ] **Puente API Boleta Electrónica SII (DTE):** Emisión legal de documentos tributarios electrónicos.

---

## 🛠️ Stack Tecnológico
* **Lenguaje:** Python 3.11+
* **Interfaz Gráfica:** CustomTkinter (Material Design 3 / Dark Slate)
* **Base de Datos:** SQLite3 (WAL Mode) + Supabase (PostgreSQL / Storage)
* **Distribución:** Binario autónomo compilado con Nuitka e instalador con Inno Setup.

---

## 💻 Requisitos del Sistema (Ultra-Ligeros)

AuraPOS fue diseñado con ingeniería de bajo consumo (C++ nativo y SQLite WAL), permitiendo una velocidad instantánea incluso en computadores antiguos, pantallas táctiles de mesón y equipos reacondicionados de bajos recursos.

| Componente | Requisitos Mínimos (Para equipos antiguos) | Requisitos Recomendados |
| :--- | :--- | :--- |
| **Memoria RAM** | **2 GB RAM** *(Consumo real de AuraPOS: ~140 MB, hasta 10 veces más liviano que sistemas web)* | **4 GB - 8 GB RAM** |
| **Procesador (CPU)** | Intel Celeron, Intel Atom, AMD E-Series o Core 2 Duo (1.6 GHz Dual-Core) | Intel Core i3 / AMD Ryzen o superior |
| **Sistema Operativo** | Windows 10 (32-bit / 64-bit) / Windows 10 Home o LTSC | Windows 10 / Windows 11 (64-bit) |
| **Almacenamiento** | **300 MB libres** en disco (Funciona en discos duros HDD tradicionales) | Disco Sólido (SSD) de cualquier tamaño |
| **Resolución de Pantalla** | **1024 x 768 px** (Compatible con monitores touch POS clásicos de 15") | 1366 x 768 px o 1920 x 1080 px Full HD |
| **Periféricos** | Teclado y mouse USB | Lector de código de barras USB y/o Impresora térmica |
| **Conexión a Internet** | **Cero para vender.** Funciona desconectado de la red todo el día. | Conexión esporádica (Wi-Fi/móvil) para sincronización |

> 🚀 **Ventaja frente a la competencia:** Los sistemas basados en navegador web (Chrome) o Electron saturan la memoria y exigen mínimo 4 GB o 8 GB de RAM para no quedarse pegados. AuraPOS corre sobre código de máquina nativo, garantizando que una máquina modesta de almacén vuele a máxima velocidad.

## 📞 Contacto y Desarrollo
* **GitHub Organization:** [@AuraPOSCL](https://github.com/AuraPOSCL)
* **Desarrollo Enterprise Offline-First**
