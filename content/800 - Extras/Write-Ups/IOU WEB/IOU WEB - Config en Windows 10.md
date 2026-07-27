# Info
> [!IMPORTANT] Notas antes de empezar
> Virtualbox no es el mejor emulador de VM que haya existido y windows no es la mejor plataforma siendo sinceros...
> Hay que modificar varias cosas para que funcione con la menor cantidad de problemas posibles

## Virtualizacion
**Virtualizacion Compartida**
[Existen un par de plataformas de virtualizacion](https://en.wikipedia.org/wiki/Comparison_of_platform_virtualization_software) pero siempre se oyen 3 competidoras:
- VMWare
- Hypr-V
- Virtualbox

La que genera mas problemas es Hypr-V al ser un componente nativo de Windows, si tienes problemas en tus maquinas (Como una tortuga con una V en la barra de estado de virtualbox), deberas desactivarlo con los siguientes comandos
Puedes leer mas info en
- [Forums Virtualbox - scottgus1 - HMR3init: Attempting fall back to NEM (Hypr-V is Active)](https://forums.virtualbox.org/viewtopic.php?f=25&t=99390)
- [SuperUser - Biswapriyo - Why Can't Virtualbox or VMware run with Hypr-V enabled on Windows 10](https://superuser.com/questions/1208850/why-cant-virtualbox-or-vmware-run-with-hyper-v-enabled-on-windows-10)

> 1. Registra al virtualizador Hypr-V como apagado
```
bcdedit /set hypervisorlaunchtype off
```

> 2. Deshabilita el componente de Hypr-V
```
DISM /Online /Disable-Feature:Microsoft-Hyper-V
```

> 3. Elimina completamente la funcion Hypr-V
> Nota: En caso de que falle el segundo comando, ejecutalo sin la bandera -all
```
PowerShell Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor -All
```

---
**Seguridad de Windows 11**
En algunos casos, al igual que con Hypr-V no funcionara correctamente la virtualizacion debido a la seguridad del aislamiento del nucleo

1. Ve a `Seguridad de Windows > Seguridad del dispositivo > Aislamiento del nucleo`
2. Y Desactiva `Aislamiento del nucleo` y luego reiniciar la maquina

---
## Virtualbox Config
Esta configuracion tiene en mente la version de IOU WEB de 64 bits `icarus` y `nirvana` para que tengan el mejor rendimiento.

Recuerda que los cambios se hacen con la maquina apagada y configurar la maquina por cada punto desde el menu de configuraciones de virtualbox


Creas una nueva VM, le pones un nombre creativo con los siguientes datos
- Nombre y Sistema Operativo
	- Nombre: iou-web-icarus
	- Tipo: Linux
	- Subtype: Red Hat
	- Version: Red Hat (64 bits)
- Omite Instalacion Desatendida
- Hardware
	- Memoria Base: 4096MB
	- Procesadores: 2vCPU
	- Omite Habilitar EFI (Por ahora)
- Disco Duro
	- Selecciona "Usa un archivo de disco duro existente"
	- Seleccion un archivo de disco virtual
		- Apreta "Añadir" y busca el disco que descargaste, posiblemente `iou-web-icarus.vdi`
- Haz Click en "Terminar"

Ahora seleccionas la maquina virtual, le das en "Configuracion" y activas el "modo experto"

**General**
Basico
- Omite estas configuraciones, ya estan aplicadas

**Sistema**
Placa Base
- Memoria Base: Configura `4096` o `8196` dependiendo de cuanta ram tengas disponible
- Chipset: ICH9
- Dispositivo Apuntador: Tableta USB
- Caracteristicas Extendidas:
	- Marca "Habilitar I/O APIC"
	- Marca "Reloj Hardware Tiempo UTC"
	- Desmarca "EFI"
	- Desmarca "Secure Boot"

Procesador
- Procesadores: Selecciona el maximo que permita la barra verde
- Limite de ejecucion: 100%
- Caracteristicas Extendidas:
	- Marca "Habilitar PAE/NX"
	- Al final podras marcar "Habilitar VT-X/AMD-V Anidado"

Aceleracion
- Interfaz de Paravirtualizacion: "KVM"
- Hardware de Virtualizacion
	- Marca "Habilitar paginacion anidada"

**Pantalla**
Pantalla
- Memoria de Video: Rango 64MB - 128MB (Es mas importante para una GUI, no es el caso ahora)
- Controlador Grafico: VMSVGA, tengo entendido que funciona mejor, ademas no hay errores
- Caracteristicas Extendidas:
	- Marca "Habilitacion Aceleracion 3D"
- Ignora las secciones Pantalla Remota y Grabacion, todo va desmarcado

**Almacenamiento**
Controladora: IDE
- Presiona el boton "Eliminar controlador"
Controladora: SATA
- Cantidad de puertos: 2
- Atributos: Marca "Usar cache de I/O anfitrion"
iou-web-icarus.vdi
- Unidad de estado solido:
	- Marca SOLO Si tienes SSD -> Check
	- Desmarca SOLO Si tienes Disco Duro -> No Check

**Audio**
Audio: Deja las opciones por defecto

**Red**
Adaptador 1
- Conectado a: "Adaptador Puente"
- Nombre: "La misma que tu interfaz de red que quieres conectar"
- Tipo de Adaptador: "virtio-net" o "Intel Pro/1000 MT Server (82545EM)"
- Modo Promuisco: "Permitir Todo"
- Direccion MAC: RENOVAR SIEMPRE
- Cable Conectado: Habilitado, por dios, imaginate desactivar esto, un terror

**Puerto Serial**: Dejalos por defecto

**USB**
- Habilita Controlador USB, Obligatorio para que funcione la pantalla tactl emulada
- Version Controlador: 2.0 (OCHI+EHCI), Mayor Compatibilidad
Interfaz de Usuario
- Minibarra de herramientas: Desmarcar "Mostar en pantalla completa/fluido"

**Lo demas dejalo como esta**

---
**Forzar marcar "Habilitar VT-X/AMD-V Anidado"**
Tambien puedes forzar la habilitacion de la opcion Nested VT-X que esta marcada en gris, aunque varios foros dicen que aunque la fuerzes no hara nada con el sistema. Pero es mejor que sosobre a que falte

> Ve a la carpeta donde instalaste Virtualbox
```
cd C:\Program Files\Oracle\VirtualBox\
```

> Habilita la opcion virtualizada, "VM-Name" es el nombre de la maquina virtual
```
.\VboxManage modifyvm "iou-web-icarus" --nested-hw-virt on
```

> Forzar Nested VT-X en Linux
```
VBoxManage modifyvm "iou-web-icarus" --nested-hw-virt on
```

---
## Programas
- Hack Nerd Font
- Putty
- Superputty
- TightVNC
- WinSCP

## Putty
**Instala Fuente Hack**
1. Accedes el Sitio Oficial de [Nerd Fonts](https://www.nerdfonts.com/)
2. Apretas donde dice "Downloads"
3. Buscas "Hack", debera aparecer "Hack Nerd Fonts" (tambien puede ser tu letra favorita la verdad)
4. Apretas el boton "Download", te descarga un archivo Hack.zip
5. Lo descomprimes y abres su carpeta
6. Le das doble click a "HackNerdFont-Regular.ttf", te abrira una pestaña del administrador de fuentes y apretas "Instalar"
7. Puedes borrar la carpeta "Hack" y el archivo "Hack.zip"

---
**Instalar Putty**
Necesitas instalar [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/snapshot.html), siempre recomiendo la version Snapshot porque esta mas actualizada, aunque si prefieres estabilidad, puedes eligir la version estable, independiente que rama elijas, siempre instala el paquete completo "`64-bit x86`"

> [!IMPORTANT] Notas sobre la ruta de instalacion
> El archivo .reg esta hecho para que funcione cuando se instala directamente en `C:\`, esto es para evitar problemas con los idiomas en Programs Files y rutas de instalacion, puedes editar las rutas manualmente y funcionaran si no quieres tener desordenado `C:\`


> **Configura Putty.exe**

> [!IMPORTANT] Notas sobre Putty
> - WIN: El archivo por defecto en el registro del sistema esta en la ruta `HKEY_CURRENT_USER>Software>SimonTatham>PuTTY>Sessions>Default%20Settigs`
> - Para empezar desde 0, debes escribir `Crtl+r`, abrira una pestaña de ejecucion, luego escribir `regedit` y darle al enter, te abrira el registro del sistema, luego debes buscar y borrar la carpeta `HKEY_CURRENT_USER>Software>SimonTatham`, despues abres `Putty.exe` y en la seccion de "`Saved Sessions`" escribes "`Default Settings`" y le apretas al boton de "`Save`", esto volvera a crear las rutas en regedit con las configuraciones por defecto.

Abre Putty y ajusta las siguientes configuraciones

**Session**
- Saved Session: Default Settings

**Logging**
- Session Logging: None

**Bell**
- Action to happen when a bell occurs: NONE

**Windows**
NOTA: Las lineas de scrollback son las lineas que guarda Putty, el default es 2000, pero con 500-1000 yo creo que queda mejor, es lo que hace que quede mas lento
- Columns: 100
- Rows: 40
- Lines of scrollback: 1000

**Appearance**
Nota: Descarga alguna [Nerd Font](https://www.nerdfonts.com/font-downloads) (Recomiendo "Hack"): luego instala HackNerdFont-Regular, HackNerdFontMono-Regular y HackNerdFontPropo-Regular en tu computador
- Cursor Appearance: Vertical Line
- Cursor Blink: Checked
- Font: Change: Fuente: Hack Nerd Font Mono Regular | Estilo de Fuente: Normal | Tamaño: 12
- Font Quality: Default

**Selection**
- Assing Copy/Paste -> Ctrl+Shift + {C,V}: System Clipboard

**Colours**
> [!IMPORTANT] Notas sobre colores
> - Seran cambiados con la configuracion de "`Putty-Registry.reg`" a la paleta de colores [Nord](https://www.nordtheme.com/) a travez de las configuraciones en [nordtheme/putty](https://github.com/nordtheme/putty)
> - Puede crear tus paletas de colores usando [4bit Terminal Color Scheme Designer](http://ciembor.github.io/4bit/) y descargando la paleta en .reg

---
Putty.reg para abrir sesiones Telnet y SSH
```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\telnet]
@="URL:Telnet Putty"
"URL Protocol"=""
"FriendlyTypeName"="@C:\\WINDOWS\\system32\\ieframe.dll.mui,-907"

[HKEY_CLASSES_ROOT\telnet\DefaultIcon]
@=hex(2):25,00,53,00,79,00,73,00,74,00,65,00,6d,00,52,00,6f,00,6f,00,74,00,25,\
  00,5c,00,73,00,79,00,73,00,74,00,65,00,6d,00,33,00,32,00,5c,00,75,00,72,00,\
  6c,00,2e,00,64,00,6c,00,6c,00,2c,00,30,00,00,00

[HKEY_CLASSES_ROOT\telnet\shell]
[HKEY_CLASSES_ROOT\telnet\shell\open]
[HKEY_CLASSES_ROOT\telnet\shell\open\command]
@="\C:\\putty.exe %1"

[HKEY_CLASSES_ROOT\ssh]
@="URL:SSH Putty"
"URL Protocol"=""

[HKEY_CLASSES_ROOT\ssh\shell]
[HKEY_CLASSES_ROOT\ssh\shell\open]
[HKEY_CLASSES_ROOT\ssh\shell\open\command]
@="\C:\\putty.exe %1"
```

---
Putty.reg para abir sesiones Telnet y SSH + Configuracion de Putty
> [!IMPORTANT] Notas sobre configuraciones extra
> Esta version tiene un par de mejoras explicadas mas arriba aplicadas de forma automatica, para hacer una instalacion mas rapida
```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\telnet]
@="URL:Telnet Putty"
"URL Protocol"=""
"FriendlyTypeName"="@C:\\WINDOWS\\system32\\ieframe.dll.mui,-907"

[HKEY_CLASSES_ROOT\telnet\DefaultIcon]
@=hex(2):25,00,53,00,79,00,73,00,74,00,65,00,6d,00,52,00,6f,00,6f,00,74,00,25,\
  00,5c,00,73,00,79,00,73,00,74,00,65,00,6d,00,33,00,32,00,5c,00,75,00,72,00,\
  6c,00,2e,00,64,00,6c,00,6c,00,2c,00,30,00,00,00

[HKEY_CLASSES_ROOT\telnet\shell]
[HKEY_CLASSES_ROOT\telnet\shell\open]
[HKEY_CLASSES_ROOT\telnet\shell\open\command]
@="\C:\\putty.exe %1"

[HKEY_CLASSES_ROOT\ssh]
@="URL:SSH Putty"
"URL Protocol"=""

[HKEY_CLASSES_ROOT\ssh\shell]
[HKEY_CLASSES_ROOT\ssh\shell\open]
[HKEY_CLASSES_ROOT\ssh\shell\open\command]
@="\C:\\putty.exe %1"

[HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\Default%20Settings]
"Present"=dword:00000001
"HostName"=""
"LogFileName"="D:\\putty.log"
"LogType"=dword:00000001
"LogFileClash"=dword:00000001
"LogFlush"=dword:00000001
"LogHeader"=dword:00000001
"SSHLogOmitPasswords"=dword:00000001
"SSHLogOmitData"=dword:00000000
"Protocol"="ssh"
"PortNumber"=dword:00000016
"CloseOnExit"=dword:00000001
"WarnOnClose"=dword:00000001
"PingInterval"=dword:00000000
"PingIntervalSecs"=dword:00000000
"TCPNoDelay"=dword:00000001
"TCPKeepalives"=dword:00000000
"TerminalType"="xterm"
"TerminalSpeed"="38400,38400"
"TerminalModes"="CS7=A,CS8=A,DISCARD=A,DSUSP=A,ECHO=A,ECHOCTL=A,ECHOE=A,ECHOK=A,ECHOKE=A,ECHONL=A,EOF=A,EOL=A,EOL2=A,ERASE=A,FLUSH=A,ICANON=A,ICRNL=A,IEXTEN=A,IGNCR=A,IGNPAR=A,IMAXBEL=A,INLCR=A,INPCK=A,INTR=A,ISIG=A,ISTRIP=A,IUCLC=A,IUTF8=A,IXANY=A,IXOFF=A,IXON=A,KILL=A,LNEXT=A,NOFLSH=A,OCRNL=A,OLCUC=A,ONLCR=A,ONLRET=A,ONOCR=A,OPOST=A,PARENB=A,PARMRK=A,PARODD=A,PENDIN=A,QUIT=A,REPRINT=A,START=A,STATUS=A,STOP=A,SUSP=A,SWTCH=A,TOSTOP=A,WERASE=A,XCASE=A"
"AddressFamily"=dword:00000000
"ProxyExcludeList"=""
"ProxyDNS"=dword:00000001
"ProxyLocalhost"=dword:00000000
"ProxyMethod"=dword:00000000
"ProxyHost"="proxy"
"ProxyPort"=dword:00000050
"ProxyUsername"=""
"ProxyPassword"=""
"ProxyTelnetCommand"="connect %host %port\\n"
"ProxyLogToTerm"=dword:00000001
"Environment"=""
"UserName"=""
"UserNameFromEnvironment"=dword:00000000
"LocalUserName"=""
"NoPTY"=dword:00000000
"Compression"=dword:00000001
"TryAgent"=dword:00000001
"AgentFwd"=dword:00000000
"GssapiFwd"=dword:00000000
"ChangeUsername"=dword:00000000
"Cipher"="aes,chacha20,aesgcm,blowfish,3des,WARN,arcfour,des"
"KEX"="ntru-curve25519,ecdh,dh-gex-sha1,dh-group16-sha512,dh-group17-sha512,dh-group18-sha512,dh-group15-sha512,dh-group14-sha1,rsa,WARN,dh-group1-sha1"
"HostKey"="ed448,ed25519,ecdsa,rsa,dsa,WARN"
"PreferKnownHostKeys"=dword:00000001
"RekeyTime"=dword:0000003c
"GssapiRekey"=dword:00000002
"RekeyBytes"="1G"
"SshNoAuth"=dword:00000000
"SshBanner"=dword:00000001
"AuthTIS"=dword:00000000
"AuthKI"=dword:00000001
"AuthGSSAPI"=dword:00000001
"AuthGSSAPIKEX"=dword:00000001
"GSSLibs"="gssapi32,sspi,custom"
"GSSCustom"=""
"SshNoShell"=dword:00000000
"SshProt"=dword:00000003
"LogHost"=""
"SSH2DES"=dword:00000000
"PublicKeyFile"=""
"RemoteCommand"=""
"RFCEnviron"=dword:00000000
"PassiveTelnet"=dword:00000000
"BackspaceIsDelete"=dword:00000001
"RXVTHomeEnd"=dword:00000000
"LinuxFunctionKeys"=dword:00000000
"NoApplicationKeys"=dword:00000000
"NoApplicationCursors"=dword:00000000
"NoMouseReporting"=dword:00000000
"NoRemoteResize"=dword:00000000
"NoAltScreen"=dword:00000000
"NoRemoteWinTitle"=dword:00000000
"NoRemoteClearScroll"=dword:00000000
"RemoteQTitleAction"=dword:00000001
"NoDBackspace"=dword:00000000
"NoRemoteCharset"=dword:00000000
"ApplicationCursorKeys"=dword:00000000
"ApplicationKeypad"=dword:00000000
"NetHackKeypad"=dword:00000000
"AltF4"=dword:00000001
"AltSpace"=dword:00000000
"AltOnly"=dword:00000000
"ComposeKey"=dword:00000000
"CtrlAltKeys"=dword:00000001
"TelnetKey"=dword:00000000
"TelnetRet"=dword:00000001
"LocalEcho"=dword:00000002
"LocalEdit"=dword:00000002
"Answerback"="PuTTY"
"AlwaysOnTop"=dword:00000000
"FullScreenOnAltEnter"=dword:00000001
"HideMousePtr"=dword:00000001
"SunkenEdge"=dword:00000000
"WindowBorder"=dword:00000002
"CurType"=dword:00000002
"BlinkCur"=dword:00000001
"Beep"=dword:00000000
"BeepInd"=dword:00000000
"BellWaveFile"=""
"BellOverload"=dword:00000001
"BellOverloadN"=dword:00000005
"BellOverloadT"=dword:000007d0
"BellOverloadS"=dword:00001388
"ScrollbackLines"=dword:000005dc
"DECOriginMode"=dword:00000000
"AutoWrapMode"=dword:00000001
"LFImpliesCR"=dword:00000000
"CRImpliesLF"=dword:00000000
"DisableArabicShaping"=dword:00000000
"DisableBidi"=dword:00000000
"WinNameAlways"=dword:00000001
"WinTitle"="IOU WEB"
"TermWidth"=dword:00000050
"TermHeight"=dword:00000018
"Font"="Hack Nerd Font"
"FontIsBold"=dword:00000000
"FontCharSet"=dword:00000000
"FontHeight"=dword:0000000c
"FontQuality"=dword:00000003
"FontVTMode"=dword:00000004
"UseSystemColours"=dword:00000000
"TryPalette"=dword:00000000
"ANSIColour"=dword:00000001
"Xterm256Colour"=dword:00000001
"TrueColour"=dword:00000001
"BoldAsColour"=dword:00000001
"Colour0"="216,222,233"
"Colour1"="216,222,233"
"Colour2"="46,52,64"
"Colour3"="46,52,64"
"Colour4"="46,52,64"
"Colour5"="216,222,233"
"Colour6"="59,66,82"
"Colour7"="76,86,106"
"Colour8"="191,97,106"
"Colour9"="191,97,106"
"Colour10"="163,190,140"
"Colour11"="163,190,140"
"Colour12"="235,203,139"
"Colour13"="235,203,139"
"Colour14"="129,161,193"
"Colour15"="129,161,193"
"Colour16"="180,142,173"
"Colour17"="180,142,173"
"Colour18"="136,192,208"
"Colour19"="143,188,187"
"Colour20"="229,233,240"
"Colour21"="236,239,244"
"RawCNP"=dword:00000000
"UTF8linedraw"=dword:00000000
"PasteRTF"=dword:00000000
"MouseIsXterm"=dword:00000000
"RectSelect"=dword:00000000
"PasteControls"=dword:00000000
"MouseOverride"=dword:00000001
"Wordness0"="0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0"
"Wordness32"="0,1,2,1,1,1,1,1,1,1,1,1,1,2,2,2,2,2,2,2,2,2,2,2,2,2,1,1,1,1,1,1"
"Wordness64"="1,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,1,1,1,1,2"
"Wordness96"="1,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,1,1,1,1,1"
"Wordness128"="1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1"
"Wordness160"="1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1"
"Wordness192"="2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,1,2,2,2,2,2,2,2,2"
"Wordness224"="2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,1,2,2,2,2,2,2,2,2"
"MouseAutocopy"=dword:00000001
"MousePaste"="explicit"
"CtrlShiftIns"="explicit"
"CtrlShiftCV"="none"
"LineCodePage"="UTF-8"
"CJKAmbigWide"=dword:00000000
"UTF8Override"=dword:00000001
"Printer"=""
"CapsLockCyr"=dword:00000000
"ScrollBar"=dword:00000001
"ScrollBarFullScreen"=dword:00000001
"ScrollOnKey"=dword:00000000
"ScrollOnDisp"=dword:00000001
"EraseToScrollback"=dword:00000001
"LockSize"=dword:00000000
"BCE"=dword:00000001
"BlinkText"=dword:00000000
"X11Forward"=dword:00000000
"X11Display"=""
"X11AuthType"=dword:00000001
"X11AuthFile"=""
"LocalPortAcceptAll"=dword:00000000
"RemotePortAcceptAll"=dword:00000000
"PortForwardings"=""
"BugIgnore1"=dword:00000000
"BugPlainPW1"=dword:00000000
"BugRSA1"=dword:00000000
"BugIgnore2"=dword:00000000
"BugHMAC2"=dword:00000000
"BugDeriveKey2"=dword:00000000
"BugRSAPad2"=dword:00000000
"BugPKSessID2"=dword:00000000
"BugRekey2"=dword:00000000
"BugMaxPkt2"=dword:00000000
"BugOldGex2"=dword:00000000
"BugWinadj"=dword:00000000
"BugChanReq"=dword:00000000
"StampUtmp"=dword:00000001
"LoginShell"=dword:00000001
"ScrollbarOnLeft"=dword:00000000
"BoldFont"=""
"BoldFontIsBold"=dword:00000000
"BoldFontCharSet"=dword:00000000
"BoldFontHeight"=dword:00000000
"WideFont"=""
"WideFontIsBold"=dword:00000000
"WideFontCharSet"=dword:00000000
"WideFontHeight"=dword:00000000
"WideBoldFont"=""
"WideBoldFontIsBold"=dword:00000000
"WideBoldFontCharSet"=dword:00000000
"WideBoldFontHeight"=dword:00000000
"ShadowBold"=dword:00000000
"ShadowBoldOffset"=dword:00000001
"SerialLine"="COM1"
"SerialSpeed"=dword:00002580
"SerialDataBits"=dword:00000008
"SerialStopHalfbits"=dword:00000002
"SerialParity"=dword:00000000
"SerialFlowControl"=dword:00000001
"WindowClass"=""
"ConnectionSharing"=dword:00000000
"ConnectionSharingUpstream"=dword:00000001
"ConnectionSharingDownstream"=dword:00000001
"SSHManualHostKeys"=""
"DetachedCertificate"=""
"AuthPlugin"=""
"SshNoTrivialAuth"=dword:00000000
"SUPDUPLocation"="The Internet"
"SUPDUPCharset"=dword:00000000
"SUPDUPMoreProcessing"=dword:00000000
"SUPDUPScrolling"=dword:00000000
"ShiftedArrowKeys"=dword:00000000
"BugDropStart"=dword:00000001
"BugFilterKexinit"=dword:00000001
"BugRSASHA2CertUserauth"=dword:00000000
```

## Superputty (WIP)

Para instalar Superputty necesitas los siguientes programas
- [Superputty](https://github.com/jimradford/superputty)
- [UltraVNC](https://uvnc.com/)
- [Filezilla Server](https://filezilla-project.org/)
- [WinSCP](https://winscp.net/eng/download.php)

Superputty Completo + Configuracion

> [!IMPORTANT] Notas sobre .reg nuevo
> - A pesar de haber instalado el anterior .reg, este de sobreescribe, fijate en tener correctamente las rutas donde instalaste los programas
```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\telnet]
@="URL:Telnet SuperPutty"
"URL Protocol"=""
"FriendlyTypeName"="@C:\\WINDOWS\\system32\\ieframe.dll.mui,-907"

[HKEY_CLASSES_ROOT\telnet\DefaultIcon]
@=hex(2):25,00,53,00,79,00,73,00,74,00,65,00,6d,00,52,00,6f,00,6f,00,74,00,25,\
  00,5c,00,73,00,79,00,73,00,74,00,65,00,6d,00,33,00,32,00,5c,00,75,00,72,00,\
  6c,00,2e,00,64,00,6c,00,6c,00,2c,00,30,00,00,00

[HKEY_CLASSES_ROOT\telnet\shell]
[HKEY_CLASSES_ROOT\telnet\shell\open]
[HKEY_CLASSES_ROOT\telnet\shell\open\command]
@="\C:\\superputty.exe %1"

[HKEY_CLASSES_ROOT\ssh]
@="URL:SSH SuperPutty"
"URL Protocol"=""

[HKEY_CLASSES_ROOT\ssh\shell]
[HKEY_CLASSES_ROOT\ssh\shell\open]
[HKEY_CLASSES_ROOT\ssh\shell\open\command]
@="\C:\\superputty.exe %1"

[HKEY_CLASSES_ROOT\vnc]
@="URL:VNC Protocol"
"URL Protocol"=""

[HKEY_CLASSES_ROOT\vnc\shell]
[HKEY_CLASSES_ROOT\vnc\shell\open]
[HKEY_CLASSES_ROOT\vnc\shell\open\command]
@="\"C:\\Program Files\\uvnc bvba\\UltraVNC\\vnc_wrapper.bat\" %1"
```

## SecureCRT (WIP)
Nunca lo he probado, pero debe ser weno, deberia seguir la misma logica de cambiar la rutas de ejecucion de putty.exe a securecrt.exe

En un sitio persa encontre SecureCRT 9.6.3.3599 [Link](https://soft98.ir/internet/network/15209-vandyke-securecrt.html) | [Mirror](https://duoccl0-my.sharepoint.com/:u:/g/personal/ga_zunigam_duocuc_cl/ERmdolx7optLuH4e8twCovsBmKD0gNr-Mneq1XcnmAxMAg?e=FdnVhL)
