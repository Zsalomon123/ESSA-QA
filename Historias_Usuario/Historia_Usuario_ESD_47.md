# Historia de Usuario 47 Visualizar los reportes de daños realizados

## YO Como
usuario autenticado de la Oficina Virtual de ESSA

## Sprint
16

## Menú de navegación
Danos y PQR/ Danos y emergencias/ Mis reportes

## Objetivo
El objetivo de esta historia de usuario es habilitar, en la Oficina Virtual de ESSA (canales web y app móvil), una funcionalidad que permita al usuario:
- Seleccionar una de sus cuentas vinculadas.
- Consultar, para dicha cuenta, hasta los cinco últimos reportes de daños o emergencias existentes, en orden cronológico descendente (del más reciente al más antiguo).
- Visualizar en solo lectura el detalle de cada reporte, incluyendo información clave de estado, fechas y observaciones técnicas.


## Precondiciones

1.	El usuario debe estar autenticado correctamente en la Oficina Virtual de ESSA.
2.	El usuario debe tener al menos una cuenta vinculada a su perfil en la Oficina Virtual.
3.	Debe existir acceso y disponibilidad operacional del servicio SP7 “Consulta por Trouble Ticket”, según la especificación de servicios:
    - URL de referencia:
    https://grupovass.sharepoint.com/:x:/r/teams/OperacionColombia/_layouts/15/Doc.aspx?sourcedoc=%7B62E074C8-E76B-4E31-A9ED-AB8F3897AA3E%7D&file=Metodos%20API%20SP7.xlsx&action=default&mobileredirect=trueConecta tu cuenta de OneDrive
4.	La funcionalidad Daños y PQRS > Daños y emergencias > Mis reportes debe estar habilitada y visible para el perfil de usuario correspondiente.
5.	Para consulta efectiva de reportes:
    - Deben existir, en el sistema de backend, reportes de daños/emergencias asociados a las cuentas cuando se espere su visualización (aunque la HU contempla también el caso en que no existan reportes).
6.	Deben estar disponibles los diseños UX/UI vigentes para web y móvil, que servirán de referencia para layout, alineaciones, íconos y textos:
    - URL de diseños:
    ESSA - Oficina Virtual Diseño UX_UI Vigente
7.	El módulo de auditoría definido en la HU ESD-187 debe estar implementado y disponible para registrar los eventos de usuario:
    - JIRA de referencia: ESD-187: Registro de auditoriaDone

## Postcondiciones

1.	Tras la selección de una cuenta y la ejecución correcta de la consulta:
    -	El sistema habrá recuperado desde el servicio SP7 “Consulta por Trouble Ticket” hasta los últimos cinco reportes de daños/emergencias asociados a la cuenta seleccionada.
    -	Se habrán construido y mostrado en pantalla (web y móvil) las cards correspondientes a cada reporte disponible (hasta un máximo de cinco).
2.	Si para algún reporte el servicio no retorna información suficiente:
    -	El reporte correspondiente no será mostrado (se omite la construcción de la card), y se mostrarán únicamente las cards de los reportes con información válida.
3.	Si la cuenta no tiene reportes generados:
    -	No se mostrará listado de reportes.
    -	Se mostrará un texto informativo indicando que la cuenta no tiene reportes disponibles.
    -	Los botones para crear un nuevo reporte se mostrarán centrados en la pantalla.
4.	En la app móvil:
    -	Se presentará inicialmente un listado de cards con resumen de cada reporte.
    -	Al seleccionar “Ver detalles” en una card, se abrirá una nueva pantalla con la información detallada del reporte en solo lectura.
5.	Todas las acciones relevantes realizadas por el usuario en esta pantalla (por ejemplo: acceso a “Mis reportes”, selección de cuenta, visualización detalle en móvil) quedarán registradas como eventos de auditoría de acuerdo con lo especificado en ESD-187.
6.	La interfaz (web y móvil) se mostrará con los elementos correctamente alineados, con íconos visibles y textos legibles, en concordancia con los diseños entregados.


## Reglas de negocio

RN01 Selección de cuenta obligatoria
La selección de una cuenta vinculada es obligatoria para realizar la consulta de reportes. El desplegable de cuentas debe ser de única selección y debe permitir búsqueda.

RN02 Contenido del desplegable de cuentas
El desplegable de cuentas debe listar únicamente las cuentas que el usuario tiene vinculadas en su Oficina Virtual, y cada opción debe mostrar:
    -	Número de la cuenta.
    -	Alias de la cuenta (si aplica, según datos maestros).

RN03 – Límite de reportes a mostrar
Para cada cuenta seleccionada, el sistema solo debe mostrar hasta los cinco últimos reportes de daños/emergencias, ordenados del más reciente al más antiguo. No se debe mostrar más de cinco reportes simultáneamente.

RN04 – Ordenamiento de reportes
Los reportes mostrados deben estar organizados en orden cronológico descendente (del más reciente al más antiguo). La referencia de “reciente/antiguo” deberá estar alineada con los campos de fecha de creación (TT.FechaCreacion) retornados por el servicio SP7.

RN05 – Dependencia de datos del servicio SP7
La información de cada reporte mostrado debe provenir del servicio SP7 “Consulta por Trouble Ticket”.
Si para un determinado reporte el servicio no retorna información suficiente o válida, no se construirá la card para dicho reporte, sin interrumpir el procesamiento del resto. Se mostrarán solamente los reportes para los que sí existe información válida hasta un máximo de cinco.

RN06 – Información mostrada por reporte (web)
Para cada reporte mostrado en web, en solo lectura, se deberán visualizar los siguientes campos:
1.	Número de cuenta:
    -	Valor: cuenta seleccionada en el desplegable.
2.	Número de reporte:
    -	Valor: número de trouble ticket consultado.
3.	Estado del reporte:
    -	Origen: campo “TT.Estado” del servicio de Consulta por Trouble Ticket.
4.	Fecha de solicitud:
    -	Origen: campo “TT.FechaCreacion”.
5.	Fecha de atención:
    -	Origen: campo “TT.FechaCierre”.
6.	Descripción del daño:
    -	Origen: campo “TT.Observacion”.
7.	Causa:
-	Origen: campo “TT.Causa”.
Todos estos datos serán solo lectura para el usuario.

RN07 – Información mostrada en app móvil (resumen en card)
En la pantalla inicial de la app móvil (vista de listado), cada card de reporte deberá mostrar en solo lectura:
1.	Número de cuenta.
2.	Número del reporte.
3.	Estado del reporte.
4.	Fecha de solicitud.
5.	Fecha de atención.

RN08 – Información mostrada en app móvil (pantalla de detalle)
Al seleccionar “Ver detalles” en una card, el sistema abrirá una nueva pantalla en solo lectura en la que se mostrará la siguiente información del reporte seleccionado:
1.	Número de cuenta.
2.	Número del reporte.
3.	Estado del reporte.
4.	Fecha de solicitud.
5.	Fecha de atención.
6.	Descripción del daño.
7.	Observación técnica (correspondiente a la descripción/observación técnica retornada por el servicio).

RN09 – Comportamiento cuando no existan reportes
Si la cuenta seleccionada no tiene reportes generados:
- No se debe mostrar ninguna card/listado de reportes.
- Se debe mostrar un texto informativo indicando que la cuenta no tiene reportes disponibles.
- Los botones para crear un nuevo reporte deben mostrarse centrados en la pantalla.

RN10 – Solo lectura de la información de los reportes
Toda la información de los reportes de daños/emergencias mostrada tanto en web como en móvil será de solo lectura. Desde esta funcionalidad no se permite modificar ni eliminar datos de los reportes.

RN11 – Registro de auditoría
Toda acción relevante del usuario en esta funcionalidad (por ejemplo: acceso a “Mis reportes”, selección de cuenta, consulta de reportes, navegación a detalle en móvil) debe generar un evento de auditoría, cumpliendo con las especificaciones funcionales y técnicas definidas en la HU ESD-187.

RN12 – Cumplimiento de diseño
La disposición, alineación de elementos, visibilidad de íconos y legibilidad de textos debe seguir estrictamente los diseños entregados para versión web y móvil, disponibles en Figma.


## Criterios de Aceptación

1.	Acceso a la funcionalidad “Mis reportes” (web)

1.1. Al ingresar al módulo Daños y PQRS > Daños y emergencias y seleccionar la opción Mis reportes, el sistema deberá mostrar una pantalla con:
-	Un desplegable de cuentas vinculadas al usuario, con función de búsqueda y de única selección.
-	El desplegable deberá listar, para cada cuenta, el Número de la cuenta y el alias correspondiente.

1.2. El desplegable de cuentas será de selección obligatoria para continuar con la visualización de reportes.

2.	Consulta y visualización de reportes por cuenta (web)

2.1. Cuando el usuario seleccione una cuenta en el desplegable, el sistema deberá invocar el servicio SP7 “Consulta por Trouble Ticket” utilizando como parámetro la cuenta seleccionada (y demás filtros que apliquen según especificación técnica).

2.2. Si existen reportes asociados a la cuenta seleccionada, el sistema deberá mostrarlos en pantalla en forma de cards o filas (según diseño) en orden del más reciente al más antiguo.

2.3. El sistema deberá limitar la visualización a máximo cinco (5) registros, mostrando únicamente los cinco reportes más recientes para la cuenta seleccionada.
2.4. Si el servicio devuelve más de cinco reportes, el sistema no mostrará los adicionales en esta vista.

3.	Comportamiento cuando no existan reportes para la cuenta (web)

3.1. Si la cuenta seleccionada no tiene reportes generados (es decir, el servicio no retorna ningún reporte asociado), el sistema deberá:
-	No mostrar ningún listado ni card de reportes.
-	Mostrar un texto en pantalla indicando claramente que la cuenta no tiene reportes disponibles.
-	Centrar en la pantalla los botones para crear un nuevo reporte, de acuerdo con los diseños.

4.	Uso del servicio SP7 “Consulta por Trouble Ticket” y omisión de reportes sin información

4.1. El sistema deberá consultar los últimos cinco reportes de daños asociados a la cuenta seleccionada a través del servicio SP7 “Consulta por Trouble Ticket”, conforme a la especificación de servicios disponible en:
https://grupovass.sharepoint.com/:x:/r/teams/OperacionColombia/_layouts/15/Doc.aspx?sourcedoc=%7B62E074C8-E76B-4E31-A9ED-AB8F3897AA3E%7D&file=Metodos%20API%20SP7.xlsx&action=default&mobileredirect=trueConecta tu cuenta de OneDrive

4.2. Si el servicio no retorna información para alguno de los reportes que deberían conformar el conjunto de los últimos cinco, el sistema deberá omitir la construcción de la card correspondiente a ese reporte.

4.3. El sistema deberá continuar procesando los siguientes reportes devueltos por el servicio hasta completar la visualización de hasta cinco daños (en caso de que existan suficientes reportes con información válida).

4.4. Si, tras aplicar esta lógica, no existieran más reportes con información válida, el sistema mostrará únicamente los reportes que sí estén disponibles para la cuenta, pudiendo ser menos de cinco.

5.	Detalle de información por reporte (web)

5.1. Para cada reporte mostrado, el sistema deberá presentar, en solo lectura, los siguientes datos, alineados con el diseño UX/UI:
-	Número de cuenta: la cuenta seleccionada en el desplegable.
-	Número de reporte: número de trouble ticket consultado.
-	Estado del reporte: valor del campo “TT.Estado” retornado por el servicio.
-	Fecha de solicitud: valor del campo “TT.FechaCreacion”.
-	Fecha de atención: valor del campo “TT.FechaCierre”.
-	Descripción del daño: valor del campo “TT.Observacion”.
-	Causa: valor del campo “TT.Causa”.

5.2. El usuario no podrá modificar ninguno de estos datos desde la pantalla de “Mis reportes”.

6.	Visualización de reportes en app móvil – vista inicial (cards)

6.1. En la app móvil, al acceder a la funcionalidad equivalente a Daños y PQRS > Daños y emergencias > Mis reportes y seleccionar una cuenta, el sistema deberá mostrar inicialmente una lista de cards, una por cada reporte disponible (hasta un máximo de cinco).

6.2. Cada card deberá presentar, en solo lectura, la siguiente información:
-	Número de cuenta.
-	Número del reporte.
-	Estado del reporte.
-	Fecha de solicitud.
-	Fecha de atención.

6.3. Cada card deberá incluir la opción o botón “Ver detalles”, claramente identificable y accesible, conforme a los diseños móviles.

7.	Visualización de detalle de reporte en app móvil

7.1. Al pulsar la opción “Ver detalles” en una card de la app móvil, el sistema deberá abrir una nueva pantalla de detalle del reporte seleccionado.

7.2. En esta nueva pantalla de detalle, el sistema deberá mostrar, en solo lectura, la siguiente información del reporte:
-	Número de cuenta.
-	Número del reporte.
-	Estado del reporte.
-	Fecha de solicitud.
-	Fecha de atención.
-	Descripción del daño.
-	Observación técnica.

7.3. El usuario no podrá editar ninguna de las informaciones mostradas en la pantalla de detalle.

8.	Registro de acciones en auditoría

8.1. Cada vez que el usuario acceda a la opción Mis reportes en web o móvil, el sistema deberá registrar un evento de auditoría conforme a la HU ESD-187.

8.2. Cada vez que el usuario seleccione una cuenta y se realice la consulta de reportes, deberá registrarse un nuevo evento de auditoría con al menos: usuario, cuenta seleccionada, canal (web/móvil) y timestamp.

8.3. En la app móvil, cada vez que el usuario pulse “Ver detalles” y se muestre la pantalla de detalle de un reporte, el sistema deberá registrar un evento de auditoría indicando el número de reporte consultado y el contexto del usuario.

9.	Cumplimiento de diseño y usabilidad

9.1. Los elementos de la pantalla (campos, labels, botones, íconos, mensajes) deberán mostrarse correctamente alineados, con íconos visibles y textos legibles, de acuerdo con el diseño entregado para la versión web y móvil, disponible en Figma:
ESSA - Oficina Virtual Diseño UX_UI Vigente

9.2. La estructura y estilo visual (colores, tipografías, tamaños de fuente, espaciados, etc.) deberán seguir las guías definidas en dichos diseños.

Datos de Entrada / Salida

Datos de Entrada
Fuente: Usuario (interfaz web / app móvil)
1.	Cuenta seleccionada
    -	Tipo de dato: Identificador de cuenta (string o numérico, de acuerdo al modelo de datos).
    -	Origen: Usuario, a través de un desplegable de búsqueda de única selección.
    -	Obligatorio: Sí (no se puede consultar sin seleccionar cuenta).
    -	Restricciones:
        - Debe ser una cuenta asociada al usuario autenticado.
        - El desplegable debe permitir búsqueda por número de cuenta y/o alias.
    -	Uso:
        - Parámetro de entrada para la consulta de reportes mediante el servicio SP7 “Consulta por Trouble Ticket”.
2.	Acción “Ver detalles” (app móvil)
    -	Tipo de dato: Selección de un reporte específico (identificado por número de reporte / trouble ticket).
    -	Origen: Usuario, pulsando el botón “Ver detalles” en una card de reporte.
    -	Obligatorio: No, es opcional por cada reporte.
    -	Restricciones:
        - Solo disponible cuando existan reportes visibles.
    -	Uso:
        - Dispara la navegación a la pantalla de detalle del reporte seleccionado.

Fuente: Sistema / Servicio SP7 “Consulta por Trouble Ticket”
Entradas detalladas al servicio SP7 deberán consultarse en la especificación técnica:
https://grupovass.sharepoint.com/:x:/r/teams/OperacionColombia/_layouts/15/Doc.aspx?sourcedoc=%7B62E074C8-E76B-4E31-A9ED-AB8F3897AA3E%7D&file=Metodos%20API%20SP7.xlsx&action=default&mobileredirect=true

Datos de Salida
Salida hacia el usuario (web y móvil, solo lectura):
Para cada reporte de daño/emergencia mostrado:
1.	Número de cuenta
    -	Origen: Cuenta seleccionada por el usuario (dato local) y/o servicio.
    -	Formato: Texto / número según modelo de datos.
    -	Uso: Identifica la cuenta a la que pertenece el reporte.
2.	Número de reporte (trouble ticket)
    -	Origen: Servicio SP7 “Consulta por Trouble Ticket”.
    -	Campo: Identificador de ticket (p.ej., “TT.Id” o equivalente según especificación).
    -	Uso: Identificador único del reporte.
3.	Estado del reporte
    -	Origen: Servicio SP7.
    -	Campo: “TT.Estado”.
    -	Uso: Indica el estado actual del reporte (abierto, en atención, cerrado, etc.).
4.	Fecha de solicitud
    -	Origen: Servicio SP7.
    -	Campo: “TT.FechaCreacion”.
    -	Formato: A definir según estándar de la Oficina Virtual (p. ej. dd/MM/yyyy HH:mm).
    -	Uso: Indica la fecha/hora en que se generó el reporte.
5.	Fecha de atención
    -	Origen: Servicio SP7.
    -	Campo: “TT.FechaCierre”.
    -	Formato: Igual al estándar de fechas definido.
    -	Uso: Indica la fecha/hora en que el reporte fue atendido/cerrado.
    -	Comportamiento ante valores nulos: si no existe fecha de cierre, se debe mostrar vacío o un indicador según diseño (por ejemplo “Pendiente”), respetando el diseño UX.
6.	Descripción del daño
    -	Origen: Servicio SP7.
    -	Campo: “TT.Observacion”.
    -	Uso: Describe el daño reportado.
7.	Causa
    -	Origen: Servicio SP7.
    -	Campo: “TT.Causa”.
    -	Uso: Indica la causa asociada al daño (cuando esté disponible).
8.	Observación técnica (en pantalla de detalle móvil)
    -	Origen: Servicio SP7.
    -	Campo: Observación técnica asociada (alineada con “TT.Observacion” o campo específico si se distingue en el servicio).
    -	Uso: Mostrar detalle técnico del reporte en la pantalla de detalle.

Mensajes y elementos de interfaz:
1.	Mensaje informativo cuando no existan reportes para la cuenta
    -	Contenido: Texto que indique que la cuenta no tiene reportes disponibles.
    -	Origen: Sistema, cuando la consulta no devuelve reportes válidos.
    -	Uso: Informar al usuario y mostrar los botones de creación de nuevo reporte centrados.
2.	Botones de creación de nuevo reporte
    -	Origen: Interfaz de usuario, posicionados según diseño.
    -	Uso: Permitir que el usuario inicie la creación de un nuevo reporte de daño/emergencia (la creación en sí puede corresponder a otra HU).

Salida hacia sistemas de auditoría:
1.	Registro de eventos de auditoría
    -	Origen: Sistema de Oficina Virtual.
    -	Destino: Módulo de auditoría definido en ESD-187.
    -	Contenido (a nivel funcional):
        -	Identificador de usuario.
        -	Acción realizada (acceso a “Mis reportes”, selección de cuenta, consulta ejecutada, navegación a detalle en móvil, etc.).
        -	Timestamp de la acción.
        -	Identificadores de contexto (cuenta seleccionada, número de reporte, canal web/móvil).
        Datos de Salida

Salida hacia el usuario (web y móvil, solo lectura):

Para cada reporte de daño/emergencia mostrado:
1.	Número de cuenta
    -	Origen: Cuenta seleccionada por el usuario (dato local) y/o servicio.
    -	Formato: Texto / número según modelo de datos.
    -	Uso: Identifica la cuenta a la que pertenece el reporte.
2.	Número de reporte (trouble ticket)
    -	Origen: Servicio SP7 “Consulta por Trouble Ticket”.
    -	Campo: Identificador de ticket (p.ej., “TT.Id” o equivalente según especificación).
    -	Uso: Identificador único del reporte.
3.	Estado del reporte
    -	Origen: Servicio SP7.
    -	Campo: “TT.Estado”.
    -	Uso: Indica el estado actual del reporte (abierto, en atención, cerrado, etc.).
4.	Fecha de solicitud
    -	Origen: Servicio SP7.
    -	Campo: “TT.FechaCreacion”.
    -	Formato: A definir según estándar de la Oficina Virtual (p. ej. dd/MM/yyyy HH:mm).
    -	Uso: Indica la fecha/hora en que se generó el reporte.
5.	Fecha de atención
    -	Origen: Servicio SP7.
    -	Campo: “TT.FechaCierre”.
    -	Formato: Igual al estándar de fechas definido.
    -	Uso: Indica la fecha/hora en que el reporte fue atendido/cerrado.
    -	Comportamiento ante valores nulos: si no existe fecha de cierre, se debe mostrar vacío o un indicador según diseño (por ejemplo “Pendiente”), respetando el diseño UX.
6.	Descripción del daño
    -	Origen: Servicio SP7.
    -	Campo: “TT.Observacion”.
    -	Uso: Describe el daño reportado.
7.	Causa
    -	Origen: Servicio SP7.
    -	Campo: “TT.Causa”.
    -	Uso: Indica la causa asociada al daño (cuando esté disponible).
8.	Observación técnica (en pantalla de detalle móvil)
    -	Origen: Servicio SP7.
    -	Campo: Observación técnica asociada (alineada con “TT.Observacion” o campo específico si se distingue en el servicio).
    -	Uso: Mostrar detalle técnico del reporte en la pantalla de detalle.

Mensajes y elementos de interfaz:

1.	Mensaje informativo cuando no existan reportes para la cuenta
    -	Contenido: Texto que indique que la cuenta no tiene reportes disponibles.
    -	Origen: Sistema, cuando la consulta no devuelve reportes válidos.
    -	Uso: Informar al usuario y mostrar los botones de creación de nuevo reporte centrados.
2.	Botones de creación de nuevo reporte
    -	Origen: Interfaz de usuario, posicionados según diseño.
    -	Uso: Permitir que el usuario inicie la creación de un nuevo reporte de daño/emergencia (la creación en sí puede corresponder a otra HU).

Salida hacia sistemas de auditoría:
1.	Registro de eventos de auditoría
    -	Origen: Sistema de Oficina Virtual.
    -	Destino: Módulo de auditoría definido en ESD-187.
    -	Contenido (a nivel funcional):
    -	Identificador de usuario.
    -	Acción realizada (acceso a “Mis reportes”, selección de cuenta, consulta ejecutada, navegación a detalle en móvil, etc.).
    -	Timestamp de la acción.
    -	Identificadores de contexto (cuenta seleccionada, número de reporte, canal web/móvil).


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

