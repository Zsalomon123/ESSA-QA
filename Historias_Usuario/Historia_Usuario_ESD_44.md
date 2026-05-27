# Historia de Usuario 44 Visualizar reportes que afecten mis cuentas

## YO Como
usuario de la Oficina Virtual (autenticado o no autenticado)

## Sprint
16

## Menú de navegación
Danos y PQR/ Danos y emergencias/ reportar un dano


## Precondiciones

Ambiente configurado y estable. Datos de prueba disponibles. Accesos y credenciales habilitadas.

## Criterios de aceptacion

1.	Al ingresar al menú en Daños y pqr, seleccionar "Reportar un daño" y seleccionar la opción "Por número de cuenta".
2.	El sistema debe mostrar un formulario tipo stepper con los pasos:
    a. Paso 1: Datos del predio
    b. Paso 2: Datos del solicitante
    c. Paso 3: Información del evento
3.	El sistema debe mostrar el Paso 1 con el título: "Datos del predio".
    a. Cuando es un usuario autenticado:
        i. El sistema debe mostrar una lista desplegable con las cuentas asociadas al usuario, el usuario puede seleccionar solo una cuenta.

        ii. El sistema debe permitir ingresar manualmente otro número de cuenta mediante la opción: "¿Quieres reportar con otro número de cuenta?" con campo de texto que le permita ingresar datos numéricos y es obligatorio. Si no se ingresan datos validos se debe resaltar el campo y no habilitar la opción de continuar.

    b. Cuando es un usuario no autenticado:
        i. El sistema debe habilitar un campo obligatorio para ingresar el número de cuenta. Si no se ingresan datos validos se debe resaltar el campo y no habilitar la opción de continuar.

    c. Debe haber disponible un botón para continuar el proceso, al ejecutar este botón el sistema debe validar que:
        i. El sistema debe validar la cuenta ingresada consultando su información. Solo se permite continuar si:
4. El estado de la cuenta en el campo intEstadoCliente es diferente a: Ver imagen 1
5.	El ciclo es diferente 
6.	La tarifa es diferente a:
        ii. Si la cuenta no existe, se debe mostrar el mensaje: "No se encontró la cuenta. Por favor verifícala.".

        iii. Si la cuenta no cumple con las validaciones, se debe mostrar el mensaje: "Para reportar daños tenemos los siguientes canales de atención dispuestos para ti: comunícate a nuestra línea gratuita para daños y/o emergencias📞115, línea de servicio al cliente 📞 01 8000 97 19 03 o conversa nuevamente con nuestra asistente virtual Luisa opción 2." - ver en figma: "Modal de error - Cuenta no apta"
    
    d. Si la cuenta cumple con las validaciones previas, se debe realizar otras validaciones para verificar que el daño si se pueda reportar, así:
        i. El sistema debe realizar la consulta al servicio "Consulta por CUD" utilizando la cuenta del usuario. Si el servicio "Consulta por CUD" retorna que existe un Trouble Ticket (daño) registrado en las últimas 48 horas, el sistema debe:

7.	Validar el estado del daño (Tipo = No programado") si su estado es "Procesado" quiere decir que el Trouble Ticket esta "EN TRAMITE", para este caso:
    a. Mostrar el modal definido en Figma "Trouble Ticket de 48 horas - Estado: EN TRAMITE".
    b. El modal debe contener las siguientes opciones:

        i. "Entiendo": El sistema debe redirigir al usuario al home de la aplicación.
8.	Validar el estado del daño (Tipo = No programado") si su estado es “COMPLETADO”, "RESTAURADO", "CERRADO" quiere decir que el Trouble Ticket esta "EN TRAMITE", para este caso:
    a. Mostrar el modal definido en Figma "Trouble Ticket de 48 horas - Estado: FINALIZADO.
    b. El modal debe contener las siguientes opciones:
        i. "Sigo presentando novedades": El sistema debe permitir al usuario continuar con la funcionalidad de registro de daño.

        ii. "Salir": El sistema debe redirigir al usuario al home de la aplicación.
    e. IMPORTANTE: Los estados que definen un daño como EN TRAMITE o FINALIZADO pueden ser varios (diferentes a los mencionados en los puntos anteriores), por lo tanto se debe realizar una configuración en la base de datos que en el futuro me permita colocar más estados que se homologuen como en tramite o finalizado.
9.	Si el servicio "Consulta por CUD" NO retorna daños en las últimas 48 horas, el sistema debe:
    a. Consultar el servicio "Consulta por CUD" para validar si existe una desconexión asociada a la cuenta.
        i. Si el servicio "Consulta por CUD" retorna una desconexión con Estado = "ACTIVA" y Tipo = "PROGRAMADO"
10.	El sistema debe: Mostrar el modal definido en Figma "Con desconexión ACTIVA - PROGRAMADA", El modal debe contener la opción: "Entiendo", que lo que haces es redirigir al usuario al home de la aplicación.

11.	Si el servicio "Consulta por CUD" retorna una desconexión con Estado = "ACTIVA" y Tipo = "NO PROGRAMADO"
    a. El sistema debe: Tomar el número de evento y consultar el servicio "Consulta por evento".
        i. Cuando se consulta el servicio "Consulta por evento", si el estado de la OT es alguno de los siguientes: "Abierta", "Asignada", "Despachada", "Recibida", "En viaje", "En sitio"

        ii. El sistema debe: Mostrar el modal definido en Figma "Con desconexión ACTIVA - NO PROGRAMADA - ESTADO OT". El modal debe contener la opción: "Entiendo": que al seleccionarla redirigir al usuario al home de la aplicación.

        iii. Si el estado de la OT retornado por el servicio "Consulta por evento" es: "Realizada", "No realizada" El sistema debe permitir al usuario continuar con la funcionalidad de registro de daños.

12.	Si las validaciones son exitosas, se debe presentar precargada la siguiente información, no se debe permitir editar la información:
a. Información de la cuenta, esta información debe estar ofuscada:
    i. Dirección
    ii. Departamento
    iii. Municipio
    iv. Barrio
b. Si las validaciones de la cuenta fueron correctas, se debe mostrar otro elemento de acción para continuar, que me dirija al siguiente paso.

13.	Al continuar, el sistema debe mostrar el Paso 2 con el título: "Datos del solicitante".
    a. Si el usuario es un usuario que no ha iniciado sesión, el sistema debe mostrar un formulario con los siguientes campos obligatorios:
        i. Tipo de persona (Natural / Jurídica), radio buttom con las dos opciones.

        ii. Nombres, campo tipo alfabético con máximo 50 caracteres.

        iii. Apellidos, campo tipo alfabético con máximo 50 caracteres.

        iv. Razón social: Aplica solo para persona jurídica, campo alfanumérico de máximo 50 caracteres.

        v. Tipo de documento, lista desplegable, la lista se obtiene de las listas disponibles en Listas Oficina Virtual ESSA.xlsx

        vi. Documento de identificación, campo numérico con máximo 20 caracteres.

        vii. Departamento expedición, lista desplegable de única selección y con búsqueda predictiva incluida de los departamentos de Colombia.

        viii. Municipio expedición, lista desplegable que solo se habilita al seleccionar un valor en la lista de "Departamento expedición" y muestra solo los municipios del departamento seleccionado. Única selección y buscador predictivo dentro de la lista incluido.

        ix. Celular, campo numérico hasta 10 caracteres.

        x. Teléfono de fijo, campo numérico hasta 10 caracteres.

        xi. Correo electrónico, se debe validar que la cadena ingresada tenga formato de correo, hasta 100 caracteres.

b. Usuario autenticado:
c. El sistema debe precargar los datos del usuario.
d. Si es persona jurídica, debe solicitar todos los campos del numeral 3, sub numeral "a"
    i. Solo se precarga el campo Razón social.

14.	El sistema debe mostrar el Paso 3 con el título: "Información del evento".
15.	El sistema debe mostrar un formulario con los siguientes campos obligatorios:}
    a. Tipo de evento (Daño / Emergencia) - lista desplegable: Obligatorio. Se obtiene del Listas Oficina Virtual ESSA.xlsx Pestaña Daños, se debe presentar al usuario el campo Clasificación.
    b. Descripción del evento: Campo de texto, obligatorio, tipo alfanumérico, permite hasta 900 caracteres.
    c. Permitir cagar un adjunto: Opcional
        i. Se debe validar el formato del adjunto, solo se permiten imágenes en formato: PNG y JPG y archivos en formato: PDF
        ii. El tamaño máximo del adjunto permitido es 9M
    d. Usuario no autenticado: El sistema debe mostrar:
        i. Casilla de aceptación de tratamiento de datos personales: Obligatorio, por defecto no seleccionado.
        ii. Casilla de aceptación de términos y condiciones: Obligatorio, por defecto no seleccionado.
        iii. Marcación de recaptcha: obligatorio, por defecto no seleccionado.
16.	El botón "Enviar reporte" solo se habilita cuando:
    a. Todos los pasos estén completos
    b. Los campos obligatorios estén diligenciados
17.	Al hacer clic en el botón "Enviar reporte", el sistema debe:
    a. Invocar el servicio SAC Controlador Procesos - Método Crear para radicar el daño.
18.	Si la respuesta del servicio es exitosa, el sistema debe:
    a. Obtener el valor del campo IngNumeroProceso de la respuesta.
    b. Posteriormente, el sistema debe:
        i. Consumir el servicio SP7 Consulta por Trouble Ticket, enviando como parámetro el valor de IngNumeroProceso.
        ii. Obtener el valor del campo TT.NumeroEvento de la respuesta.
19.	Si ambas respuestas son exitosas, el sistema debe mostrar al usuario el mensaje: "¡Tu reporte ha sido enviado!. Número de reporte: #XXX. Puedes seguir el estado del reporte desde: Mis reportes, donde XXX es IngNumeroProceso.
    a. El modal tiene las opciones de:
        i. Ver mis reportes: que redirige al usuario a la funcionalidad ESD-46: Buscar un reporte de un daño TO DO.
        ii. Reportar otro daño: que pone al usuario al inicio de esta funcionalidad.
        iii. Salir: que redirige al usuario a home de la aplicación.
20.	El número de cada reporte de daño realizado con éxito por un usuario se debe guardar en base de datos (se debe guardar el numero IngNumeroProceso para consultar posteriormente en otras funcionalidades).
21.	Si ocurre una falla técnica en el consumo de cualquiera de los servicios, el sistema debe mostrar el mensaje: "Ocurrió un error técnico. Por favor, inténtalo nuevamente más tarde."
22.	Las acciones realizadas por el usuario en la pantalla deberán registrar un evento de auditoría - ESD-187.
23.	Los elementos de la pantalla deberán mostrarse conforme al diseño aprobado, con iconos visibles y textos legibles, de acuerdo a la versión web y móvil.



