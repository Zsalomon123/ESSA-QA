# Historia de Usuario 16 cerrar sesion

## YO Como
usuario autenticado en la oficina virtual de ESSA-Soluciones Digitales.

## Sprint
16

## Menú de navegación
Menú / cerrar sesion

## Objetivo
El propósito de esta historia de usuario es proporcionar al usuario un mecanismo seguro, accesible y consistente para finalizar su sesión activa dentro de la oficina virtual.

Problema que resuelve: Sin un proceso formal de cierre de sesión, la sesión del usuario podría permanecer activa indefinidamente, exponiendo información sensible a terceros no autorizados, especialmente en dispositivos compartidos o públicos.

Valor que aporta:
-	Garantiza la seguridad del usuario al invalidar tokens de sesión tanto a nivel local como en el proveedor de identidad (B2C).
-	Proporciona trazabilidad mediante el registro de eventos de auditoría asociados al cierre de sesión.
-	Mejora la experiencia de usuario al ofrecer un flujo claro con confirmación previa y posibilidad de cancelación.

Resultado esperado en el sistema: La sesión del usuario queda completamente invalidada (local y B2C cuando aplique), se genera un registro de auditoría del evento y el usuario es redirigido a la pantalla de inicio de sesión.


## Precondiciones

PRE-01	El usuario debe haber iniciado sesión exitosamente en la oficina virtual y contar con una sesión activa válida.

PRE-02	El servicio de autenticación B2C debe estar disponible y operativo para la notificación de cierre de sesión (cuando aplique).

PRE-03	El componente de opción "Cerrar sesión" debe estar visible y accesible desde todas las pantallas de la aplicación (menú global, header o componente persistente).

PRE-04	El servicio de registro de auditoría debe estar disponible para la escritura de eventos.

PRE-05	El dispositivo del usuario debe contar con conectividad de red activa.


## Postcondiciones

POST-01	La sesión local del usuario queda completamente invalidada (tokens de sesión, cookies y almacenamiento local eliminados).

POST-02	El servicio B2C ha sido notificado del cierre de sesión (cuando aplique), invalidando el token en el proveedor de identidad.

POST-03	El usuario ha sido redirigido a la pantalla de inicio de sesión de la oficina virtual.

POST-04	Se ha registrado un evento de auditoría con la información del cierre de sesión (fecha/hora, correo electrónico, IP de origen, resultado).

POST-05	El usuario no puede acceder a ninguna funcionalidad protegida sin volver a autenticarse.

POST-06	En caso de cancelación, la sesión permanece activa sin pérdida de información ni alteración del estado previo.


## Reglas de negocio

RN01	La opción "Cerrar sesión" debe estar disponible y visible desde cualquier pantalla de la aplicación, sin excepción.

RN02	Antes de ejecutar el cierre de sesión, el sistema debe solicitar confirmación explícita al usuario mediante un mensaje/diálogo de confirmación.

RN03	Si el usuario confirma el cierre de sesión, el sistema debe invalidar la sesión local (eliminar tokens, cookies y datos de sesión del almacenamiento del navegador/dispositivo).

RN04	Si la sesión fue autenticada a través del servicio B2C, el sistema debe notificar al servicio B2C para invalidar la sesión en el proveedor de identidad.

RN05	Si el usuario cancela la acción de cierre de sesión, el sistema debe mantener la sesión activa sin pérdida de información, datos en formularios o estado de navegación.

RN06	Tras el cierre exitoso de sesión, el sistema debe redirigir al usuario a la pantalla de inicio de sesión.

RN07	Toda acción de cierre de sesión (exitosa o fallida) debe generar un registro de auditoría con la siguiente información mínima: fecha y hora del evento, correo electrónico del usuario, dirección IP de origen y resultado del intento (Exitoso / Fallido).

RN08	Los elementos de la interfaz de usuario relacionados con el cierre de sesión deben mostrarse correctamente alineados, con íconos visibles y textos legibles, conforme al diseño aprobado para las versiones web y móvil.

RN09	En caso de fallo en la comunicación con el servicio B2C, el sistema debe igualmente invalidar la sesión local y registrar el evento de auditoría con resultado "Fallido" en la notificación B2C, sin bloquear al usuario.

## Criterios de Aceptación

11.	Acceso a la opción "Cerrar sesión": Desde cualquier pantalla de la aplicación (web y móvil), el usuario debe visualizar la opción "Cerrar sesión" de forma accesible y visible (Para mobile en el menú de la barra inferior y en web en el menú que se despliega desde el footer de la app, ingresando a la opción de “mi perfil”). El sistema deberá garantizar que esta opción esté disponible de manera persistente en todas las vistas de la aplicación.

2.	Visualización del mensaje de confirmación: Al seleccionar la opción "Cerrar sesión", el sistema deberá mostrar un diálogo/modal de confirmación con un mensaje claro que solicite al usuario validar su intención de cerrar sesión (por ejemplo: "¿Está seguro de que desea cerrar sesión?"), presentando las opciones "Confirmar" y "Cancelar".

3.	Flujo de confirmación – Invalidación de sesión local: Si el usuario selecciona "Confirmar", el sistema deberá invalidar la sesión local del usuario, eliminando todos los tokens de autenticación, cookies de sesión y datos almacenados localmente asociados a la sesión activa.

4.	Flujo de confirmación – Notificación al servicio B2C: Inmediatamente después de invalidar la sesión local, el sistema deberá enviar una solicitud al servicio B2C para notificar el cierre de sesión e invalidar el token en el proveedor de identidad, cuando la sesión haya sido autenticada a través de dicho servicio.

5.	Flujo de confirmación – Redirección: Una vez completada la invalidación de sesión (local y B2C cuando aplique), el sistema deberá redirigir automáticamente al usuario a la pantalla de inicio de sesión de la oficina virtual.

6.	Flujo de confirmación – Bloqueo de acceso posterior: Tras el cierre de sesión, si el usuario intenta acceder a cualquier funcionalidad protegida mediante navegación directa (URL, botón atrás del navegador), el sistema deberá redirigirlo a la pantalla de inicio de sesión sin permitir el acceso.

7.	Flujo de cancelación: Si el usuario selecciona "Cancelar" en el diálogo de confirmación, el sistema deberá cerrar el diálogo/modal y mantener la sesión activa sin pérdida de información, preservando el estado actual de la pantalla, datos en formularios y contexto de navegación.

8.	Registro de auditoría – Cierre exitoso: Al completarse exitosamente el cierre de sesión, el sistema deberá registrar un evento de auditoría con la siguiente información mínima: fecha y hora del evento, correo electrónico del usuario, dirección IP de origen y resultado = "Exitoso".

9.	Registro de auditoría – Cierre fallido: En caso de que el proceso de cierre de sesión falle (por ejemplo, error en la comunicación con B2C), el sistema deberá registrar un evento de auditoría con la misma información mínima y resultado = "Fallido". El sistema deberá igualmente invalidar la sesión local y redirigir al usuario a la pantalla de inicio de sesión.

10.	Consistencia visual: Todos los elementos de la interfaz relacionados con el cierre de sesión (opción en menú, diálogo de confirmación, íconos, botones) deberán mostrarse correctamente alineados, con íconos visibles y textos legibles, de acuerdo con el diseño entregado para la versión web y la versión móvil.

11.	Referencia técnica: las especificaciones técnicas se pueden ver en: https://grupovass.sharepoint.com/:w:/t/OperacionColombia/IQBw7tneF1KARZ56LG9UKJpAAap4jregtKkXDjqtyF3zp10?e=FQp85CConecta tu cuenta de OneDrive 

## Datos de Entrada / Salida

Datos de Entrada:

| Campo                                | Tipo de dato                | Obligatorio | Origen                                   | Restricciones / Formato                                                                 |
|-------------------------------------|-----------------------------|--------------|--------------------------------------------|------------------------------------------------------------------------------------------|
| Acción del usuario (Confirmar / Cancelar) | Booleano (interacción UI) | Sí           | Usuario                                    | Selección en diálogo de confirmación. Solo dos opciones válidas: Confirmar o Cancelar. |
| Token de sesión activa              | String (JWT / Token)        | Sí           | Sistema (almacenamiento local / cookies)   | Debe ser un token válido y vigente.                                                     |
| Correo electrónico del usuario      | String                      | Sí           | Sistema (sesión activa)                    | Formato de correo electrónico válido. Obtenido del contexto de sesión.                  |
| Dirección IP de origen              | String                      | Sí           | Sistema (request HTTP)                     | Formato IPv4 o IPv6 válido. Capturado automáticamente del request.                      |
| Fecha y hora del evento             | DateTime                    | Sí           | Sistema                                    | Formato ISO 8601 (YYYY-MM-DDTHH:mm:ss.sssZ). Generado automáticamente al momento del evento. |
| Identificador de sesión B2C         | String                      | Condicional  | Sistema (proveedor B2C)                    | Requerido solo cuando la sesión fue autenticada vía B2C.                                |


Datos de Salida:

| Salida                                         | Tipo                              | Descripción                                                                                                      |
|------------------------------------------------|-----------------------------------|------------------------------------------------------------------------------------------------------------------|
| Mensaje de confirmación                        | UI (Diálogo/Modal)                | Mensaje solicitando al usuario confirmar o cancelar el cierre de sesión.                                        |
| Redirección a pantalla de inicio de sesión     | Navegación                        | El sistema redirige al usuario a la pantalla de login tras el cierre exitoso.                                   |
| Registro de auditoría                          | Registro en base de datos / log   | Evento registrado con: fecha/hora, correo electrónico, IP de origen, resultado (Exitoso / Fallido).            |
| Notificación de cierre al servicio B2C         | Llamada a servicio externo        | Solicitud de invalidación de sesión al proveedor de identidad B2C (cuando aplique).                             |

## Criterios de Aceptación No Funcionales
CANF-01	Rendimiento	La redirección al flujo de cambio de contraseña del servicio B2C debe completarse en un tiempo no mayor a 3 segundos bajo condiciones normales de red.

CANF-01	Rendimiento	El proceso de cierre de sesión (invalidación local + notificación B2C + redirección) no debe superar los 3 segundos en condiciones normales de conectividad.

CANF-02	Rendimiento	El diálogo de confirmación debe renderizarse en menos de 500 milisegundos tras la selección de la opción "Cerrar sesión".

CANF-03	Seguridad	Todos los tokens de sesión, cookies y datos de almacenamiento local deben ser eliminados de forma completa e irreversible al confirmar el cierre de sesión.

CANF-04	Seguridad	La comunicación con el servicio B2C para la notificación de cierre de sesión debe realizarse a través de un canal seguro (HTTPS/TLS).

CANF-05	Seguridad	Tras el cierre de sesión, no debe ser posible reutilizar tokens expirados para acceder a recursos protegidos (protección contra replay de tokens).

CANF-06	Accesibilidad	El diálogo de confirmación y la opción "Cerrar sesión" deben ser accesibles mediante navegación por teclado (Tab, Enter, Escape) y compatibles con lectores de pantalla (atributos ARIA).

CANF-07	Compatibilidad	La funcionalidad debe operar correctamente en los navegadores soportados (Chrome, Firefox, Safari, Edge – últimas 2 versiones) y en las versiones móviles (responsive) según el diseño aprobado.

CANF-08	Disponibilidad	En caso de indisponibilidad del servicio B2C, el sistema debe completar el cierre de sesión local sin bloquear al usuario, registrando el fallo en auditoría.

## Dependencias
DS-01	HU de Inicio de sesión (Épica ESD-332: Acceso a usuarios)	La funcionalidad de cierre de sesión depende de que el flujo de inicio de sesión esté implementado, ya que requiere una sesión activa previa y la pantalla de login como destino de redirección.

DS-02	HU de Registro de auditoría	Si existe una HU específica para la implementación del servicio de auditoría, esta HU depende de su finalización para el registro de eventos.

Diseños

Menú web: ESSA - Oficina Virtual Diseño UX_UI Vigente 
Menú mobile: (pendiente de entrega de UX)


