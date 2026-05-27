# Historia de Usuario 11 Iniciar sesión

## YO Como
usuario de la oficina virtual de ESSA.

## Sprint
16

## Menú de navegación
Iniciar sesion

## Objetivo
Esta historia de usuario tiene como propósito implementar el módulo de inicio de sesión de la oficina virtual de ESSA, integrando el proceso de autenticación con el servicio de identidad centralizado Azure AD B2C gestionado por EPM.

Problema que resuelve: Actualmente no existe un mecanismo seguro y centralizado de autenticación que permita a los usuarios de ESSA acceder a los servicios personalizados de la oficina virtual. La integración con B2C garantiza la delegación de la gestión de credenciales, políticas de seguridad y control de intentos fallidos a un proveedor de identidad corporativo.

Valor que aporta:
-	Acceso seguro a la oficina virtual con autenticación federada a través de B2C.
-	Experiencia de usuario unificada con el ecosistema EPM.
-	Reducción de riesgo de seguridad al delegar la gestión de credenciales a un proveedor especializado.
-	Disponibilidad de funcionalidades de acceso rápido (accesos directos) sin necesidad de autenticación.

Resultado esperado: El usuario puede autenticarse exitosamente mediante el flujo B2C (custom policy B2C_1A_SIGNUP_SIGNIN) y acceder a la oficina virtual, donde el sistema crea la sesión local, registra el evento de auditoría y redirige al usuario según corresponda (actualización de datos si es primer ingreso, o gestión de facturación si no lo es).

## Precondiciones

PRE-01	El servicio Azure AD B2C de EPM debe estar disponible y operativo (tenant: b2cnpepmco.onmicrosoft.com).

PRE-02	La custom policy B2C_1A_SIGNUP_SIGNIN debe estar desplegada y configurada en el tenant B2C.

PRE-03	El redirect URI de la oficina virtual debe estar registrado bajo la plataforma Single-page Application (SPA) en el App Registration de Azure B2C (NO bajo plataforma Web).

PRE-04	El clientId de la aplicación debe estar configurado y parametrizado (40181510-1867-491a-854d-c853626111b5 en ambiente de no producción).

PRE-05	Las variables de entorno del frontend (VITE_AZURE_B2C_CLIENT_ID, VITE_AZURE_B2C_TENANT_NAME, VITE_AZURE_B2C_AUTHORITY_HOST, VITE_AZURE_B2C_REDIRECT_URI, VITE_AZURE_B2C_POST_LOGOUT_URI) deben estar configuradas en el pipeline correspondiente.

PRE-06	Los servicios referenciados por los accesos directos (Paga tu factura, Reportar daños, Oficinas, Identifícanos, Información) deben estar disponibles.

PRE-07	El módulo de auditoría (ESD-187) debe estar implementado y disponible para el registro de eventos.

PRE-08	La pantalla de actualización de datos (ESD-14) y la pantalla de gestión de facturación (ESD-38) deben estar implementadas para la redirección post-autenticación.

## Postcondiciones

POST-01	Autenticación exitosa — primer ingreso: Se crea la sesión del usuario en la oficina virtual y se redirige a la pantalla de actualización de datos (ESD-14).

POST-02	Autenticación exitosa — ingreso recurrente: Se crea la sesión del usuario en la oficina virtual y se redirige a la pantalla de gestión de facturación (ESD-38).

POST-03	Los tokens de sesión (id_token, access_token, refresh_token) quedan almacenados en localStorage mediante MSAL y disponibles para consumo del sistema.

POST-04	Se registra un evento de auditoría con los datos de la acción de autenticación (ESD-187).
POST-05	Autenticación fallida: El sistema muestra un mensaje informativo, no se crea sesión y el usuario permanece en la pantalla de login con posibilidad de reintento.

POST-06	Error técnico: El sistema muestra un mensaje indicando imposibilidad de completar el inicio de sesión, no se crea sesión y el usuario puede reintentar.

## Reglas de negocio

RN-01	El sistema debe enviar al servicio B2C un atributo que identifique a ESSA como la compañía consumidora del servicio. La definición técnica del atributo quedará parametrizada para su configuración posterior.

RN-02	El parámetro no estándar channelName=web debe inyectarse obligatoriamente en las peticiones a los endpoints /authorize y /token de B2C, conforme a los requerimientos de la custom policy B2C_1A_SIGNUP_SIGNIN de EPM. Sin este parámetro, B2C responde con errores AADB2C90002 (en /authorize) y AADB2C90085 (en /token).

RN-03	La validación de credenciales, las políticas de seguridad (complejidad de contraseña, bloqueo de cuenta) y el control de intentos fallidos son responsabilidad exclusiva del servicio B2C. La oficina virtual no implementa lógica propia de validación de credenciales.

RN-04	La autenticación debe utilizar el flujo Authorization Code con PKCE (Proof Key for Code Exchange) a través de MSAL.js como public client. No se debe utilizar client_secret en el frontend.

RN-05	Los redirect URI deben estar registrados exclusivamente bajo la plataforma Single-page Application (SPA) en Azure B2C. El registro bajo plataforma "Web" causa error 400 invalid_grant en el endpoint /token.

RN-06	Si es la primera autenticación del usuario en el sistema, se debe redirigir a la pantalla de actualización de datos (ESD-14: Actualizar datos de usuario In Progress).

RN-07	Si NO es la primera autenticación del usuario en el sistema, se debe redirigir a la pantalla de gestión de facturación (ESD-38: Consultar facturaIn Progress).

RN-08	Todas las acciones de autenticación (exitosas, fallidas, errores técnicos) deben generar un registro en el módulo de auditoría (ESD-187: Registro de auditoriaDone).

RN-09	Los accesos directos disponibles antes de iniciar sesión no requieren autenticación y deben ser accesibles por cualquier usuario que ingrese a la aplicación.

RN-10	La opción de "Oficinas" en los accesos directos debe mostrar la funcionalidad de oficinas (ESD-74: Mostrar oficinas cercanas a mi ubicaciónIn Progress y ESD-75: Ver todas las oficinasIn Progress) sin ofrecer la opción de pedir un turno.

RN-11	Los tokens tienen las siguientes vigencias configuradas por B2C: access_token = 3600 s (1 hora), refresh_token = 1.209.600 s (14 días), id_token = 3600 s (1 hora). MSAL refresca silenciosamente mediante acquireTokenSilent mientras el refresh_token sea válido.

RN-12	La configuración storeAuthStateInCookie: true debe mantenerse para mitigar el bloqueo de third-party storage en Safari/iOS.

RN-13	La ubicación del caché de MSAL será localStorage para persistir la sesión entre tabs y reloads.

RN-14	Los elementos visuales deben cumplir con el diseño definido para web y móvil según los diseños en Figma.


## Criterios de Aceptación

1.	A1.	Al ingresar a la aplicación de la oficina virtual, el sistema deberá mostrar una pantalla previa al inicio de sesión que contenga una sección de accesos directos y las opciones de Ingresar y Registrarme.
2.	En la sección de accesos directos, el sistema deberá mostrar las siguientes funcionalidades que no requieren autenticación:
    -	Paga tu factura: Al seleccionar esta opción, el sistema deberá redirigir al usuario a la URL externa: https://sites.placetopay.com/essa/login.
    -	Reportar daños: Al seleccionar esta opción, el sistema deberá redirigir al usuario a la funcionalidad descrita en ESD-44: Reportar un daño a una cuentaIn Progress, específicamente por el flujo de reportar un daño sin cuenta.
    -	Oficinas: Al seleccionar esta opción, el sistema deberá redirigir al usuario a la funcionalidad descrita en ESD-74: Mostrar oficinas cercanas a mi ubicaciónIn Progress y ESD-75: Ver todas las oficinasIn Progress, sin ofrecer la opción de pedir un turno en las oficinas.
    -	Identifícanos: Al seleccionar esta opción, el sistema deberá redirigir al usuario a la funcionalidad descrita en ESD-49: Identificar funcionario In Progress.
    -	Información: Al seleccionar esta opción, el sistema deberá redirigir al usuario a la funcionalidad descrita en ESD-51: Mostrar información relacionada con la app Done.
3.	Al seleccionar la opción Ingresar o Registrarme, el sistema deberá redirigir al usuario al flujo de autenticación gestionado por el servicio Azure AD B2C de EPM mediante un enlace embebido, conforme a la documentación técnica de integración disponible.
4.	Durante la redirección al servicio B2C, el sistema deberá enviar un atributo que identifique a ESSA como la compañía consumidora del servicio. La definición técnica del atributo y del enlace quedará parametrizada para su configuración posterior.
5.	El sistema deberá inyectar el parámetro channelName=web tanto en la petición al endpoint /authorize (vía onRedirectNavigate para navegar manualmente) como en la petición al endpoint /token (vía intercepción del fetch nativo), conforme a los requerimientos de la custom policy B2C_1A_SIGNUP_SIGNIN de EPM.
6.	El proceso de validación de credenciales, políticas de seguridad y control de intentos fallidos será gestionado exclusivamente por el servicio B2C. La oficina virtual no deberá implementar lógica propia de validación de credenciales.
7.	Si la autenticación es exitosa, el sistema deberá:
    -	Recibir la confirmación del servicio B2C (tokens: id_token, access_token, refresh_token).
    -	Crear la sesión del usuario en la oficina virtual almacenando los tokens en localStorage mediante MSAL.
    -	Extraer los claims del usuario (oid, email, name, given_name, family_name, phone_number, personInformation).
    - Registrar un evento de auditoría (ESD-187: Registro de auditoriaDone).
8.	Si es la primera autenticación del usuario en el sistema, el sistema deberá redirigir al usuario a la pantalla de actualización de datos (ESD-14: Actualizar datos de usuario In Progress).
9.	Si NO es la primera autenticación del usuario en el sistema, el sistema deberá redirigir al usuario a la pantalla de gestión de facturación (ESD-38: Consultar facturaIn Progress).
10.	Si la autenticación falla, el sistema deberá mostrar un mensaje informativo acorde a la respuesta recibida desde B2C y permitir al usuario reintentar el acceso sin perder la navegación.
11.	Si ocurre un error técnico durante la comunicación con B2C (timeout, servicio no disponible, error de red), el sistema deberá mostrar un mensaje indicando que no fue posible completar el inicio de sesión y permitir reintentar.
12.	En las rutas protegidas (/app/*), el componente de ruta protegida deberá:
    -	Mientras inProgress === 'startup': mostrar un componente de fallback (evitar flash de contenido no autorizado).
    -	Si el usuario no está autenticado: disparar loginRedirect automáticamente.
    -	Si el usuario está autenticado: renderizar el contenido protegido.
13.	Todas las acciones de autenticación (exitosas, fallidas, errores técnicos) deberán registrar un evento de auditoría conforme a ESD-187: Registro de auditoriaDone.
14.	Los elementos visuales de la pantalla de pre-login y de los flujos de error deberán cumplir con el diseño definido para web y móvil según los ESSA - Oficina Virtual Diseño UX_UI Vigente
Vista previa.


## Datos de Entrada / Salida

Datos de Entrada:

| Campo                                           | Tipo               | Obligatorio | Origen                                              | Restricciones / Formato                                                                                                                                           |
|------------------------------------------------|--------------------|--------------|-----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| clientId                                       | String (UUID)      | Sí           | Configuración del sistema (variable de entorno)     | UUID v4. Identifica la aplicación ante B2C. NPE: 40181510-1867-491a-854d-c853626111b5                                                                           |
| authority                                      | String (URL)       | Sí           | Configuración del sistema                           | URL completa del tenant B2C + policy. Formato: https://{tenant}.b2clogin.com/{tenantDomain}/{policyName}                                                        |
| redirectUri                                    | String (URL)       | Sí           | Configuración del sistema                           | URL registrada como SPA en App Registration. Debe coincidir exactamente con la registrada en Azure.                                                              |
| channelName                                    | String             | Sí           | Sistema (inyección automática)                      | Valor fijo: web. Requerido por custom policy EPM.                                                                                                                 |
| code                                           | String             | Sí           | Servicio B2C (respuesta de /authorize)              | Authorization code generado por B2C tras autenticación exitosa del usuario.                                                                                      |
| code_verifier                                  | String             | Sí           | MSAL (generado automáticamente)                     | PKCE code verifier generado por MSAL para el flujo Authorization Code + PKCE.                                                                                    |
| Credenciales del usuario (email/contraseña)    | String             | Sí           | Usuario (ingresadas en la pantalla B2C)             | Gestionadas exclusivamente por B2C; la oficina virtual no las recibe ni las almacena.                                                                            |

Datos de Salida:

| Dato                                             | Tipo                                  | Descripción                                                                                                                                                                                                                     |
|--------------------------------------------------|---------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id_token                                         | JWT (String)                          | Token de identidad emitido por B2C. Contiene claims del usuario: oid, sub, email, name, given_name, family_name, phone_number, personInformation (JSON con datos de registro). Vigencia: 1 hora.                           |
| access_token                                     | JWT (String)                          | Token de acceso para consumo de APIs backend. Contiene los mismos claims base. Vigencia: 1 hora.                                                                                                                               |
| refresh_token                                    | String cifrado                        | Token de refresco emitido por B2C (cifrado, no decodificable como JWT). Vigencia: 14 días. Usado por MSAL para renovación silenciosa.                                                                                        |
| Claims del usuario extraídos del id_token        | Objeto JSON                           | oid / sub (ID único del usuario), email, name, given_name, family_name, phone_number, personInformation (JSON embebido con: correoElectronico, idTipoIdentificacion, numeroIdentificacion, nombres, apellidos, telefonoMovil, codigoPais, aceptaTerminosYCondiciones, aceptaPoliticaPrivacidad, fechaAceptacionTerminosDatos, estadoRegistro). |
| Sesión del usuario                               | Objeto en memoria / localStorage      | Sesión creada por MSAL con account keys, token keys e información de la cuenta activa.                                                                                                                                          |
| Evento de auditoría                              | Registro                              | Registro de auditoría generado según ESD-187: Registro de auditoriaDone con los datos de la acción de autenticación.                                                                                                          |
| Mensaje de error (autenticación fallida)         | String                                | Mensaje informativo acorde a la respuesta recibida desde B2C.                                                                                                                                                                   |
| Mensaje de error (error técnico)                 | String                                | Mensaje indicando que no fue posible completar el inicio de sesión por problemas de comunicación.                                                                                                                               |

## Criterios de Aceptación No Funcionales
CANF-01	Rendimiento	El proceso de redirección a B2C y retorno post-autenticación no deberá agregar más de 2 segundos de latencia adicional sobre el tiempo de respuesta de B2C.

CANF-02	Rendimiento	La renovación silenciosa de tokens (acquireTokenSilent) deberá ejecutarse de forma transparente sin bloquear la interacción del usuario.

CANF-03	Seguridad	La autenticación debe utilizar el flujo Authorization Code con PKCE. No se deberá incluir client_secret en el código frontend bajo ninguna circunstancia.

CANF-04	Seguridad	Los tokens no deberán ser leídos directamente desde localStorage. Todo acceso a tokens debe realizarse a través de msalInstance.acquireTokenSilent().

CANF-05	Seguridad	El refresh_token está cifrado por B2C y no debe intentar decodificarse como JWT en el frontend.

CANF-06	Seguridad	La configuración storeAuthStateInCookie: true debe mantenerse activa para mitigar el bloqueo de third-party storage en Safari/iOS.

CANF-07	Compatibilidad	La funcionalidad debe ser compatible con los navegadores: Chrome (última versión), Firefox (última versión), Safari (última versión, iOS incluido), Edge (última versión).

CANF-08	Compatibilidad	El diseño debe ser responsive y funcionar correctamente en web y móvil conforme a los diseños en Figma.

CANF-09	Accesibilidad	La pantalla de pre-login y los mensajes de error deben cumplir con estándares WCAG 2.1 nivel AA como mínimo.

CANF-10	Disponibilidad	El sistema deberá manejar graciosamente la indisponibilidad del servicio B2C, mostrando mensajes claros al usuario y sin generar errores no controlados en consola.


## Dependencias
DESD-44: Reportar un daño a una cuentaIn Progress
Reportar daños	Acceso directo — flujo de reporte de daño sin cuenta.
ESD-74: Mostrar oficinas cercanas a mi ubicaciónIn Progress
Oficinas (parte 1)	Acceso directo — visualización de oficinas sin opción de turno.
ESD-75: Ver todas las oficinasIn Progress
Oficinas (parte 2)	Acceso directo — visualización de oficinas sin opción de turno.
ESD-49: Identificar funcionario In Progress
Identifícanos	Acceso directo — funcionalidad de identificación.
ESD-51: Mostrar información relacionada con la app Done
Información	Acceso directo — funcionalidad de información.
ESD-14: Actualizar datos de usuario In Progress
Actualización de datos	Redirección post-autenticación en primer ingreso.
ESD-38: Consultar facturaIn Progress
Gestión de facturación	Redirección post-autenticación en ingresos recurrentes.
ESD-187: Registro de auditoriaDone
Auditoría	Registro de eventos de autenticación.

