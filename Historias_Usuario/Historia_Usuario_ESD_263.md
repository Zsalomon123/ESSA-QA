# Historia de Usuario 263 Visualizar afectaciones de mi cuenta en el mapa

## YO Como
Usuario que ha iniciado sesión en la oficina virtual

## Sprint
16

## Menú de navegación
Danos y PQR/ Danos y emergencias/ Mis reportes


## Criterios de Aceptación
1.	El sistema deberá mostrar dentro de la funcionalidad “Afectaciones a mi cuenta” ( HU ESD-260: Visualizar reportes que afecten mis cuentasTo Do) un elemento de acción que le indique al usuario que puede ver las afectaciones en un mapa.
    a.	Al seleccionar esta opción, el sistema deberá redirigir al usuario a una nueva pantalla de visualización geográfica de afectaciones de la cuenta seleccionada.

    b.	La transición entre pantallas deberá conservar la cuenta previamente seleccionada en la funcionalidad ESD-260.
2.	Al ingresar a la pantalla, el sistema deberá mostrar un mapa junto con un selector de cuentas.
    a.	El selector de cuentas deberá ser de única y obligatoria selección.

    b.	El selector deberá listar únicamente las cuentas asociadas al usuario autenticado y aquellas cuentas tienen afectaciones activas (daños o suspensiones) deben mostrarse disponibles para seleccionar.

    c.	Si el usuario llega desde “Afectaciones a mi cuenta (ESD-260)” con una cuenta previamente seleccionada, esta deberá mostrarse seleccionada por defecto.

    d.	Si no existe una cuenta seleccionada previamente, el sistema deberá seleccionar automáticamente la primera cuenta de la lista asociada al usuario.

3.	Una vez seleccionada la cuenta, el sistema deberá consultar las afectaciones activas relacionadas.
    a.	El sistema deberá identificar si la cuenta presenta:
        i.	Daños o emergencias no programadas.
        ii.	Suspensiones o desconexiones programadas.
        iii.	El sistema únicamente deberá mostrar afectaciones que se encuentren en curso o activas al momento de la consulta.
        iv.	Si la cuenta no presenta afectaciones activas, el sistema deberá mostrar el mensaje: 
            1.	Título: “No encontramos resultados.”
            2.	Te xto: “Tus cuentas no presentan interrupción por reportes de daños ni suspensiones programadas”
            3.	Acción: “Realizar un reporte” elemento de acción que redirige al usuario a la funcionalidad descrita en la HU ESD-44: Reportar un daño a una cuentaIn Progress.
    b.	En caso de no existir afectaciones, la opción de “Ver en mapa” se debe deshabilitar.
4.	Cuando existan afectaciones asociadas a la cuenta, el sistema deberá consultar la ubicación geográfica de la cuenta utilizando el método Clientes/Clientes.
    a.	El sistema deberá obtener:
        i.	Latitud.
        ii.	Longitud.
        iii.	Con la información geográfica obtenida, el sistema deberá posicionar automáticamente el mapa sobre la ubicación de la cuenta.
            1.	El sistema deberá dibujar un área circular con un diámetro de 50 metros alrededor del punto geográfico de la cuenta.
5.	El sistema deberá representar visualmente las afectaciones utilizando diferenciación de colores.
    a.	Si la afectación corresponde a daño o emergencia no programada, el área o marcador deberá mostrarse en color rojo.
    b.	Si la afectación corresponde a suspensión o desconexión programada, el área o marcador deberá mostrarse en color naranja.
    c.	Los colores utilizados deberán mantenerse consistentes en toda la aplicación para el mismo tipo de afectación.
    d.	Si una cuenta presenta al mismo tiempo daños y suspensiones se debe mostrar el círculo de color rojo 
6.	El sistema deberá mostrar una card informativa por cada afectación activa asociada a la cuenta seleccionada.
    a.	Cada card deberá mostrar inicialmente la siguiente información en modo solo lectura:
        i.	Número de reporte: Tipo numérico, longitud máxima 20 caracteres, obligatorio.
        ii.	Fecha y hora de inicio: Tipo fecha y hora en formato DD/MM/AAAA HH:mm, obligatorio.
        iii.	Fecha y hora de finalización: Tipo fecha y hora en formato DD/MM/AAAA HH:mm, obligatorio.
        iv.	Número de evento: Tipo numérico, longitud máxima 20 caracteres, obligatorio.
        v.	Tipo de afectación: Rojo o naranja el círculo según sea la selección realizada.
7.	Cada card deberá incluir un elemento de acción denominado “Ver detalles”.
    a.	Al seleccionar “Ver más”, el sistema deberá expandir la card correspondiente.
    b.	La expansión deberá mostrar información adicional relacionada con la afectación:
    c.	Observaciones: Tipo alfanumérico con caracteres especiales, longitud máxima 500 caracteres.
    d.	Causa: Tipo alfanumérico con caracteres especiales, longitud máxima 300 caracteres.
    e.	El sistema deberá permitir únicamente una card expandida simultáneamente.
    f.	Si el usuario selecciona “Ver detalles” sobre otra afectación mientras existe una card abierta, el sistema deberá cerrar automáticamente la card previamente expandida y abrir la nueva seleccionada.
    g.	La expansión y contracción de cards deberá ejecutarse sin recargar la pantalla.
8.	Cuando el usuario cambie la cuenta seleccionada en el desplegable, el sistema deberá:
    a.	Limpiar las afectaciones previamente mostradas.
    b.	Consultar las nuevas afectaciones asociadas a la cuenta seleccionada.
    c.	Actualizar el mapa con la nueva ubicación geográfica.
    d.	Dibujar nuevamente las zonas afectadas correspondientes.
    e.	Actualizar las cards de información asociadas.
9.	En caso de error técnico durante las consultas de afectaciones o ubicación geográfica, el sistema deberá mostrar el mensaje:
9.1. “No fue posible consultar las afectaciones de la cuenta seleccionada. Por favor, intenta nuevamente.”
9.2. El sistema deberá permitir reintentar la consulta sin necesidad de salir de la pantalla.
10.	Las acciones realizadas por el usuario en la pantalla se deberán registrar un evento de auditoría ESD-187.
11.	Los elementos de la pantalla deberán mostrarse correctamente alineados, con íconos visibles y textos legibles, de acuerdo con el diseño entregado para la versión web y móvil.
Servicios: 
    1.	Controlador Clientes/clientes
    2.	servicios implementado en la HU ESD-260
Diseños:
Ingreso: ESSA - Oficina Virtual Diseño UX_UI Vigente 
Funcionalidad: ESSA - Oficina Virtual Diseño UX_UI Vigente 

