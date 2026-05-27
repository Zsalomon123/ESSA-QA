# Historia de Usuario 12 Registrar usuarios

## YO Como
usuario de la oficina virtual de ESSA.

## Sprint
16

## Menú de navegación
Iniciar sesión o registrarse

## Objetivo
Propósito de negocio: Garantizar la continuidad del acceso de los usuarios a la Oficina Virtual de ESSA, reduciendo la carga operativa del equipo de soporte al ofrecer un mecanismo de autoservicio para la recuperación de contraseñas.

Propósito funcional: Integrar el flujo de recuperación de contraseña delegado al servicio Azure AD B2C de EPM, de manera que la Oficina Virtual actúe como orquestador de la navegación (redirección al flujo B2C y retorno), mientras que toda la lógica de validación de identidad, envío de notificaciones y restablecimiento de credenciales sea gestionada íntegramente por B2C.

Resultado esperado: El usuario puede recuperar su contraseña de forma autónoma desde la pantalla de inicio de sesión, completando el proceso a través de B2C y siendo redirigido de vuelta a la Oficina Virtual para iniciar sesión con sus nuevas credenciales.


## Precondiciones

PRE-01	El usuario debe estar previamente registrado en el servicio B2C de EPM con credenciales válidas asociadas a la compañía ESSA.

PRE-02	El servicio Azure AD B2C de EPM debe estar disponible y operativo.

PRE-03	La aplicación (Oficina Virtual) debe estar configurada con el enlace embebido al flujo de autenticación/recuperación de B2C.

PRE-04	El atributo identificador de compañía "ESSA" debe estar configurado y ser enviado al servicio B2C en cada invocación del flujo.

PRE-05	El usuario debe tener acceso a la pantalla inicial de la aplicación (no requiere sesión activa).

PRE-06	El servicio de registro de auditoría (https://proyectosvass.atlassian.net/browse/ESD-187) debe estar implementado y disponible.

## Postcondiciones

POST-01 Si el proceso de recuperación fue exitoso, la contraseña del usuario ha sido restablecida en B2C y el usuario es redirigido a la pantalla de inicio de sesión de la Oficina Virtual.

POST-02 El usuario puede iniciar sesión con la nueva contraseña establecida.

POST-03 Se ha enviado un correo electrónico de recuperación/confirmación al usuario (gestionado íntegramente por B2C; no personalizable desde la Oficina Virtual).

POST-04 Todas las acciones relevantes del flujo han sido registradas en el sistema de auditoría conforme a la especificación de https://proyectosvass.atlassian.net/browse/ESD-187.

POST-05 Si el proceso fue cancelado o falló, el usuario permanece en la Oficina Virtual con un mensaje informativo y la posibilidad de reintentar.

## Reglas de negocio
RN01	El proceso de validación de identidad, envío de notificaciones y restablecimiento de contraseña es gestionado completamente por el servicio Azure AD B2C de EPM. La Oficina Virtual no interviene en la lógica de recuperación.

RN02	La Oficina Virtual debe enviar al servicio B2C el atributo identificador de compañía con valor "ESSA" en cada invocación del flujo de autenticación/recuperación. Las especificaciones técnicas de este envío se encuentran documentadas en el https://grupovass.sharepoint.com/:w:/t/OperacionColombia/IQBw7tneF1KARZ56LG9UKJpAAap4jregtKkXDjqtyF3zp10?e=FQp85C.

RN03	El correo electrónico de recuperación de contraseña es gestionado por B2C. No es posible personalizar el contenido, diseño ni remitente de dicho correo desde la Oficina Virtual.

RN04	La opción de recuperación/cambio de contraseña dentro del flujo de B2C es gestionada exclusivamente por B2C de EPM. La Oficina Virtual no tiene control sobre la interfaz ni el comportamiento de dicha opción.

RN05	Tras completar exitosamente el proceso de recuperación en B2C, el sistema debe redirigir automáticamente al usuario a la pantalla de inicio de sesión de la Oficina Virtual.

RN06	Si B2C retorna un error o el usuario cancela el proceso, el sistema debe mostrar un mensaje informativo al usuario y permitir reintentar la operación de recuperación.

RN07	Todas las acciones realizadas durante el flujo de recuperación de contraseña deben registrarse en el sistema de auditoría conforme a los criterios definidos en https://proyectosvass.atlassian.net/browse/ESD-187.

RN08	El registro de auditoría debe incluir los atributos: Fecha de Registro, Canal, Dispositivo, IP Pública, Usuario (opcional), Cuenta (opcional), Servicio (URL completa del endpoint), Verbo HTTP, Resultado HTTP y Estado (Exitosa/Fallida).

RN09	Si ocurre un error al registrar la auditoría, el sistema no debe interrumpir la operación principal del usuario. El error debe registrarse en el log técnico.

RN10	El flujo de recuperación de contraseña no requiere que el usuario tenga una sesión activa en la Oficina Virtual.


## Criterios de Aceptación
1.	Al acceder a la pantalla inicial de la aplicación (Oficina Virtual), el sistema debe mostrar la opción "Iniciar sesión o registrarse" como un elemento de acción visible y accesible para el usuario.
2.	Al seleccionar la opción "Iniciar sesión o registrarse", el sistema debe redirigir al usuario al flujo de autenticación gestionado por el servicio Azure AD B2C de EPM mediante un enlace embebido configurado en la aplicación.
3.	El sistema debe enviar al servicio B2C el atributo identificador de compañía con valor "ESSA" como parte de la invocación del flujo, conforme a las especificaciones técnicas documentadas en el https://grupovass.sharepoint.com/:w:/t/OperacionColombia/IQBw7tneF1KARZ56LG9UKJpAAap4jregtKkXDjqtyF3zp10?e=FQp85C.
4.	Dentro del flujo de B2C, el usuario debe visualizar la opción para recuperar o cambiar la contraseña. Esta opción es gestionada exclusivamente por B2C de EPM; la Oficina Virtual no tiene control sobre su presentación ni comportamiento.
5.	Al seleccionar la opción de recuperación de contraseña, el servicio B2C ejecutará el proceso completo de:
    -	Validación de identidad del usuario.
    -	Envío de notificación (correo electrónico de recuperación).
    -	Restablecimiento de la contraseña.
    La Oficina Virtual no interviene en ninguna de estas etapas.
6.	El correo electrónico de recuperación de contraseña es enviado y gestionado íntegramente por B2C. No es posible personalizar su contenido, diseño ni remitente desde la Oficina Virtual.
7.	Una vez que el proceso de recuperación de contraseña sea completado exitosamente en B2C, el sistema debe redirigir automáticamente al usuario a la pantalla de inicio de sesión de la Oficina Virtual.
8.	Si el servicio B2C retorna un error durante el proceso de recuperación, el sistema debe mostrar un mensaje informativo al usuario indicando que no fue posible completar la operación, y debe permitir al usuario reintentar la recuperación de contraseña.
9.	Si el usuario cancela el proceso de recuperación dentro del flujo de B2C, el sistema debe mostrar un mensaje informativo y permitir al usuario reintentar o regresar a la pantalla inicial.
10.	Todas las acciones realizadas durante el flujo de recuperación de contraseña (invocación del servicio B2C, redirección, respuesta de B2C, errores) deben registrarse en el sistema de auditoría conforme a los criterios definidos en https://proyectosvass.atlassian.net/browse/ESD-187, incluyendo los atributos: Fecha de Registro, Canal, Dispositivo, IP Pública, Usuario (opcional), Cuenta (opcional), Servicio (URL completa), Verbo HTTP, Resultado HTTP y Estado.
11.	Si ocurre un error al registrar la auditoría, el sistema no debe interrumpir la operación principal del usuario. El error debe registrarse en el log técnico de la aplicación.

## Datos de Entrada / Salida

- Datos de Entrada:
| Campo                                                                 | Tipo de Dato | Obligatorio | Origen                                   | Restricciones / Formato                                                                                     |
|------------------------------------------------------------------------|---------------|--------------|--------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| Identificador de compañía                                              | String        | Sí           | Sistema (configuración)                    | Valor fijo: "ESSA". Enviado automáticamente al servicio B2C.                                                |
| URL del flujo B2C                                                      | String (URL)  | Sí           | Sistema (configuración)                    | Enlace embebido al servicio de autenticación/recuperación de B2C de EPM.                                   |
| Datos de identidad del usuario (correo, código de verificación, nueva contraseña) | Varios        | Sí           | Usuario (a través de la interfaz de B2C)   | Gestionados íntegramente por B2C. La Oficina Virtual no captura ni valida estos datos.                     |


- Datos de Salida:

| Salida                                   | Tipo          | Descripción                                                                                                                                                                  |
|-------------------------------------------|---------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Redirección a pantalla de inicio de sesión | Navegación    | Tras recuperación exitosa, el sistema redirige al usuario a la pantalla de login de la Oficina Virtual.                                                                    |
| Mensaje informativo de error/cancelación  | String (UI)   | En caso de error de B2C o cancelación del usuario, se muestra un mensaje informativo con opción de reintento.                                                              |
| Correo de recuperación de contraseña      | Email         | Enviado por B2C al usuario. No personalizable desde la Oficina Virtual.                                                                                                     |
| Registro de auditoría                     | Registro en BD | Cada acción relevante genera un registro de auditoría con los atributos definidos en ESD-187 (Fecha, Canal, Dispositivo, IP, Usuario, Cuenta, Servicio, Verbo HTTP, Resultado HTTP, Estado). |


## Criterios de Aceptación No Funcionales
CANF-01	Rendimiento	La redirección al flujo de B2C debe ejecutarse en un tiempo no mayor a 3 segundos bajo condiciones normales de red.

CANF-02	Rendimiento	La redirección de retorno desde B2C a la Oficina Virtual debe ejecutarse en un tiempo no mayor a 3 segundos.

CANF-03	Seguridad	La comunicación entre la Oficina Virtual y el servicio B2C debe realizarse mediante protocolo HTTPS (TLS 1.2 o superior).

CANF-04	Seguridad	El atributo identificador de compañía "ESSA" no debe ser expuesto ni modificable por el usuario final.

CANF-05	Seguridad	Los registros de auditoría no deben poder ser modificados ni eliminados por usuarios finales (inmutabilidad garantizada conforme a ESD-187).

CANF-06	Compatibilidad	El flujo de recuperación debe ser funcional en los navegadores principales (Chrome, Firefox, Edge, Safari) en sus últimas dos versiones estables.

CANF-07	Compatibilidad	El flujo debe ser responsive y funcional tanto en dispositivos de escritorio como en dispositivos móviles (aplicación web y móvil).

CANF-08	Disponibilidad	El servicio B2C debe contar con mecanismos de timeout y manejo de errores para evitar que el usuario quede en un estado indefinido si el servicio no responde.


## Dependencias
ESD-187: Registro de auditoriaDone
Debe estar implementada para que los eventos de auditoría de esta HU se registren correctamente. Estado actual: Finalizada.

ESD-332: Acceso a usuariosTo Do
Épica padre que agrupa las funcionalidades de acceso de usuarios. Esta HU forma parte de dicha épica.

