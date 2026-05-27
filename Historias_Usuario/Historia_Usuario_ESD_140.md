# Historia de Usuario 140 Solicitar financiación express

## YO Como
usuario autenticado en la oficina virtual (web y app móvil)

## Sprint
16

## Menú de navegación
Gestion de facturacion (home)

## Objetivo
- Problema que resuelve: Los usuarios con facturas vencidas o deudas vigentes actualmente deben acudir a canales presenciales o telefónicos para negociar acuerdos de pago, lo que genera fricción, demoras y riesgo de suspensión del servicio.

- Valor que aporta: Digitalizar el proceso de solicitud de financiación express, permitiendo al usuario gestionar un acuerdo de pago en cuotas desde la oficina virtual, con validación de identidad fotográfica, firma digital y generación de volante de confirmación descargable.

- Resultado esperado: El sistema permite al usuario elegir una opción de financiación, firmar digitalmente, recibir un volante de confirmación y radicar la solicitud con sus anexos, todo dentro de un flujo guiado de dos pasos sin intervención manual de un operador.


## Precondiciones

PRE-01	El usuario debe estar autenticado en la oficina virtual (sesión activa válida).

PRE-02	El usuario debe tener al menos una factura vencida o una deuda vigente cuya fecha límite de pago haya sido superada.

PRE-03	El servicio de opciones de crédito (Créditos/Opciones) debe estar disponible y operativo.

PRE-04	El servicio de creación de crédito (Créditos/Crear) debe estar disponible y operativo.

PRE-05	El servicio de anexar archivos (Controlador procesos → AnexarArchivo) debe estar disponible y operativo.

PRE-06	El perfil del usuario debe contener los datos personales requeridos (nombres, documento, dirección, teléfono, etc.).

PRE-07	El dispositivo móvil debe contar con cámara funcional (para la captura de fotografías en app mobile).

PRE-08	La funcionalidad de firma digital (HU ESD-141) debe estar implementada y disponible.

## Postcondiciones

POST-01	Se crea exitosamente la solicitud de financiación express en el sistema mediante el servicio Créditos/Crear.

POST-02	Las tres fotografías del documento de identidad y el volante de confirmación se adjuntan al proceso mediante el servicio Controlador procesos → AnexarArchivo.

POST-03	Se genera un volante de confirmación en formato descargable con la información de la financiación acordada.

POST-04	El sistema muestra al usuario el número de proceso (IngNumeroProceso) como confirmación de radicación exitosa.

POST-05	Se registran eventos de auditoría por cada acción realizada por el usuario en la pantalla (según HU ESD-187).

POST-06	El usuario puede regresar al home del sistema tras finalizar el proceso.


## Reglas de negocio

RRN01	El elemento de acción "Financiación express" solo se muestra si el usuario tiene al menos una factura vencida o deuda vigente con fecha límite de pago superada Y el servicio de opciones de crédito retorna opciones válidas (la respuesta contiene el atributo lisCuotas).

RN02	Si el servicio de opciones de crédito no retorna opciones (la respuesta NO contiene el atributo lisCuotas), el elemento "Financiación express" no debe mostrarse bajo ninguna circunstancia.

RN03	Los datos del solicitante (Paso 1) se muestran en modo solo lectura y ofuscados, obtenidos del perfil del usuario autenticado. No son editables desde esta pantalla.

RN04	La fecha de expedición del documento es un campo obligatorio de tipo selector de fecha. Solo permite seleccionar fechas anteriores a la fecha actual; las fechas iguales o posteriores a hoy deben estar deshabilitadas.

RN05	En app móvil, las tres fotografías (frente del documento, reverso del documento, selfie con documento visible) son obligatorias y deben capturarse exclusivamente con la cámara del dispositivo. No se permite selección desde galería.

RN06	En versión web, las fotografías se adjuntan como archivos. Solo se permiten formatos JPG y PNG. Cualquier otro formato debe ser rechazado y no permitir la selección del archivo.

RN07	Cada fotografía debe poder previsualizarse antes de su confirmación. El usuario puede retomar o eliminar cualquier fotografía antes de avanzar.

RN08	Al intentar eliminar una fotografía, el sistema debe solicitar confirmación explícita al usuario con el mensaje: "¿Está seguro que desea eliminar la imagen?".

RN09	No se permite avanzar del Paso 1 al Paso 2 si no se han capturado/adjuntado y confirmado correctamente las tres fotografías y la fecha de expedición del documento.

RN10	El usuario debe seleccionar exactamente una opción de financiación. No se permite selección múltiple ni avanzar sin selección.

RN11	La firma digital es obligatoria. El lienzo de firma debe contener algún trazo para considerarse válido. La funcionalidad de firma se rige por la HU ESD-141.

RN12	El check de aviso de privacidad es obligatorio y solo se muestra después de que el usuario haya diligenciado la firma digital. El usuario debe marcarlo para poder continuar.

RN13	El texto subrayado del aviso de privacidad debe abrir embebida la URL configurada del aviso de privacidad.

RN14	El botón "Enviar" solo se habilita después de que el usuario haya marcado el check del aviso de privacidad.

RN15	El volante de confirmación debe descargarse con el formato de nombre: financiación_express_DD/MM/AAAA_YYYY, donde DD/MM/AAAA es la fecha de generación y YYYY es el número de cuenta asociada a la financiación.

RN16	El volante generado y las fotografías deben adjuntarse al proceso mediante el servicio Controlador procesos → AnexarArchivo, dado que el método Créditos/Crear no recibe adjuntos.

RN17	El mensaje de confirmación final solo se muestra si y solo si tanto la creación de la financiación (Créditos/Crear) como la adjuntación de anexos (AnexarArchivo) fueron exitosas.

RN18	El usuario puede reintentar la operación de forma ilimitada en caso de fallo.

RN19	Todas las acciones del usuario en esta pantalla deben registrar un evento de auditoría según lo definido en la HU ESD-187.

## Criterios de Aceptación

1.	Al ingresar a la pantalla de Gestión de facturación (home), el sistema deberá evaluar la condición financiera del usuario autenticado, validando que tenga al menos una factura vencida o una deuda vigente cuya fecha límite de pago ya haya sido superada.
2.	Si el usuario cumple la condición de factura vencida, el sistema deberá ejecutar el servicio de opciones de crédito (Créditos/Opciones). Si el servicio retorna opciones de financiación (la respuesta contiene el atributo lisCuotas), el sistema deberá mostrar un elemento de acción visible denominado "Financiación express" en el home.
3.	Si el usuario no cumple la condición de factura vencida o el servicio de opciones no retorna opciones (la respuesta NO contiene el atributo lisCuotas), el elemento de acción "Financiación express" no deberá mostrarse.
4.	Al seleccionar la opción "Financiación express", el sistema deberá redirigir al usuario a una nueva pantalla que gestione el proceso en dos pasos secuenciales: Paso 1 – Información del solicitante y Paso 2 – Selección de financiación y firma.
5.	En el Paso 1, el sistema deberá mostrar en modo solo lectura y ofuscada la información del solicitante obtenida del perfil del usuario autenticado: Nombres y apellidos, Tipo de persona, Número de documento, Lugar de expedición del documento (municipio), Teléfono, Calidad de cliente, Dirección, Departamento y Municipio.
6.	En el Paso 1, el sistema deberá solicitar la selección de la fecha de expedición del documento mediante un selector tipo fecha obligatorio. Las fechas futuras (iguales o posteriores a hoy) deberán estar deshabilitadas; solo se permitirá seleccionar fechas anteriores a la fecha actual.
7.	El sistema deberá mostrar un mensaje informativo indicando que, si algún dato se encuentra desactualizado, el usuario puede actualizarlo desde su perfil. Este mensaje deberá incluir un enlace o botón que redirija directamente a la funcionalidad de actualización de datos del perfil.
8.	En el Paso 1, el sistema deberá solicitar obligatoriamente la captura/adjuntación de tres (3) fotografías:
    -	Fotografía del frente del documento de identidad.
    -	Fotografía del reverso del documento de identidad.
    -	Fotografía del usuario con el documento de identidad visible (selfie).
9.	En app móvil, antes de tomar cada fotografía, el sistema deberá mostrar una pantalla estática de guía con una descripción clara de cómo se espera la imagen y recomendaciones para obtener un registro fotográfico de calidad. Esta pantalla contará con una opción para continuar; al seleccionarla, el usuario podrá proceder a capturar la fotografía utilizando exclusivamente la cámara del dispositivo.
10. En app móvil, el sistema deberá permitir la previsualización de cada fotografía tomada antes de su confirmación. El sistema deberá permitir al usuario retomar la fotografía si considera que no cumple condiciones adecuadas de visibilidad.
11. El sistema deberá permitir eliminar una fotografía tomada mediante un elemento de acción visible asociado a cada imagen. Al intentar eliminar, el sistema deberá mostrar un mensaje de confirmación: "¿Está seguro que desea eliminar la imagen?", solicitando la validación de la acción antes de proceder.
12. En versión web, las fotografías se solicitan como archivos adjuntos. El sistema deberá validar que los archivos tengan formato JPG o PNG. Si el archivo no cumple con esta validación, no se permitirá su selección para adjuntarlo.
13. El sistema no deberá permitir avanzar al Paso 2 si no se han capturado/adjuntado y confirmado correctamente las tres fotografías obligatorias y la fecha de expedición del documento.
14. Al ingresar al Paso 2, el sistema deberá consultar el servicio correspondiente (Créditos/Opciones) y mostrar una pantalla informativa que incluya: el total de la deuda actual (decSaldoTotal, numérico en moneda local) y el valor máximo a financiar (decSaldoFinanciar, numérico en moneda local).
15. El sistema deberá mostrar las opciones de financiación disponibles obtenidas del servicio. Para cada opción se mostrará: Número de cuotas (IntNumeroCuotas), Valor de la cuota (decCuotaPer, moneda local), Tasa de interés aplicable (decInteres, porcentaje) y Cuota inicial (decCuotaIni, moneda local).
16. El sistema deberá permitir seleccionar únicamente una opción de financiación. Hasta que el usuario no seleccione una opción válida, el proceso no podrá continuar.
17. Al continuar tras la selección de la opción de financiación, el sistema deberá mostrar un recuadro con un lienzo de firma digital según la funcionalidad descrita en la HU ESD-141.
18. Al validar que el usuario haya diligenciado la firma (lienzo con algún contenido/trazo), el sistema deberá mostrar un check de aviso de privacidad que el usuario debe marcar obligatoriamente para poder continuar. Al seleccionar el texto subrayado del aviso de privacidad, el sistema deberá mostrar embebida la URL configurada del aviso.
19. Después de que el usuario haya marcado el check del aviso de privacidad, el sistema deberá habilitar el botón "Enviar" que permite enviar la solicitud de financiación express al servicio correspondiente (Créditos → Método: Crear).
20. Si el servicio Créditos/Crear responde de forma exitosa, el sistema deberá generar un volante de confirmación de la financiación que se mostrará en pantalla para su descarga. El volante deberá contener:
    -	Header: Imagen adjunta a la HU.
    -	Título: "Financiación express".
    -	Número de Orden.
    -	Cliente.
    -	Dirección.
    -	Sección "Datos del solicitante": Tipo de Documento, Número Documento, Fecha Expedición, Lugar Expedición, En Calidad, Nombre completo.
    -	Sección "Financiación Express": Cédula, Saldo Total, Saldo a Financiar, Número Cuotas, Interés, Cuota Periódica, Número Proceso (IngNumeroProceso), Mensaje personalizado.
    -	Sección "Cliente Digital": Teléfono (número de celular del deudor).
    -	Sección "Autorización para el tratamiento de datos": Texto legal completo de autorización según Ley 1581 de 2012.
    -	Firma: Firma realizada por el cliente junto con su nombre e identificación.
    -	Fecha y hora de impresión.
21. El nombre del archivo descargado deberá seguir el formato: financiación_express_DD/MM/AAAA_YYYY, donde DD/MM/AAAA es la fecha de generación y YYYY es el número de cuenta asociada a la financiación.
22. Si el servicio Créditos/Crear responde con error, el sistema deberá mostrar el mensaje: "No fue posible completar la financiación, por favor intenta nuevamente más tarde" y permitir reintentar la operación.
23. El volante generado y las fotografías adjuntas se deben adjuntar al proceso utilizando el servicio Controlador procesos → AnexarArchivo, dado que el método Créditos/Crear no recibe adjuntos.
24. Si y solo si la creación de la financiación (Créditos/Crear) y la adjuntación de anexos (AnexarArchivo) fueron exitosas, el sistema deberá mostrar el mensaje de confirmación: "Tu financiación ha sido creada" y "Número de proceso XXXXXXX" (donde XXXXXXX es el atributo IngNumeroProceso), con la opción "Volver al inicio" que regresa al usuario al home del sistema.
25. Si el proceso de radicado falla:
    -	Si no se crea la solicitud de financiación: El sistema deberá mostrar el mensaje: "No fue posible completar tu solicitud. Ocurrió un error durante el procesamiento. Verifica tu conexión o inténtalo más tarde."
    -	Si se crea la solicitud pero no se pueden adjuntar los archivos: El sistema deberá mostrar el mensaje: "Ocurrió un error durante el procesamiento. Verifica tu conexión a internet o intenta nuevamente más tarde. También puedes contactarnos a través de la línea 01 8000 971 903, donde con gusto te brindaremos asistencia."
    -	En ambos casos, se mostrarán las opciones: "Reintentar" (vuelve a intentar la conexión con el servicio; el usuario puede reintentar de forma ilimitada) y "Volver al inicio" (regresa al home del sistema).
26. Todas las acciones realizadas por el usuario en esta pantalla deberán registrar un evento de auditoría según lo definido en la HU ESD-187.
27. Los elementos de la pantalla deberán mostrarse correctamente alineados, con íconos visibles y textos legibles, de acuerdo con el diseño entregado para la versión web y móvil de la oficina virtual.


## Datos de Entrada / Salida
- Datos de entrada:
| Campo                               | Tipo de Dato                          | Formato / Restricción                                           | Obligatorio      | Origen                    | Editable |
|------------------------------------|----------------------------------------|------------------------------------------------------------------|------------------|---------------------------|-----------|
| Nombres y apellidos                | Alfabético                             | Solo lectura, ofuscado                                           | Sí (precargado)  | Perfil del usuario        | No        |
| Tipo de persona                    | Alfanumérico                           | Solo lectura, ofuscado                                           | Sí (precargado)  | Perfil del usuario        | No        |
| Número de documento                | Numérico                               | Solo lectura, ofuscado                                           | Sí (precargado)  | Perfil del usuario        | No        |
| Lugar de expedición del documento  | Alfanumérico                           | Municipio de expedición, solo lectura, ofuscado                  | Sí (precargado)  | Perfil del usuario        | No        |
| Teléfono                           | Numérico                               | Solo lectura, ofuscado                                           | Sí (precargado)  | Perfil del usuario        | No        |
| Calidad de cliente                 | Alfanumérico                           | Solo lectura, ofuscado                                           | Sí (precargado)  | Perfil del usuario        | No        |
| Dirección                          | Alfanumérico con caracteres especiales | Solo lectura, ofuscado                                           | Sí (precargado)  | Perfil del usuario        | No        |
| Departamento                       | Alfabético                             | Solo lectura, ofuscado                                           | Sí (precargado)  | Perfil del usuario        | No        |
| Municipio                          | Alfabético                             | Solo lectura, ofuscado                                           | Sí (precargado)  | Perfil del usuario        | No        |
| Fecha de expedición del documento  | Fecha                                  | Selector tipo fecha; solo fechas anteriores a hoy                | Sí               | Usuario                   | Sí        |
| Fotografía frente del documento    | Imagen                                 | JPG o PNG (web); captura por cámara (mobile)                     | Sí               | Usuario / Cámara          | Sí        |
| Fotografía reverso del documento   | Imagen                                 | JPG o PNG (web); captura por cámara (mobile)                     | Sí               | Usuario / Cámara          | Sí        |
| Fotografía selfie con documento    | Imagen                                 | JPG o PNG (web); captura por cámara (mobile)                     | Sí               | Usuario / Cámara          | Sí        |
| Opción de financiación seleccionada| Selección única                        | Una opción del listado retornado por el servicio                 | Sí               | Usuario                   | Sí        |
| Firma digital                      | Imagen/Trazo                           | Lienzo con contenido (no vacío)                                  | Sí               | Usuario (HU ESD-141)      | Sí        |
| Check aviso de privacidad          | Booleano                               | Marcado = true                                                   | Sí               | Usuario                   | Sí        |

- Datos de salida:
| Dato                                      | Tipo                          | Descripción                                                                                                                                                                                                 |
|-------------------------------------------|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Elemento de acción "Financiación express" | Visual (UI)                   | Se muestra u oculta en el home según evaluación de condición financiera y respuesta del servicio de opciones.                                                                                              |
| Datos del solicitante (Paso 1)            | Visual (UI)                   | Información del perfil del usuario mostrada en modo solo lectura y ofuscada.                                                                                                                               |
| Opciones de financiación (Paso 2)         | Visual (UI)                   | Listado de opciones con: número de cuotas (IntNumeroCuotas), valor de cuota (decCuotaPer), tasa de interés (decInteres), cuota inicial (decCuotaIni).                                                   |
| Total de deuda actual                     | Numérico (moneda local)       | Valor del atributo decSaldoTotal.                                                                                                                                                                           |
| Valor máximo a financiar                  | Numérico (moneda local)       | Valor del atributo decSaldoFinanciar.                                                                                                                                                                       |
| Volante de confirmación                   | Documento descargable         | Contiene: header, datos del solicitante, datos de financiación, número de proceso (IngNumeroProceso), firma del cliente, fecha/hora de impresión, autorización de tratamiento de datos.                  |
| Mensaje de éxito                          | Texto                         | "Tu financiación ha sido creada" + "Número de proceso XXXXXXX".                                                                                                                                             |
| Mensaje de error (creación fallida)       | Texto                         | "No fue posible completar tu solicitud. Ocurrió un error durante el procesamiento. Verifica tu conexión o inténtalo más tarde."                                                                           |
| Mensaje de error (adjuntos fallidos)      | Texto                         | "Ocurrió un error durante el procesamiento. Verifica tu conexión a internet o intenta nuevamente más tarde. También puedes contactarnos a través de la línea 01 8000 971 903, donde con gusto te brindaremos asistencia." |
| Mensaje de error (financiación fallida en Paso 2) | Texto                | "No fue posible completar la financiación, por favor intenta nuevamente más tarde."                                                                                                                         |
| Evento de auditoría                       | Registro                      | Registro de cada acción del usuario según HU ESD-187.                                                                                                                                                       |


## Criterios de Aceptación No Funcionales
CANF-01	Rendimiento	La consulta al servicio de opciones de crédito (Créditos/Opciones) y la evaluación de condición financiera en el home deben ejecutarse en un tiempo que no afecte la experiencia de carga de la pantalla principal.

CANF-02	Rendimiento	La generación del volante de confirmación debe completarse en un tiempo razonable que no genere percepción de bloqueo en el usuario.

CANF-03	Seguridad	Los datos personales del solicitante deben mostrarse ofuscados en pantalla para proteger la información sensible.

CANF-04	Seguridad	Las fotografías del documento de identidad y la firma digital deben transmitirse de forma segura al servidor (protocolo HTTPS).

CANF-05	Seguridad	Todas las acciones deben registrar eventos de auditoría (HU ESD-187) para trazabilidad y cumplimiento normativo.

CANF-06	Compatibilidad	La funcionalidad debe operar correctamente en la versión web y en la app móvil, respetando los diseños entregados para cada plataforma.

CANF-07	Compatibilidad	En app móvil, la captura de fotografías debe utilizar exclusivamente la cámara nativa del dispositivo.

CANF-08	Accesibilidad	Los elementos de la pantalla deben mostrarse correctamente alineados, con íconos visibles y textos legibles según el diseño UI/UX entregado.

CANF-09	Usabilidad	Los mensajes de error deben ser claros, informativos y orientar al usuario sobre la acción a seguir (reintentar o contactar soporte).

## Dependencias
ESD-141	Funcionalidad de firma digital (lienzo de firma). Debe estar implementada antes de esta HU.

ESD-187	Funcionalidad de registro de eventos de auditoría. Debe estar implementada o en paralelo.

ESD-97	 Epic padre — Contexto funcional general del módulo de gestión de facturación.


