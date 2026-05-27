# Historia de Usuario 260 Visualizar reportes que afecten mis cuentas

## YO Como
usuario autenticado de la Oficina Virtual de ESSA

## Sprint
16

## Menú de navegación
Danos y PQR/ Danos y emergencias/ Mis reportes

## Objetivo
Esta historia de usuario tiene como propósito proporcionar al usuario autenticado de la Oficina Virtual ESSA una funcionalidad que le permita consultar, de forma centralizada y en tiempo real, los reportes de daños, emergencias y suspensiones programadas que afecten las cuentas registradas en su perfil.
Problema que resuelve: Actualmente el usuario no cuenta con un mecanismo integrado dentro de la Oficina Virtual para conocer si sus cuentas están siendo afectadas por eventos de daño, emergencia o suspensión programada del servicio.
Valor que aporta:
-	Visibilidad inmediata sobre afectaciones activas en las cuentas del usuario.
-	Reducción de llamadas al centro de atención al cliente.
-	Mejora en la experiencia del usuario al centralizar la información de novedades.
-	Posibilidad de redirigir al usuario a la funcionalidad de reporte de daños cuando no existan afectaciones vigentes.
Resultado esperado en el sistema: El sistema consulta el servicio SP7 (Consulta por CUD) para cada cuenta del usuario, filtra los resultados según reglas de negocio definidas (estado, tipo y rango de fechas) y presenta la información agrupada por cuenta en formato de acordeón con los campos de detalle correspondientes.



## Precondiciones

PRE-01	El usuario debe estar autenticado en la Oficina Virtual ESSA con sesión activa y válida.
PRE-02	El usuario debe tener al menos una cuenta inscrita y asociada a su perfil en la Oficina Virtual.
PRE-03	El servicio SP7 – Consulta por CUD debe estar disponible y operativo.
PRE-04	El usuario debe haber accedido previamente a la funcionalidad de "Buscar un reporte" desde donde se accede a "Visualizar reportes que afecten mis cuentas".
PRE-05	Los números de cuenta asociados al usuario deben estar correctamente registrados en el sistema.

## Postcondiciones

POST-01	    El sistema presenta en pantalla los reportes de daños, emergencias y suspensiones programadas vigentes, agrupados por número de cuenta en formato de acordeón.

POST-02	    Si no existen afectaciones vigentes para ninguna cuenta, el sistema muestra un mensaje informativo y un botón "Realizar nuevo reporte".

POST-03	    Se registra un evento de auditoría conforme a lo definido en ESD-187 por cada acción realizada por el usuario en la pantalla.

POST-04	    En caso de error técnico del servicio SP7, el sistema muestra un mensaje de error controlado y permite reintentar la consulta sin recargar la página.



## Reglas de negocio

RN01	El sistema deberá ejecutar la consulta al servicio SP7 – Consulta por CUD de forma individual por cada número de cuenta asociado al perfil del usuario autenticado.

RN02	Se consideran daños aquellos registros cuyo campo Estado sea igual a "Activo" y cuyo campo Tipo sea igual a "No programado".

RN03	Los daños deberán mostrarse únicamente cuando su campo FechaFinProbable se encuentre dentro del rango comprendido entre la fecha y hora actual de consulta y las 23:59:59 del mismo día de la consulta.

RN04	Se consideran suspensiones programadas aquellos registros cuyo campo Estado sea igual a "Activo" o "Aprobado" y cuyo campo Tipo sea igual a "Programado".

RN05	Las suspensiones programadas deberán mostrarse únicamente cuando su campo FechaFinProbable se encuentre dentro del rango comprendido entre la fecha y hora actual de consulta y las 23:59:59 del mismo día de la consulta.

RN06	Los resultados deberán presentarse agrupados por número de cuenta, utilizando componentes de tipo acordeón desplegable. Cada acordeón llevará como título el nombre y alias de la cuenta.

RN07	Todos los campos de detalle de los reportes se presentarán en modo solo lectura.

RN08	Si ninguna de las cuentas del usuario presenta afectaciones vigentes según las reglas RN02 a RN05, el sistema deberá mostrar un mensaje informativo indicando la ausencia de reportes y un botón "Realizar nuevo reporte".

RN09	Al seleccionar el botón "Realizar nuevo reporte", el sistema redirigirá al usuario a la funcionalidad de registrar un nuevo reporte de daño o emergencia (referencia: SD-44).

RN10	Toda acción realizada por el usuario en esta pantalla deberá generar un registro de evento de auditoría conforme a lo especificado en ESD-187.

RN11	En caso de error, timeout o indisponibilidad del servicio SP7, el sistema deberá mostrar un mensaje de error controlado y permitir reintentar la consulta sin recargar la página.

## Criterios de Aceptación

1.	El sistema deberá habilitar el acceso a la funcionalidad "Visualizar reportes que afecten mis cuentas" desde la funcionalidad de buscar un reporte. El punto de acceso deberá ser visible y accesible para el usuario autenticado.
2.	Al ingresar a la funcionalidad, el sistema deberá identificar automáticamente todos los números de cuenta asociados al perfil del usuario autenticado, sin requerir intervención manual del usuario.
3.	El sistema deberá consumir el método "Consulta por CUD" del servicio SP7, ejecutando una consulta individual por cada número de cuenta asociado al usuario.
4.	Una vez obtenida la respuesta del servicio SP7, el sistema deberá aplicar los siguientes filtros sobre los resultados:
    -	Daños: Incluir únicamente registros con Estado = "Activo" y Tipo = "No programado", cuya FechaFinProbable se encuentre entre la fecha/hora actual y las 23:59:59 del día de la consulta.
    -	Suspensiones programadas: Incluir únicamente registros con Estado = "Activo" o "Aprobado" y Tipo = "Programado", cuya FechaFinProbable se encuentre entre la fecha/hora actual y las 23:59:59 del día de la consulta.
5.	Si existen reportes que cumplan las condiciones de filtrado, el sistema deberá presentar los resultados en una tabla de resultados agrupada por número de cuenta, utilizando componentes de tipo acordeón desplegable. Cada acordeón deberá llevar como título el nombre y alias de la cuenta.
6.	Por cada registro encontrado, el sistema deberá mostrar los siguientes campos en modo solo lectura:
    -	Número de reporte (TT.CodigoTT) — Tipo: numérico, longitud máxima: 20 caracteres.
    -	Fecha y hora inicio (Evento.FechaInicioProbable) — Tipo: fecha (YYYY-MM-DD).
    -	Fecha y hora fin (Evento.FechaFinProbable) — Tipo: fecha (YYYY-MM-DD).
    -	Número de evento (TT.NumeroEvento) — Tipo: alfanumérico, longitud máxima: 50 caracteres.
7.	Si ninguna de las cuentas asociadas al usuario presenta afectaciones por daños, emergencias o suspensiones programadas dentro del rango consultado, el sistema deberá mostrar un mensaje informativo indicando que no existen reportes de afectación para las cuentas del usuario.
8.	En el escenario de ausencia de afectaciones (criterio 7), el sistema deberá mostrar adicionalmente un botón denominado "Realizar nuevo reporte".
9.	Al seleccionar el botón "Realizar nuevo reporte", el sistema deberá redirigir al usuario a la funcionalidad correspondiente para registrar un nuevo reporte de daño o emergencia, conforme a lo definido en SD-44 (Reportar un daño a una cuenta).
10.	Si el servicio SP7 – Consulta por CUD presenta error, timeout o indisponibilidad, el sistema deberá mostrar el siguiente mensaje de error controlado: "No fue posible consultar los reportes de daños en este momento. Por favor, intenta nuevamente."
11.	En el escenario de error técnico (criterio 10), el sistema deberá permitir que el usuario ejecute nuevamente la consulta sin necesidad de recargar la página (botón de reintento o mecanismo equivalente).
12.	Todas las acciones realizadas por el usuario en la pantalla deberán generar un registro de evento de auditoría conforme a lo especificado en ESD-187.
13.	Los elementos de la pantalla deberán mostrarse correctamente alineados, con íconos visibles y textos legibles, de acuerdo con el diseño entregado para la versión web y móvil.

## Datos de Entrada / Salida
- Datos de entrada:
| Campo                               | Tipo de Dato | Longitud | Obligatorio | Origen                                           | Descripción                                                                 |
|-------------------------------------|---------------|-----------|--------------|--------------------------------------------------|-----------------------------------------------------------------------------|
| Números de cuenta del usuario       | Numérico      | Variable  | Sí           | Sistema (perfil del usuario autenticado)         | Listado de cuentas inscritas en el perfil del usuario. Se obtienen automáticamente al ingresar a la funcionalidad. |
| Token de sesión / autenticación     | Alfanumérico  | Variable  | Sí           | Sistema (sesión activa)                          | Credencial de autenticación del usuario para validar la sesión y autorizar la consulta. |

- Datos de salida:

| Campo                                     | Tipo de Dato | Longitud Máxima | Obligatorio | Origen (Servicio SP7)              | Descripción                                                                                                                             |
|------------------------------------------|---------------|------------------|--------------|------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| Nombre y alias de la cuenta              | Alfanumérico  | Variable         | Sí           | Sistema (perfil del usuario)       | Título del acordeón que agrupa los reportes por cuenta.                                                                                |
| Número de reporte                        | Numérico      | 20 caracteres    | Sí           | TT.CodigoTT                        | Identificador único del reporte de daño, emergencia o suspensión programada.                                                           |
| Fecha y hora inicio                      | Fecha (YYYY-MM-DD) | 10 caracteres | Sí           | Evento.FechaInicioProbable         | Fecha y hora estimada de inicio del evento.                                                                                             |
| Fecha y hora fin                         | Fecha (YYYY-MM-DD) | 10 caracteres | Sí           | Evento.FechaFinProbable            | Fecha y hora estimada de finalización del evento.                                                                                       |
| Número de evento                         | Alfanumérico  | 50 caracteres    | Sí           | TT.NumeroEvento                    | Identificador del evento asociado al reporte.                                                                                           |
| Mensaje informativo (sin afectaciones)   | Texto         | Variable         | Condicional  | Sistema                            | Mensaje que indica que no existen reportes de afectación para las cuentas del usuario. Se muestra cuando no hay resultados vigentes. |
| Mensaje de error técnico                 | Texto         | Variable         | Condicional  | Sistema                            | Mensaje: "No fue posible consultar los reportes de daños en este momento. Por favor, intenta nuevamente." Se muestra ante fallo del servicio SP7. |


## Criterios de Aceptación No Funcionales
CNF-01	Rendimiento	La consulta al servicio SP7 y la presentación de resultados en pantalla deberá completarse en un tiempo máximo razonable (se recomienda ≤ 5 segundos por cuenta consultada). En caso de superar el tiempo de espera, se deberá aplicar el manejo de timeout definido en RN11.

CNF-02	Rendimiento	El sistema deberá mostrar un indicador de carga (spinner o barra de progreso) mientras se ejecutan las consultas al servicio SP7 para cada cuenta.

CNF-03	Seguridad	Solo los usuarios autenticados con sesión activa y válida podrán acceder a la funcionalidad. El sistema deberá validar el token de sesión antes de ejecutar cualquier consulta.

CNF-04	Seguridad	El sistema deberá garantizar que un usuario solo pueda consultar reportes asociados a las cuentas registradas en su propio perfil, impidiendo el acceso a información de cuentas de otros usuarios.

CNF-05	Accesibilidad	Los componentes de acordeón, tablas y botones deberán ser navegables mediante teclado y compatibles con lectores de pantalla.

CNF-06	Compatibilidad	La funcionalidad deberá ser responsive y visualizarse correctamente en versión web (navegadores Chrome, Firefox, Edge, Safari en sus últimas dos versiones) y en versión móvil, conforme al diseño entregado.

CNF-07	Auditoría	Los eventos de auditoría deberán registrarse de forma asíncrona para no afectar el rendimiento de la funcionalidad principal.

## Dependencias
ESD-187	—	Definición del registro de eventos de auditoría. Esta HU debe estar implementada para que las acciones del usuario se registren correctamente.

ESD-44 – Reportar un daño a una cuenta Funcionalidad destino al seleccionar el botón "Realizar nuevo reporte". Debe estar implementada para que la redirección funcione correctamente.

Funcionalidad "Buscar un reporte"	—	Punto de acceso desde el cual el usuario ingresa a "Visualizar reportes que afecten mis cuentas". Debe estar implementada previamente.

