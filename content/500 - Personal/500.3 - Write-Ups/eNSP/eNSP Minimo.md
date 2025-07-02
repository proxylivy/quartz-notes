# Info
## Datos

Muchas gracias a ([Youtube - Dark Bird Tech](https://www.youtube.com/@darkbirdtech/videos)) por ayudarme a comprender sobre la optimizacion de discos vdi mediante discos VHD y su enfoque off-line que resulta ser mas ligero :D

Asi que este metodo se divide en 3 partes
1. Crear disco VHD y recopilar materiales
2. Optimizar iso de Windows 7 (O utilizar uno ya optimizado)
3. Instalacion y exportacion

**Tamaño VM**
> [!NOTE] Significados
> - `All`: Todas las imagenes instaladas
> - `F`: Solo instalacion del firewall

> [!NOTE] Sobre la exportacion
> - [DMTF - OVF FAQ](https://www.dmtf.org/about/faq/ovf_faq)
> - OVF 1.0 vs 2.0 no tienen modificaciones en tamaño final pero OVA 2.0 esta optimizado para sistemas en la nube, mi eleccion sobre las exportaciones de OVA estan basadas en la version 2.0

Comparacion del tamaño en Windows por cada Instalacion Base (Sin hacer nada)
- Windows 7 SuperLite: 10.3GB
- Windows 7 Ultimate Limpio: 20.3GB
- Windows 7 Ultimate Limpio pero modificado: TO-DO

Tamaños Finales usando SL
- Exportacion en .OVA
	- OVA All: 7.7GB
	- OVA F: 3.2GB
- Disco VDI
	- VDI All: 5.45GB
	- VDI F: 2.63GB
- Uso Windows
	- `C:\` All: 26.7GB
	- `C:\` F: 15.3GB

# Preparacion
## Disco VHD

> [!NOTE] Uso de VM
> - Recomiendo utilizar una maquina windows 10 (o un windows 7 que sea seguro) que tenga acceso a internet y un buen navegador

**Crear Disco VHD**

En tu host, debes abrir Virtualbox, ir al menu "Herramientas" y seleccionar desde la barra "Medio" (Administrador de discos).
1. En las opciones superiores, seleccionas "Crear"
2. Dentro de la ubicacion, al final, cambias el nombre de "`NewVirtualDisk`" a "`eNSP`" (O el nombre que mas te guste)
3. El tamaño del disco puede ser de 10GB a 15GB
4. Cambiar es tipo de disco a "VHD (Virtual Hard Disk)"

**Añadir Disco VHD a Maquina Internet**

Ahora en tu maquina con acceso a internet, abres las Configuraciones
1. Vas al menu "Almacenamiento"
2. Seleccionas "Controlador: Sata" y apretas el boton "Añadir Conexion" y seleccionas "Disco Duro"
3. En la ventana "Selector de medio", seleccionas el disco creado "`eNSP.vhd`" (Posiblemente este en el menu "Not Attached")
4. Le das en "Aceptar" y en la ventana de configuraciones apretas "Aceptar"

Ahora puedes iniciar la maquina para configurar el disco

**Formatear disco**

Abre el menu inicio y selecciona "`Crear y formatear particiones de disco duro`"
1. Te saldra un pop-up para Inicializar el disco automaticamente
	- Selecciona MBR (Defecto) para tener compatibilidad y haz click en "Aceptar"
2. Seleccion el disco 1 (O el que hayas creado)
3. Das click derecho y seleccionas "Nuevo volumen simple"
4. Te da la bienvenida el asistente, le das en "Siguiente"
5. Te pedira especificar el tamaño del volumen, por defecto es todo el disco, asi que "Siguiente"
6. Asignas el disco "`D:`" (Por defecto la siguiente disponible) y "Siguiente"
7. Configuras el formateo (Solo debes cambiar la etiqueta)
	- Sistema de Archivos: NTFS (Defecto)
	- Tamaño de la unidad de asignacion: Predeterminado
	- Etiqueta del volumen: `eNSP`
8. Das click en "Siguiente"
9. Ahora click en "Finalizar"

## Descargar Materiales
> [!TIP] Descargar eNSP
> - [Huawei Forums - Download eNSP simulator installation Software](https://forum.huawei.com/enterprise/intl/en/thread/download-ensp-simulator-installation-software-here/667238396713648128?from=latestPostsReplies&blogId=667238396713648128)
> - [Huawei Forums - Resource Downloading for eNSP](https://forum.huawei.com/enterprise/intl/en/thread/resources-downloading-for-ensp/667245683301826561?blogId=667245683301826561)
> - [Dark Bird Tech - Google Drive](https://drive.google.com/drive/folders/1T4v1idJec_3p9F3FhwuTA6JyuUpj05un) | [Onedrive Mirror](https://duoccl0-my.sharepoint.com/:f:/g/personal/ga_zunigam_duocuc_cl/EuCvefQ9MB1Br3OH4Iqon6UBkFA8pfqbXkYACdvtlEiDBA?e=XAptqZ)

> [!NOTES] Status
> - Npcap
> 	- [Npcap - NPcap or WinPcap?](https://npcap.com/vs-winpcap.html)
> 	- Cuando lo instales debes seleccionar "WinPcap API-compatible mode"
> - Wireshark
> 	- Realmente depende del punto anterior, windows 7 tiene como limite la version 4.0.17 (2024-08-28), talvez pueda funcionar...
> - Huawei Images-Extras
> 	- [Huawei Forums - Resource Downloading for eNSP](https://forum.huawei.com/enterprise/intl/en/thread/resources-downloading-for-ensp/667245683301826561?blogId=667245683301826561)
> 	- [Huawei Forums - Download eNSP USG6000v Image](https://forum.huawei.com/enterprise/intl/en/thread/download-ensp-usg6000v-image/667245289389572096?blogId=667245289389572096)
> 	- [Huawei Forums - Download eNSP NE40E Image](https://forum.huawei.com/enterprise/intl/en/thread/ne40e-image-for-ensp-v100r003c00spc100/667245683289243648?blogId=667245683289243648)
> 	- [Youtube - Dark Bird Tech](https://www.youtube.com/@darkbirdtech)
> 		- [Youtube - Adding the missing devices in Huawei eNSP v1.3](https://youtu.be/WH7xrq8Mqx8?si=nJfUunV92hITMcIZ)

Desde el sistema Guest con internet, debes descargar

- Drivers
	- Virtualbox 5.2.44 | [Oracle - Virtualbox Old build 5.2](https://www.virtualbox.org/wiki/Download_Old_Builds_5_2)
	- Virtualbox Extension Pack vbox-extpack 5.2.44 | [PUEL License](https://www.virtualbox.org/wiki/VirtualBox_PUEL)
	- VBoxGuestAdditions.iso | [Oracle - Download Center](https://download.virtualbox.org/virtualbox/) (Ultima version)
	- VCRedist Repack | [Major Geeks - Visual C Redistribute Runtimes AIO Repack](https://www.majorgeeks.com/files/details/visual_c_redistributable_runtimes_aio_repack.html)
- eNPS
	- eNSP 1.3.00.510
	- WinPcap 4.1.3 | [WinPcap](https://www.winpcap.org/install/)
	- Npcap | [Npcap - Download](https://npcap.com/#download) (TO-DO)
	- Wireshark 2.6.6 | [Go Spelunking](https://www.wireshark.org/download.html#spelunking) | [Main Mirror - win64/all-versions](https://1.na.dl.wireshark.org/win64/all-versions/)
	- Wireshark 4.0.17 (Failed Linked dll in SL)
	- Wireshark 3.6.24 (Last LTS...)
- Fuentes
	- Nerd Font | [Official Page](https://www.nerdfonts.com/) | [Github - ryanoasis/nerd-fonts](https://github.com/ryanoasis/nerd-fonts)
- Imagenes Extras
	- [Dark Bird Tech - Google Drive - eNSP Optional Extra Devices](https://drive.google.com/drive/folders/1xhb71AzTOcy_IvVnjFi0rEme6iGZJ7uy) | [Onedrive - Mirror](https://duoccl0-my.sharepoint.com/:f:/g/personal/ga_zunigam_duocuc_cl/EjgfGZjaSOxGvwhi6Z0C7scBNLstbk1iFPxq23X1N5B0IA?e=XPUmZI)
- Herramientas
	- Massgrave | [Official Page](https://massgrave.dev/) | [Github - massgravel/Microsoft-Activation-Scripts](https://github.com/massgravel/Microsoft-Activation-Scripts)
	- 7zip | [7-zip](https://www.7-zip.org/)
	- Firefox ESR 115.25.0 | [Mozilla Release FTP - Firefox 115.25.0esr win64](https://releases.mozilla.org/pub/firefox/releases/115.25.0esr/win64/)
	- WinCDEmu | [Github - sysprogs/WinCDEmu - Releases](https://github.com/sysprogs/WinCDEmu/releases)
- Labs
	- (TO-DO)
- Updates
	- UpdatePack7R2 | [Blog Simplix Info - UpdatePack7r2 (Russian)](https://blog.simplix.info/updatepack7r2/) | [Blog Simplix Info - Update7 (Russian)](https://blog.simplix.info/update7/) | [MajorGeeks - Full exe Mirror](https://www.majorgeeks.com/files/details/simplix_updatepack.html)

**Ordenar Materiales**

Recomiendo el siguiente esquema que fue utilizado en la guia, no es obligatorio ordenarlo, pero te solucionara la vida :D
```
/vhd-eNSP (D:\)/
├── Drivers/
│   ├── Oracle_VM_VirtualBox_Extension_Pack-5.2.44
│   ├── VBoxGuestAdditions_7.1.10
│   ├── virtio-win-0.1.173-4
│   └── VisualCppRedist_AIO_x86_x64
├── ENSP/
│   ├── eNSP_Setup V100R003C00SPC100 1.3.00.100.exe
│   ├── npcap-1.82
│   ├── VirtualBox-5.2.44-139111-Win.exe
│   ├── WinPcap_4.1.3.exe
│   ├── Wireshark-win64-2.6.6.exe
│   └── Wireshark-win64-4.0.17.exe
├── Fonts/
│   └── HackNerdFont.ttf
├── Images-Extras/
│   ├── CE/
│   │   └── CE.img
│   ├── CX/
│   │   └── CX.img
│   ├── NE40E/
│   │   └── NE40E.img
│   ├── NE5000E/
│   │   └── NE5000E.img
│   ├── NE9000/
│   │   └── NE9000.img
│   └── vfw_usg/
│       └── vfw_usg.vdi
├── Labs/
│   └── (TO-DO)
├── Tools/
│   ├── Microsoft-Activation-Scripts-master/
│   ├── 7z2409-x64.exe
│   ├── Firefox Setup 115.25.0esr
│   └── WinCDEmu-4.1.exe
└──  Updates
    └── UpdatePack7R2-25.6.10
```

# Instalacion

**Desde Virtualbox (Host)**

Abre Virtualbox, desde la pagina principal, presiona "Nueva", y creas los siguientes datos
- Nombre y sistema operativo
	- Nombre: eNSP-Win7-Minimal
	- Imagen ISO: `Win7Ult-SP1-SuperLite-x64.iso` (O tu .iso de win7 preferida)
	- Tipo: Microsoft Windows
	- Version: Windows 7 (64-bits)
	- Activa la opcion "Omitir instalacion desatentida"
- Instalacion desatendida (Omitir)
- Hardware
	- Memoria Base: 8196MB (Minimo 4096MB)
	- Procesadores: 2vCPU (La verdad podria ser el maximo posible)
	- Deshabilitas "EFI"
- Disco Duro
	- Creas un disco duro virtual (VDI) de 32GB

> [!NOTE] No inicies la maquina
> Presiona "Terminar" y seleccionas "Configuracion" para configurar en mas detalle

Abre "Configuracion" del VM

General
- Basico (Omitir)
- Avanzado (Omitir)
- Sistema
	- Placa Base
		- Dispositivo Apuntador: Tableta USB
	- Procesador
		- Habilitar PAE/NX
		- Forzar Habilitar VT-x/AMD-V anidado con "`VBoxManage modifyvm "eNSP-Win7-Minimal" --nested-hw-virt on`"
		- Interfaz de paravirtualizacion: "Hypr-V"
		- Activar "Hardware de virtualizacion"
	- Pantalla
		- Memoria de Video: 64MB
	- Almacenamiento (Depende si tienes SSD o HDD, si tienes HDD ignora esta parte)
		- Controlador SATA
			- Tipo: AHCI
			- Activa "Usar cache de I/O anfitrion"
		- VM-name.vdi
			- Activa "Unidad de estado solido"
	- Audio (Omitir)
	- Red
		- Adaptador 1
			- Activar esta interfaz
			- Conectado a: Adaptador Puente
			- Modo Promiscuo: Permitir todo
	- Puertos Serie (Omitir)
	- USB (Omitir)
	- Interfaz de Usuario
		- Desactiva "Minibarra de Herramientas: Mostrar en pantalla completa/fluida"

Ahora le das en "Iniciar" al VM

Te aparecera la ventana de instalacion de Windows, seleccionas el disco y le das en "Next", luego se instalara normalmente, cuando aparesca el escritorio, apagas el VM

Vas a la configuracion del VM, luego a "Almacenamiento", eliminas el iso de instalacion y añades una conexion a un Disco Duro, te saldra un menu, añades el disco VHD creado antes "`eNSP.vhd`", le das en "Aceptar" e inicias el VM

**Instalacion desde VHD**
1. Instala herramientas desde `D:\Tools`
	- WinCDEmu
	- 7zip
2. Instala Drivers desde `D:\Drivers`
	- Instala `Visual CppRedist_AIO_x86_x64`
	- Montar `virtio-win` y ejecuta "`virtio-win-gt-x64.msi`" y desactiva "Spice Agent"
	- Montar `VBoxGuestAdditions.iso` y ejecuta "`VBoxWindowsAdditions-amd64.exe`" y dale en "`reboot now`"
3. Instalar stack eNSP desde `D:\eNSP`
	- WinPcap 4.1.3
	- Virtualbox 5.2.44
	- Wireshark 2.6.6 (Sin WinUSB)
	- eNSP 1.3.00.100 directamente (Sin instalar 1.2 antes)
4. Configura Imagenes adicionales en ENSP (Solo las que necesites)
	- Crea la carpeta `C:\Program Files\Huawei\Images-Extra`
	- Copiar las imagenes desde `D:\Images-Extra` a esa carpeta
	- Luego cargalas dentro de eNSP
		- Firewall USG6000V -> `vfw_usg.vdi`
		- Router NE40E -> `NE40E.img`
		- Router NE5000E -> `NE5000E.img`
		- Router NE9000 -> `NE9000.img`
		- Router CX200 -> `CX.img`
		- Switch CE6800 -> `CE.img`

**Inicia eNSP**
1. Ejecuta eNSP
2. Cuando aparesca el aviso del firewall, marca las redes "Privadas" y "Publicas" para darle acceso completo a las funcionalidades que necesita
3. Ve a la parte de Firewall, pon un nodo USG6000v
4. Haz click en "Start" y te pedira un archivo y le das en "Search"
5. Vas a `C:\Program Files\Huawei\Images-Extra`, luego USG6000V y elije "`vfw_usg.vdi`"
6. Ahora elimina el nodo
7. Ve a "Menu" > "Tools" > "Options", luego ve a "Fonts" y haz click en "Select", Cambia la fuente de eNSP a "Hack Nerd Font - Regular - 12"

**Arregla la licencia**

Ve a `D:\Tools\Microsoft-Activation-Scripts-master/MAS/All-In-One-Version-KL/` y ejecuta `MAS_AIO.cmd` como administrador
1. Selecciona `[9]` "Troubleshooting"
2. Selecciona `[5]` "Fix License" y confirma con `[9]` "Continue"
3. Cuando termine apreta cualquier tecla
4. Selecciona `[0]` para ir atras
5. Selecciona `[3]` "TSforge" y activa la licencia con `[1]` "Windows"

# Optimiza

**Desactivar Firewall**

Abre el menu de inicio, Ve a "Panel de Control" y selecciona "Windows Firewall"
1. Selecciona "Turn Windows Firewall on or off" y desactivalos, dale en "Ok"

**Desactivar Caracteristicas innecesarias**

Ir a "Panel de Control" > "Programas" > "Activar o desactivar caracteristicas"
- Desactiva todo menos
	- "Windows Search"
	- ".NET Framework 3.5.1"

**Minimiza Efectos Visuales**

Abre el menu de inicio, dale click derecho a "Computer" y entra a Propiedades, abre la opcion "Advanced system settings"
1. En el menu "Performance" selecciona "Settings..."
2. Desactiva todos menos
	- `Smooth edge of screen fonts`
	- `Smoth-scroll list boxes`
	- `Use visual styles on windows and buttons`

**Permisos admin**
Haz Click derecho en eNSP y ve a "Propiedades" (Lo mismo para Virtualbox)
1. En el menu "General" ve a "Advanced" y activa "Run as Administrator"
2. En el menu "Security" ve a "Edit..." y permite a "Win7Lite" e "Interactive" los permisos completos

**Desactiva Desfragmentar**

Abre el menu de inicio, busca y ejecuta "Disk desfragmenter"
1. Desactiva desde "Configure Schedule" y ejecutalo una vez sobre el disco C:\


**Limpia sectores vacios**

Ve a la barra de busqueda, busca y ejecuta "CMD" como administrador
- Escribe y ejecuta `cipher /w:C:\`

**Despidete de la maquina**

Finalmente Apaga la maquina...

# Exportacion

Ahora quita el disco VHD

Exporta como OVF 2.0

Y quita las MAC.

Y espera sus 30 minutos...


# Extra
## Estado Actual
Sigo buscando una ISO que se adapte al 100% a mis necesidades...

Por ahora estoy probando `SuperLite`
- ISO: Windows 7 Ultimate SP1 x64 SuperLite (La que estoy probando)
	- Nombre de Archivo: Win7Ult-SP1-SuperLite-x64.iso
	- Descarga: [Internet Archive - kanyos/Super Lite](https://archive.org/details/win7ultsp1superlitex64)
		- Tamaño: 0.98GB
		- Hash:
			- MD5: 6963A7137F92D2C14090E1EDDBF90369
			- SHA-1: 7DF6A9B3F460C7DE9AB2959F11E5349CE02CF4AD

Alternativas
- https://ghostspectre.the-ninja.jp/WIN7.X64.html
- Windows 7 Ultimate SP1 Genuino
- Windows 7 Ultimate SP1 Modificado por mi

Wireshark 4.0.17 le faltan librerias en SuperLite... Intentando con 3.6

## Invalido (Por ahora)

Razon Principal: SuperLite no tiene WUA

**Cambia el Idioma**
> [!TIP] Lecturas Recomendadas
> - [Internet Archive - Windows 7 - MUI all language packs](https://archive.org/details/windows-7-mui-all-language-packs-sp0-sp1-x86-x64)

Descargas `windows6.1-kb2483139-x64-es-es_fdbdf4061b960324efb9eedf7106df543ed8ce33.exe`, desde el menu "`Windows 7 SP1 64-bit (x64) MUI Language Packs`"

NO ES POSIBLE EN LA ISO DE SuperLite...

**Actualizaciones**
> [!TIP] Lecturas Recomendadas
> - [Blog Simplix Info - UpdatePack7R2 (Ucranian)](https://blog.simplix.info/updatepack7r2/) | [Update7](https://blog.simplix.info/update7/) | [MajorGeeks Mirror](https://www.majorgeeks.com/files/details/simplix_updatepack.html)
> - [Blog Simplix Info - VCRedist 1.3 (Ucranian)](https://blog.simplix.info/vcredist/)
