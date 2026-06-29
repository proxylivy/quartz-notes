# Info
## Datos

Este Write-up sigue un metodo de instalacion dividido en 3 partes
1. Crear disco VHD y recopilar materiales
2. Optimizar iso de Windows 7 (o utilizar uno ya optimizado)
3. Instalacion y exportacion

**Tamaño VM**
> [!NOTE] Significados
> - `All`: Todas las imagenes extras de Huawei instaladas
> - `F`: Solo instalacion del firewall

> [!NOTE] Sobre la exportacion
> - [DMTF - OVF FAQ](https://www.dmtf.org/about/faq/ovf_faq)
> - OVF 1.0 vs 2.0 no tienen modificaciones en tamaño final pero OVA 2.0 esta optimizado para sistemas en la nube, mi eleccion sobre las exportaciones de OVA estan basadas en la version 2.0

Comparacion del tamaño en Windows por cada Instalacion Base (Sin hacer nada)
- Windows 7 SuperLite: 10.3GB
- Windows 7 Ultimate Clean: 20.3GB
- Windows 7 Ultimate Clean pero modificado: TO-DO

Tamaños Finales usando SuperLite
- Exportacion en .OVA
	- OVA All: 7.7GB
	- OVA F: 3.2GB
- Disco VDI
	- VDI All: 5.45GB
	- VDI F: 2.63GB
- Discos Internos en Windows
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

> [!CAUTION] ¿Porque algunos nombres tienen versiones exactas y otros no?
> - Nombres con Version: Esa es la ultima version compatible con el entorno, deberias buscar exactamente esa
> - Nombres sin Version: Deberias probar la ultima version publicada

> [!TIP] Descargar eNSP
> - [Huawei Forums - Download eNSP simulator installation Software](https://forum.huawei.com/enterprise/intl/en/thread/download-ensp-simulator-installation-software-here/667238396713648128?from=latestPostsReplies&blogId=667238396713648128)
> - [Huawei Forums - Resource Downloading for eNSP](https://forum.huawei.com/enterprise/intl/en/thread/resources-downloading-for-ensp/667245683301826561?blogId=667245683301826561)
> - [Dark Bird Tech - Google Drive](https://drive.google.com/drive/folders/1T4v1idJec_3p9F3FhwuTA6JyuUpj05un) | [Onedrive Mirror](https://duoccl0-my.sharepoint.com/:f:/g/personal/ga_zunigam_duocuc_cl/EuCvefQ9MB1Br3OH4Iqon6UBkFA8pfqbXkYACdvtlEiDBA?e=XAptqZ)

> [!NOTES] Sobre Imagenes Extras de Huawei
> - [Huawei Forums - Resource Downloading for eNSP](https://forum.huawei.com/enterprise/intl/en/thread/resources-downloading-for-ensp/667245683301826561?blogId=667245683301826561)
> - [Huawei Forums - Download eNSP USG6000v Image](https://forum.huawei.com/enterprise/intl/en/thread/download-ensp-usg6000v-image/667245289389572096?blogId=667245289389572096)
> - [Huawei Forums - Download eNSP NE40E Image](https://forum.huawei.com/enterprise/intl/en/thread/ne40e-image-for-ensp-v100r003c00spc100/667245683289243648?blogId=667245683289243648)
> - [Youtube - Dark Bird Tech](https://www.youtube.com/@darkbirdtech)
> 	- [Youtube - Adding the missing devices in Huawei eNSP v1.3](https://youtu.be/WH7xrq8Mqx8?si=nJfUunV92hITMcIZ)

Desde el sistema Guest con internet, debes descargar

- Drivers
	- Oracle Virtualbox Extension Pack vbox-extpack 5.2.44 | [PUEL License](https://www.virtualbox.org/wiki/VirtualBox_PUEL)
	- VBoxGuestAdditions.iso (Version del Virtualbox de tu Host) | [Oracle - Download Center](https://download.virtualbox.org/virtualbox/)
	- virtio-win-0.1.173-4 | [Fedora People - Virtio-win 0.1.173-4](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/virtio-win-0.1.173-4/)
- eNSP
	- eNSP 1.3.00.100 (Mira la nota de arriba)
	- Oracle Virtualbox 5.2.44 | [Oracle - Virtualbox Old build 5.2](https://www.virtualbox.org/wiki/Download_Old_Builds_5_2)
	- Putty | [Chiark Greened - sgtataham/putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/)
	- Npcap 1.79 | [Npcap Release Archive](https://npcap.com/dist/) | [Github nmap/npcap - Issue #751](https://github.com/nmap/npcap/issues/751)
		- ~~WinPcap 4.1.3~~ es reemplazado por npcap, se debe instalar temporalmente
			- [WinPcap Official Site](https://www.winpcap.org/)
	- Wireshark 4.0.17 (Talvez si falla 3.2.18) | [Go Spelunking](https://www.wireshark.org/download.html#spelunking) | [Main Mirror - win64/all-versions](https://1.na.dl.wireshark.org/win64/all-versions/)
- Fuentes
	- Nerd Font | [Official Page](https://www.nerdfonts.com/) | [Github - ryanoasis/nerd-fonts](https://github.com/ryanoasis/nerd-fonts)
- GNS3 (Rama v3.x NO ES COMPATIBLE DE FORMA SENCILLA)
	- GNS3 2.2.41 | [Github - GNS3/gns3-gui releases](https://github.com/GNS3/gns3-gui/releases) | [Tag 2.2.41](https://github.com/GNS3/gns3-gui/releases/tag/v2.2.41) | [Issue #3541](https://github.com/GNS3/gns3-gui/issues/3541)
	- c7200-adventerprisek9-mz.152-4.S6.bin | [Labhub - Dynamips](https://labhub.eu.org/es/addons/dynamips/) [Onedrive - Mirror](https://duoccl0-my.sharepoint.com/:f:/g/personal/ga_zunigam_duocuc_cl/Elk93uuCkudEr5q7C25wFZkB4rMWccPoxUBLxXFnuk1l1g?e=fpCT1a)
- Imagenes eNSP
	- [Dark Bird Tech - Google Drive - eNSP Optional Extra Devices](https://drive.google.com/drive/folders/1xhb71AzTOcy_IvVnjFi0rEme6iGZJ7uy) | [Onedrive - Mirror](https://duoccl0-my.sharepoint.com/:f:/g/personal/ga_zunigam_duocuc_cl/EjgfGZjaSOxGvwhi6Z0C7scBNLstbk1iFPxq23X1N5B0IA?e=XPUmZI)
- Labs
	- Optativo Huawei (WIP)
- Tools
	- Massgrave | [Official Page](https://massgrave.dev/) | [Github - massgravel/Microsoft-Activation-Scripts](https://github.com/massgravel/Microsoft-Activation-Scripts)
	- 7zip | [7-zip](https://www.7-zip.org/)
	- Firefox ESR 115.29.0 | [Mozilla Release FTP - Firefox 115.29.0esr win64](https://releases.mozilla.org/pub/firefox/releases/115.29.0esr/win64/) | [Mozilla - What Train in is it now? - Firefox ESR](https://whattrainisitnow.com/release/?version=esr)
	- IrfanView | [Official Page](https://www.irfanview.com/)
	- K-Lite Codec Pack Standard | [Codec Guide - K-Lite Codec](https://www.codecguide.com/download_kl.htm)
	- WinCDEmu | [Github - sysprogs/WinCDEmu - Releases](https://github.com/sysprogs/WinCDEmu/releases)
- Updates
	- UpdatePack7R2 | [Blog Simplix Info - UpdatePack7r2 (Russian)](https://blog.simplix.info/updatepack7r2/) | [Blog Simplix Info - Update7 (Russian)](https://blog.simplix.info/update7/) | [MajorGeeks - Full exe Mirror](https://www.majorgeeks.com/files/details/simplix_updatepack.html)
	- Visual C++ Redistributable Runtimes AIO Repack | [Major Geeks - abbodi1406/Visual C++ Redistribute Runtimes AIO Repack](https://www.majorgeeks.com/files/details/visual_c_redistributable_runtimes_aio_repack.html)

**Ordenar Materiales**

Recomiendo el siguiente esquema que fue utilizado en la guia, no es obligatorio ordenarlo, pero te solucionara la vida :D
```
/vhd-eNSP (D:\)/
├── Drivers/
│   ├── Oracle_VM_VirtualBox_Extension_Pack-5.2.44.vbox-extpack
│   ├── VBoxGuestAdditions_7.2.4.iso
│   └── virtio-win-0.1.173.iso
├── eNSP/
│   ├── eNSP_Setup V100R003C00SPC100 1.3.00.100.exe
│   ├── npcap-1.79.exe
│   ├── putty-64bit-0.83-installer.msi
│   ├── VirtualBox-5.2.44-139111-Win.exe
│   ├── WinPcap_4.1.3.exe
│   └── Wireshark-win64-4.0.17.exe
├── Fonts/
│   └── Hack (Nerd Fonts)
├── GNS3/
│   ├── GNS3-2.2.40-all-in-one.exe
│   └── c7200-adventerprisek9-mz.152-4.S6.bin
├── Imagenes-eNSP/
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
│   └── Optativo-Huawei-Labs.7z
├── Tools/
│   ├── Microsoft-Activation-Scripts-master/
│   ├── 7z2500-x64.exe
│   ├── Firefox Setup 115.29.0esr
│   ├── iview472 x64 setup
│   ├── K-lite Codec Pack 1905 Standard
│   └── WinCDEmu-4.1.exe
└──  Updates
    ├── UpdatePack7R2-25.9.10
    └── VisualCppRedist_AIO_x86_x64
```


El contenido del Optativo de Huawei es el siguiente
```
/Optativo-Huawei-Labs.7z
└── Optativo-Huawei/
    ├── Casos de Estudio/
    │   ├── Caso de Estudio 01/
    │   └── Caso de Estudio 02/
    ├── Labs en Clases/
    │   ├── Lab 01 - Configuracion Basica y Enrutamiento Estatico/
    │   ├── Lab 02 - Implementacion de RIPv2 y RIPng en Huawei/
    │   ├── Lab 03 - Implementacion de VLANs y Enrutamiento InterVLAN IPv4-IPv6/
    │   ├── Lab 04 - Implementacion de Enrutamiento Intervlan Multivendor/
    │   ├── Lab 05 - Implementando DHCP en Redes Huawei/
    │   ├── Lab 08 - Implementacion de Solucion de STP/
    │   ├── Lab 09 - Implementacion de Eth-Trunk, Smart Links y Monitor Links/
    │   ├── Lab 10 - Implementacion de ACL y NAT en Redes Huawei/
    │   ├── Lab 13 - Implementacion de Conectividad WAN Segura/
    │   └── Lab 14 - Implementacion de BGP/
    ├── Labs Guiados/
    │   ├── Lab Guiado 02 - Configuracion de RIPv2 y RIPng/
    │   ├── Lab Guiado 03 - Configuracion de Enrutamiento Intervlan IPv4-IPv6/
    │   ├── Lab Guiado 04 - Enrutamiento Intervlan y OSPF en Multivendor/
    │   ├── Lab Guiado 05 - Configuracion Servicio DHCPv4/
    │   ├── Lab Guiado 08 - Configuracion de RSTP y MSTP/
    │   ├── Lab Guiado 09 - Configuracion Eth-Trunk, Smart Links, Monitor Links/
    │   ├── Lab Guiado 10 - Configuracion de ACL y NAT/
    │   ├── Lab Guiado 13 - Configuracion Tunnel GRE y VPN Site-To-Site/
    │   └── Lab Guiado 14 - Configuracion de BGP Basico/
    └── Video Tutorial (2018-08-26)/
        ├── Lab Guiado 01 - Configuracion Basica y Acceso Remoto Huawei/
        ├── Lab Guiado 02 - Configuracion de RIPv2 y RIPng/
        ├── Lab Guiado 03 - Configuracion de Enrutamiento Intervlan IPv4-IPv6/
        ├── Lab Guiado 04 - Enrutamiento Intervlan y OSPF en Multivendor/
        ├── Lab Guiado 05 - Configuracion Servicio DHCPv4/
        ├── Lab Guiado 08 - Configuracion de RSTP y MSTP/
        ├── Lab Guiado 09 - Implementacion de Eth-Trunk, Smart Links y Monitor Links/
        ├── Lab Guiado 10 - Configuracion de ACL y NAT/
        ├── Lab Guiado 13 - Configuracion Tunnel GRE y VPN Site-To-Site/
        └── Lab Guiado 14 - Configuracion de BGP Basico/
```

# Instalacion

Primero debes tener una imagen de Windows 7, mis pasos sobre esto estan en [[300 - Conocimiento/Write-Ups/Win7/Win7 - Optimizar ISO|Win7 - Optimizar ISO]]

## Virtualizador

**Desde Virtualbox (Host)**

Abre Virtualbox, desde la pagina principal, presiona "Nueva", y creas los siguientes datos
- Nombre y sistema operativo
	- Nombre: eNSP-Win7-Creativo
	- Imagen ISO: `Win7Ultimate-SP1-Creativo-x64.iso` (O tu .iso de win7 preferida)
	- Tipo: Microsoft Windows
	- Version: Windows 7 (64-bits)
	- Activa la opcion "Omitir instalacion desatentida"
- Instalacion desatendida (Omitir)
- Hardware
	- Memoria Base: 8196MB (Minimo 4096MB)
	- Procesadores: 2vCPU (La verdad podria ser el maximo posible)
	- Deshabilitas "EFI"
- Disco Duro
	- Creas un disco duro virtual (VDI) de 32,10GB

> [!NOTE] No inicies la maquina
> Presiona "Terminar" y seleccionas "Configuracion" para configurar en mas detalle

> [!TIP] Activa Instrucciones VT-X
> - Linux: `VBoxManage modifyvm "eNSP-Win7-Creativo" --nested-hw-virt on`
> - Windows: "`VBoxManage.exe modifyvm "eNSP-Win7-Creativo" --nested-hw-virt on`"

Abre "Configuracion" del VM

General
- Basico (Omitir)
- Avanzado (Omitir)
- Sistema
	- Placa Base
		- Dispositivo Apuntador: Tableta USB
	- Procesador
		- Habilitar PAE/NX
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

## Windows 7

**Instalacion de Win7**

1. Pantalla de Bienvenida
	- Idioma: Español
	- Formato de hora y moneda: Español (Chile)
	- Teclado o metodo de entrada: Latinoamericano
2. Click en "Instalar Ahora"
3. Lees la licencia, la aceptar y le das en "Siguiente"
4. Haces una instalacion "Personalizada (Avanzada)"
5. Seleccionas el Disco 0 y le das en "Siguiente"
6. Comenzara la instalacion, esperas unos 6 minutos, reiniciara un par de veces y te aparecera una ventana para crear usuarios
7. Saldra una ventana para crear al usuario de Windows
	- Nombre de Usuario: "`alumno`"
	- Nombre de Equipo: "`alumno-PC`" (Automatico)
8. Te pedira crear una contraseña
	- Contraseña "`12345`"
	- Recordar contraseña y como ayuda a reconocer "`1 2 3 4 5`"
9. En la ventana de Ayude a proteger el equipo, elige "Instalar solo las actualizaciones importantes"
10. Te pedira confirmar la hora deberia estar correcta
11. Seleccionas la ubicacion del equipo en "Red de Trabajo"
12. Ahora Windows terminara de finalizar la configuracion

Ahora deberas abrir el menu de inicio y apagar la maquina Windows, cuando se apague, abres la configuracion del VM

Vas a "Almacenamiento", eliminas el iso de instalacion y añades una conexion a un Disco Duro, te saldra un menu, añades el disco VHD creado antes "`eNSP.vhd`", le das en "Aceptar" e inicias el VM

**Evita Molestias**

Para desactivar las notficaciones acerca de cambios en el equipo
1. Ve a "Panel de Control" > "Cuentas de Usuarios"
2. Abre "Cuentas de Usuarios" > "Cambiar configuracion de Control de cuentas de usuario"
3. Bajas el Slide a "No notificarme nunca", luego seleccionas "Aceptar" y ya no saldran tantos popup

## Modificaciones Rapidas

**Desactiva Puntos de Restauracion**

Abre el menu de inicio, dale click derecho a "Equipo" y entra a Propiedades, abre la opcion "Configuracion avanzada del sistema"
Ve al menu "Proteccion del sistema"
- Selecciona el disco C:\ y selecciona "Configurar"
	- Selecciona "Desactivar proteccion del sistema"
	- Selecciona "Eliminar" y luego "Continuar" y despues en "Cerrar"
	- Haz click en "Aceptar" y selecciona "Si" y luego borra los puntos mas viejos, con "Si"

**Desactiva Desfragmentar**
> [!WARNING] Expansion DVI
> Gracias a [[#Gracias|DBT]], descubri que dentro de la virtualizacion de virtualbox, los discos vdi son tontos, los mecanismos de vaciado luego de que la data fue utilizada, no se borra, la desfragmentacion y eliminar sectores vacios (como con `cipher` o `sdelete`) lo llena aun mas, no lo uses si deseas un sistema ligero, con una exportacion de OVA mas limpia

Abre el menu de inicio, busca y ejecuta "Desfragmentador de discos"
1. Abre "Configurar programacion" y desactiva "Ejecucion programada" y dale en "Aceptar"
2. NO analizes el disco ni desfragmentes el disco

**Activa la Licencia de Win7**

Ve a `D:\Tools\Microsoft-Activation-Scripts-master/MAS/All-In-One-Version-KL/` y ejecuta `MAS_AIO.cmd` como administrador
1. Selecciona `[3]` "TSforge" y activa la licencia con `[1]` "Windows"

## Actualizaciones Obligatorias
**Instala KB4474419 Soporte Firmas Digitales SHA-2**
> [!TIP] Lecturas Recomendadas
> - [Microsoft Catalog Update - KB4474419](https://www.catalog.update.microsoft.com/Search.aspx?q=KB4474419)
> 	- [Scoped View Inline](https://www.catalog.update.microsoft.com/ScopedViewInline.aspx?updateid=38222b3b-2bd2-4269-880d-35feddf42984)
> - [Microsoft - Actualizacion de soporte de firma de codigo SHA-2](https://support.microsoft.com/es-es/topic/actualizaci%C3%B3n-de-soporte-de-firma-de-c%C3%B3digo-sha-2-para-windows-server-2008-r2-windows-7-y-windows-server-2008-23-de-septiembre-de-2019-84a8aad5-d8d9-2d5c-6d78-34f9aa5f8339)

El `KB4474419` o `Windows6.1-KB4474419-v3-x64_{checksum}.msu` es la actualizacion para permitir firmas digitales SHA-2 que utiliza Microsoft para sus programas, Npcap lo necesita para funcionar en version mas nuevas.

La instalacion es simple
1. Descarga el archivo .msu de los repositorios originales de Microsoft
	- Debes elegir la version "Windows 7 para sistemas basados en x64"


Simplemente se abre, verificas que el KB sea correcto, y lo instalas, el gestor de actualizaciones verificara la actualizacion y la instalara, se demora 1 minuto y luego apretas "Reiniciar ahora", Configurara las actualizaciones, llega al 30% luego se reinicia y continua en el 60% y luego inicias sesion

MIRA TU; LO ARREGLO AAAAAAAAAAAAAA

## Programas

**Instalacion desde VHD**

Debes revisar si windows reconoce correctamente el disco, deberia ser el siguiente disco, por ende, el disco `D:\`

1. Instala las siguientes herramientas desde `D:\Tools`
	- 7zip (`7z`)
	- WinCDEmu
2. Instala las librerias de C++ desde `D:\Updates`
	- Instala `VisualCppRedist_AIO_x86_x64`
3. Instala Drivers desde `D:\Drivers`
	- Montar `virtio-win`
		- Ejecuta "`virtio-win-gt-x64.msi`" y desactiva el componente "Spice Agent"
		- Ve a `Guest-Agent` e Instala `qemu-ga-x86_64.msi`
	- Montar `VBoxGuestAdditions.iso` y ejecuta "`VBoxWindowsAdditions-amd64.exe`" y dale en "`reboot now`"
4. Instalar stack eNSP desde `D:\eNSP`
	- WinPcap 4.1.3
	- Virtualbox 5.2.44
	- Wireshark 3.2.18 (4.0.17 le falta `ADVAPI32.dll`)
	- eNSP 1.3.00.100 directamente (Sin instalar 1.2 antes)
5. Copia `D:\GNS3\c7200-adventerprisek9-mz.152-4.S6.image` a `C:\Users\alumno\GNS3\images`
6. Descomprime los archivos de `D:\Imagenes-eNSP`
	- Debes ir a cada carpeta, descomprimir, y borrar el .zip
7. Configura Imagenes adicionales en ENSP (Solo las que necesites)
	- Crea la carpeta `C:\Program Files\Huawei\Imagenes-eNSP`
	- Copiar las imagenes desde `D:\Images-eNSP` a esa carpeta
	- Luego cargalas dentro de eNSP
		- Firewall USG6000V -> `vfw_usg.vdi`
		- Router NE40E -> `NE40E.img`
		- Router NE5000E -> `NE5000E.img`
		- Router NE9000 -> `NE9000.img`
		- Router CX200 -> `CX.img`
		- Switch CE6800 -> `CE.img`
8. Instala las herramientas extras de `D:\Tools`
	- IrfanView (`iview`)
	- K-Lite Codec Pack
		- Tambien debes instalar "MPC-HM"
9. Instala las fuentes en `D:\Fonts`
	- Abre la carpeta "Hack", selecciona todas las fuentes, click derecho y selecciona "Instalar"

**Instala Npcap**

Npcap es un reemplazo a WinPcap, eNSP necesita reconocer a WinPcap para su instalacion pero no necesariamente para su funcionamiento, asi que con Npcap puedes

- npcap 1.79
	- Activa "Install Npcap in WinPcap API-compatible Mode" (Por defecto)

## Configura eNSP

**Verifica la Instalacion de eNSP**
1. Ejecuta eNSP como Administrador
2. Cuando aparesca el aviso del firewall, marca las redes "Privadas" y "Publicas" para darle acceso completo a las funcionalidades que necesita
3. Cierra eNSP

**Registra los dispositivos Base en eNSP**
1. Ejecuta eNSP como Administrador
2. Ve a "Menu" > "Tools" > "Register Device"
3. Selecciona todos los dispositivos y presiona "Register"
4. Si falla AR-Device, dale en "Exit" y haz los pasos 2 y 3, para mi se soluciono y ahora se ven en Virtualbox

**Agrega Firewall a eNSP**
1. Ve a la parte de Firewall, pon un nodo USG6000v
2. Haz click en "Start" y te pedira un archivo y le das en "Search"
3. Vas a `C:\Program Files\Huawei\Images-Extra`, luego USG6000V y elije "`vfw_usg.vdi`"
4. Ahora elimina el nodo

**Configura Fuentes para eNSP**
1. Ve a "Menu" > "Tools" > "Options", luego ve a "Fonts" y haz click en "Select"
	- Selecciona la Fuente: "Hack Nerd Font"
	- Selecciona el Estilo de Fuente: "Regular"
	- Selecciona el Tamaño: "12"
	- Haz Click en "Aceptar" y luego "OK"

**Optimiza la memoria (Experimental)**
1. Ve a "Menu" > "Tools" > "Options", luego ve a "Tools"
	- Cambia el valor de Memory lower limit, de 50 a 100
	- Haz Click en "Aceptar" y luego "OK"

**Instala GNS3**
> [!TIP] Lecturas Recomendadas
> - [Github - GNS3/gns3-gui](https://github.com/GNS3/gns3-gui)
> 	- [Github - GNS3/gns3-gui - Issue #3541](https://github.com/GNS3/gns3-gui/issues/3541)
> - [GNS3 Docs - Getting Started - Installation on Windows](https://docs.gns3.com/docs/getting-started/installation/windows/)
> - [Youtube - Davil Bombal - GNS3 2.1 Install and configuration on Windows 10 (Part 1): Components and software requirements](https://youtu.be/x9pGYyEqLYs?si=3ZFAu2f847vmItEW)
> - [Youtube - Davil Bombal - GNS3 2.1 - Install and configuration on Windows 10 (Part 2): GUI Install](https://youtu.be/lFEDmM_lsxI?si=TJ0vVbU-VxqS_GgW)

> [!CAUTION] Compatibilidad con Windows 7
> He estado calculando la ultima version compatible, y parece ser que Python 3.8.x tiene problemas con windows 7, segun mis pruebas
> - Rama v3 (Falla)
> - 2.2.51 - 2.2.42 (Falla falta `api-ms-win-core-path-|1-1-0.dll`)
> - 2.2.41 (Funciona)

Instala GNS3 desde `D:\GNS3`
- Debes apretar Next hasta llegar a "Choose Components"
	- Abres "Tools"
		- Deselecciona WinPCAP 4.1.3
		- Deselecciona Wireshark 4.0.3
- Cuando te salga la oferta de "Solarwind Hybrid Cloud Observability", seleccionas "No" y seleccionas "Next"
- Deselecciona "Start GNS3" y dale en "Finish"

**Configura GNS3**

Okey, entonces finalmente inicio...
Setup Wizard
- Run applicances on my local computer
- Selecciona "Don't show this again"
- Next hasta el final
- Di que no a la actualizacion

Ve a "Edit" > "Preference" y selecciona
- General
	- Interface Style: Charcoal
- Miscellaneous
	- Deselecciona "Automatically check for update"
- Dynamips > IOS Routers
	- Haz click en "New"
	- Como ya moviste la imagen, reconoce `c7200-adventerprisek9-mz.152-4.S6.image` automaticamente como imagen existente, le das en "Next"
	- Click en "Next"
	- Click en "Next"
	- Click en "Next"
	- Debes seleccionar "Idle-PC Finder" para que encuentre el valor que redusca el valor de la CPU cuando este en uso, mi calculo fue `0x62a5af34`
	- Le das click en "Finish"

**Desabilita Auto Updates de Wireshark**
> [!TIP] Lecturas Recomendadas
> - [Wireshark Ask QA - Christian_R asked: How can i turn off the automatic update function?](https://ask.wireshark.org/question/2986/how-can-i-turn-off-the-automatic-update-function/)

Pasos
1. Abres Wireshark
2. Vas a "Edicion" > "Preferencias" > "Avanzado"
3. Buscas "`gui.update.enabled`" y le das doble click a "True" para que cambie a "False"
4. Apretas "Ok" y listo

**Mueve los laboratorios de eNSP**
- Crea una carpeta en `C:\Program Files\Huawei` que se llame `Labs Huawei Optativo`
- Ve a `D:\Labs\Optativo Huawei` y muevelo dentro de la carpeta que creaste

# Optimiza

**Permite a los programas a travez del Firewall**
Abre el menu de inicio, Ve a "Panel de Control" y selecciona "Firewall de Windows"
1. Haz click en "Permitir un programa o una caracteristica a traves de Firewall de Windows"
2. Selecciona "Permitir otro programa"
	- Agrega "GNS3"
	- Agrega "Oracle VirtualBox"
	- Agrega "eNSP"
	- Agrega "Wireshark"
3. Selecciona todos los programas para que funcionen tanto en red Domestica/Trabajo como Publica

**Minimiza Efectos Visuales**

Abre el menu de inicio, dale click derecho a "Equipo" y entra a Propiedades, abre la opcion "Configuracion avanzada del sistema"
1. En la parte "Rendimiento" selecciona "Configuracion..."
2. Desactiva todos menos
	- `Desplazamiento suave de los cuadros de lista`
	- `Suavizar bordes para las fuentes de pantalla`
	- `Usar estilos visuales en ventanas y botones`
3. En la parte "Inicio y Recuperacion" y selecciona "Configuracion..."
	- Desactiva "Mostar la lista de sistemas operativos por"
	- Desactiva "Grabar un evento en el registro del sistema"

**Permisos admin**
Debes hacer esto con:
- eNSP
- Oracle Virtualbox
- GNS3
- Wireshark

Ve al escritorio, haz click derecho en eNSP y ve a "Abrir ubicacion del archivo", haz esto tambien para Virtualbox
1. Haz click derecho en el programa, selecciona "Propiedades" y ve a la seccion "Compatibilidad"
2. Haz click en "Cambiar la configuracion para todos los usuarios" y selecciona "Ejecutar este programa como administrador" y le das en "Aceptar"
3. Ve a la seccion "Seguridad" y haz click en "Editar..." ve a "Usuarios" y selecciona todos los cuadrados de la fila "Permitir"

Ahora haz lo mismo con los accesos directos
1. Haz click derecho en el acceso directo, seleccionana "Propiedades"
2. En la seccion "Acceso directo", ve a "Opciones avanzadas" y haz click en "Ejecutar como administrador" y dale en "Aceptar"
3. En la seccion "Seguridad", haz click en "Editar..." y tanto para "Usuarios" como "Interactive", dale todos los permisos 

**Activa Auto-Login**

Ve a la barra de busqueda, busca y ejecuta "netplwiz" y desactivas "Los usuarios deben escribir su nombre y contraseña para usar el equipo", das en "Aceptar", ingresas la contraseña (`12345`) 2 veces

**Desactiva mensajes de Alerta**

Ve a la barra de busqueda, busca y ejecuta "Centro de Actividades" y le das en "Descartar" a todos los mensajes

Selecciona la opcion "Cambiar configuracion del centro de actividades" y desactiva todos los mensajes y dale en "Aceptar"


**Borra archivos temporales y puntos de restauracion**

Ve a la barra de busqueda, busca y ejecuta "`cleanmgr`", se demora un rato en iniciar
1. Desde la pestaña "Liberador de espacio en disco", selecciona todo y dale en "Aceptar"
2. Selecciona "Eliminar archivos" y esperas a que termine
3. Opcionalmente puedes volver a ejecutar "`cleanmgr`", ir a "Opciones avanzadas" y hacer click en "Limpiar puntos de restauracion"

**Configura barra de inicio**
- Quita de la lista todos los programas
- Ancla al Menu de Inicio
	- Oracle VM Virtualbox
	- eNSP
	- GNS3
	- Firefox
	- Wireshark
	- Simbolo del sistema

**Configurar Firefox**
> [!TIP] Lecturas Recomendadas
> - [End Of Life Dates - Firefox](https://endoflife.date/firefox)
> - [Mozilla What train is it now - Firefox ESR Release](https://whattrainisitnow.com/release/?version=esr)
> - [Mozilla Firefox - 115.25.0 Release Notes](https://www.mozilla.org/en-US/firefox/115.25.0/releasenotes/)
> - [Mozilla Releases FTP - Firefox ESR 115.25.0](https://releases.mozilla.org/pub/firefox/releases/115.25.0esr/)

*Nota*: Debes actualizar los certificados, puedes hacerlo con CTLDownload de Microsoft, mas informacion en [Learn Microsoft - Trusted Root - Release Notes](https://learn.microsoft.com/en-us/security/trusted-root/release-notes), descargar un archivo .cab, lo extraes e instalas el archivo .stl con las firmas

Es increible pero al momento de escribir este write-up, Mozilla continua soportando Windows 7, mediante la rama 115.x, el ultimo lanzamiento es "115.25.0" (24 de Junio de 2025)

Si quieres instalar un navegador, puedes instalar Firefox desde `D:\Tools`

La instalacion sigue los siguientes pasos
1. Aparecera la Bienvenida al asistente, le das en "Siguiente"
2. Le das en "Personalizada" y "Siguiente"
3. Dejas la carpeta de destino por defecto y "Siguiente"
4. Desactivas "Instalar servicio de mantenimiento" y "Siguiente"
5. Dejas los 3 iconos y das en "Siguiente"
6. Revisas el resumen, y le das en "Instalar"

Configura la interfaz
1. Salta los pasos de la bienvenida a Firefox
2. Haz click derecho en cada marcador de la barra de marcadores, y selecciona "eliminar marcador" o "Remover de la barra"
3. Haz click en la barra de marcadores y ve a la opcion "Personalizar barra de Herramientas" (Para eliminar arrastra cada icono a la lista de elementos)
	- Elimina "Firefox View" (Icono en la esquina izquierda)
	- Elimina los espacios flexibles a los lados de la barra
	- Elimina Cuenta de Firefox
	- Haz click en "Hecho"

Instala Extensiones y Temas
- Extensiones
	- Ve a la barra y busca `Ublock Origin` ([Ublock Origin](https://ublockorigin.com/)) e instalalo, permite su funcionamiento en ventanas privadas
		- Puedes instalar [Github - fmyh/FMYHFilterlist](https://github.com/fmhy/FMHYFilterlist)
- Temas
	- Selecciona tema "Oscuro", luego sobreescribe con el tema "[Catppuccin Mocha - Lavender](https://addons.mozilla.org/es-CL/firefox/addon/catppuccin-mocha-lavender-git/)"

Selecciona las tres lineas horizontales de la parte derecha, y ve a "Ajustes"
- General
	- Pestañas
		- Activa "Confirmar antes de cerrar multiples ventanas"
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
		- Elige "DuckDuckGo"
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
		- Haz Click en "Limpiar Historial"
			- Rango de tiempo para limpiar: "Todo"
			- Selecciona todas las opciones de "Historial"
			- Selecciona todas las opciones de "Datos"
	- Barra de direcciones
		- Desactiva todos los items

**Modifica Flags**
> [!TIP] Lecturas Recomendadas
> - [Reddit - r/firefox - What are Your Must Have Changes in about:config? - 001Guy001 Comment](https://old.reddit.com/r/firefox/comments/17hlkhp/what_are_your_must_have_changes_in_aboutconfig/k6ogtuv/)

Debes buscar en la barra del navegador `about:config` y dale click a "Aceptar el riesgo y continuar"

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
- `mousewheel.default.delta_multiplier_y` a `150`

> Fisicas suaves nuevas
- `general.smoothScroll.msdPhysics.enabled` a `true`

> Abre un marcador en una pestaña nueva
- `browser.tabs.loadBookmarksInTabs` a `true`

> Consume mas RAM, menos HDD (Funciona en maquinas mas antiguas)
- `browser.cache.disk.enable` a `false`
- `browser.cache.memory.capacity` a `-1`

> Elimina anuncios
- `browser.vpn_promo.enabled` to `false`
- `browser.newtabpage.activity-stream.feeds.recommendationprovider` to `false`
- `browser.newtabpage.activity-stream.showSponsored` to `false`
- `extensions.htmlaboutaddons.recommendations.enabled` to `false`

> Deshabilita los reportes de crasheo
- `breakpad.reportURL` to `""`
- `browser.tabs.crashReporting.sendReport` to `false`

> Configura GFX.accelerated
- `gfx.canvas.accelerated.cache-items` to `4096` | default `2048`
- `gfx.canvas.accelerated.cache-size` to `1024` | default `256`
- `gfx.content.skia-font-cache-size` to `20` | default `5`

**Despidete de la maquina**

Finalmente Apaga la maquina...

# Exportacion

Ahora quita el disco VHD

Exporta como OVF 2.0

Y quita las MAC.

Y espera sus 30 minutos...

# Extra
## Gracias
Cuando estaba asumiendo que era imposible conseguir una exportacion de OVA mas pequeña, le pedi ayuda a Dark Bird Tech ([Youtube](https://www.youtube.com/@darkbirdtech/videos)) y me dio una gran idea, utilizar discos secundarios VHD con los instaladores para no utilizar el espacio del VDI, el cual es tonto y no elimina el espacio libre, dejo ambas respuestas originales:

```
@Answer 1

Hi Gabo,
I skimmed through your write-up (I relied on auto-translate. I never got far learning Spanish), and it is a very thorough process you've done there, and considering the security issues you are trying to address, I can see why your image is sitting at around 23GB.  
  
So, I didn't do that much that was special apart from keeping what I wrote to the VM's disk to a minimum as dynamically expanding doesn't automatically decrease, even if you free up storage.

For the Windows 7 installation, I went to the Internet Archive and found an already updated ISO file ([https://archive.org/details/windows-7-iso](https://archive.org/details/windows-7-iso)) and downloaded Win7Pro English x64. So updates were already installed, and didn't have to be extracted to be installed, and pushing the disk space up.

I used Oracle VirtualBox V7.0.26R168464 to make my VM, but I don't think that made much difference, and I used OVF1.0 when I exported the image.

Another trick I used was to make a VHD on my Windows host machine in Disk Management (VHD, not VHDX), format it and all that and copy across whatever I wanted to install. Then I would detach that VHD from the host OS, and attach it as a secondary virtual drive in VirtualBox (it does work with VHDs, but not VHDXs). Once the VM booted and saw the additional drive, I would install directly from that secondary drive, again avoiding the VM's main VDI from needing to expand. I only copied across what I absolutely needed to.

That's probably how I managed to keep it so compact, which is convenient for me to use Google Drive to make it available to everyone.

I did think about trying to make my VM secure to go online, but decided against it. It would have given me more work, introduced risk to people using it, and given me a liability issue, which is why I advise against my VM being given internet access, and have the vNIC set to 'Not Attached' by default.  

...

Again, I am really impressed by your write-up for setting it all up and your efforts to make it run securely. I hope the info in this email helps you continue that great work.  

@Answer 2

Apologies for the VHD part of my mail being confusing. I probably should have said I treat it like a USB flash drive between the host and the VM. I think the reason the trick isn't that widely known is because there are commands for managing more advanced functions for VirtualBox, but by the time people want to do that, they have moved onto more professional types of hypervisors. I like VirtualBox for it's ease of use and accessibility for people who may not be that familiar with running VMs.

At some point I do want to create some Huawei training content there to add to my eNSP setup videos.

Kind regards,  
DBT
```

## Haiku
Yo cayendo en la locura...

> Japones Romanizado
```
"Kōdo no tsubasa
kā panikku no taiyō
log wa kiezu ni."
```

> Ingles
```
"Code wings ablaze
the sun, a kernel panic
logs outlive the crash."
```

> Español
```
"Alas de código
el sol es un kernel panic
pero el log persiste."
```

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

## Invalido (Por ahora)

**Cambia el Idioma**
> [!TIP] Lecturas Recomendadas
> - [Internet Archive - Windows 7 - MUI all language packs](https://archive.org/details/windows-7-mui-all-language-packs-sp0-sp1-x86-x64)

Descargas `windows6.1-kb2483139-x64-es-es_fdbdf4061b960324efb9eedf7106df543ed8ce33.exe`, desde el menu "`Windows 7 SP1 64-bit (x64) MUI Language Packs`"

**Actualizaciones**
> [!TIP] Lecturas Recomendadas
> - [Blog Simplix Info - UpdatePack7R2 (Ucranian)](https://blog.simplix.info/updatepack7r2/) | [Update7](https://blog.simplix.info/update7/) | [MajorGeeks Mirror](https://www.majorgeeks.com/files/details/simplix_updatepack.html)
> - [Blog Simplix Info - VCRedist 1.3 (Ucranian)](https://blog.simplix.info/vcredist/)

Las firmas de drivers mas nuevos para windows 7 necesitan de "`KB4474419`" para soportar las firmas SHA-2
- www.catalog.update.microsoft.com/ScopedViewInline.aspx?updateid=38222b3b-2bd2-4269-880d-35feddf42984


PARECE QUE NOS PODEMOS DESHACER DE WINPCAP YEAH BABY
WinPcap 4.1.3 | [WinPcap](https://www.winpcap.org/install/)



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

## Labs
https://tiagojsilva.github.io/en/networking/simul/gns3-ensp/
https://www.telecomate.com/es/mastering-ensp-pro-image-creation-a-network-engineers-practical-guide/


Para las fotos de los labs en eNSP, puedes usar los labs

https://getsharex.com/

https://youtu.be/NyIdfdIOyEM?si=Vp7OTSsBpP6iIFLK

https://youtu.be/kvd9Q8HB-QU?si=isWA9ohGPgpih71J

https://github.com/y-guang/eNSP-topologies

https://github.com/shehab06/Huawei_Network_HRP