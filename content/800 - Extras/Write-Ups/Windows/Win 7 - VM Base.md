# Info

> [!TIP] Lecturas Recomendadas
> - [Proxmox Docs - Windows 7 Guest Best Practices](https://pve.proxmox.com/wiki/Windows_7_guest_best_practices)
> - [Superuser Forum - How to select paravirtualization interface in Virtualbox](https://superuser.com/questions/945910/how-to-select-paravirtualization-interface-in-virtualbox)
> 	- [Virtualbox Docs - Manual #10.5. Paravirtualization Providers](https://www.virtualbox.org/manual/ch10.html#gimproviders)
> - [SuperUser Forum - What are difference between VBoxVGA, VMSVGA and VBoxSVGA](https://superuser.com/questions/1403123/what-are-differences-between-vboxvga-vmsvga-and-vboxsvga-in-virtualbox)
> - [Github virtio-win/virtio-win - Issue #40 - Widnows 7 no more working](https://github.com/virtio-win/virtio-win-pkg-scripts/issues/40)
> 	- [This reply](https://github.com/virtio-win/virtio-win-pkg-scripts/issues/40#issuecomment-1704103962)
> - [Windows 7 DotNet support](https://learn.microsoft.com/en-us/dotnet/core/install/windows#windows-7--81--server-2012) | [Powershell DotNet Framework vs DotNet Core](https://learn.microsoft.com/en-us/powershell/scripting/whats-new/differences-from-windows-powershell?view=powershell-7.5#net-framework-vs-net-core)

Win 7 es EOL, ya se le acabo el ESU, asi que no se si seguira siendo utilizable para un VM y que no sea un peligro publico

# Preparacion
## Disco VHD Base

**Crear Disco VHD Base**

En tu host, debes abrir Virtualbox, ir al menu "Herramientas" y seleccionar desde la barra "Medio" (Administrador de discos).
1. En las opciones superiores, seleccionas "Crear"
2. Dentro de la ubicacion, al final, cambias el nombre de "`NewVirtualDisk`" a "`Base`" (O el nombre que mas te guste)
3. El tamaño del disco puede ser de 10GB a 15GB
4. Cambiar es tipo de disco a "VHD (Virtual Hard Disk)"

**Añadir Disco VHD a Maquina con Internet**

Ahora en tu maquina con acceso a internet, abres las Configuraciones
1. Vas al menu "Almacenamiento"
2. Seleccionas "Controlador: Sata" y apretas el boton "Añadir Conexion" y seleccionas "Disco Duro"
3. En la ventana "Selector de medio", seleccionas el disco creado "`Base.vhd`" (Posiblemente este en el menu "Not Attached")
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
	- Etiqueta del volumen: ``
8. Das click en "Siguiente"
9. Ahora click en "Finalizar"

## Descargar Materiales

El disco VHD se utiliza como un Pendrive, debido a que el Windows 7 normal no tiene acceso a internet

- Drivers
	- VCRedist Repack | [Major Geeks - Visual C Redistribute Runtimes AIO Repack](https://www.majorgeeks.com/files/details/visual_c_redistributable_runtimes_aio_repack.html)
- Fuentes
	- Nerd Font | [Official Page](https://www.nerdfonts.com/) | [Github - ryanoasis/nerd-fonts](https://github.com/ryanoasis/nerd-fonts)
- Herramientas
	- Massgrave | [Official Page](https://massgrave.dev/) | [Github - massgravel/Microsoft-Activation-Scripts](https://github.com/massgravel/Microsoft-Activation-Scripts)
	- 7zip | [7-zip](https://www.7-zip.org/)
	- Firefox ESR 115.29.0 | [Mozilla Release FTP - Firefox 115.37.0esr win64](https://releases.mozilla.org/pub/firefox/releases/115.37.0esr/win64/)
	- WinCDEmu | [Github - sysprogs/WinCDEmu - Releases](https://github.com/sysprogs/WinCDEmu/releases)
- Updates
	- UpdatePack7R2 | [Blog Simplix Info - UpdatePack7r2 (Russian)](https://blog.simplix.info/updatepack7r2/) | [Blog Simplix Info - Update7 (Russian)](https://blog.simplix.info/update7/) | [MajorGeeks - Full exe Mirror](https://www.majorgeeks.com/files/details/simplix_updatepack.html)

**Ordenar Materiales**

Recomiendo el siguiente esquema que fue utilizado en la guia, no es obligatorio ordenarlo, pero te sera mas facil utilizarlo
```
/vhd-Base (D:\)/
├── Drivers/
│   └── VisualCppRedist_AIO_x86_x64
├── Fonts/
│   └── HackNerdFont.ttf
├── Tools/
│   ├── Microsoft-Activation-Scripts-master/
│   ├── 7z2409-x64.exe
│   ├── Firefox Setup 115.37.0esr
│   └── WinCDEmu-4.1.exe
└──  Updates
    └── UpdatePack7R2-25.6.10
```

Ahora deberas apagar el VM y entrar a sus configuraciones

Vas a "Almacenamiento", eliminas el iso de instalacion y añades una conexion a un Disco Duro, te saldra un menu, añades el disco VHD creado antes "`Base.vhd`", le das en "Aceptar" e inicias el VM

# Creacion del VM
**Desde QEMU/KVM**

Configuracion de QEMU (virt-viewer)

Vista General
- Nombre: Win7-Base
- Titulo: Win7 Base
- Chipset: Q35
- Firmware: BIOS
CPU (Depende Obviamente de tu cantidad de CPU, en mi caso, tengo 1 CPU, con 2 Nucleo, 4 Hilos)
- Configuracion: Marcar "host-passthrough"
- Topologia
	- Marcar: "Establecer Manualmente la topologia de CPU"
	- Socket: 1
	- Centros: 2
	- Hilos: 2
Memoria (Depende cuanta memoria tengas)
- Asignacion Actual: 4096 o 8196
- Asignacion Maxima: 4096 o 8196

Le das en instalar nomas

**Desde Virtualbox**

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
	- Creas un disco duro virtual (VDI) de 32GB

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

**Instalacion de Windows 7**

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
11. Seleccionas la ubicacion del equipo en "Red de Trabajo", y saldra el escritorio y lo apagas

# Instalacion
**Materiales Previos**
- Windows 7: Puede ser [Original desde Massgrave](https://massgrave.dev/windows_7_links) o Modificada, recomiendo la de Tester Machine [FastOS7v4](https://www.projectfastos.top/2025/03/fastos-7.html) | [Link Mediafire Directo](https://www.mediafire.com/file/09pnm2rh17vr9hz/FastOS7V4FEx64B10.iso/file)

Das en "Aceptar" y estamos listos para instalar

## Instalacion de Windows 7
Enciende Windows, y te hable el instalador

- Pantalla de inicio
	- idioma: Español (España)
	- Formato y moneda: Español (Chile)
	- Teclado o metodo entrada: Latinoamerica
- Le das en "Instalar Ahora", cargara por un momento (~15 segundos)
- Te pedira una licencia para activar windows, seleccionas "No tengo clave del producto"
- Ahora seleccionas "FastOS 7 Pro x64" y das en "Siguiente"
- Lees los terminos y condiciones, la aceptas y das en "Siguiente"
- Seleccionas "Personalizada: Instalar solo Windows (Avanzada)"
	- Seleccionas el disco vdi de 40GB como unidad y simplemente seleccionas "Siguiente"
- Empezara la instalacion, demora aproximadamente ~5 minutos en un SSD NVMe, luego se reinicia automaticamente
- (Esto me paso por porfiao) Si te sale una ventana donde windows no pudo iniciar correctamente, posiblemente es porque lo instalaste con EFI activado, desactivalo y vuelve a probar

**Configuracion Inicial Windows 7**

- Creacion del usuario administrador
	- Nombre de usuario: alumno
	- Nombre de equipo: alumno-PC
- Contraseña
	- Duoc.2025 (Indicios: "D u o c . 2 0 2 5" sin espacios)
- Selecciona "Usar la configuracion recomendada"
- Como configuramos la Hora UTC desde hardware, ahora la hora deberia estar automatica, sino, ajustala y apreta "Siguiente"
- Con esto finaliza los ultimos toques de configuracion, se demora unos ~25 segundos.
- Luego se reinciara y hara unos ajustes extras

---
**Post Instalacion por AST**
- Selecciona el disco que estes usando en el host, en mi caso "NVMe-M.2"
- Opciones por mensajes
	- Desactivar Windows Update: no
	- Funcione "Bluetooth": No, solo si no lo usaras
	- Menu Contextual: No
	- Optimizar la CPU: Si
	- Optimizar la GPU: No, al ser virtualizado podria dar problemas
	- Funciones Rapidas: No
	- Instalar Fast Menu: No. En caso de si quererlo, presiona Si y luego Avanzado, Luego "Instalar", se demora un rato
	- Tipo de optimizacion: "Oficina" o "Gaming", NUNCA LAPTOP, porque bloquea funciones del sistema
- Continua con "Optimizar", se demora un minuto
- Luego apretas en "Finalizar" y despues otra vez en "Finalizar" para salir de AST, cierra sesion, se reinicia y deberia Iniciar Windows
- Se conectara a Ethernet, te pedira una red, le das en "Red Domestica", luego das en "Siguiente" y finalmente en "Finalizar"

---
**Detalles en Virtualbox**
NOTA: Parece que debes instalar extpack para el uso remoto

Apagas la maquina, abres las configuraciones de la maquina

- Abres el menu "Almacenamiento"
	- Eliminas la unidad optica de instalacion "Win 7 - FastOS 7 v4 FE x64 Boot10.iso", te saldra un menu y aceptas
	- Añades una conexion de unidad optica, seleccionas "Añadir" y haces el proceso con los siguientes 2 archivos:
		- VBoxGuestAdditions.iso
		- virtio-win.iso
	- Luego das en "Aceptar"

## Paravirtualizacion
> [!TIP] Lecturas Recomendadas
> - [WinCDEmu Download](https://wincdemu.sysprogs.org/download/)
> - [WinCDEmu Wiki - Mount an ISO](https://wincdemu.sysprogs.org/tutorials/mount/)

Debes tener una forma de montar los .iso para instalar su contenido, recomiendo WinCDEmu, facil, como y sencillo

### Virtualbox
- Ahora con "Virtualbox Guest Additions"
	- Instala "VBoxWindowsAdditions-amd64" (o x86) como administrador
	- Seleccionas 3 veces "Next", se empezara a instalar
	- Saldra un popup, seleccionas "Siempre confiar en el software de Oracle Corporation" y en "Instalar"
	- Saldra otro popup, seleccionas "Instalar este software de controlador de todas formas"
	- Cuando termine de instalar, simplemente le das en "reboot now" y la maquina ahora iniciara con los drivers correctos

### QEMU/KVM
> [!TIP] Lecturas Recomendadas
> - [Virtio Wiki - Driver Installation](https://virtio-win.github.io/Knowledge-Base/Driver-installation.html)
> - [Proxmox Wiki - Qemu-guest-agent](https://pve.proxmox.com/wiki/Qemu-guest-agent)
> - [Fedora - Virtio Download](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/?C=M;O=D) | [0.1.173-4](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/virtio-win-0.1.173-4/)

Debido a que Windows 7 es una version ya viejita, no se comportara bien con sistemas modernos. Debes descarga exactamente la version "0.1.173-4"

Abres la carpeta "virtio-win"
- Instala "virtio-win-gt-x64" (o x86)
- Seleccionas "Next", lees y aceptas la licencia y le das en "Next"
- Sale un menu de caracteristicas, debes deshabilitar "Spice Agent", luego le das en "Next" y esperas que se instale

Abres la carpeta "Qemu-Agent"
- Instalas "qemu-ga-x86_64"

# Configuracion

**Navegador**

Tienes 2 opciones para probar
1. Aun puedes instalar Firefox ESR 115.25.0 (24 de Junio de 2025)
2. [Github adeii/supermium-portable](https://github.com/adeii/supermium-portable/releases)", probe "[Firefox Portable 132 x64](https://github.com/adeii/supermium-portable/releases)"

- Instala [Ublock Origin](https://ublockorigin.com/)
- Eliminas todos los marcadores y dejas la barra de marcadores solo disponible para nuevas pestañas
- Vas a los ajustes del navegador
	- General
		- Haces el navegador tu opcion por defecto
		- Activas el modo oscuro, para que no duelan los ojos
	- Home
		- Homepage and new windows: Blank Page
		- New Tabs: Blank Page
		- Desactivas "Shortcuts"
	- Search
		- Default Search Engine: DuckDuckGo, el mejor de todos los males
		- Personalmente desactivo "Provide search suggestions"
		- En "Search Shortcut" desactivo todos excepto DuckDuckGo y las utilidades de Firefox
	- Privacy & Security
		- Browser Privacy: Strict
		- En "Cookies and Site Data" Activa la opcion "Delete cookies and site data when Slimbrowser is closed"
		- En "Login and Password" desactiva la opcion "Ask to save logins and passwords for website"
		- En "History", Slimbroser will: "Never remember history" y reinicia el navegador desde el popup (Intenta otra vez activar esta opcion porque cambia)

**Cambiar de Professional a Ultimate**
Partamos por activar Windows, se utilizara [Massgrave](https://massgrave.dev/#method-2---traditional-windows-vista-and-later) de manera tradicional, para windows 7, se utiliza el metodo [TSForge](https://massgrave.dev/tsforge). El metodo Tradicional, debes descargar directamente desde [Github (Autodescarga)](https://github.com/massgravel/Microsoft-Activation-Scripts/archive/refs/heads/master.zip), extraes el .zip, luego abres las carpetas hasta llegar a "All-In-One-Version" y ejecutas "MAS_AIO.cmd" con permisos de administrador

- Ahora lo primero que haremos sera cambiar la version de Professional a Ultimate, por lo que cuando cargue el script, seleccionamos "`[7] Change Windows Edition`"
	- Luego nos detectara la version, y seleccionamos "`[1] Ultimate`" y presionamos "Enter", 
	- Nos avisa que cuando termine de hacer el cambio se reiniciara automaticamente, presionamos "`[1] Continue`" y el proceso empezara, se demora ~3.5 minutos y luego se actualiza, reiniciandose 2 veces

**Activar Windows**
- Ejecutamos otra vez "MAS_AIO.cmd" en modo administrador
	- Seleccionamos "`[3] TSforge`", luego "`[1] Activate - Windows`", hara unas validaciones, y luego saldra un mensaje "`[Ultimate] is permanently activated with ZeroCID`", apretamos cualquier tecla y cerramos la ventana

**Actualizar Sistema**
Con el sistema activado, aprovechamos de actualizarlo para no tener problemas de compatibilidad con las herramientas que aun existen
- NOTA: Por alguna razon se demora 1 hora, asi que hace otras cosas por mientras, luego que termine de buscar, instala las actualizaciones importantes solo de Windows 7
- Abre "Panel de Control", "Sistema" y luego "Windows Update", selecciona "Buscar Actualizaciones". Las actualizacion son:
	- Obligatorios
		- 2022-12 Paquete Acumulativo de .NET Framework (KB5021091)
		- Seguridad Windows 7 y IE11 (KB3185319)
		- Seguridad Windows 7 (KB2676562)
		- Seguridad Windows 7 (KB2813347)
		- Actualizar Windows 7 (KB2952664)
	- Opcionales
		- 2019-09 - Actualizacion Windows 7 x86 (KB4493132)
		- 2022-01 - Actualizacion Windows 7 x86 (KB5010798)
		- 2022-10 - Paquete Actumulativo .NET Framework (KB5018547)
		- Actualizacion Windows 7 (KB3021917)
		- Actualizacion Windows 7 (KB3068708)
		- Actualizacion Windows 7 (KB3080149)
		- Actualizacion Windows 7 (KB3118401)
		- Actualizacion Windows 7 (KB3150513)
		- Actualizacion Windows 7 (KB3172605)
		- Actualizacion Microsoft Edge (KB5001027)

Instala los siguientes programas
- BCUninstaller | [Github Klocman/Bulk-Crap-Uninstaller](https://github.com/Klocman/Bulk-Crap-Uninstaller)
- Wireshark | [x64](https://www.wireshark.org/download.html) (4.2.2)
- VLC | [Download](https://www.videolan.org/vlc/download-windows.html)
- Powershell | [x86](https://github.com/PowerShell/PowerShell/releases/tag/v7.2.24) 7.2.x (7.2.24) |
	- C++ 2015-2019 Redistributable | [x64](https://aka.ms/vs/16/release/vc_redist.x64.exe) | [x86](https://aka.ms/vs/16/release/vc_redist.x86.exe)
	- (No es aplicable para el sistema x86) KB3063858 | [x64](https://www.microsoft.com/download/details.aspx?id=47442) | [x86](https://www.microsoft.com/download/details.aspx?id=47409)
	- Microsoft Root Certificate Authority 2011 | [Download crl](https://www.microsoft.com/pkiops/Docs/Repository.htm)
	- DotNet 6.0 | [Download](https://dotnet.microsoft.com/en-us/download/dotnet/6.0)
	- Windows Management Framework 5.1 (KB3191566) | [Microsoft Docs - WMF](https://learn.microsoft.com/en-us/powershell/scripting/windows-powershell/wmf-overview?view=powershell-7.5) | [Microsoft Docs - WMF Availability](https://learn.microsoft.com/en-us/powershell/scripting/windows-powershell/wmf-overview?view=powershell-7.5#wmf-availability-across-windows-operating-systems) | [Download](https://www.microsoft.com/en-us/download/details.aspx?id=54616)
- SSH via Win32-OpenSSH | [Github PowerShell/Win32-OpenSSH](https://github.com/PowerShell/Win32-OpenSSH/releases)
- Putty | [Download Snapshot](https://www.chiark.greenend.org.uk/~sgtatham/putty/snapshot.html)
- Cmder | [Official Page](https://cmder.app/) | [Github cmderdev/cmder](https://github.com/cmderdev/cmder) | Recomiendo Full
- eNSP -> Sigue [[500 - Personal/500.3 - Write-Ups/eNSP/Instalar eNSP|Instalar eNSP]]

Modificar las opciones con "`netplwiz`"
- Modificar ambos usuarios como administradores
- Desactivar "Los usuarios deben escribir su nombre y contraseña para usar el equipo" y apreta "Aceptar" y escribes la contraseña "Duoc.2025"

# Extras
## Desactivar Servicios
Los servicios que dejaria encendidos
- Auto
	- AudioSrv
	- Themes
	- DcomLaunch + RPCSS
	- PlugPlay
	- LanmanServer + LanmanWorkstation
	- DHCP + DNS Client
	- Winmgmt (WMI)
	- CryptSvc
	- EventLog
	- Tcpip NetBIOS Helper
	- NlaSvc
- Manual
	- Spooler
	- 

> Windows Update
```
sc config wuauserv start= disabled     # Windows Update (UNA VEZ parcheado)
```
> Security Center
```
sc config wscsvc   start= disabled     # Security Center
```
> Aero
```
sc config Themes   start= disabled     # Aero/Temas
```
> Superfetch
```
sc config SysMain  start= disabled     # Superfetch
```
> Indexador
```
sc config WSearch  start= disabled     # Indexing
```
> Tablet Input Service
```
sc config TabletInputService start= disabled
```
> Link Tracking
```
sc config TrkWks start= disabled     # Distributed Link Tracking
```

---
**Deshabilitar Tareas Programadas**
Puedes encontrarlo dentro de "Programador de Tareas"
- Biblioteca del Programador de Tareas > Microsoft > Windows
	- Deshabilitar
		- End of Support
		- Defrag

> Posiblemente
```
net user

net user "nombre_de_usuario"

net localgroup Administrators "nombre_de_usuario" /add

secedit /configure /cfg %windir%\inf\defltbase.inf /db defltbase.sdb /verbose

netsh advfirewall set allprofiles state on

netdom remove "NombrePC" /domain:"Tester Machine" /ud:UsuarioAdmin /pd:Contraseña

regedit

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon

net user Administrator /active:yes

dism /online /cleanup-image /restorehealth


```


## Pasos para Exportar

> Limpia Roll-Back
```
dism /online /cleanup-image /spsuperseded
```


1. Limpia los datos extras con "`cleanmgr`", debes limpiar cache, archivos temporales y puntos de restauracion
2. Desinstala componenentes del sistema con BCUnnistaler
	- Winrar
	- Internet Explorer
3. Deshabilita Windows Defender y Windows Update, ya que esta actualizada la maquina
4. Limpia los espacios vacios (Zerofile) con "`cipher /w:C:\`"

Apaga la maquina
1. `VBoxManage modifymedium "disk.vdi" --compact`

## Deprecated

Ya no utilizare FastOS 7 v4 para los sistemas que cree, pero me ayudo
Sistema:
- Basado gran parte en el trabajo de [FastOS 7 v4 Pro F.E (Final Edition)](https://www.projectfastos.top/2025/03/fastos-7.html) by [Tester Machine](https://www.youtube.com/c/TesterMachine). La cual esta basada en Windows 7 Professional Version 6.1 SP1 (Compilacion: 7601), se recomienda apoyar usando el acordator [Cuty](https://cuty.io/VOPMYM5tpVuC), pero dejare el Link directo a [Mediafire - FastOS7V4FEx64B10](https://www.mediafire.com/file/09pnm2rh17vr9hz/FastOS7V4FEx64B10.iso/file)
	- Integrado Bypass ESU (Solo para Recibir Actualizaciones de Microsoft Security Essentials)
	- Integrado las ultimas actualizaciones 2023 - 2024
	- Certificados Raíz Actualizados (La navegación web funciona perfectamente).
	- Integrado AST v3.1.1 (Mas configuraciones y correcciones).
	- Nuevo Menú Extendido (Fast Menu)
	- Integrado Net Framework 4.8 + Updates
	- Integrado Visual C++ Ultima Versión
	- Integrado DirectX Ultima Versión.
	- Integrado XNA Framework 3.0/3.1/4.0 Refresh
	- Integrado .NET Desktop Runtimes
	- Integrado NVidia PhysX
	- Integrado WinRAR, Notepad2Mod
	- Integrado Drivers USB 3.0/3.1 Genéricos
	- Integrado SlimBrowser (Navegador liviano basado en Firefox)
	- Integrado UnHIDER USB File (Para Desinfectar USBs) cortesía de hiberhernandez.com
	- Integrado Nuevo Administrador de Tareas estilo W10
	- Script Fast OS Extras: Un script capaz de cambiar el administrador de tareas, actualizar certificados raíz e instalar varios navegadores web compatibles con Windows 7. Ubicado en el Menu de Inicio > Todos los programas