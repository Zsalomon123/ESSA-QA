# Historia de Usuario 14 Actualizar datos de usuario 

## YO Como
 Usuario que ha iniciado sesión en la oficina virtual

## Sprint
16

## Menú de navegación
inicio sesion


## Criterios de Aceptación

1.	Acceso a la funcionalidad de actualización de datos
    -	Cuando el usuario inicia sesión por primera vez, el sistema debe redirigirlo de forma obligatoria a la pantalla Actualizar datos del usuario, sin permitir el acceso a otras secciones hasta finalizar o cancelar el proceso.
    -	Cuando el usuario ya ha iniciado sesión previamente, el sistema debe permitir el acceso a la funcionalidad mediante la ruta:
    Menú > Mi perfil > Actualizar datos.
    -	El sistema debe mostrar el formulario con los datos actuales del usuario precargados y habilitados para edición, excepto los campos definidos como no editables.
2.	Visualización y edición de información básica
    -	El sistema debe permitir seleccionar el tipo de persona:
        i.	Persona Natural.
        ii.	Persona Jurídica.
    -	Si el usuario selecciona Persona Natural, el sistema debe permitir editar los siguientes campos:
        i.	Nombres: campo obligatorio, tipo alfanumérico y caracteres especiales, longitud máxima de 100 caracteres. No editable.

        ii.	Apellidos: campo obligatorio, tipo alfanumérico y caracteres especiales, longitud máxima de 100 caracteres. No editable.

        iii.	Tipo de documento: campo obligatorio, selección única mediante lista desplegable, cuyos valores deben cargarse desde el Excel adjunto, pestaña General, lista “Tipo de identificación”. No editable.

        iv.	Número de documento: campo obligatorio, tipo alfanumérico, longitud máxima de 15 caracteres. No editable.

        v.	Departamento de expedición del documento: campo obligatorio, selección única mediante lista desplegable, cuyos valores deben cargarse desde el Excel de la URL que se encuentra al final de la HU.

        vi.	Municipio de expedición del documento: campo obligatorio, selección única mediante lista desplegable, cucuyos valores deben cargarse desde el Excel de la URL que se encuentra al final de la HU. Este campo debe permanecer deshabilitado hasta que se seleccione un departamento y solo debe mostrar municipios asociados al departamento seleccionado.

        vii.	Fecha de nacimiento: campo obligatorio, tipo fecha, que permita selección mediante calendario (año, mes y día) y escritura manual respetando el formato DD/MM/AAAA.

        viii.	Género: campo obligatorio, selección única mediante lista desplegable. → Masculino/Femenino/ No binario.

        ix.	Ocupación: campo obligatorio, selección única mediante lista desplegable que se encuentra en el Excel de listas.

        x.	Nivel de escolaridad: campo obligatorio, selección única mediante lista desplegable que se encuentra en el Excel de listas.

        xi.	Estado civil: campo obligatorio, selección única mediante lista desplegable que se encuentra en el Excel de listas.

        xii.	Edad: campo obligatorio, selección única mediante lista desplegable que se encuentra en el Excel de listas.

    -	Si el usuario selecciona Persona Jurídica, el sistema debe permitir editar los siguientes campos:
        i.	Razón social: campo obligatorio, tipo alfanumérico y caracteres especiales, longitud máxima de 100 caracteres. No editable. se toma el nombre que retorna B2C para este campo.

        ii.	Nombre del representante legal: campo obligatorio, tipo alfanumérico, longitud máxima de 100 caracteres. No editable. se toma el apellido que retorna B2C para este campo.

        iii.	Tipo de documento: campo obligatorio, selección única mediante lista desplegable , cuyos valores deben cargarse desde el Excel adjunto, pestaña General, lista “Tipo de identificación”. Debe cargar por defecto NIT. No editable.

        iv.	Número de documento: campo obligatorio, tipo alfanumérico, longitud máxima de 15 caracteres. No editable.

        v.	Tipo de sociedad: campo obligatorio, selección única mediante lista desplegable que se encuentra en el Excel de listas.

        vi.	Sector de la empresa: campo obligatorio, selección única mediante lista desplegable que se encuentra en el Excel de listas.

    -	Se debe presentar el botón Continuar, el cual solo se habilita cuando el usuario haya ingresado los valores obligatorios.

3.	Visualización y edición de dirección y contacto:
    -	Si el usuario selecciona Persona Natural, el sistema debe permitir editar los siguientes campos:
        i.	Departamento de residencia: campo obligatorio, selección única mediante lista desplegable, cargado desde el Excel adjunto, pestaña General, lista “Departamento”.

        ii.	Municipio de residencia: campo obligatorio, selección única mediante lista desplegable, cargado desde el Excel adjunto, pestaña General, lista “Municipio”. Este campo debe habilitarse únicamente tras seleccionar el departamento correspondiente y mostrar solo los municipios asociados.

        iii.	Dirección: campo obligatorio, tipo alfanumérico y caracteres especiales, longitud máxima de 100 caracteres.

        iv.	Barrio: campo obligatorio, Lista desplegable con búsqueda predictiva de única selección, que trabaja en función de la lista de municipios internos de ESSA. Se adjunta el excel.

        v.	Sector solicitante: campo obligatorio, selección única radio buttom → Hogar/empresa/gobierno.

        vi.	Tipo de predio: campo obligatorio, selección única selección única radio buttom → Arriendo / propio.

        vii.	Celular: campo obligatorio, tipo numérico, longitud máxima de 15 caracteres. No editable.

        viii.	Teléfono Fijo: campo opcional, tipo numérico, longitud máxima de 15 caracteres.

        ix.	Correo Electrónico: campo obligatorio, validando que tenga la estructura de un correo, longitud máxima de 100 caracteres. No editable.

        x.	Medio de contacto: Campo obligatorio, selección MULTIPLE selección y debe marcar al menos dos opciones, mediante lista desplegable → los medios de contacto son: Teléfono fijo / Celular / Mensaje de texto / Whatsapp/ Correo Electrónico.
        
    -	Si el usuario selecciona Persona Jurídica, el sistema debe permitir editar los siguientes campos:
        i.	Departamento de residencia: (No editable) campo obligatorio.

        ii.	Municipio de residencia: (No editable) campo obligatorio.

        iii.	Dirección: campo obligatorio, tipo alfanumérico y especiales, longitud máxima de 100 caracteres. No editable.

        iv.	Tipo de predio: campo obligatorio, campo obligatorio, selección única selección única radio buttom → Arriendo / propio.

        v.	Celular: campo obligatorio, tipo numérico, longitud máxima de 15 caracteres. No editable
        vi.	Teléfono Fijo: campo opcional, tipo numérico, longitud máxima de 15 caracteres.

        vii.	Correo Electrónico: campo obligatorio, validando que tenga la estructura de un correo, longitud máxima de 100 caracteres. No editable.

        viii.	Medio de contacto: Campo obligatorio, selección selección MULTIPLE selección y debe marcar al menos dos opciones, mediante lista desplegable → los medios de contacto son: Teléfono fijo / Celular / Mensaje de texto / Whatsapp/ Correo Electrónico.

    -	Cuando es actualización de datos después de iniciar sesión por primera vez, el sistema debe solicitar al usuario la aceptación de:
        i.	Términos y condiciones.
        ii.	Tratamiento de datos personales
        iii.	Autorización de canales de contacto (en este check sobre el hipervínculo se debe embeber el PDF que se adjunta a la HU).
        iv.	Estas autorizaciones deben ser obligatorias para continuar con el proceso y se deben guardar en la BD.
4.	Validaciones del formulario
    -	El sistema debe validar que todos los campos obligatorios estén diligenciados antes de permitir el guardado de la información.
    -	El sistema debe validar el tipo de dato y la longitud máxima permitida para cada campo.
    -	Si existen campos obligatorios sin diligenciar, con información incompleta, el sistema debe resaltar dichos campos y mostrar un mensaje informativo indicando que existen datos pendientes por corregir.
5.	Acciones disponibles en la pantalla
    -	Al seleccionar Guardar cambios, el sistema debe ejecutar todas las validaciones definidas.
    -	Si el proceso de guardado es exitoso, el sistema debe mostrar el mensaje “Acción realizada con éxito”.
    -	Si ocurre un error técnico durante el proceso, el sistema debe mostrar el mensaje “Ocurrió un error técnico. Por favor, inténtalo nuevamente más tarde”.
6.	Auditoría y aspectos visuales
    -	Las acciones realizadas por el usuario en la pantalla deberán registrar un evento de auditoría con la siguiente información mínima: ESD-187
    -	Los elementos de la pantalla deberán mostrarse conforme al diseño aprobado, con íconos visibles y textos legibles, de acuerdo a la versión web y móvil.

Nota:
Los departamento y municipios que se deben parametrizar en la base de datos son: https://www.fopep.gov.co/sheempoo/2019/02/Tabla-Códigos-Dane.pdf 
