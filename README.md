<div align="center">

<img src="https://img.shields.io/badge/C%2B%2B%2FCLI-.NET%20Framework-blue?style=for-the-badge&logo=cplusplus&logoColor=white"/>
<img src="https://img.shields.io/badge/ESP32--S3-Arduino-red?style=for-the-badge&logo=arduino&logoColor=white"/>
<img src="https://img.shields.io/badge/BLE-Bluetooth%20Low%20Energy-blueviolet?style=for-the-badge&logo=bluetooth&logoColor=white"/>
<img src="https://img.shields.io/badge/LVGL-v9.4-orange?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Estado-Sprint%201%20Completado-success?style=for-the-badge"/>

# 🩺 Sistema Integrado de Asistencia y Gestión de Salud

**Programación Orientada a Objetos — PUCP 2026**

*Sistema de monitoreo para adultos mayores con integración BLE al smartwatch JNOX*

</div>

---

## 📋 Descripción del Proyecto

Este proyecto consiste en el desarrollo de un **sistema de escritorio en C++/CLI** que actúa como central de monitoreo y gestión para adultos mayores, integrándose vía **Bluetooth Low Energy (BLE)** con el smartwatch **JNOX**, dispositivo basado en el microcontrolador **ESP32-S3**.

El sistema permite a familiares y cuidadores:
- 👤 Administrar perfiles clínicos de pacientes
- ⏰ Programar recordatorios diarios (medicación, citas, rutinas)
- 🚨 Recibir alertas críticas en tiempo real ante caídas detectadas por el sensor **BMI160**
- 🩻 Gestionar recomendaciones médicas por parte de profesionales de salud

---

## 🏗️ Arquitectura del Sistema

```
┌─────────────────────────────────┐        BLE (JSON)        ┌─────────────────────────────┐
│   Aplicación de Escritorio      │ ◄───────────────────────► │     Smartwatch JNOX         │
│        (C++/CLI)                │                           │     (ESP32-S3 + LVGL)       │
│                                 │                           │                             │
│  • Gestión de Pacientes         │                           │  • Pantalla LVGL v9.4       │
│  • Programación de Recordatorios│                           │  • Acelerómetro BMI160      │
│  • Panel del Cuidador/Médico    │                           │  • Vibración + Alertas      │
│  • Historial de Eventos         │                           │  • Confirmación de Caída    │
└─────────────────────────────────┘                           └─────────────────────────────┘
```

---

## 📐 Diagrama de Clases — Modelo de Dominio

El modelo está compuesto por **10 clases** organizadas en dos jerarquías de herencia:

### Jerarquía de Usuarios
```
Usuario (base)
├── Cuidador
└── Médico
```

### Jerarquía de Eventos de Salud
```
EventoSalud (base)
├── Recordatorio
└── AlertaCaída
```

### Clases Independientes
```
Paciente · ContactoEmergencia · DispositivoJNOX · GestorBLE
```

### Resumen de Clases

| Clase | Tipo | Atributos principales | Métodos destacados |
|---|---|---|---|
| `Usuario` | Clase base | `Id`, `Nombre`, `Edad`, `Telefono`, `UsuarioLogin`, `Contrasena` | constructor |
| `Cuidador` | Hereda de Usuario | `Turno`, `PacientesAsignados` | constructor |
| `Médico` | Hereda de Usuario | `Especialidad`, `CentroDeSalud`, `PacientesAsignados` | `GenerarRecomendaciones()` |
| `Paciente` | Independiente | `IDPaciente`, `TipoSangre`, `SeguroMedico`, `Dispositivo`, `HistorialMedico` | `addContactoEmergencia()`, `showPacienteInfo()` |
| `ContactoEmergencia` | Independiente | `IDCE`, `NombreCompleto`, `Parentesco`, `Telefono` | `MostrarContactoEmergencia()` |
| `DispositivoJNOX` | Independiente | `DireccionMAC`, `NivelBateria`, `EstadoConexion`, `UmbralLibre`, `UmbralImpacto` | `setConfigCaida()`, `getNivelBat()` |
| `EventoSalud` | Clase base eventos | `IdEvento`, `FechaHora`, `Estado` | `setEstado()`, `getEstado()` |
| `Recordatorio` | Hereda de EventoSalud | `MensajeTexto`, `Categoria`, `Repetir`, `IntervaloMinutos` | `setIntervalo()` |
| `AlertaCaída` | Hereda de EventoSalud | `MagnitudImpacto`, `ConfirmacionReloj` | `CaidaDetectada()`, `getMagnitudCaida()` |
| `GestorBLE` | Independiente | `EstaConectado`, `MacConectada` | `ConectarReloj()`, `SerializarEventoSalud()` |

---

## ✅ Historias de Usuario

| ID | Actor | Historia |
|---|---|---|
| HU01 | Cuidador | Registrarme en el sistema para guardar mis datos de acceso y personales |
| HU02 | Cuidador | Administrar los datos clínicos del adulto mayor (tipo de sangre, seguro, contactos) |
| HU03 | Cuidador | Agendar recordatorios por categoría para que el reloj JNOX alerte al paciente |
| HU04 | Adulto mayor | Recibir alertas de vibración y texto legible en el reloj al momento de una actividad |
| HU05 | Cuidador | Ser notificado visual y auditivamente cuando el paciente registre una caída |
| HU06 | Adulto mayor | Confirmar desde el reloj que estoy a salvo para cancelar la alarma de caída |
| HU07 | Cuidador | Visualizar el historial de alarmas y caídas detectadas por paciente |
| HU08 | Médico | Acceder a la información del paciente y registrar recomendaciones médicas |

---

## 🗂️ Product Backlog

### Sprint 1 — ✅ Completado

- [x] Modelado y programación de las 10 clases en C++/CLI (encapsulamiento, herencia, polimorfismo)
- [x] Diseño del Diagrama de Clases en StarUML con jerarquías y relaciones completas
- [x] Configuración del entorno de desarrollo para JNOX HEALTH en Arduino con LVGL v9.4
- [x] Configuración del repositorio en GitHub y tablero de seguimiento en Trello
- [x] Redacción del Catálogo de Requisitos e Historias de Usuario

### Pendiente (Sprints futuros)

- [ ] Construcción de formularios GUI (Windows Forms): Login, Dashboard, CRUD de pacientes
- [ ] Implementación de persistencia de datos para Paciente e HistorialMedico
- [ ] Integración de `Windows.Devices.Bluetooth` en `GestorBLE` para emparejamiento BLE
- [ ] Programación de vistas en JNOX con LVGL para notificaciones al usuario
- [ ] Driver I2C para el chip BMI160 (detección de caídas en el reloj)
- [ ] Implementación de multihilo (Concurrency) para paquetes JSON sin bloquear la GUI
- [ ] Pruebas y depuración unitaria del emparejamiento BLE y transmisión JSON bidireccional

---

## 🛠️ Tecnologías

| Componente | Tecnología |
|---|---|
| Aplicación de escritorio | C++/CLI (.NET Framework) — Visual Studio |
| Interfaz de usuario | Windows Forms |
| Comunicación inalámbrica | Bluetooth Low Energy (BLE) |
| Protocolo de datos | JSON |
| Firmware del reloj | Arduino (ESP32-S3) |
| Interfaz gráfica del reloj | LVGL v9.4 |
| Sensor de caídas | Acelerómetro BMI160 (I2C) |
| Control de versiones | Git / GitHub |
| Gestión de proyecto | Trello |

---

## 👥 Equipo

| Integrante | Código | Responsabilidad Sprint 1 |
|---|---|---|
| Javier Armando Bonilla Flores | 20234901 | Configuración entorno JNOX HEALTH (Arduino + LVGL v9.4) |
| Esthefany Hualparuca Sedano| 20236286 | Diseño del Diagrama de Clases en StarUML |
| Anthony Aquiles William Tufiño Ugarte | 20226010 | Programación de las 10 clases en C++/CLI |
| Piero Elguera Quichcas | 20236454 | GitHub, Trello y redacción del Catálogo de Requisitos |

---

## 📁 Estructura del Repositorio

```
ProyectoPOO_2026_PUCP/
├── ProyectoPOO/
│   ├── pch.h
│   ├── PROYECTO_POO.cpp        ← 10 clases del modelo de dominio
│   └── ProyectoPOO.sln         ← Solución de Visual Studio
├── JNOX_Firmware/              ← Código Arduino (ESP32-S3 + LVGL)
├── docs/
│   └── Entregable_1_Sistema_Salud_FINAL.docx
└── README.md
```

---

## 🚀 Cómo ejecutar

### Aplicación de Escritorio (C++/CLI)

1. Clonar el repositorio:
   ```bash
   git clone https://github.com/C-137J/ProyectoPOO_2026_PUCP.git
   ```
2. Abrir `ProyectoPOO.sln` en **Visual Studio 2022** (o superior)
3. Seleccionar configuración `Debug | x64`
4. Compilar y ejecutar (`F5`)

### Firmware JNOX (ESP32-S3)

1. Instalar [Arduino IDE](https://www.arduino.cc/en/software) con soporte para ESP32-S3
2. Añadir la librería **LVGL v9.4** desde el Library Manager
3. Abrir el sketch correspondiente
4. Seleccionar la placa `ESP32S3 Dev Module` y el puerto correcto
5. Cargar el firmware

---

## 📄 Documentación

- 📑 [Entregable 1 — Sprint 1 (Word)](./docs/Entregable_1_Sistema_Salud_FINAL.docx)
- 📋 Tablero Trello: *https://trello.com/b/Uoays2LN/proyecto-poo*

---

<div align="center">

**Pontificia Universidad Católica del Perú — Facultad de Ciencias e Ingeniería**

*Programación Orientada a Objetos · 2026*

</div>
