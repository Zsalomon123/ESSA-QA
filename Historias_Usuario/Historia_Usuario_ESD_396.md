# Historia de Usuario 396 Consultar estado solicitud de nueva conexión

## YO Como
 usuario autenticado en la Oficina Virtual ESSA.

## Sprint
16

## Menú de navegación
Modulo Servicios / vinculación de clientes 


## Criterios de Aceptación
1.	Al ingresar al módulo Servicios > vinculación de clientes el sistema deberá mostrar la opción "Consultar estado solicitud de vinculación" junto a la funcionalidad de definición del tipo de conexión, de forma visible y accesible.
2.	Al seleccionar la opción, el sistema deberá redirigir al usuario a una pantalla de consulta que contenga dos campos de entrada de texto y un botón de acción.
3.	El sistema deberá mostrar los campos "Número de cuenta" y “Número de proceso” con las siguientes características: tipo de dato numérico, longitud máxima de 50 caracteres, carácter obligatorio. El campo deberá permitir únicamente la entrada de dígitos numéricos (0-9).
4.	Si el usuario intenta ingresar caracteres alfabéticos o especiales en el campo de código, el sistema deberá impedir su ingreso en tiempo real.
5.	El sistema deberá mostrar el botón "Consultar", el cual deberá permanecer en estado deshabilitado (disabled) hasta que los campos obligatorio contengan información válida (numérica y no vacía).
6.	Al dar clic en el botón "Consultar", el sistema deberá validar que los campos no estén vacíos. Si estan vacios, el sistema deberá resaltar visualmente los campos que no estén diligenciados.
7.	Si la validación es exitosa (campos con valores numéricos válidos), el sistema deberá consumir el servicio de consulta de estado de solicitud (Consultar el Estado del Proceso en SAC (ConsultarProceso)), enviando como parámetro el Número de cuenta y el Número de proceso.
8.	Si el servicio responde exitosamente con información asociada al código, el sistema deberá presentar los resultados en pantalla en formato estructurado y de solo lectura, mostrando los siguientes campos: 
    -	Número de cuenta, 
    -	Número de proceso.
    -	Fecha de solicitud.
    -	Estado del trámite.
    -	Fecha máxima de respuesta.
    -	Fecha respuesta, se debe mostrar solo si el estado del trámite es “Aprobado” o “No aprobado”.
    -	Paso a seguir, se debe mostrar solo si el estado del trámite es “Aprobado” o “No aprobado”. Debe ser un objeto interactivo que permita redireccionar al formulario del siguiente paso.
    -	Observación.
    -	Anexo de entrada, se deben mostrar listados bajo la solicitud y debe permitir la descarga, si el servicio NO retorna información en este parámetro esta sección se oculta. 
    -	Anexo de salida, mostrar solamente cuando haya archivos, se deben mostrar listados bajo la solicitud y debe permitir la descarga, si el servicio NO retorna información en este parámetro esta sección se oculta. 
9.	La información que se retorna en el “Paso a seguir” puede variar, por lo tanto se debe guardar en base de datos la relación entre el nombre del paso y una URL de redirección de tal manera que cada paso tenga por debajo un link asociado hacia el cual el usuario se pueda redireccionar.
10.	Toda la información presentada en la sección de resultados deberá mostrarse en modo solo lectura, sin posibilidad de edición o modificación por parte del usuario.
11.	Para la app de movile cuando no hay anexos se oculta la sección correspondiente, ya sea Salida, entrada o cuando son ambos los que cuentan sin anexos la opción de “Ver anexos” se inhabilita.
12.	Si el servicio responde exitosamente pero sin información asociada al código ingresado o Si el servicio presenta error, timeout o indisponibilidad, el sistema deberá mostrar el mensaje: 
    -	Título: “No encontramos resultados”
    -	Cuerpo: “Revisa que el número de solicitud y cuenta sean correctos.“
    -	Acción: Elemento de accion “Definir tipo de conexión“, que al seleccionarlo redireccional usuario a la funcionalidad descrita en la HU ESD-258: Definir el tipo de conexión que necesita el cliente To Do.
13.	Tras mostrar los resultados o un mensaje de error/sin datos, el sistema deberá mantener visible y funcional el campo de búsqueda y el botón "Consultar", permitiendo al usuario realizar una nueva consulta sin necesidad de recargar la página.
14.	Cada acción realizada por el usuario en la pantalla de consulta deberá registrar un evento de auditoría ESD-187: Registro de auditoriaDone.
15.	Los elementos de la pantalla (campos, botones, etiquetas, íconos, mensajes) deberán mostrarse correctamente alineados, con íconos visibles y textos legibles, de acuerdo con el diseño entregado para la versión web y la versión móvil.
 

