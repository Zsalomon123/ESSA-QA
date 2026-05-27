# Historia de Usuario 15 Cambiar contraseña

## YO Como
usuario autenticado en la Oficina Virtual de ESSA,

## Sprint
16

## Menú de navegación
Menú / Mi perfil / Cambiar contraseña.

## Objetivo
Esta historia de usuario tiene como propósito permitir al usuario autenticado en la Oficina Virtual realizar el cambio de su contraseña de acceso de forma segura y controlada.
Problema que resuelve: Los usuarios necesitan un mecanismo accesible dentro de la Oficina Virtual para actualizar su contraseña sin depender de canales externos o soporte técnico, garantizando la autonomía en la gestión de la seguridad de su cuenta.
Valor que aporta:
-	Fortalece la seguridad de las cuentas de usuario al facilitar la actualización periódica de contraseñas.
-	Reduce la carga operativa del soporte técnico al ofrecer un flujo de autogestión.
-	Garantiza trazabilidad mediante el registro de auditoría de la acción realizada.

Resultado esperado en el sistema: El usuario puede cambiar su contraseña a través de un flujo delegado al servicio B2C de EPM, recibir confirmación del cambio exitoso, y la acción queda registrada en el sistema de auditoría.

## Precondiciones

PRE-01	El usuario debe estar autenticado (sesión activa) en la Oficina Virtual.

PRE-02	El servicio B2C de EPM debe estar disponible y operativo para gestionar el flujo de cambio de contraseña.

PRE-03	El usuario debe tener acceso a la sección "Mi perfil" desde el menú principal de la Oficina Virtual.

PRE-04	El servicio de registro de auditoría (https://proyectosvass.atlassian.net/browse/ESD-187) debe estar implementado y disponible.

PRE-05	Los diseños de interfaz aprobados para esta funcionalidad deben estar disponibles como referencia de implementación.

## Postcondiciones

POST-01	Cambio exitoso: La contraseña del usuario ha sido actualizada en el servicio B2C de EPM.

POST-02	Cambio exitoso: El sistema muestra un mensaje informativo de confirmación al usuario.

POST-03	Cambio exitoso: El usuario es redirigido automáticamente a la pantalla Home de la Oficina Virtual.

POST-04	Cambio exitoso: La acción de cambio de contraseña queda registrada en el sistema de auditoría conforme a https://proyectosvass.atlassian.net/browse/ESD-187.

POST-05	Error o cancelación: La sesión del usuario permanece activa.

POST-06	Error o cancelación: El sistema muestra un mensaje informativo correspondiente al error o cancelación.

POST-07	Error o cancelación: Si el error ocurre en el registro de auditoría, la operación principal no se interrumpe y el error se registra en el log técnico.

## Reglas de negocio

RN-01	El flujo de cambio de contraseña debe ser gestionado íntegramente por el servicio B2C de EPM. La Oficina Virtual no gestiona directamente las validaciones de contraseña, reglas de seguridad ni la confirmación del cambio.

RN-02	El sistema debe enviar al servicio B2C el atributo identificador de la compañía con el valor fijo "ESSA" en cada solicitud de cambio de contraseña.

RN-03	Las reglas de complejidad, longitud mínima/máxima, historial de contraseñas y demás políticas de seguridad de la contraseña son definidas y validadas exclusivamente por el servicio B2C de EPM.
RN-04	Si el cambio de contraseña es exitoso, el sistema debe redirigir al usuario al Home de la Oficina Virtual.

RN-05	Si el usuario cancela el proceso de cambio de contraseña o si ocurre un error, la sesión del usuario debe mantenerse activa y no debe cerrarse.

RN-06	Toda acción de cambio de contraseña (exitosa o fallida) debe registrarse en el sistema de auditoría conforme a la especificación de https://proyectosvass.atlassian.net/browse/ESD-187, incluyendo los atributos: Fecha de Registro, Canal, Dispositivo, IP Pública, Usuario, Servicio (endpoint), Verbo HTTP, Resultado HTTP y Estado.

RN-07	Si ocurre un error al registrar la auditoría, el sistema no debe interrumpir la operación principal del cambio de contraseña. El error debe registrarse en el log técnico del sistema.

RN-08	Los componentes visuales de la funcionalidad deben ajustarse estrictamente al diseño de interfaz aprobado.

RN-09	El envío de correo de confirmación de cambio de contraseña (si aplica) es responsabilidad del servicio B2C de EPM, no de la Oficina Virtual. (Nota: pendiente de confirmación con el equipo si B2C gestiona el envío del correo de notificación).

## Criterios de Aceptación

1.	Acceso a la funcionalidad: Desde la pantalla Home de la Oficina Virtual, el usuario debe poder navegar a la opción Menú > Mi perfil > Cambiar contraseña. La opción debe ser visible y accesible para todo usuario autenticado.

2.	Redirección al servicio B2C: Al seleccionar la opción "Cambiar contraseña", el sistema debe redirigir al usuario a un flujo de cambio de contraseña gestionado por el servicio B2C de EPM mediante un enlace embebido dentro de la Oficina Virtual.

3.	Envío del identificador de compañía: El sistema debe enviar al servicio B2C el atributo identificador de la compañía con el valor "ESSA" como parte de la solicitud de cambio de contraseña. las especificaciones técnicas se pueden ver en: https://grupovass.sharepoint.com/:w:/t/OperacionColombia/IQBw7tneF1KARZ56LG9UKJpAAap4jregtKkXDjqtyF3zp10?e=FQp85C

4.	Delegación de validaciones al servicio B2C: Las validaciones de la contraseña actual, la nueva contraseña (complejidad, longitud, historial), las reglas de seguridad y la confirmación del cambio deben ser gestionadas íntegramente por el servicio B2C de EPM. La Oficina Virtual no debe implementar validaciones propias sobre la contraseña.

5.	Cambio exitoso — Mensaje de confirmación: Si el cambio de contraseña es exitoso, el sistema debe mostrar un mensaje informativo de confirmación al usuario indicando que la contraseña ha sido actualizada correctamente.

6.	Cambio exitoso — Redirección al Home: Tras mostrar el mensaje de confirmación, el sistema debe redirigir automáticamente al usuario a la pantalla Home de la Oficina Virtual.

7.	Error en el proceso: Si ocurre un error durante el proceso de cambio de contraseña (error del servicio B2C, timeout, error de red u otro), el sistema debe mantener la sesión del usuario activa y mostrar un mensaje de error informativo correspondiente al tipo de fallo.

8.	Cancelación por el usuario: Si el usuario cancela el proceso de cambio de contraseña, el sistema debe mantener la sesión activa y mostrar un mensaje informativo indicando que el proceso fue cancelado, sin afectar el estado de la contraseña actual.

9.	Registro de auditoría: El sistema debe registrar en el sistema de auditoría (https://proyectosvass.atlassian.net/browse/ESD-187) la acción de cambio de contraseña, incluyendo los atributos: Fecha de Registro, Canal (web/móvil), Dispositivo (navegador/SO), IP Pública, Usuario, Servicio (endpoint consumido), Verbo HTTP, Resultado HTTP y Estado (Exitosa/Fallida).

10.	Resiliencia del registro de auditoría: Si ocurre un error al registrar la auditoría, el sistema no debe interrumpir la operación principal de cambio de contraseña. El error debe registrarse en el log técnico del sistema.

11.	Conformidad visual: Todos los componentes visuales de la funcionalidad (pantallas, botones, mensajes, navegación) deben mostrarse conforme al diseño de interfaz aprobado para esta historia de usuario.


## Datos de Entrada / Salida

Datos de Entrada:
| Campo                               | Tipo de Dato | Obligatorio | Origen                    | Restricciones / Formato                                                                 |
|------------------------------------|---------------|--------------|---------------------------|------------------------------------------------------------------------------------------|
| Identificador de compañía          | String        | Sí           | Sistema (valor fijo)      | Valor constante: "ESSA". Enviado automáticamente al servicio B2C.                       |
| Token de sesión del usuario        | String        | Sí           | Sistema                   | Token de autenticación activo del usuario en la Oficina Virtual.                        |
| Contraseña actual                  | String        | Sí           | Usuario (vía B2C)         | Capturada y validada por el servicio B2C de EPM.                                        |
| Nueva contraseña                   | String        | Sí           | Usuario (vía B2C)         | Capturada y validada por el servicio B2C de EPM según sus políticas de seguridad.       |
| Confirmación de nueva contraseña   | String        | Sí           | Usuario (vía B2C)         | Capturada y validada por el servicio B2C de EPM.                                        |

Datos de Salida:
| Campo                               | Tipo de Dato | Obligatorio | Origen               | Restricciones / Formato                                                           |
|------------------------------------|---------------|--------------|----------------------|------------------------------------------------------------------------------------|
| Identificador de compañía          | String        | Sí           | Sistema (valor fijo) | Valor constante: "ESSA". Enviado automáticamente al servicio B2C.                 |
| Token de sesión del usuario        | String        | Sí           | Sistema              | Token de autenticación activo del usuario en la Oficina Virtual.                  |
| Contraseña actual                  | String        | Sí           | Usuario (vía B2C)    | Capturada y validada por el servicio B2C de EPM.                                  |
| Nueva contraseña                   | String        | Sí           | Usuario (vía B2C)    | Capturada y validada por el servicio B2C de EPM según sus políticas de seguridad. |
| Confirmación de nueva contraseña   | String        | Sí           | Usuario (vía B2C)    | Capturada y validada por el servicio B2C de EPM.                                  |


## Criterios de Aceptación No Funcionales
CANF-01	Rendimiento	La redirección al flujo de cambio de contraseña del servicio B2C debe completarse en un tiempo no mayor a 3 segundos bajo condiciones normales de red.

CANF-02	Rendimiento	El registro de auditoría debe ejecutarse de forma asíncrona para no impactar el tiempo de respuesta de la operación principal.

CANF-03	Seguridad	La comunicación entre la Oficina Virtual y el servicio B2C de EPM debe realizarse mediante protocolo HTTPS.

CANF-04	Seguridad	El token de sesión del usuario debe validarse antes de iniciar el flujo de cambio de contraseña.

CANF-05	Seguridad	No se debe almacenar ni registrar en logs la contraseña actual ni la nueva contraseña del usuario en ningún componente de la Oficina Virtual.

CANF-06	Compatibilidad	La funcionalidad debe ser compatible con los navegadores soportados por la Oficina Virtual (Chrome, Firefox, Edge, Safari en sus últimas dos versiones).

CANF-07	Compatibilidad	La interfaz debe ser responsive y funcionar correctamente en dispositivos móviles y de escritorio.

CANF-08	Disponibilidad	En caso de indisponibilidad del servicio B2C, el sistema debe mostrar un mensaje informativo al usuario indicando que el servicio no está disponible temporalmente.


## Dependencias
ESD-187 Debe estar implementada y disponible para que el registro de auditoría de esta funcionalidad opere correctamente. Estado actual: Finalizada.

ESD-193 Esta HU pertenece a la épica "Mi perfil". La navegación Menú > Mi perfil debe estar disponible.

