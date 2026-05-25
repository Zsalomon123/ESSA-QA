# Historia de Usuario 46 Buscar un reporte de un daño

## YO Como
cliente de ESSA (usuario de la Oficina Virtual)

## Sprint
16

## Menú de navegación
Danos y PQR/ Danos y emergencias/ Buscar un reporte

## Objetivo
Permitir al cliente consultar, de forma rápida y confiable, la información de un reporte de daño/emergencia previamente radicado, utilizando como llave de búsqueda el número de reporte (radicado).

Con esta funcionalidad se espera:
- Reducir la incertidumbre del cliente sobre el avance/resultado del reporte.
- Exponer en la Oficina Virtual un resumen en solo lectura del reporte consultado (estado y datos relevantes), consumiendo el servicio Trouble Ticket de SP7.
- Asegurar manejo de errores y ausencia de resultados con mensajes claros al usuario.
- Garantizar trazabilidad registrando auditoría de acciones en pantalla (referencia ESD-187).

## Precondiciones

1. El usuario se encuentra en la Oficina Virtual y tiene acceso al módulo de daños y PQR.
2. La opción de navegación Daños o emergencias > Buscar reporte está disponible para el usuario.
3. El servicio Trouble Ticket de SP7 se encuentra disponible y accesible desde la aplicación (conectividad, enrutamiento, credenciales/tokens si aplica).
4. Existe un número de radicado (reporte) previamente generado por el cliente (dato entregado al crear el reporte).
5. Se cuenta con los lineamientos de diseño aprobados (web/móvil) para esta pantalla.
    - Fuente de diseño: Figma: https://www.figma.com/design/pZHVFxet0huCXwnhxFynmj/ESSA---Oficina-Virtual--Dise%C3%B1o-UX_UI-Vigente?node-id=3309-89339&t=v77tIMVCJdoK1QSQ-1

## Postcondiciones

Tras una ejecución correcta de la búsqueda:
1. El sistema muestra al usuario una card con el detalle del reporte consultado (si existe información asociada al radicado).
2. La información presentada queda en modo solo lectura (sin edición).
3. Se registra un evento de auditoría por las acciones realizadas en la pantalla, según la historia/lineamiento ESD-187.
4. En caso de no existir información asociada o ante error de servicio, el sistema deja visible el mensaje correspondiente al escenario ocurrido.

## Reglas de negocio

RN01. Acceso a la funcionalidad
La opción “Buscar reporte” debe estar disponible desde el módulo daños y pqr > Daños o emergencias.

RN02. Campo obligatorio para búsqueda
El campo Número de radicado es obligatorio para ejecutar la consulta. No se permite enviar la búsqueda si el campo está vacío.

RN03. Habilitación de acción “Buscar”
Si el campo de búsqueda está vacío, el sistema no habilita la acción “Buscar”.

RN04. Consumo de servicio por radicado
Al ejecutar la búsqueda, el sistema debe consumir el servicio Trouble Ticket de SP7 enviando el número de radicado ingresado como dato de consulta.

RN05. Presentación de resultados (existencia de datos)
Si el servicio retorna información del reporte, el sistema debe mostrar una card con los datos mínimos definidos en los criterios.

RN06. Presentación de resultados (sin datos)
Si el número de radicado ingresado no tiene información asociada, el sistema debe mostrar el mensaje:
“No encontramos resultados. Revisa que el número de reporte sea correcto o intenta con otro.”

RN07. Manejo de error de servicio
Si ocurre un error al consumir el servicio Trouble Ticket de SP7, el sistema debe mostrar el mensaje:
“No fue posible consultar el reporte en este momento. Por favor, intenta nuevamente.”

RN08. Datos en solo lectura
Toda la información del reporte mostrada en la card debe ser solo lectura.

RN09. Auditoría obligatoria
Todas las acciones realizadas por el usuario en esta pantalla deberán registrar un evento de auditoría según ESD-187.

RN10. Cumplimiento de UI/UX aprobado (web y móvil)
Los elementos deben mostrarse correctamente alineados, con íconos visibles y textos legibles, conforme al diseño aprobado para versión web y móvil.

Datos de Entrada/Salida

Entrada
•	Número de radicado (número de reporte)
o	Tipo: texto (string)
o	Obligatorio: sí
o	Restricciones/validaciones: no vacío (si está vacío no habilitar “Buscar”)
o	Origen: usuario
Nota: no se especifican longitudes ni patrón (numérico/alfanumérico). Se infiere únicamente la validación básica de “no vacío” porque está explícitamente requerida.
Salida (UI / mensajes / datos mostrados)
1. En caso exitoso con datos: card con información mínima (solo lectura):
    1.	Número de reporte: corresponde al valor ingresado en el buscador.
    2.	Estado del reporte: desde respuesta del servicio, campo TT.Estado.
    3.	Fecha de solicitud: desde respuesta del servicio, campo TT.FechaCreacion.
    4.	Fecha de cierre: desde respuesta del servicio, campo TT.FechaCierre.
    5.	Observaciones del daño: desde respuesta del servicio, campo TT.Observacion.
    6.	Causas del daño: desde respuesta del servicio, campo TT.Causa.
2. En caso sin resultados: mensaje informativo:
    - “No encontramos resultados. Revisa que el número de reporte sea correcto o intenta con otro.”
3. En caso error de servicio: mensaje de error:
    - “No fue posible consultar el reporte en este momento. Por favor, intenta nuevamente.”
4. Auditoría:
    - Registro de eventos de auditoría por acciones del usuario en la pantalla (referencia ESD-187).

## Criterios de Aceptación

1.	Al ingresar al módulo Daños y PQR > Daños o emergencias, el sistema deberá permitir acceder a la opción “Buscar reporte”.
2.	Al ingresar a la funcionalidad “Buscar reporte”, el sistema deberá mostrar un campo de búsqueda para que el usuario ingrese el número de radicado del reporte.
3.	En la pantalla, el sistema deberá mostrar un elemento de acción (botón u opción) para ejecutar “Buscar”.
4.	Antes de realizar la consulta, el sistema deberá validar que el campo número de radicado no se encuentre vacío.
5.	Si el usuario no ha ingresado un número de radicado, el sistema deberá no habilitar la opción “Buscar”, impidiendo la ejecución de la consulta.
6.	Cuando el usuario ingrese el número de radicado y seleccione la opción “Buscar”, el sistema deberá consultar el servicio Trouble Ticket de SP7, enviando el número de radicado ingresado.
7.	Si el servicio retorna información del reporte, el sistema deberá mostrar una card con el detalle del reporte.
8.	La card deberá mostrar como mínimo, y de acuerdo con la respuesta del servicio:
    a.	Número de reporte: el mismo dato ingresado por el usuario en el buscador.
    b.	Estado del reporte: TT.Estado.
    c.	Fecha de solicitud: TT.FechaCreacion.
    d.	Fecha de cierre: TT.FechaCierre.
    e.	Observaciones del daño: TT.Observacion.
    f.	Causas del daño: TT.Causa.
9.	La información mostrada en la card deberá ser de solo lectura (sin permitir modificaciones por parte del usuario).
10.	Si el número de radicado ingresado no tiene información asociada, el sistema deberá mostrar el mensaje:
“No encontramos resultados. Revisa que el número de reporte sea correcto o intenta con otro.”
11.	Si ocurre un error al consumir el servicio Trouble Ticket de SP7, el sistema deberá mostrar el mensaje:
“No fue posible consultar el reporte en este momento. Por favor, intenta nuevamente.”
12.	Todas las acciones realizadas por el usuario en esta pantalla deberán registrar un evento de auditoría conforme a lo definido en ESD-187.
13.	Los elementos de la pantalla deberán mostrarse correctamente alineados, con íconos visibles y textos legibles, de acuerdo con el diseño entregado y aprobado para la Oficina Virtual en versión web y móvil.

Datos de Entrada/Salida
Entrada
•	Número de radicado (número de reporte)
o	Tipo: texto (string)
o	Obligatorio: sí
o	Restricciones/validaciones: no vacío (si está vacío no habilitar “Buscar”)
o	Origen: usuario
Nota: no se especifican longitudes ni patrón (numérico/alfanumérico). Se infiere únicamente la validación básica de “no vacío” porque está explícitamente requerida.
Salida (UI / mensajes / datos mostrados)
1. En caso exitoso con datos: card con información mínima (solo lectura):
    1.	Número de reporte: corresponde al valor ingresado en el buscador.
    2.	Estado del reporte: desde respuesta del servicio, campo TT.Estado.
    3.	Fecha de solicitud: desde respuesta del servicio, campo TT.FechaCreacion.
    4.	Fecha de cierre: desde respuesta del servicio, campo TT.FechaCierre.
    5.	Observaciones del daño: desde respuesta del servicio, campo TT.Observacion.
    6.	Causas del daño: desde respuesta del servicio, campo TT.Causa.
2. En caso sin resultados: mensaje informativo:
    - “No encontramos resultados. Revisa que el número de reporte sea correcto o intenta con otro.”
3. En caso error de servicio: mensaje de error:
    - “No fue posible consultar el reporte en este momento. Por favor, intenta nuevamente.”
4. Auditoría:
    - Registro de eventos de auditoría por acciones del usuario en la pantalla (referencia ESD-187).

## Criterios de Aceptación No Funcionales
1.	La pantalla debe cumplir el diseño aprobado (alineación, legibilidad, iconografía) en web y móvil, según Figma:
https://www.figma.com/design/pZHVFxet0huCXwnhxFynmj/ESSA---Oficina-Virtual--Dise%C3%B1o-UX_UI-Vigente?node-id=3309-89339&t=v77tIMVCJdoK1QSQ-1
2.	Se debe garantizar el registro de auditoría de las acciones del usuario en esta pantalla conforme a ESD-187 (trazabilidad de acciones).
3.	Ante error de integración con el servicio Trouble Ticket de SP7, el sistema debe responder con el mensaje definido, sin exponer detalles técnicos al usuario final.
Servicios
1.	Servicio: Trouble Ticket (SP7)
    •	Tipo: API / servicio externo (integración con SP7)
    •	Método: no especificado en la HU (pendiente de definición técnica)
    •	Endpoint: no especificado en la HU (pendiente de definición técnica)
    •	Propósito: consultar la información del reporte a partir del número de radicado enviado por el usuario.
    •	Campos relevantes de respuesta (mapeo UI):
        o	TT.Estado → Estado del reporte
        o	TT.FechaCreacion → Fecha de solicitud
        o	TT.FechaCierre → Fecha de cierre
        o	TT.Observacion → Observaciones del daño
        o	TT.Causa → Causas del daño
2.	Servicio/Componente: Auditoría (ESD-187)
    •	Tipo: interno (mecanismo/plataforma de auditoría)
    •	Método/Endpoint: no especificado en la HU (referenciado por historia ESD-187)
    •	Propósito: registrar eventos de auditoría por acciones del usuario en la pantalla “Buscar reporte”.
## Dependencias
Técnicas (servicios/APIs)
    •	Disponibilidad y correcta integración con el servicio Trouble Ticket de SP7.
    •	Disponibilidad del mecanismo de auditoría definido en ESD-187.
    Datos (fuentes de información)
    •	Existencia de reportes en SP7 asociados a un número de radicado consultable.
    •	Correcto mapeo de campos TT.* para pintar la información en la card.
    Secuencia (otras HU necesarias)
    •	ESD-187: requerida para asegurar el registro de auditoría “todas las acciones realizadas por el usuario en esta pantalla”.
    •	Relación épica indicada en Jira: ESD-190 (la HU pertenece a esta épica).

