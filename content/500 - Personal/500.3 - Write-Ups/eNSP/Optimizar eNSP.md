# Info
## Datos
> [!TIP] Lecturas Recomendadas
> - [Proxmox Docs - Windows 7 Guest Best Practices](https://pve.proxmox.com/wiki/Windows_7_guest_best_practices)
> - [Huawei Forums - Network Simulation Tools 2022 Challenge](https://forum.huawei.com/enterprise/intl/en/collection/667213828401807360?mod=collection&action=view&ctid=699&themeId=667213828401807360)
> 	- [Youtube - Rundown of basic eNSP tools](https://youtu.be/NyIdfdIOyEM?si=8nwUta3FknINy8KV)
> - [Oracle Virtualbox - Docs Index](https://docs.oracle.com/en/virtualization/virtualbox/index.html)

El objetivo de este Write-Up es contruir una version menos mala de eNSP. Este esta siendo ejecutado sobre Windows 7 Professional 64 bits, mi idea es hacer que funcione lo mejor posible, pero tu sabes, Windows es una muy mala plataforma...

- Usuario: RTH010V
- Contraseña: 12345

**Tamaños Finales**

- Exportacion en .OVA
	- OVA Original: 11.3GB
	- OVA 2.0 Modificado v1: 13.9GB
	- OVA 2.0 Modificado v2: 20.9GB
- VDI
	- VDI Original: 25.4GB
	- VDI Modificado v1: 25.0GB
	- VDI Modificado v2: 32.0GB

**Mejoras**

- Windows Activado Permanentemente gracias a Massgrave y TSForge
- Version de Professional a Ultimate con soporte ESU
- Actualizaciones y Parches a 2020
- Instalado Virtualbox Guest Additions `7.1.10`
- Instalado Drivers Virtio `0.1.173-4` (Excepto Spice Agent)
- eNSP actualizado a `1.3.00.100` (`V100R003C00SPC100`)
- Imagenes extras importadas, como Firewall USG6000V (vfw_usg.vdi)
- Virtualbox Actualizado a `5.2.44`
- Avisos y alertas de inseguridad desactivadas
- Permisos de administrador tanto para eNSP como Virtualbox
- Inicio Automatico sin contraseña con `netplwiz` (Pass: 12345)
- Chequeos de Windows Defender desactivado
- Limpieza de archivos temporales con `cleanmgr`
- Desfragmentacion Automatica desactivada
- Desfragmentar y ordenar bytes con `cipher` para una exportacion mas liviana
- VDI Extendido de 32GB a 40GB

# Instalacion
## Windows 7
> [!TIP] Lecturas Recomendadas
> - [Massgrave Official Page](https://massgrave.dev/)
> 	- [Changelog](https://massgrave.dev/changelog)
> 	- [News](https://massgrave.dev/news)

**Cambiar de Professional a Ultimate**
> [!TIP] Lecturas Recomendadas
> - [Massgrave - Metodo 2](https://massgrave.dev/#method-2---traditional-windows-vista-and-later)
> - [Github - Massgravel/MAS-Scripts/archive/refs/heads/master.zip (Autodescarga)](https://github.com/massgravel/Microsoft-Activation-Scripts/archive/refs/heads/master.zip)
> - [Gitea Instance (activated win) - Massgrave/MAS-Scripts/archive/master.zip (Autodescarga)](https://git.activated.win/massgrave/Microsoft-Activation-Scripts/archive/master.zip)

> [!NOTE] Tiempo
> Este proceso se demora ~3.5 Minutos y luego se reinicia 2 veces

Utilizando Massgrave con el metodo 2 (Tradicional), en el cual debes descargar directamente desde github un .zip, luego lo descomprimes, abres las carpetas hasta llegar a "All-In-One-Version" y ejecutas "MAS_AIO.cmd" con permisos de adminsitrador

1. Selecciona `[7]` "Change Windows Edition"
2. Seleccionamos la version, `[1]` "Ultimate" y presionamos "Enter"
3. Cerramos todas las paginas extras, y presionamos `[1]` "Continue" y el proceso empezara

**Activar Licencia**
> [!TIP] Lecturas Recomendadas
> - [Github Massgravel/Microsoft-Activation-Scripts](https://github.com/massgravel/Microsoft-Activation-Scripts)
> - [Massgrave - TSforge](https://massgrave.dev/tsforge)
> - [Massgrave Blog - TSforge](https://massgrave.dev/blog/tsforge)

> [!WARNING] Limitaciones
> - Deberas utilizar el Metodo 2 (Tradicional), descargando el repo, y ejecutarlo a mano
> - Windows 7, solo puede utilizar TSforge `[3]` para poder ser activado

Deberas descargar el archivo, descomprimirlo, ejecutar el script como administrador, luego apretar `[3]` para seleccionar "TSforge" y luego `[1]` para seleccionar solamente Windows

**Actualiza el sistema** (WIP)

> [!CAUTION] Demora
> Ni idea que lo provoca pero se demora un buen rato en empezar a buscar, y no avisa cuando termina, asi que miralo como es que va

Windows 7 permite actualizar el sistema, asi que ve a "Panel de Control" > "Sistema y seguridad" > "Windows Update" y dale en "Activar Actualizaciones", luego selecciona 

Puedes Ocultar todas las actualizaciones de "Windows 7 Language Pack" para que no molesten, a lo menos que quieras algun cambio de idioma

Dice que funcionaron 16 y fallaron 4 actualizaciones, Reincia el equipo e intenta otra vez

Ve al mismo menu de Windows Update, te dira que hay una actualizacion pendiente, asi que la instalas, se demora poco

Las actualizaciones son??
(TO-DO) Anotar las actualizaciones pendientes exactas

- KB4019990
- KB4040980
- KB971033
- KB2603229
- KB3021917
- KB3068708
- KB3080149
- KB3133977
- KB3172605
- KB3179573
- KB3184143

- KB4474419
- KB4054518

- KB2533552
- KB4490628
- KB4503575

Luego se instalan otras actualizaciones

Importantes
- KB4516065
- KB

Opcionales
- KB

## eNSP
> [!TIP] Lecturas Recomendadas
> - [Huawei Forums - Download eNSP simulator installation Software](https://forum.huawei.com/enterprise/intl/en/thread/download-ensp-simulator-installation-software-here/667238396713648128?from=latestPostsReplies&blogId=667238396713648128)
> - [Huawei Forums - Resource Downloading for eNSP](https://forum.huawei.com/enterprise/intl/en/thread/resources-downloading-for-ensp/667245683301826561?blogId=667245683301826561)
> - [Youtube - Dark Bird Tech](https://www.youtube.com/@darkbirdtech)
> 	- [Youtube - Install eNSP on Windows 10 in 2025](https://youtu.be/Zx4A_Vu22Q8?si=RNh-wOPUVLe2hnci)
> 	- [Youtube - Instaling eNSP on Windows 11 in 2025](https://youtu.be/XtqjnSyGSMA?si=l0nn4pEE6uQrR4Az)
> 	- [Youtube - Running eNSP on any OS - Guaranteed to work](https://youtu.be/Fk2zISws4yA?si=w1ru2yyXcrjJD7Iy)
> 	- [Youtube - eNSP Troubleshooting](https://youtu.be/Kecte0rp9XI?si=ajXp2BpGDh3ugCEx)

Para el correcto funcionamiento de Huawei eNSP, necesitas una maquina Guest Microsoft Windows, encuentro que para aislar en un VM, el punto exacto, es Windows 7, aprovechando la maquina VM de vitoco para mejorar, este agujero negro, no va a nada mas que empeorar

La version eNSP 1.2.00.510 instala las siguientes versiones
- Virtualbox 5.1.24
- WinPcap 4.1.3
- Wireshark 1.4.3

1. Instala eNSP Setup 1.2.00.510 (Ya hecho en la maquina base)
	- Selecciona "English" y luego seleccionas el boton de la izquierda
	- Luego seleccionas "Next"
	- Lees el acuerdo, lo aceptas y apretas "Next"
	- Dejas la ruta por defecto "`C:\Program Files\Huawei\eNSP`" y apretas "Next"
	- Apretas "Next" y luego "Next"
	- Ahora Instalara WinPcap, Wireshark y Virtualbox, le das en "Next", descomprimira y te aparecera un popup por cada programa
		- WinPcap
			- Simplemente "Next" con las opciones por defecto hasta que se termine de instalar
		- Wireshark
			- Simplemente "Next" con las opciones por defecto hasta que termine de instalar
		- Virtualbox
			- Simplemente "Next" con las opciones por defecto hasta que termine de instalar
			- Recuerda dar que "Si" a la interfaz de red, y confiar siempre en certificados de "Oracle Corporation"
			- No inicies Virtualbox luego de instalar
	- Recuerda deseleccionar las opciones "Launch eNSP" y "show update log" para que no inicie y le das en "Finish"

2. Instala eNSP Setup 1.3.00.100
	- Empieza la instalacion, "Next", recuerda leer, aceptar la licencia y apretar "Next"
	- Dejas la misma ruta por defecto "`C:\Program Files\Huawei\eNSP`" y apretas "Next"
	- "Next" y "Next"
	- Ahora hara una verificacion de las versiones exactas instaladas por la anterior version, los tres items deberian decir "`It is detected that {program} has been installed on your computer`" y das en "Next"
	- Verificas los datos de instalacion y das en "Install"
	- Luego de instalar, puedes mantener "Launch eNSP" para revisar el estado

---
## Aplicaciones

**WinCDEmu**

> [!TIP] Lecturas Recomendadas
> - [Github - sysprogs/WinCDEmu](https://github.com/sysprogs/WinCDEmu)

Es una herramienta ligera para montar discos, nos ayudara para hacer la instalacion de Virtualbox Guest y los drivers de Virtio

**7zip**

> [!TIP] Lecturas Recomendadas
> - [7-ZIP - Official Page](https://www.7-zip.org/)

La instalacion es simplemente ejecutar e instalar

Para configurar 7zip
1. Ve al menu de inicio, luego a todos los programas y abre "7-Zip File Manager" en modo Adminitrador
2. Abre el Menu "Herramientas" > "Opciones"
	- Sistemas
		- Apretas el boton "+" para el usuario RTH010V, y deseleccionas los que estan ocupados como "zip", "cab" o "iso", entre otros
		- Luego seleccionas los que tienen espacios libres en el espacio de "Todos los usuarios", si te equivocas, das clicks hasta que aparesca la herramienta que ya estaba siendo usada
	- 7-Zip
		- Deseleccionas "`Agregar a <Archivo Comprimido>.zip`"
		- Deseleccionas "`Comprimir y enviar por correo electronico`"
		- Deseleccionas "`Comprimir a <Archivo Comprimido>.7z y enviar por correo electronico`"
		- Deseleccionas "`Comprimir a <Archivo Comprimido>.zip y enviar por correo electronico`"
		- Deseleccionas "`CRC > CRC SHA >`"
		- Deseleccionas "`7-Zip > CRC SHA >`"
3. Le das en "Aplicar" y luego en "Aceptar" y esta listo

**Virtualbox**

> [!TIP] Lecturas Recomendadas
> - [Download Virtualbox - Old Builds 5.2.x](https://www.virtualbox.org/wiki/Download_Old_Builds_5_2)

> [!CAUTION] Licencias
> Virtualbox es un programacion con licencia GPLv2, pero algunas herramientas

El limite de eNSP, esta en la rama 5.2.x de virtualbox, siendo la ultima con soporte, la `5.2.44-139111`

1. Ejecutas el instalador
2. Le das en "Next" para comenzar la instalacion
3. Le das en "Next" hasta que termine la instalacion


**Wireshark**

> [!TIP] Lecturas Recomendadas
> - [Wireshark - Develpment/LifeCycle](https://wiki.wireshark.org/Development/LifeCycle)

Depende de la rama de compatibilidad con eNSP, la cual todavia no puedo probar...

Esto es aun experimental
1. Prueba la version 3.6.16 que es compatible con la rama 3.x que viene con el sistema
2. Prueba la version 4.2.2 que es la mas actual
3. En caso de fallar la anterior, prueba la 4.0.17

Al instalar Wireshark 4.2.2, instala Npcap 1.78, lo que desinstala la vieja version 1.31, ademas de que permite instalar USBcap 1.5.4.0

- Npcap | [x64](https://npcap.com/), la cual se puede instalar con una API compatible con WinPcap

**VLC**

> [!TIP] Lecturas recomendadas
> - [Videlan - VLC](https://www.videolan.org/vlc/)

Le das click en Descargar VLC (Yo de mañoso me aseguro de descargar Windows 64 bits), al momento de escribir, todavia tiene soporte completo para Windows 7 y utilizo la version `3.0.21`

1. Inicias la instalacion en Modo Administrador
2. Seleccionas el idioma Español y le das en "Siguiente"
3. Das en "Siguiente"
4. Lees la licencia y pulsas "Siguiente"
5. Ves los componentes que instalaras y le das en "Siguiente"
6. Dejas el directorio por defecto y le das en "Instalar"
7. Cuando termine de instalar, le das en "Terminar" y verificas la instalacion

**K-Lite Codecs**

> [!TIP] Lecturas recomendadas
> - [K-Lite Codecs Pack Download](https://www.codecguide.com/download_kl.htm)

El clasico que siempre funciona, todavia soporta Win 7, yo seleccione la version Standard `19.0.5`

1. Deja seleccionado "Normal" y dale en "Next"
2. Selecciona en ambos "VLC Media Player" y deselecciona "Install MPC-HC as a secondary player"
3. En "Additional Shorcuts" Deselecciona todos
4. En "System Tray Icons" Deselecciona todos
5. En "Miscellaneous" Deselecciona "Check for updates"
6. En "Information" Deselecciona "Show configuration tips"
7. Dale en "Next"
8. Debes seleccionar tus idiomas primarios para el lenguaje y subtitulos, mi lenguaje primario es "Spanish" y el segundario es "English" y le das en "Next"
9. Le das en "Next"

**Irfanview**

> [!TIP] Lecturas Recomendadas
> - [IrfanView - Official Page](https://www.irfanview.com/)

Ver imagenes a la velocidad del rayo

1. Descargas la version para Windows 64 Bits, descargando la primera opcion "Download IrfanView Windows Installer"
2. Lo ejecutas en modo normal, y seleccionas instalar "For all users" y le das "Siguiente"
3. Lees el changelog, y le das "Siguiente"
4. Le das click en "Try to change associations in System-Registry" y seleccionas en la parte de abajo "Images Only" y le das "Siguiente"
5. Le dejas la opcion de instalacion por defecto y le das en "Siguiente"
6. Deseleccionas ambas opciones y le das en "Done"

## Para-Virtualizacion

**Virtualbox Guest Additions**

> [!TIP] Lecturas Recomendadas
> - [Virtualbox - Download Center](https://download.virtualbox.org/virtualbox/)
> - [Whirpool Archive - Virtualbox: Guest Additions hangs/freezes install](https://forums.whirlpool.net.au/archive/35pnn7yj)
> - [Virtualbox User Manual - Guest Additions for Windows](https://www.virtualbox.org/manual/UserManual.html#additions-windows)
> - [Virtualbox Download Old Build - 5.2.x](https://www.virtualbox.org/wiki/Download_Old_Builds_5_2)
> - [Superuser Forum - How to select paravirtualization interface in Virtualbox](https://superuser.com/questions/945910/how-to-select-paravirtualization-interface-in-virtualbox)
>	- [Virtualbox Docs - Manual #10.5. Paravirtualization Providers](https://www.virtualbox.org/manual/ch10.html#gimproviders)
> - [SuperUser Forum - What are difference between VBoxVGA, VMSVGA and VBoxSVGA](https://superuser.com/questions/1403123/what-are-differences-between-vboxvga-vmsvga-and-vboxsvga-in-virtualbox)

> [!CAUTION] Montar Disco
> Windows 7 no tiene la funcionalidad para montar discos, te recomiendo instalar temporalmente desde [Github - sysprogs/WinCDEmu](https://github.com/sysprogs/WinCDEmu), funciona de muy buena manera

1. Debes buscar la misma version que tienes instalada en el Hosts, en mi caso "`7.1.10`", luego buscas "`VBoxGuestAdditions_{version}.iso`"
2. Con WinCDEmu instalado, puedes montar el disco .iso e instalar automaticamente los drivers
3. Reinicia la maquina para aplicar los cambios

**Qemu/KVM Virtio Drivers**

> [!TIP] Lecturas Recomendadas
> - [Github virtio-win/virtio-win - Issue #40 - Windows 7 no more working](https://github.com/virtio-win/virtio-win-pkg-scripts/issues/40)
>	- [Issue #40 - AdityaKKhullar Comment](https://github.com/virtio-win/virtio-win-pkg-scripts/issues/40#issuecomment-1704103962)
> - [Fedora - Virtio Download](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/?C=M;O=D) | [0.1.173-4](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/virtio-win-0.1.173-4/)

La ultima version con soporte de instalador automatico es la version `0.1.173-4` (20 de Enero de 2020) y debes deseleccionar `Spice Agent` cuando lo instales, de caso contrario, fallara

## Imagenes Extras
> [!TIP] Lecturas recomendadas
> - [Huawei Forums - Resource Downloading for eNSP](https://forum.huawei.com/enterprise/intl/en/thread/resources-downloading-for-ensp/667245683301826561?blogId=667245683301826561)
> - [Huawei Forums - Download eNSP USG6000v Image](https://forum.huawei.com/enterprise/intl/en/thread/download-ensp-usg6000v-image/667245289389572096?blogId=667245289389572096)
> - [Huawei Forums - Download eNSP NE40E Image](https://forum.huawei.com/enterprise/intl/en/thread/ne40e-image-for-ensp-v100r003c00spc100/667245683289243648?blogId=667245683289243648)
> - [Youtube - Dark Bird Tech](https://www.youtube.com/@darkbirdtech)
> 	- [Youtube - Adding the missing devices in Huawei eNSP v1.3](https://youtu.be/WH7xrq8Mqx8?si=nJfUunV92hITMcIZ)
> 		- [Google Drive - eNSP Optional Extra Devices](https://drive.google.com/drive/folders/1xhb71AzTOcy_IvVnjFi0rEme6iGZJ7uy) | [Mirror - Onedrive](https://duoccl0-my.sharepoint.com/:f:/g/personal/ga_zunigam_duocuc_cl/EjgfGZjaSOxGvwhi6Z0C7scBNLstbk1iFPxq23X1N5B0IA?e=XPUmZI)

1. Crea la carpeta necesaria en "`C:\Archivos de programa\Huawei\`" y que se llame "`Extra-Images`"
2. Luego debes descargar los archivos comprimidos, Descomprimir, y mover las imagenes y las dejas en carpetas que puedas identificar dentro de "`Extra-Images`"
3. Abre eNSP, arrastra el nodo que quieras instalar, y lo inicias, te saldra una ventana pidiendo un archivo, debes cargar los discos basados en los siguiente
	- Firewall USG6000V (usar vfw_usg.vdi)
	- Router NE40E (usar NE40E.img)
	- Router NE5000E (usar NE5000E.img)
	- Router NE9000 (usar NE9000.img)
	- Router CX200 (usar CX.img)
	- Switch CE6800 (usar CE.img)

Luego debes dar segundo click y apretar en "Start", te saldra una ventana llamada "Import Package", pidiendote la ruta y el paquete que necesitas

Y le das la ruta `C:\Program Files\Huawei\Extra-Images\`, la imagen correspondiente y le das en "Import"

Ahora le das el segundo click, "Start", ahora la imagen deberia empezar a cargar, puede demorarse de 3-5 minutos en recien iniciar, asi que paciencia

> [!Warning] Contraseña
> El Firewall es el unico que tiene contraseña y que debes cambiar luego de iniciar sesion, yo elegi `Wena@123`
> - User: `Admin`
> - Pass: `Admin@123`

---
# Configuracion

**Extender Disco de 31 a 40GB**

Debes tener la maquina apagada, vas al menu de Herramientas y seleccionas "Medio", luego seleccionas el disco .vdi. en este caso "`Windows7-ENSP-Creativo-disk001.vdi`" y extiendes de 31.0GB a 40.0GB y le das en "Aplicar"

Enciendes la maquina y ve al menu de inicio y abre el "Panel de Control"
1. Ve a "Sistema y seguridad" > "Crear y formatear particiones de disco duro"
2. Seleccionas el Disco "`C:/`", Click Derecho y seleccionas "Extender Volumen"
3. Haces click en "Siguiente", "Siguiente" y "Siguiente" y ahora el espacio libre debe sumarse al disco, dando como resultado `39.90GB NTFS`

**Cambia el modo de energia**

Ve al menu de inicio y abre el "Panel de Control"
1. Ve a "Hardware de Sonido"
2. Ve a "Opciones de Energia"
3. Abre los "Planes Adicionales" y Elije "Alto Rendimiento"

**Configurar Fuentes**

> [!TIP] Lecturas Recomendadas
> - [NerdFonts - Official Page](https://www.nerdfonts.com/)
> - [Github - ryanoasis/nerd-fonts](https://github.com/ryanoasis/nerd-fonts)

1. Debes descargar la fuente que mas te guste, yo recomiendo cualquiera de Nerd Fonts, en mi caso, Hack.ttf
2. Luego de descargar, descomprime el archivo, ve a la carpeta, abre las fuentes Regular como `HackNerdFontMono-Regular.ttf` y `HackNerdFont-Regular`, y luego dale en "Instalar"
3. Abre eNSP, ve al Icono de Configuracion, luego ve a "Font", dentro del menu "CLI Font", selecciona una fuente
4. Ahora busca y selecciona "Hack Nerd Font" y con tamaño de letra "11"

**Da permisos de Administrador**

Ve a las propiedades tanto de "Virtualbox" como "eNSP"
1. Desde la ventana de "Acceso directo", ve a "Opciones Avanzadas" y selecciona "Ejecutar como Administrador"
2. Desde la ventana de "Compatibilidad", ve a "Cambiar la configuracion para todos los usuarios", y en "Nivel de privilegio" selecciona "Ejecutar este programa como administrador" y dale en "Aceptar"

**Desactiva Windows Defender**

Ve a la barra de busqueda, busca y ejecuta "Windows Defender" y ve a "Opciones"
1. En "Examen Automatico" desactiva "Examinar Equipo Automaticamente"
2. Ve a "Proteccion en tiempo real" y desactiva "Usar proteccion en tiempo real"
3. Ve a "Avanzada" y desactiva todas las opciones
4. Ve a "Administrador" y desactiva "Usar este programa"

Dale en "Guardar" y ahora tu equipo estara inseguro :D

**Desactiva mensajes de Alerta**

Ve a la barra de busqueda, busca y ejecuta "Centro de Actividades" y le das en "Descartar" a todos los mensajes

**Activa Auto-Login**

Ve a la barra de busqueda, busca y ejecuta "netplwiz" y desactivas "Los usuarios deben escribir su nombre y contraseña para usar el equipo", das en "Aceptar", ingresas la contraseña (`12345`) 2 veces

**Desactiva Animaciones, ni proteccion, ni debug**

1. Abre el menu de inicio, dale click derecho a "Equipo" y abre las propiedades, ahora ve a "Configuracion avanzada del sistema"
2. En la pestaña de "Opciones Avanzadas", apreta la Configuracion de la seccion de "Rendimiento"
3. En "Efectos Visuales", dale en "Personalizar", y solo deja activo "Mostrar vistas en miniaturas en lugar de iconos" y "Suavisar bordes para la fuentes de pantalla", "Usar estilos visuales en ventanas y botones" y dale en "Aceptar"
4. En la misma pestaña de "Opciones Avanzadas", en la seccion de "Inicio y Recuperacion" selecciona "Configuracion", desactiva todas las opciones y dale en "Aceptar"
5. Ve a "Proteccion del sistema", seleccionas "Configurar..." y le das a la 3ra opcion "Desactivar proteccion del sistema", luego en "Eliminar" y al final en "Aceptar"

Le das en "Aceptar" y ya tienes un sistema mas responsivo

**Configurar Firefox**

> [!TIP] Lecturas Recomendadas
> - [End Of Life Dates - Firefox](https://endoflife.date/firefox)
> - [Mozilla What train is it now - Firefox ESR Release](https://whattrainisitnow.com/release/?version=esr)
> - [Mozilla Firefox - 115.25.0 Release Notes](https://www.mozilla.org/en-US/firefox/115.25.0/releasenotes/)
> - [Mozilla Releases FTP - Firefox ESR 115.25.0](https://releases.mozilla.org/pub/firefox/releases/115.25.0esr/)

Abre Firefox, Ve a "Ajustes", luego en "General" baja hasta "Actualizaciones de Firefox" y selecciona "Reiniciar para actualizar Firefox", esto instalara la ultima version disponible.

Es increible pero al momento de escribir este write-up, Mozilla continua soportando Windows 7, mediante la rama 115.x, el ultimo lanzamiento es "115.25.0" (24 de Junio de 2025)

Configura la interfaz
- Haz click derecho en cada marcador de la barra de marcadores, y selecciona "eliminar marcador" o "Remover marcador", de esa forma quedara limpia

Selecciona las tres lineas horizontales de la parte derecha, y ve a "Ajustes"
- General
	- Aparencia del sitio web
		- Obviamente modo oscuro
	- Tipografia
		- Fuente predeterminada: `Hack Nerd Font`
		- Tamaño: `13`
		- ve a "Menu Avanzado"
			- Proporcional: `Sans-Serif`
			- Sans-serif: `Hack Nerd Font`
			- Monospace: `Hack Nerd Font Mono`
			- Tamaño de fuente minimo: `9`
	- Idioma
		- Desactiva "Revisar ortografia al escribir"
	- Navegacion
		- Desactiva "Activar controles de video picture-in-picture"
		- Desactiva "Recomendar extensiones mientras se navega"
		- Desactiva "Recomendar funciones mientras navegas"
- Inicio
	- Contenido de Inicio de Firefox
		- Desactiva "Atajos"
- Buscar
	- Motor de busqueda predeterminado
		- Elige DuckDuckGo
	- Sugerencia de busqueda
		- Desactiva "Proveer sugerencia de busqueda"
	- Atajos de busqueda
		- Desactiva "Google", "Bing", "MercadoLibre Chile", "Wikipedia (es)"
- Privacidad y seguridad
	- Privacidad del navegador
		- Selecciona "Estricta"
	- Envia a los sitios web una señal "No rastrear" para que sepan que no quieras ser rastreado
		- Siempre
	- Cookies y datos de sitio
		- Selecciona "Limpiar datos" y luego en "Limpiar" y acepta con "Limpiar ahora"
		- Selecciona "Eliminar cookies y datos de sitio cuando Firefox sea cerrado"
	- Credenciales y contraseñas
		- Desactiva "Preguntar si guardar credenciales y contraseñas webs"
	- Historial
		- Firefox: cambiar a "`Nunca recordara el historial`" (Reinicias Firefox Ahora y vuelves a este mismo punto)
	- Barra de direcciones
		- Desactiva todos los items
- Extensiones y temas
	- Extensiones
		- Ve a la barra y busca `Ublock Origin` ([Ublock Origin](https://ublockorigin.com/)) e instalalo, permite su funcionamiento en ventanas privadas
			- Puedes instalar [Github - fmyh/FMYHFilterlist](https://github.com/fmhy/FMHYFilterlist)
	- Temas
		- Selecciona tema "Oscuro", si tiene errores, puedes probar alguno como "[Camarada - Black](https://addons.mozilla.org/es-CL/firefox/addon/2black/)" o "[Catppuccin Mocha - Lavender](https://addons.mozilla.org/es-CL/firefox/addon/catppuccin-mocha-lavender-git/)"

**Personaliza tema CSS de Firefox**

> [!TIP] Lecturas Recomendadas
>  - [Youssuff Quips - Useful Customizations for Firefox](https://www.quippd.com/firefox/wiki/useful-customizations/)
>  - [Github - yokoffin/Betterfox - Fastfox.js blob](https://github.com/yokoffing/Betterfox/blob/main/Fastfox.js)
>  - [Arkenfox GUI](https://arkenfox.github.io/gui/)

Para cambiar el tema de Firefox, debes encontrar uno acorde, recomiendo buscar en "[Reddit - r/firefoxCSS](https://old.reddit.com/r/FirefoxCSS/)"

> Debes ir a
```
C:\Users\RTH010V\AppData\Roaming\Mozilla\Firefox\Profiles
```

1. Ahora creas la carpeta chrome dentro del directorio "XXX.default-release" (El mas pesado)
2. Y mueves los archivos "`userChrome`" y "`userContent`" dentro de la carpeta creada de `chrome`

**Modifica Flags**

> [!TIP] Lecturas Recomendadas
> - [Reddit - r/firefox - What are Your Must Have Changes in about:config? - 001Guy001 Comment](https://old.reddit.com/r/firefox/comments/17hlkhp/what_are_your_must_have_changes_in_aboutconfig/k6ogtuv/)

> Para permitir modificar el tema CSS, configura a `True`
- `toolkit.legacyUserProfileCustomizations.stylesheets`
- `layers.acceleration.force-enabled`
- `gfx.webrender.all`
- `svg.context-properties.content.enabled`

> Tamaño minimo de cada pestaña
- `browser.tabs.tabMinWidth` a `50`

> Desactiva Firefox Screenshot
- `extensions.screenshots.disabled` to `true`

> Desactiva autorellenado, configura a `False`)
- `browser.formfill.enable`
- `browser.search.suggest.enabled`
- `extensions.formautofill.addresses.enabled`
- `extensions.formautofill.creditCards.enabled`

> Usar mas Cache en Red
- `network.buffer.cache.size` to `524288` -> 512KB
- `network.buffer.cache.count` to `128`
- `network.http.max-connections` to `1800`
- `network.http.max-persistent-connections-per-server` to `12`
- `network.http.max-urgent-start-excessive-connections-per-host` to `8`
- `network.http.pacing.requests.min-parallelism` to `8`
- `network.websocket.max-connections` to `400`
- `network.ssl_tokens_cache_capacity` to `32768`

> Mover a pantalla completa mas rapido
- `full-screen-api.warning.timeout` to `20`
- `full-screen-api.warning.delay` to `20`
- `full-screen-api.transition.timeout` to `10`

> Acepta mas formatos y no solo webp (Parece estar ~~deprecated~~)
- `image.http.accept` a `*/*`

> Te avisa si cierras firefox con pestañas abiertas
- `browser.tabs.warnOnClose` a `true`

> Desactiva la pantalla de actualizacion despues de una actualizar
- `browser.startup.upgradeDialog.enabled` a `false`

> Desactiva el POP-UP automatico cuando una descarga termina
- `browser.download.alwaysOpenPanel` a `false`

> Desplazamiento del raton mas rapido
- `mousewheel.default.delta_multiplier_y` a `200`

> Fisicas suaves nuevas
- `general.smoothScroll.msdPhysics.enabled` a `true`

> Abre un marcador en una pestaña nueva
- `browser.tabs.loadBookmarksInTabs` a `true`

> Consume mas RAM, menos HDD (Funciona en maquinas mas antiguas)
- `browser.cache.disk.enable` a `false`
- `browser.cache.memory.capacity` a `-1`
- `media.memory_cache_max_size` a un numero mas grande

> Elimina anuncios
- `browser.vpn_promo.enabled` to `false`
- `browser.newtabpage.activity-stream.feeds.recommendationprovider` to `false`
- `browser.newtabpage.activity-stream.showSponsored` to `false`
- `extensions.htmlaboutaddons.recommendations.enabled` to `false`

> Deshabilita la telemetria
- `browser.discovery.enabled` to `false`
- `datareporting.policy.dataSubmissionEnabled` to `false`
- `datareporting.healthreport.uploadEnabled` to `false`
- `toolkit.telemetry.unified` to `false`
- `toolkit.telemetry.archive.enabled` to `false`
- `toolkit.telemetry.newProfilePing.enabled` to `false`
- `toolkit.telemetry.shutdownPingSender.enabled` to `false`
- `toolkit.telemetry.updatePing.enabled` to `false`
- `toolkit.telemetry.bhrPing.enabled` to `false`
- `toolkit.telemetry.firstShutdownPing.enabled` to `false`
- `toolkit.telemetry.coverage.opt-out` (hidden|logic) to `true`
- `toolkit.coverage.opt-out` (hidden|logic) to `true`
- `toolkit.coverage.endpoint.base` to `""`
- `browser.ping-centre.telemetry` to `false`
- `browser.newtabpage.activity-stream.feeds.telemetry` to `false`
- `browser.newtabpage.activity-stream.telemetry` to `false`
- `app.normandy.api_url` to `""`

> Deshabilita los reportes de crasheo
- `breakpad.reportURL` to `""`
- `browser.tabs.crashReporting.sendReport` to `false`

> Previene el movimiento de ventanas
- `dom.disable_window_move_resize` to `true`

> Configura GFX.accelerated
- `gfx.canvas.accelerated.cache-items` to `4096` | default `2048`
- `gfx.canvas.accelerated.cache-size` to `1024` | default `256`
- `gfx.content.skia-font-cache-size` to `20` | default `5`

# Limpieza

**Borra archivos temporales y puntos de restauracion**

Ve a la barra de busqueda, busca y ejecuta "`cleanmgr`", se demora un rato en iniciar
1. Desde la pestaña "Liberador de espacio en disco", selecciona todo y dale en "Aceptar"
2. Selecciona "Eliminar archivos" y esperas a que termine
3. Opcionalmente puedes volver a ejecutar "`cleanmgr`", ir a "Opciones avanzadas" y hacer click en "Limpiar puntos de restauracion"

**Desactiva Caracteristicas del sistema**

Ve al menu de inicio y abre el "Panel de Control"
1. Ve a "Programas"
2. Selecciona "Activar o Desactivar las caracteristicas del sistema"
3. Desactiva Casi todas las caracteristicas del sistema
4. Recuerda dejar activadas
	- Windows Search
	- Microsoft .NET Framework 3.5.1
5. Le das en "Aceptar" y te preguntara por un reinicio, le dices "Reiniciar mas Tarde"

**Desactiva la desfragmentacion**

Ve a la barra de busqueda, busca y ejecuta "Desfragmentador de disco"
1. Desactiva la desfragmentacion programada
2. Desfragmenta el disco por una ultima vez

**Inicio mas Rapido**

Ve a la barra de busqueda, busca y ejecuta "`msconfig`"
1. Ve a "Arranque"
	- Pon el tiempo de espera en 0 segundos
	- Selecciona "Sin arranque de GUI"
	- Selecciona la opcion "Convertir en permanente toda la configuracion de arranque"
2. Ve a "Servicios"
	- Activa "Ocultar todos los servicios de Microsoft" y Desactiva
		- Mozilla Maintenance Service
		- Remote Packet Capture Protocol
	- Desactiva "Ocultar todos los servicios de Microsoft" y Desactiva
		- Experiencia con Aplicaciones
		- Telemetria
		- Windows Update
		- Superfetch
		- NOTA: Me dio flojera, pero parece que se pueden desactivar todos los servicios que estan detenidos

Ahora te preguntara si quieres, reiniciar la maquina, y le dices que si

**Desactiva Servicios del Sistema**

Ve a la barra de busqueda, busca y ejecuta "Servicios"

(WIP) Me dio flojera hacerlo realmente, pero deberia de tener que optimizar los servicios innecesarios

**Limpia sectores vacios**

Ve a la barra de busqueda, busca y ejecuta "CMD" como administrador
- Escribe y ejecuta `cipher /w:C:\`

# Hardening
**WeakDH**

> [!TIP] Lecturas Recomendadas
> - [Weak Diffie Hellman Vulnerabily](https://weakdh.org)
> - [Microsoft - Conjunto de cifrado TLS/SSL](https://learn.microsoft.com/es-mx/windows/win32/secauthn/cipher-suites-in-schannel?redirectedfrom=MSDN)
> - [Microsoft - Conjunto de cifrado TLS en Windows 7](https://learn.microsoft.com/es-mx/windows/win32/secauthn/tls-cipher-suites-in-windows-7)
> - [Mozilla - Security/Server Side TLS](https://wiki.mozilla.org/Security/Server_Side_TLS)
> - [LeadSSL - SSL received a weak ephemeral DF key](https://www.leaderssl.com/news/415-ssl-received-a-weak-ephemeral-diffie-hellman-key-how-to-solve-this-problem)

Debes abrir `gpedit.msc`
1. Ve a "Configuracion de equipo" > "Plantillas Administrativas" > "Red" > "Configuracion de SSL"
2. Debes editar la configuracion y en el cuadrado debes cambiar el primer bloque por el segundo recomendado por Firefox

> Antigua Configuracion
```
TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P384,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P384,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P384,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P384,TLS_DHE_RSA_WITH_AES_256_GCM_SHA384,TLS_DHE_RSA_WITH_AES_128_GCM_SHA256,TLS_DHE_RSA_WITH_AES_256_CBC_SHA,TLS_DHE_RSA_WITH_AES_128_CBC_SHA,TLS_RSA_WITH_AES_256_GCM_SHA384,TLS_RSA_WITH_AES_128_GCM_SHA256,TLS_RSA_WITH_AES_256_CBC_SHA256,TLS_RSA_WITH_AES_128_CBC_SHA256,TLS_RSA_WITH_AES_256_CBC_SHA,TLS_RSA_WITH_AES_128_CBC_SHA,TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P384,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P384,TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384_P384,TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256_P384,TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA_P384,TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA_P256,TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA_P384,TLS_DHE_DSS_WITH_AES_256_CBC_SHA256,TLS_DHE_DSS_WITH_AES_128_CBC_SHA256,TLS_DHE_DSS_WITH_AES_256_CBC_SHA,TLS_DHE_DSS_WITH_AES_128_CBC_SHA,TLS_RSA_WITH_3DES_EDE_CBC_SHA,TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA,TLS_RSA_WITH_RC4_128_SHA,TLS_RSA_WITH_RC4_128_MD5,TLS_RSA_WITH_NULL_SHA256,TLS_RSA_WITH_NULL_SHA,SSL_CK_RC4_128_WITH_MD5,SSL_CK_DES_192_EDE3_CBC_WITH_MD5
```

> Nueva Configuracion
```
TLS_AES_128_GCM_SHA256,TLS_AES_256_GCM_SHA384,TLS_CHACHA20_POLY1305_SHA256,ECDHE-ECDSA-AES128-GCM-SHA256,ECDHE-RSA-AES128-GCM-SHA256,ECDHE-ECDSA-AES256-GCM-SHA384,ECDHE-RSA-AES256-GCM-SHA384,ECDHE-ECDSA-CHACHA20-POLY1305,ECDHE-RSA-CHACHA20-POLY1305,DHE-RSA-AES128-GCM-SHA256,DHE-RSA-AES256-GCM-SHA384,DHE-RSA-CHACHA20-POLY1305
```

> Reinicia y ahora deberias probar el comando de nmap y no deberia aparecer el error
```
nmap -sS -sV -O -p- --script vuln {ip}
```

**Analisis Completo de NMAP**

```
Nmap scan report for {IP}
Host is up (0.00040s latency).
Not shown: 65502 closed tcp ports (reset)
PORT      STATE    SERVICE       VERSION
135/tcp   open     msrpc         Microsoft Windows RPC
139/tcp   open     netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open     microsoft-ds  Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
2869/tcp  open     http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-csrf: Couldn't find any CSRF vulnerabilities.
3389/tcp  open     ms-wbt-server Microsoft Terminal Service
|_ssl-ccs-injection: No reply from server (TIMEOUT)
5357/tcp  open     http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-server-header: Microsoft-HTTPAPI/2.0
8308/tcp  filtered unknown
16213/tcp filtered unknown
23418/tcp filtered unknown
25823/tcp filtered unknown
30256/tcp filtered unknown
30589/tcp filtered unknown
34080/tcp filtered unknown
35375/tcp filtered unknown
41023/tcp filtered unknown
44365/tcp filtered unknown
44382/tcp filtered unknown
44659/tcp filtered unknown
47110/tcp filtered unknown
47318/tcp filtered unknown
48881/tcp filtered unknown
49152/tcp open     msrpc         Microsoft Windows RPC
49153/tcp open     msrpc         Microsoft Windows RPC
49154/tcp open     msrpc         Microsoft Windows RPC
49155/tcp open     msrpc         Microsoft Windows RPC
49156/tcp open     msrpc         Microsoft Windows RPC
49160/tcp open     msrpc         Microsoft Windows RPC
56639/tcp filtered unknown
56697/tcp filtered unknown
57117/tcp filtered unknown
57881/tcp filtered unknown
61563/tcp filtered unknown
62545/tcp filtered unknown
MAC Address: {RANDOM-MAC} (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Microsoft Windows 2008|7|Vista|8.1
OS CPE: cpe:/o:microsoft:windows_server_2008:r2 cpe:/o:microsoft:windows_7 cpe:/o:microsoft:windows_vista cpe:/o:microsoft:windows_8.1
OS details: Microsoft Windows Vista SP2 or Windows 7 or Windows Server 2008 R2 or Windows 8.1
Network Distance: 1 hop
Service Info: Host: RTH010V-PC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_smb-vuln-ms10-054: false
|_smb-vuln-ms10-061: NT_STATUS_ACCESS_DENIED
|_samba-vuln-cve-2012-1182: NT_STATUS_ACCESS_DENIED

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 2730.55 seconds
```

# Exporta la maquina

Ahora apaga la maquina

> Compacta el disco vdi
```
VBoxManage modifymedium Windows7-ENSP-Creativo-disk001.vdi --compact
```

> Exporta desde el menu de Virtualbox como OVA 2.0 y quita las MAC

# Extra

**Changelog eNSP 1.3.00.100**

```
eNSP 1.3.00.100 updates
Fixed Bugs:
1.- Fixing the bug which CE/NE/CX can not be started at the second time.
2.- Fixing the bug which eNSP can not connect to CE/NE/CX's command line sometimes.
======================================================================
eNSP 1.3.00 updates
New added:
1.- The latest version of the USG6000V device (V500R005C10SPC300) is integrated (default username: admin, password: Admin@123).
2.- Integrated NE40E, NE5KE, NE9K and CX (V800R011C00SPC607B607).
3.- Integrated with the latest version of CE6800 and CE12800 (V800R011C00SPC607B607).
4.- The web device is added to the AC network management function.

Fixed Bugs:
1.- Repairing AR can't open multiple problems at once.
2.- Fix the problem that STA roaming cannot be a one-time success.
3.- Repair SVRP devices can only open 16 problems.
4.- Repair SVRP device connection can only connect up to 20 questions.
5.- Change the interface placement of NE, CX, and CE devices, classify NE and CX as routers, and return CE to switches.
```

## Expermiental
- https://stackoverflow.com/questions/28309819/shrink-a-vmdk-virtualbox-disk-image
- https://www.experts-exchange.com/articles/12938/HOW-TO-Shrink-a-VMware-Virtual-Machine-Disk-VMDK-in-15-minutes.html
- https://dev.to/otomato_io/how-to-reduce-the-vmdk-disk-image-size-in-virtualbox-38e0

Necesito que el disco este lo mas comprimido posible, para que el resultado en OVA sea lo mas pequeño posible y convenza a Duoc de utilizarlo, a pesar de pesar 32GB :P

> Reconstruye el disco de vdi, a vmdk, luego de vmdk a vdi, y luego compactalo
```
VBoxManage clonehd disk.vdi disk-temp.vmdk --format VMDK
VBoxManage clonehd disk-temp.vmdk disk-clean.vdi --format VDI
VBoxManage modifymedium disk-clean.vdi --compact
```

> Tambien puedes solo exportar el disco a vmdk
```
VBoxManage clonehd disk.vdi disk-temp.vmdk --format VMDK
```
> Luego Comprimir con las herramientas de vmware
```
/usr/bin/vmware-vdiskmanager -k ~/VirtualBox\ VMs/<virtual disk.vmdk>
```

## Troubleshooting

> En caso de que Virtualbox solo muestre configuraciones con 32 bits, debes ir a tu host y desactivar Hypr-V con
```
bcdedit /set hypervisorlaunchtype off
```

> Errores de inicio, puedes leer [Huawei Forums - How to solve the NE40E/NE9000/NE5000E/CX200/CE6800/CE12800 start timeout on eNSP](https://forum.huawei.com/enterprise/intl/en/thread/how-to-solve-the-ne40e-ne9000-ne5000e-cx200-ce6800-ce12800-start-timeout-on-ensp/667227419901313025?blogId=667227419901313025)

## Ideas y Problemas
- Ideas
	- Remover las fuentes que no se utilizen
	- eNSP dice tener soporte oficial para, que tanto se puede extender??
		- WinPcap: 4.1.3
		- Wireshark: 2.6.6
		- Virtualbox: 4.2.x - 5.2.x
	- Un analisis de seguridad con nmap, Nessus, si me da la gana, algun otro y  revisar el sistema para hacerlo mas seguro
		- nmap: `nmap -sS -sV -O -p- --script vuln {ip}` (Puede durar de 5 minutos a 30 minutos)
		- Nessus Essentials (En Proceso)


- Problemas
	- VLC no reproduce videos correctamente..., talvez es mejor dejar solamente MPC-HM de los codecs K-Lite, extraño...
	- Parece que el Chipset debe ser PIIX3 porque el ICH9 falla en el HOST Linux, pero no tengo la menor idea porque ocurre...