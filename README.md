# Aura POS v1.0 Enterprise 🚀
> **El Sistema de Punto de Venta (POS) Offline-First definitivo para el retail tradicional y almacenes de barrio en Chile.**

[![Python](https://img.shields.io/badge/Python-3.11%2B-blue.svg)](https://www.python.org/)
[![CustomTkinter](https://img.shields.io/badge/UI-CustomTkinter-green.svg)](https://github.com/TomSchimansky/CustomTkinter)
[![Database](https://img.shields.io/badge/Database-SQLite%20WAL-lightgrey.svg)](https://sqlite.org/)
[![Cloud](https://img.shields.io/badge/Cloud-Supabase-orange.svg)](https://supabase.com/)

---

## 💡 ¿Qué es Aura POS?
**Aura POS** es un software de escritorio comercializado como **SaaS B2B**, diseñado específicamente para minimarkets, almacenes de barrio, panaderías y botillerías en Chile. A diferencia de los POS tradicionales basados 100% en la nube (que colapsan y dejan de vender cuando falla la conexión a internet), AuraPOS opera bajo una arquitectura **Offline-First indestructible**.

---

## 🌟 Ventajas Competitivas Principales
1. **100% Operativo sin Internet:** Funciona todo el día sin conexión a la red local ni a internet. Cuando detecta conectividad, sincroniza de forma asíncrona telemetría y licencias hacia la nube.
2. **Cumplimiento Tributario y Normativo Chileno:**
   * Cálculo automático de la **Ley de Redondeo (Ley N° 20.956)** para pagos en efectivo.
   * Soporte e intérprete nativo de balanzas chilenas (Prefijo EAN-13 `20` para pesaje de fiambrería y panadería).
   * Gestión estricta de márgenes considerando el 19% de IVA.
3. **Seguridad de Grado Bancario:**
   * Bloqueo estricto por firma única de hardware (**HWID** extraído de placa madre y CPU).
   * Cifrado de contraseñas con **PBKDF2-HMAC-SHA256** (250.000 iteraciones) y protección contra ataques de tiempo (*Timing Attacks*).
   * Auditoría ciega (`audit_logs`) con trazabilidad hiperdetallada de ventas, retiros de caja y mermas.
4. **Modelo de Negocio Modular (*Aura Store*):**
   * Estructura de *Feature Flags* que permite activar en caliente módulos adicionales (Fiado digital, Promociones, Mayorista WhatsApp, Mermas, Boleta Electrónica SII y Transbank POS).

---

## 🛠️ Stack Tecnológico
* **Lenguaje:** Python 3.11+
* **Interfaz Gráfica (GUI):** CustomTkinter (Material Design 3 dark theme nativo) + Pillow.
* **Base de Datos Local:** SQLite3 con modo `WAL` (Write-Ahead Logging) y concurrencia transaccional atómica (`BEGIN IMMEDIATE`).
* **Backend Cloud & Telemetría:** Supabase (PostgreSQL + PostgREST + Storage Buckets organizados por HWID/slug comercial).
* **Distribución:** Compilación nativa a binario `.exe` independiente mediante Nuitka/PyInstaller e instalador profesional con Inno Setup.

---

## 📦 Módulos del Sistema
* **Caja de Ventas (POS):** Búsqueda inteligente, multiplicador de supermercado (`3*CODIGO`), pagos mixtos (Efectivo + Tarjeta) e impresión térmica ESC/POS con respaldo dual local (.json y .txt).
* **Gestión de Bodega:** Control de stock, alertas de stock crítico, creador de listas mayoristas para WhatsApp, control de mermas y calculadora comercial de márgenes con redondeo `.990`.
* **Libreta de Fiados:** Cuentas corrientes de vecinos con modalidad flexible (fiado parcial) y notas personalizadas.
* **Arqueo y Turnos:** Cuadre de caja ciego con fórmula contable de efectivo esperado, apertura/cierre de turnos y protocolo de entrega entre cajeros.

---
*Desarrollado con arquitectura Enterprise Offline-First para el comercio en Chile.*
