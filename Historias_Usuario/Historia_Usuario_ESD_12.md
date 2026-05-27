# Historia de Usuario 12 Registrar usuarios

## YO Como
usuario de la oficina virtual de ESSA.

## Sprint
16

## Menú de navegación
Iniciar sesión o registrarse

## Objetivo
Esta historia de usuario tiene como propósito habilitar el flujo de registro de nuevos usuarios en la oficina virtual de ESSA. El registro se delega al servicio de identidad B2C de EPM, el cual gestiona la creación de credenciales, validaciones de identidad y confirmaciones. La oficina virtual actúa como orquestadora de la redirección, enviando el atributo de identificación de la compañía consumidora ("ESSA") y gestionando las respuestas del servicio B2C (registro exitoso, error o cancelación). Adicionalmente, se garantiza la trazabilidad mediante el registro de eventos de auditoría conforme a lo definido en ESD-187: Registro de auditoriaDone.

Valor de negocio: Permite a nuevos usuarios acceder a la plataforma digital de ESSA de forma autónoma, reduciendo la carga operativa de registro manual y centralizando la gestión de identidad en el servicio B2C de EPM.

## Precondiciones

PRE-01	La pantalla de inicio de sesión de la oficina virtual debe estar disponible y accesible desde navegador web y dispositivo móvil.

PRE-02	El servicio B2C de EPM debe estar operativo y accesible desde la oficina virtual.

PRE-03	El enlace de redirección al servicio B2C debe estar configurado y parametrizado en el sistema, incluyendo el atributo de identificación de la compañía consumidora "ESSA".

PRE-04	El mecanismo técnico de envío del atributo de compañía debe estar definido conforme a la especificación entregada por EPM (documento de referencia: https://grupovass.sharepoint.com/:w:/t/OperacionColombia/IQBw7tneF1KARZ56LG9UKJpAAap4jregtKkXDjqtyF3zp10?e=FQp85CConecta tu cuenta de OneDrive).

PRE-05	El servicio de registro de auditoría (ESD-187: Registro de auditoriaDone) debe estar implementado y disponible para registrar los eventos de esta funcionalidad.

PRE-06	El usuario no debe tener una sesión activa en la oficina virtual.

## Postcondiciones

POST-01	Si el registro es exitoso en B2C, el usuario es redirigido a la pantalla de inicio de sesión de la oficina virtual, listo para autenticarse con sus nuevas credenciales.

POST-02	El correo de bienvenida por nuevo registro es enviado por el servicio B2C de EPM (no por la oficina virtual).

POST-03	Se registra un evento de auditoría con el resultado del intento de registro (exitoso, fallido o cancelado), conforme a los atributos definidos en ESD-187: Registro de auditoriaDone.

POST-04	Si el registro falla o es cancelado, no se crea sesión activa ni se persisten datos parciales del usuario en la oficina virtual.

POST-05	El sistema retorna al usuario a la pantalla de inicio de sesión en caso de cancelación o error.

## Reglas de negocio
RN01	El flujo de registro, validaciones de identidad, creación de credenciales y envío de confirmaciones (incluido el correo de bienvenida) es gestionado exclusivamente por el servicio B2C de EPM. La oficina virtual no interviene en estos procesos.

RN02	En cada invocación al servicio B2C, el sistema deberá enviar un atributo que identifique a la compañía consumidora como "ESSA". El mecanismo técnico, nombre del atributo y forma de envío deben estar parametrizados conforme a la especificación de EPM.

RN03	La redirección al servicio B2C debe realizarse de forma transparente para el usuario, sin exponer información técnica (URLs internas, tokens, parámetros de configuración) ni requerir acciones adicionales por parte del usuario.

RN04	Toda acción relevante del usuario en la pantalla de registro debe generar un evento de auditoría conforme a lo definido en ESD-187: Registro de auditoriaDone, incluyendo como mínimo: fecha y hora del intento, correo electrónico (cuando aplique), dirección IP de origen y resultado del intento (Exitoso / Fallido / Cancelado).

RN05	Si el servicio B2C retorna un error funcional o técnico, el sistema debe mostrar un mensaje informativo genérico al usuario sin exponer detalles técnicos del error.

RN06	Si el usuario cancela el flujo desde B2C, el sistema debe retornarlo a la pantalla de inicio de sesión sin crear sesión activa ni registrar datos parciales.

RN07	El manejo de errores del servicio B2C es exclusivo de dicho servicio; la oficina virtual solo gestiona la presentación del mensaje al usuario y la opción de reintento.

RN08	Los elementos visuales de la pantalla deben cumplir con los diseños entregados para versión web y móvil, garantizando alineación, legibilidad de textos y visibilidad de íconos.

## Criterios de Aceptación

1.	Al acceder a la pantalla inicial de la aplicación (pantalla de inicio de sesión), el sistema debe mostrar la opción "Iniciar sesión o registrarse" como un elemento de acción visible, accesible y claramente diferenciado en la interfaz, tanto en versión web como móvil.
2.	Al seleccionar la opción "Iniciar sesión o registrarse", el sistema debe redirigir al usuario al servicio B2C de EPM mediante la apertura de un enlace embebido. B2C es el encargado de ofrecer al usuario la opción de registrarse o iniciar sesión.
3.	La redirección al servicio B2C debe realizarse de forma transparente para el usuario, sin exponer información técnica (URLs internas, tokens, parámetros de configuración) ni requerir acciones adicionales.
4.	En la invocación del enlace al servicio B2C, el sistema deberá enviar un atributo que identifique a la compañía consumidora como "ESSA". El mecanismo técnico, nombre del atributo y forma de envío deben estar parametrizados conforme a la especificación entregada por EPM (ver documento de referencia en https://grupovass.sharepoint.com/:w:/t/OperacionColombia/IQBw7tneF1KARZ56LG9UKJpAAap4jregtKkXDjqtyF3zp10?e=FQp85CConecta tu cuenta de OneDrive).
5.	El flujo de registro, validaciones de identidad, creación de credenciales y confirmaciones será gestionado exclusivamente por el servicio B2C. La oficina virtual no debe intervenir en ninguno de estos pasos.
6.	Una vez el servicio B2C confirme el registro exitoso del usuario, el sistema deberá redirigir automáticamente al usuario a la pantalla de inicio de sesión de la oficina virtual, donde podrá autenticarse con sus nuevas credenciales.
7.	Si el servicio B2C retorna un error funcional o técnico, el sistema deberá:
    -	Mostrar un mensaje informativo genérico indicando que no fue posible completar el registro (sin exponer detalles técnicos).
    -	Permitir al usuario reintentar la operación desde la misma pantalla.
    -	El manejo del error es exclusivo de B2C; la oficina virtual solo gestiona la presentación del mensaje y la opción de reintento.
8.	Si el usuario cancela el flujo desde B2C, el sistema deberá retornarlo a la pantalla de inicio de sesión de la oficina virtual sin crear sesión activa ni registrar datos parciales del usuario.
9.	Las acciones realizadas por el usuario en esta pantalla deberán registrar un evento de auditoría conforme a lo definido en ESD-187: Registro de auditoriaDone, con la siguiente información mínima:
    -	Fecha y hora del intento (Datetime)
    -	Canal (web o móvil)
    -	Dispositivo (navegador o sistema operativo móvil)
    -	Dirección IP pública de origen
    -	Correo electrónico del usuario (cuando aplique)
    -	Servicio consumido (URL completa del endpoint)
    -	Verbo HTTP utilizado
    -	Código HTTP de respuesta y mensaje de error (cuando aplique)
    -	Resultado del intento: Exitoso / Fallido / Cancelado
10.	Los elementos de la pantalla deberán mostrarse correctamente alineados, con íconos visibles y textos legibles, de acuerdo con el diseño entregado para la versión web y móvil.
11.	El correo de bienvenida por nuevo registro es enviado por el servicio B2C de EPM. La oficina virtual no es responsable del envío de este correo.


## Datos de Entrada / Salida

Datos de Entrada:

| Dato                                                     | Tipo           | Obligatorio | Origen                      | Restricciones / Formato                                                                                     |
|----------------------------------------------------------|----------------|--------------|-----------------------------|--------------------------------------------------------------------------------------------------------------|
| Atributo de compañía consumidora                         | String         | Sí           | Sistema (configuración)     | Valor fijo: "ESSA". Nombre del atributo y mecanismo de envío definidos por EPM.                             |
| URL del servicio B2C                                     | String (URL)   | Sí           | Sistema (configuración)     | URL válida del servicio B2C de EPM. Debe estar parametrizada en la configuración del sistema.              |
| Datos de registro del usuario (nombre, correo, contraseña, etc.) | Varios | Sí           | Usuario (capturados en B2C) | Gestionados exclusivamente por B2C. La oficina virtual no captura ni valida estos datos.                   |

Datos de Salida:

| Dato                                           | Tipo                 | Destino                                              | Descripción                                                                                                                                                 |
|------------------------------------------------|----------------------|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Redirección a pantalla de inicio de sesión     | Navegación           | Usuario                                               | Se ejecuta tras registro exitoso o cancelación en B2C.                                                                                                     |
| Mensaje informativo de error                   | String               | Usuario (pantalla)                                    | Se muestra cuando B2C retorna un error funcional o técnico. Mensaje genérico sin detalles técnicos.                                                       |
| Correo de bienvenida                           | Email                | Usuario (correo electrónico)                          | Enviado por el servicio B2C de EPM tras registro exitoso.                                                                                                  |
| Registro de auditoría                          | Objeto de auditoría  | Sistema (ESD-187: Registro de auditoriaDone)          | Contiene: fecha/hora, canal, dispositivo, IP pública, usuario (opcional), servicio, verbo HTTP, resultado HTTP, estado (Exitoso/Fallido).               |

## Criterios de Aceptación No Funcionales
CANF-01	Rendimiento	El proceso de redirección a B2C y retorno post-autenticación no deberá agregar más de 2 segundos de latencia adicional sobre el tiempo de respuesta de B2C.

CANF-01	Rendimiento	La redirección al servicio B2C debe ejecutarse en un tiempo no mayor a 3 segundos desde la acción del usuario.

CANF-02	Rendimiento	La redirección de retorno desde B2C (registro exitoso, error o cancelación) a la oficina virtual debe ejecutarse en un mayor a 3 segundos.

CANF-03	Seguridad	No se debe exponer información técnica al usuario durante la redirección (URLs internas, tokens, parámetros de configuración, mensajes de error técnicos).

CANF-04	Seguridad	La comunicación entre la oficina virtual y el servicio B2C debe realizarse mediante protocolo HTTPS.

CANF-05	Seguridad	El atributo de compañía consumidora ("ESSA") debe transmitirse de forma segura, conforme al mecanismo definido por EPM.

CANF-06	Accesibilidad	Los elementos de la pantalla deben cumplir con estándares de accesibilidad (contraste de colores, tamaño de fuente legible, navegación por teclado).

CANF-07	Compatibilidad	La funcionalidad debe ser compatible con los navegadores principales (Chrome, Firefox, Safari, Edge) en sus versiones más recientes.

CANF-08	Compatibilidad	La interfaz debe ser responsive y adaptarse correctamente a dispositivos móviles (iOS y Android) conforme a los diseños entregados.

CANF-09	Disponibilidad	Si el servicio de auditoría (ESD-187: Registro de auditoriaDone) no está disponible, el sistema no debe interrumpir la operación principal de registro; el error debe registrarse en el log técnico.

## Dependencias
ESD-187: Registro de auditoriaDone
Debe estar implementada para que los eventos de auditoría de esta HU se registren correctamente. Estado actual: Finalizada.

ESD-332: Acceso a usuariosTo Do
Épica padre que agrupa las funcionalidades de acceso de usuarios. Esta HU forma parte de dicha épica.

