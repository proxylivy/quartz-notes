# Info
## Datos
PARECE QUE NECESITAS HACERLO EN 64 BITS ;P
Y que wireshark al final y al cabo si puede ser la mas moderna


Estas son las ultimas versiones compatibles con Windows 32 bits, estan disponibles en [Onedrive](https://duoccl0-my.sharepoint.com/:f:/g/personal/ga_zunigam_duocuc_cl/EuCvefQ9MB1Br3OH4Iqon6UBkFA8pfqbXkYACdvtlEiDBA?e=AJDBlS)
- Virtualbox | [x86](https://download.virtualbox.org/virtualbox/5.2.44/) (5.2.44) | x64 (5.2.44) | Es el limite de eNSP
- Wireshark | x86 (???) | x64 (4.2.2 o 4.0.17)
- eNSP Setup V100R002C00B510 (1.2.00.510) y su actualizacion V100R003C00SPC100 (1.3.00.100)
- Npcap | [x64](https://npcap.com/) tiene una API compatible con WinPcap, por lo que se podria cambiar eventualmente, se instala la version 1.7.8 al instalar Wireshark 4.2.2 en x64

Lee sobre eNSP
- [Huawei Forums - Download eNSP simulator installation Software](https://forum.huawei.com/enterprise/intl/en/thread/download-ensp-simulator-installation-software-here/667238396713648128?from=latestPostsReplies&blogId=667238396713648128)
- [Huawei Forums - Resource Downloading for eNSP](https://forum.huawei.com/enterprise/intl/en/thread/resources-downloading-for-ensp/667245683301826561?blogId=667245683301826561)
- [Huawei Forums - Download eNSP USG6000v Image](https://forum.huawei.com/enterprise/intl/en/thread/download-ensp-usg6000v-image/667245289389572096?blogId=667245289389572096)
- [Huawei Forums - Download eNSP NE40E Image](https://forum.huawei.com/enterprise/intl/en/thread/ne40e-image-for-ensp-v100r003c00spc100/667245683289243648?blogId=667245683289243648)
- [Huawei Forums - How to solve the NE40E/NE9000/NE5000E/CX200/CE6800/CE12800 start timeout on eNSP](https://forum.huawei.com/enterprise/intl/en/thread/how-to-solve-the-ne40e-ne9000-ne5000e-cx200-ce6800-ce12800-start-timeout-on-ensp/667227419901313025?blogId=667227419901313025)
- [Huawei Forums - Network Simulation Tools 2022 Challenge](https://forum.huawei.com/enterprise/intl/en/collection/667213828401807360?mod=collection&action=view&ctid=699&themeId=667213828401807360)
- [Youtube - Dark Bird Tech](https://www.youtube.com/@darkbirdtech)
	- [Youtube - Install eNSP on Windows 10 in 2025](https://youtu.be/Zx4A_Vu22Q8?si=RNh-wOPUVLe2hnci)
	- [Youtube - Adding the missing devices in Huawei eNSP v1.3](https://youtu.be/WH7xrq8Mqx8?si=nJfUunV92hITMcIZ)
		- [Google Drive - eNSP Optional Extra Devices](https://drive.google.com/drive/folders/1xhb71AzTOcy_IvVnjFi0rEme6iGZJ7uy) | [Mirror - Onedrive](https://duoccl0-my.sharepoint.com/:f:/g/personal/ga_zunigam_duocuc_cl/EjgfGZjaSOxGvwhi6Z0C7scBNLstbk1iFPxq23X1N5B0IA?e=XPUmZI)
	- [Youtube - Rundown of basic eNSP tools](https://youtu.be/NyIdfdIOyEM?si=8nwUta3FknINy8KV)

Los dispositivos que faltan en eNSP 1.3.00.100 son:
- Router NE Series (NE40E) (NE5000E) (NE9000)
- Router CX Series (CX200)
- Switch CE Series (CE6800)
- USG6000V Firewall (using vfw_usg.vdi)

**Changelog 1.3.00.100**
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

# Instalacion
Se recomienda usar una maquina Win 7, sigue esta guia [[500 - Personal/500.3 - Write-Ups/Optimizar VM Win 7 64 bits|Optimizar VM Win 7 64 bits]], sobre esa maquina instalaras Virtualbox, WinPcap, Wireshark y por ultimo eNSP Setup.

1. Instala eNSP Setup 1.2.00.510
	- Selecciona "English" y luego seleccionas el boton de la izquierda
	- Luego seleccionas "Next"
	- Lees el acuerdo, lo aceptas y apretas "Next"
	- Dejas la ruta por defecto "`C:\Program Files\Huawei\eNSP`" y apretas "Next"
	- Apretas "Next" y luego "Next"
	- Ahora Instalara WinPcap 4.1.3, Wireshark (1.4.3) y Virtualbox (5.1.24), le das en "Next", descomprimira y te saldras algunos popup
		- WinPcap
			- Simplemente "Next" con las opciones por defecto hasta que se termine de instalar
		- Wireshark
			- Simplemente "Next" con las opciones por defecto hasta que termine de instalar
		- Virtualbox
			- Simplemente "Next" con las opciones por defecto hasta que termine de instalar
			- Recuerda dar que "Si" a la interfaz de red, y confiar siempre en certificados de "Oracle Corporation"
			- No inicies Virtualbox luego de instalar
	- Recuerda desmarcar las opciones "Launch eNSP" y "show update log" para que no inicie y le das en "Finish"
2. Instala eNSP Setup 1.3.00.100
	- Empieza la instalacion, "Next", recuerda leer, aceptar la licencia y apretar "Next"
	- Dejas la misma ruta por defecto "`C:\Program Files\Huawei\eNSP`" y apretas "Next"
	- "Next" y "Next"
	- Ahora hara una verificacion de las versiones exactas instaladas por la anterior version, los tres items deberian decir "`It is detected that {program} has been installed on your computer`" y das en "Next"
	- Verificas los datos de instalacion y das en "Install"
	- Luego de instalar, puedes mantener "Launch eNSP" para revisar el estado
3. Instala Virtualbox 5.2.44
	- Ejecuta e instala de la misma manera


## Configuracion
Instala Hack [Nerd Font](https://www.nerdfonts.com/)
Cambiar Fuentes
Ve a Opciones > Fonts > CLI Fonts: y elegimos Hack Nerd Font 10



¿Que Beneficios Tiene?
- Windows 7 Permanentemente activado con TSForge
- Programas actualizados
	- eNSP Actualizado a 1.3.00.100 (V100R003C00SPC100)
	- Virtualbox Actualizado a 5.2.44-139111
	- Wireshark actualizado a 3.6.16
- Imagenes Actualizadas
	- Router NE Series (NE40E) (NE5000E) (NE9000)
	- Router CX Series (CX200)
	- Switch CE Series (CE6800)
	- USG6000V Firewall (using vfw_usg.vdi)


Tomando como base el OVA del vitoco, permite la ejecucion de USG6000V Firewall
## Activar Licencia por siempre
Descarga Massgrave desde el [Link autodescarga](`https://github.com/massgravel/Microsoft-Activation-Scripts/archive/refs/heads/master.zip`)

Luego lo descomprimes, abres carpetas, ejecutas la cosa, luego 3, y luego 1, y fin, activado para siempre

## Virtualbox Guest Addons

Descarga desde el [centro de descargas](https://download.virtualbox.org/virtualbox/) la ultima version, en mi caso 7.0.10

Descarga [Wincdemu](https://wincdemu.sysprogs.org/) para montar imagenes .iso si es que no hay ninguno

Instala los drivers y reinicia la maquina

Ahora deberia poder ajustarse automaticamente a la ventana

## Instala RSYNC
> [!TIP] Sitios Recomendados
> - [Said - Install Rsync on Windows](https://ayewo.com/how-to-install-rsync-on-windows/)
> - 



Asi poder pasar las imagenes desde un host mas rapido


## Actualizar eNSP
Descarga la imagen fea

Next
Acepta la licencia y Next
Next

Espera que se instale la actualizacion, desmarca "Launch eNSP" y "Show update log"

## Actualiza los dispositivos faltantes

Primero debes descargar las imagenes necesarias desde algun lugar misterioso

Lo dejas en alguna carpeta igual de misteriosa, se me podria ocurrir `C:\Program Files\Huawei` en una carpeta llamada `Extra Images`

Luego debes abrir eNSP, arrastrar un nodo de cada router que quieras instalar, tengo las imagenes de
- CE6800 (Using CE.xd)
- USG6000V Firewall (using vfw_usg.vdi)

Luego debes dar segundo click y apretar en "Start", te saldra una ventana llamada "Import Package", pidiendote la ruta y el paquete que necesitas

Y le das la ruta `C:\Program Files\Huawei\Extra Images\`, la imagen correspondiente y le das en "Import"

Ahora le das el segundo click, "Start", ahora la imagen deberia empezar a cargar, puede demorarse de 3-5 minutos en recien iniciar, asi que paciencia

Y fin, con eso tienes las ultimas imagenes funcionando



Luego lo inicias y listo

Bueno, debes iniciar sesion
- User: `admin`
- Pass: `Admin@123`

Cuando inicia, debes cambiar la contraseña, yo elegi `Wena@123`

