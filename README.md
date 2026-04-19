# Sistema de Monitoreo Central JNOX MODO HEALTH - Software PC

Este repositorio contiene el código fuente del software de escritorio desarrollado en C++/CLI, el cual forma parte del sistema integral JNOX MODO HEALTH para el monitoreo de adultos mayores. 

## Descripción del Proyecto
El sistema funciona como una central de comando operada por cuidadores y familiares, que se integra vía Bluetooth Low Energy (BLE) con el smartwatch JNOX (basado en ESP32-S3). Permite administrar perfiles de pacientes, programar recordatorios diarios y alertar inmediatamente ante eventos críticos como caídas bruscas. La aplicación de escritorio administra la información transmitida por el reloj inteligente para proveer notificaciones visuales y auditivas en tiempo real.

## Componentes y Características
* Administración Orientada a Objetos: Implementación en C++/CLI basada en 11 clases interrelacionadas que estructuran el modelo de datos.
* Roles del Sistema: Registro de cuidadores, médicos y contactos de emergencia con permisos específicos de operabilidad.
* Monitorización Bluetooth (BLE): Envío y recepción de comandos y tramas JSON para agendar notificaciones en la pantalla dinámica LVGL del reloj.
* Detección de Caídas: Recepción continua de las mediciones de impacto (del sensor BMI160) logrando alertar de manera fidedigna incidentes en el entorno del paciente.
* Multihilo (Concurrency): Recepción estructurada de paquetes JSON en simultáneo sin bloquear la interfaz de usuario de Windows Forms (GUI).
* Historial Médico: Guardado persistente de eventos generados por usuarios de perfil adulto mayor, aportando al seguimiento clínico por parte de los médicos asignados.

## Estructura del Código
El programa fue desarrollado aplicando herencia, polimorfismo y encapsulación a través de las siguientes clases principales:

1. Usuario: Entidad abstracta representativa de personas integradas en el sistema.
2. Cuidador: Hereda de Usuario. Encargado de monitorear un grupo de pacientes asignados.
3. Medico: Hereda de Usuario. Especialista calificado para otorgar recomendaciones médicas.
4. Paciente: Hereda de Usuario. Entidad núcleo interconectada al reloj inteligente y a su historial médico.
5. ContactoEmergencia: Registro alterno de aviso en situaciones críticas.
6. DispositivoJNOX: Atributos y estados de hardware del equipo (MAC, batería, etc.).
7. ConfiguracionCaida: Modificadores y umbrales de alta sensibilidad enviados al reloj.
8. EventoSalud: Entidad abstracta para estructurar situaciones de aviso y registros crónicos.
9. Recordatorio: Alerta personalizada configurable (texto, horario y repeticiones), hereda de EventoSalud.
10. AlertaCaida: Evento detonado por movimiento errático fuera de umbrales seguros, hereda de EventoSalud.
11. GestorBLE: Controlador directo de enlaces lógicos y parser JSON para el ecosistema Bluetooth.

Aviso: El código C++ nativo de microcontroladores y configuraciones del ESP32-S3 no se encuentra en esta distribución, este repositorio contiene exclusivamente la interfaz lógica de escritorio para PC.
