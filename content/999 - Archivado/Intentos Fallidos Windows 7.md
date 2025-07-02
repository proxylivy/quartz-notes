

# Crear Windows Limpio
> [!TIP] Lecturas Recomendadas
> - [Massgrave - Windows 7 Download](https://massgrave.dev/windows_7_links)


> [!TIP] Sobre espacio
> NTLite necesita 10GB libres para poder trabajar, si quieres utilizar el VM para trabajar, necesitas extender el disco vdi de 32GB a 60GB :P y luego extender el volumen simple

Se utilizara Windows 7 Ultimate SP1 x64 ES-es 677350 (Build 7601.17514)

Bueno, se puede descargar en el mismo windows 7 jijiji, que malulo

Y con el pobre NTLite Pirata para probar si es que funciona o si es fake

Debes extraer la imagen iso ("`es_windows_7_ultimate_with_sp1_x64_dvd_u_677350.iso`") con 7zip, para poder modificar el contenido y luego reempaquetar

Abres NTLite y seleccionas la carpeta del disco montado

Elimina las versiones
- Windows 7 Home Basic
- Windows 7 Home Premium
- Windows 7 Professional

Eliges cargar la version "Windows 7 Ultimate" para trabajar sobre ella, se demora un poco, debe salir un boton verde y ninguna barra cargando y saldra una barra lateral de menu, aqui pondre mi configuracion...

Para las actualizaciones, debes descargar el paquete de simplix, descargar e instalar en el host, para que del 1.5MB, descargue todas las demas actualizaciones, resulta en un paquete de ~800MB (Demora su buen rato), la version que descargue es la "`UpdatePack7R2-25.6.10`"

Inicio
- Fuente
	- Borra Windows 7 Home
	- Borra Windows 7 Home Premium
	- Borra Windows 7 Professional
	- Carga "Windows 7 Ultimate"
Eliminar
- Componentes
	- Selecciona el boton de "Compatibilidad"
		- Quita "Discord"
		- Quita "Imprimir"
		- Quita "Microsoft Office"
		- Quita "Nvidia Driver setup Installer"
		- Quita "Reproductor de Video"
		- Agrega "Virtual Box VM"
- Multimedia (Ahora selecciona desde el menu)
	- API de voz
	- Consejos (Al iniciar)
	- Creador de DVD
	- Explorer de juegos
	- Muestras Multimedia
	- Temas de mercado
- Red
	- Internet Explorer
- Remoto y privacidad
	- Control Parental
	- Servicios biometricos de Windows
- Sistema
	- Barra lateral de windows
	- Cache y archivos temporales
	- PC Movil
	- Transferencia Facil
	- Windows Defender
- Soporte de Hardware
	- Imprimir
Configurar
- Caracteristicas
	- Deselecciona "Plataforma de gadgets de Windows"
	- Deselecciona "Juegos"
	- Deselecciona "Internet Explorer 8"
	- Deselecciona "Caracteristicas Multimedia"
	- Deselecciona "Servicios XPS"
	- Deselecciona "Compresion diferencial remota"
	- Deselecciona "Componentes de Tablet PC"
	- Deselecciona "Visor de XPS"
Integrar
- Actualizaciones
- Controladores
	- Selecciona "Importar Host (anfitrion)" (O talvez no...)

Ahora le damos en "Aplicar"

