<div align="center">

<img src="https://img.shields.io/badge/C%2B%2B%2FCLI-.NET%20Framework-blue?style=for-the-badge&logo=cplusplus&logoColor=white"/>

<img src="https://img.shields.io/badge/ESP32--S3-Arduino-red?style=for-the-badge&logo=arduino&logoColor=white"/>

<img src="https://img.shields.io/badge/BLE-Bluetooth%20Low%20Energy-blueviolet?style=for-the-badge&logo=bluetooth&logoColor=white"/>

<img src="https://img.shields.io/badge/LVGL-v9.4-orange?style=for-the-badge"/>

<img src="https://img.shields.io/badge/Estado-Sprint%202%20Completado-success?style=for-the-badge"/>

# 🩺 Sistema Integrado de Asistencia y Gestión de Salud

**Programación Orientada a Objetos — PUCP 2026**

*Sistema de monitoreo para adultos mayores con integración BLE al smartwatch JNOX*

</div>

---

## 📋 Descripción del Proyecto

Este proyecto consiste en el desarrollo de un **sistema de escritorio** que actúa como central de monitoreo y gestión para adultos mayores, integrándose vía **Bluetooth** con el smartwatch **JNOX**, dispositivo basado en el microcontrolador **ESP32-S3**.

El sistema permite a familiares y cuidadores:
- 👤 Administrar perfiles clínicos de pacientes
- ⏰ Programar recordatorios diarios (medicación, citas, rutinas)
- 🚨 Recibir alertas críticas en tiempo real ante caídas detectadas por el sensor **BMI160**
- 🩻 Gestionar recomendaciones médicas por parte de profesionales de salud

---

## 🏗️ Arquitectura del Sistema

### Arquitectura en Capas (Entregable 2)

El software de escritorio fue reestructurado bajo el **patrón de arquitectura en capas**, compuesto por cuatro niveles:

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                        CAPA DE PRESENTACIÓN (GUI)                            │
│   Windows Forms: Login, Mantenimientos CRUD, Monitoreo, Historial Clínico    │
├──────────────────────────────────────────────────────────────────────────────┤
│                        CAPA DE CONTROLADOR (Controller)                      │
│   UsuarioController · PacienteController · ContactoController                │
│   EventoController · DispositivoController                                   │
├──────────────────────────────────────────────────────────────────────────────┤
│                        CAPA DE PERSISTENCIA (Persistence)                    │
│   UsuarioAccesoDatos · PacienteAccesoDatos · ContactoAccesoDatos             │
│   EventoAccesoDatos · DispositivoAccesoDatos                                 │
│   Archivos: usuarios.txt · pacientes.txt · contactos.txt · eventos.txt       │
├──────────────────────────────────────────────────────────────────────────────┤
│                        CAPA DE MODELO (Model)                                │
│   Usuario · Cuidador · Medico · Paciente · ContactoEmergencia                │
│   DispositivoJNOX · EventoSalud · Recordatorio · AlertaCaida · GestorBLE     │
└──────────────────────────────────────────────────────────────────────────────┘
```

### Integración con Hardware

```
┌─────────────────────────────────┐        BLE (JSON)         ┌─────────────────────────────┐
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

## 📐 Diagrama de Clases — Modelo Completo

El sistema completo está compuesto por **20 clases** organizadas en 3 capas (Model, Persistence, Controller):

### Capa Model — Jerarquía de Usuarios
```
Usuario (base)
├── Cuidador
└── Médico
```

### Capa Model — Jerarquía de Eventos de Salud
```
EventoSalud (base)
├── Recordatorio
└── AlertaCaída
```

### Capa Model — Clases Independientes
```
Paciente · ContactoEmergencia · DispositivoJNOX · GestorBLE
```

### Capa Persistence — Acceso a Datos
```
UsuarioAccesoDatos · PacienteAccesoDatos · ContactoAccesoDatos
EventoAccesoDatos · DispositivoAccesoDatos
```

### Capa Controller — Controladores
```
UsuarioController · PacienteController · ContactoController
EventoController · DispositivoController
```

### Resumen de Clases del Modelo

| Clase | Tipo | Atributos principales | Métodos destacados |
|---|---|---|---|
| `Usuario` | Clase base | `Id`, `Nombre`, `Edad`, `Telefono`, `UsuarioLogin`, `Contrasena` | constructor |
| `Cuidador` | Hereda de Usuario | `Turno`, `PacientesAsignados` | constructor |
| `Médico` | Hereda de Usuario | `Especialidad`, `CentroDeSalud`, `PacientesAsignados` | `GenerarRecomendaciones()` |
| `Paciente` | Independiente | `IDPaciente`, `NombrePaciente`, `TipoSangre`, `SeguroMedico`, `RecomendacionesMedicas (List)`, `ContactosEmergencia`, `HistorialMedico` | `addRecomendacion()`, `addContactoEmergencia()`, `addHistorial()`, `showPacienteInfo()` |
| `ContactoEmergencia` | Independiente | `IDCE`, `IDPacienteAsociado`, `NombreCompleto`, `Parentesco`, `Telefono` | `MostrarContactoEmergencia()` |
| `DispositivoJNOX` | Independiente | `DireccionMAC`, `NivelBateria`, `EstadoConexion`, `UmbralLibre`, `UmbralImpacto` | `setConfigCaida()`, `getNivelBat()` |
| `EventoSalud` | Clase base eventos | `IdEvento`, `IDPacienteAsociado`, `FechaHora`, `Estado` | `setEstado()`, `getEstado()` |
| `Recordatorio` | Hereda de EventoSalud | `MensajeTexto`, `Categoria`, `Repetir`, `IntervaloMinutos` | `setIntervalo()` |
| `AlertaCaída` | Hereda de EventoSalud | `MagnitudImpacto`, `ConfirmacionReloj` | `caidaDetectada()`, `getMagnitudCaida()` |
| `GestorBLE` | Independiente | `EstaConectado`, `MacConectada` | `ConectarReloj()`, `SerializarEventoSalud()` |

### Resumen de Clases de Persistencia

| Clase | Archivo de datos | Métodos CRUD | Característica especial |
|---|---|---|---|
| `UsuarioAccesoDatos` | `usuarios.txt` | Add, QueryAll, QueryById, Update, Delete | Serialización polimórfica Cuidador/Médico |
| `PacienteAccesoDatos` | `pacientes.txt` | Add, QueryAll, QueryById, Update, Delete | Recomendaciones serializadas con separador `\|`, vinculación con DispositivoJNOX por MAC |
| `ContactoAccesoDatos` | `contactos.txt` | Add, QueryAll, QueryByPaciente, Delete | Filtrado por IDPacienteAsociado |
| `EventoAccesoDatos` | `eventos.txt` | Add, QueryAll, QueryByPaciente, Delete | Serialización polimórfica Recordatorio/AlertaCaida |
| `DispositivoAccesoDatos` | `dispositivos.txt` | Add, QueryAll, QueryByMAC, Update, Delete | Clave primaria por DireccionMAC |

### Resumen de Clases Controller

| Clase | Métodos proxy | Métodos de negocio |
|---|---|---|
| `UsuarioController` | CRUD + GuardarDatos/CargarDatos | `ValidarLogin()` — autenticación de usuarios |
| `PacienteController` | CRUD + GuardarDatos/CargarDatos | — |
| `ContactoController` | CRUD + GuardarDatos/CargarDatos | — |
| `EventoController` | CRUD + GuardarDatos/CargarDatos | — |
| `DispositivoController` | CRUD + GuardarDatos/CargarDatos | — |

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

### Sprint 2 — ✅ Completado

- [x] Reestructuración del proyecto bajo el patrón de arquitectura en capas (Model, Persistence, Controller, GUI)
- [x] Modificación de clases del modelo para soportar persistencia (nuevos atributos de vinculación entre entidades)
- [x] Implementación de 5 clases de acceso a datos (Persistence) con operaciones CRUD y persistencia en archivos `.txt`
- [x] Implementación de 5 clases de control (Controller) como intermediarios entre GUI y Persistence
- [x] Desarrollo de interfaces gráficas: `frmLogin`, `JnoxMainForm` (MDI), `frmMantenimientoUsuario`, `frmMantenimientoPaciente`, `frmMantenimientoDispositivo`
- [x] Desarrollo de prototipos transaccionales: `frmMonitoreoAlertas` y `frmHistorialClinico`
- [x] Actualización del Diagrama de Clases en StarUML con las 20 clases del sistema

### Pendiente (Sprints futuros)

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
| Persistencia de datos | Archivos de texto plano (.txt) con System::IO |
| Comunicación inalámbrica | Bluetooth Low Energy (BLE) |
| Protocolo de datos | JSON |
| Firmware del reloj | Arduino (ESP32-S3) |
| Interfaz gráfica del reloj | LVGL v9.4 |
| Sensor de caídas | Acelerómetro BMI160 (I2C) |
| Control de versiones | Git / GitHub |
| Gestión de proyecto | Trello |

---

## 👥 Equipo

| Integrante | Código | Responsabilidad Sprint 1 | Responsabilidad Sprint 2 |
|---|---|---|---|
| Javier Armando Bonilla Flores | 20234901 | Configuración entorno JNOX HEALTH (Arduino + LVGL v9.4) | Arquitectura en capas, migración Model, modificaciones al modelo, frmMonitoreoAlertas |
| Esthefany Hualparuca Sedano | 20236286 | Diseño del Diagrama de Clases en StarUML | Actualización del Diagrama de Clases (20 clases), frmMantenimientoPaciente, ContactoAccesoDatos/Controller |
| Anthony Aquiles William Tufiño Ugarte | 20226010 | Programación de las 10 clases en C++/CLI | UsuarioAccesoDatos, PacienteAccesoDatos, UsuarioController, PacienteController, frmLogin, frmMantenimientoUsuario, frmMantenimientoDispositivo |
| Piero Elguera Quichcas | 20236454 | GitHub, Trello y redacción del Catálogo de Requisitos | EventoAccesoDatos, DispositivoAccesoDatos, EventoController, DispositivoController, frmHistorialClinico, JnoxMainForm, Trello Sprint 2 |

---

## 📁 Estructura del Repositorio

```
JNOX_MODO_HEALTH/
├── SOFTWARE_PC/
│   └── PROYECTO_POO/
│       ├── pch.h / pch.cpp
│       ├── PROYECTO_POO.cpp           ← Punto de entrada principal
│       ├── PROYECTO_POO.slnx          ← Solución de Visual Studio
│       │
│       ├── ── Model ──────────────────
│       ├── Usuario.h / .cpp
│       ├── Cuidador.h / .cpp
│       ├── Medico.h / .cpp
│       ├── Paciente.h / .cpp
│       ├── ContactoEmergencia.h / .cpp
│       ├── DispositivoJNOX.h / .cpp
│       ├── EventoSalud.h / .cpp
│       ├── Recordatorio.h / .cpp
│       ├── AlertaCaida.h / .cpp
│       ├── GestorBLE.h / .cpp
│       │
│       ├── ── Persistence ────────────
│       ├── UsuarioAccesoDatos.h / .cpp
│       ├── PacienteAccesoDatos.h / .cpp
│       ├── ContactoAccesoDatos.h / .cpp
│       ├── EventoAccesoDatos.h / .cpp
│       ├── DispositivoAccesoDatos.h / .cpp
│       │
│       ├── ── Controller ─────────────
│       ├── UsuarioController.h / .cpp
│       ├── PacienteController.h / .cpp
│       ├── ContactoController.h / .cpp
│       ├── EventoController.h / .cpp
│       ├── DispositivoController.h / .cpp
│       │
│       └── ── Datos persistidos ──────
│           ├── usuarios.txt
│           ├── pacientes.txt
│           ├── contactos.txt
│           ├── eventos.txt
│           └── dispositivos.txt
│
├── ENTREGABLES/
│   ├── ENTREGABLE 1/
│   └── ENTREGABLE 2/
│       └── Entregable_2_FINAL.docx
│
├── Diagrama de clases/                ← Archivos StarUML (.mdj)
├── *.ino                              ← Firmware JNOX (ESP32-S3 + LVGL)
└── README.md
```

---

## 🚀 Cómo ejecutar

### Aplicación de Escritorio (C++/CLI)

1. Clonar el repositorio:
   ```bash
   git clone https://github.com/C-137J/ProyectoPOO_2026_PUCP.git
   ```
2. Abrir `PROYECTO_POO.slnx` en **Visual Studio 2022** (o superior)
3. Seleccionar configuración `Debug | x64`
4. Compilar y ejecutar (`F5`)

> **Nota:** Los archivos de datos (`.txt`) se generan automáticamente en la carpeta de ejecución al cerrar la aplicación. Al reabrirla, los datos se cargan desde dichos archivos.

### Firmware JNOX (ESP32-S3)

1. Instalar [Arduino IDE](https://www.arduino.cc/en/software) con soporte para ESP32-S3
2. Añadir la librería **LVGL v9.4** desde el Library Manager
3. Abrir el sketch correspondiente
4. Seleccionar la placa `ESP32S3 Dev Module` y el puerto correcto
5. Cargar el firmware

---

## 📄 Documentación

- 📑 [Entregable 1 — Sprint 1 (Word)](./ENTREGABLES/ENTREGABLE%201/)
- 📑 [Entregable 2 — Sprint 2 (Word)](./ENTREGABLES/ENTREGABLE%202/Entregable_2_FINAL.docx)
- 📋 Tablero Trello: *https://trello.com/b/Uoays2LN/proyecto-poo*

---

<div align="center">

**Pontificia Universidad Católica del Perú — Facultad de Ciencias e Ingeniería**

*Programación Orientada a Objetos · 2026*

</div>
