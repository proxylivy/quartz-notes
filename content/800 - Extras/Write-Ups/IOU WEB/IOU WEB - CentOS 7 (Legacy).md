# Info
La version recomendad es: [[300 - Conocimiento/Write-Ups/IOU-WEB/IOU WEB - Atlas - Rocky Linux 8.10|IOU WEB - Atlas - Rocky Linux 8.10]]

## ¿Que hay de nuevo?

IOU WEB Interface 64 Bits
Based on work of Andrea Dainese (dainok) and modded by @Proxylivy

Version 2 (Icarus):
- Instalacion desde .iso limpio en 64 bits
- IOU WEB Actualizado (`1.2.2-23`)
- IOU WEB, Base de datos limpia, logs limpios y arreglado los tiempos de espera
- Activada la Paravirtualizacion de CPU para instrucciones VT-X y emulacion KVM en virtualbox
- Instalacion y configuracion de drivers virtio
- Instalacion Guest Additions de virtualbox
- Muestra IP Automatica cuando aparece el login de inicio de sesion
- CentOS Actualizado a `7.9-2207` + Epel y Remi Release
- Kernel Actualizado a `6.9.7-1`, ElRepo Release (Con 91 modulos)
- Mejoras Guest a Virtualbox, QEMU/KVM y VMware
- No IPTABLES, No Firewall
- Hora y Fecha Sincronizadas con NTP Chile
- Links simbolicos de las nuevas imagenes, reemplazando las antiguas
- Varias servicios innecesarios abajo
- Documentacion lo mas completa y transparente posible
- Instalacion base con UEFI y soporte BIOS legacy
- `$PATH` configurado para priorizar programas compilados, IOU WEB y luego el sistema

> [!WARNING] Sobre Emulacion x86_64
> - A pesar de ser una instalacion 64 bits, no soporta los routers y switch XE (`x86_64_crb_linux`), intente compilar una version de GLIBC mayor a >=2.27 (2.34) y solo XE L3 funciona sin problemas, XE L2 tiene problemas extraños y no me da el cerebro para solucionarlos
> - Si quieres ayuda, puedes leer [[300 - Conocimiento/Write-Ups/IOU-WEB/IOU WEB - Intentos Fallidos|IOU WEB - Intentos Fallidos]], compilar los otros programas y solucionar el problema de los XE L2, y me mandas un correo ^^
> - La compilacion de OpenSSL y OpenSSH todavia no se hace realmente

## Porfiado y Creativo: Asi nacio IOU WEB Icarus

Este proyecto nacio durante las clases de *Routing y Switching Coporativo* de 5to Semestre de la carrera de Ing. Conectividad y Redes (DuocUC). Un profesor muy particular, Victor Araneda (Vitoco), nos enseño a utilizar IOU WEB, soltando frases que se me quedaron muy grabadas. Una en especial me hizo mucho ruido:

> "Cuando configuren `ip nat inside` dentro de una interfaz, ese router quedara cargando por una hora."

A simple vista no parecia normal, asi que empeze a investigar. Descubri que IOU WEB tiene dos ramas principales de soporte en su epoca: una basada en la familia Debian y otra en RedHat. Durante 1 año estuve trabajando en la variante de 32 bits de CentOS 6.x como lo documente en [[300 - Conocimiento/Write-Ups/IOU-WEB/IOU WEB - 32 Bits CentOS 6 (Legacy)|IOU WEB - 32 Bits CentOS 6 (Legacy)]], pero siempre me incomodo no contar con un entorno completamente funcional de 64 bits.

A partir de esa inquietud, decidi contruir el sistema desde cero, haciendo ingeniera inversa al funcionamiento, investigando sobre la estructura, apache2, php y mas intentos de otras personas por lograr algo parecido. 

Pero tambien comenzo la verdadera odisea, IOU WEB no recibe mas actualizaciones desde 2014-2015, utiliza un stack muy especifico, `php56`, `apache2` y otras dependencias que han ido cambiando con el tiempo. Intente solventar los errores de plano con Rocky Linux 8.10, pero a pesar de escribir mucha documentacion, nunca logre hacer funcionar esa version, asi que la archive. Luego probe con Arch Linux, pero por ahora, el paquete `php56` esta roto en AUR, asi que lo abandone y archive hasta nuevo aviso.

Al final lo clasico siempre gana, y me decante por CentOS 7.9 (2207) la cual es la ultima version de la rama 7.x, y fue el mejor equilibrio, la unica gran aspereza son `glibc 2.17` (deberia ser 2.27), OpenSSL y OpenSSH un poco viejos y otros detalles menores, pero el sistema como reemplazo directo de 32 bits, es espectacular

Aprendi que *es mejor un pajaro practico en mano que cien teoricos en el aire*. El papel aguanta mucho, pero hacer que un entorno asi funcione de verdad... ya es otro cuento. En este camino lei de todo, hasta unir muchas piezas sueltas que encontre en internet, probando y ajustando todo con cariño para la persona que este leyendo esto ^^

**¿Porque Icarus?**

El nombre IOU WEB Icarus nace de la leyenda de Icaro: el joven que volo demaciado cerca del sol y cayo al mar con sus alas derretidas. Asi veo el proyecto: Hermoso, ambicioso pero con limitaciones fuertes que lo llevaran tarde o temprano a su muerte

Porque aunque hoy existen opciones mejores como GNS3 o EVE-NG, IOU WEB tiene su encanto, especialmente para entornos livianos o Cisco-Only, utilizando imagenes en su version 15.x. 

Aun sabiendo sobre sus limites, *Quise hacerlo volar*

No para reemplazar lo moderno, sino para honrar lo antiguo y mostrar que aun no esta viejo, solo hay que cuidarlo

> [!QUOTE] QUOTE
> "dictum sapienti sat est"
> 
> — Plauto
> (Para el sabio, basta con una palabra)

# Preparacion
> [!TIP] Lecturas Recomendadas
> - [EndOfLife - CentOS EOL](https://endoflife.date/centos)
> - [CentOS - CentOS Linux EOL](https://www.centos.org/centos-linux-eol/)

## Descarga Materiales
CentOS, como producto, murio hace un tiempo, la ultima version en terminar de recibir soporte fue la rama 7.x que su soporte de seguridad termino el 30 de junio de 2024. Aun asi, existen distintos mirrors archivados que podemos utilizar y configurar para continuar utilizando CentOS.

La ultima rama disponible es la `7.9-2009` y la iso que recomiendo es la `CentOS-7-x86_64-Minimal-2207-02`

> [!TIP] Mirrors Disponibles
> - [CentOS - CentOS Vault](https://vault.centos.org/)
> - [Archive Kernel - CentOS Vault](http://archive.kernel.org/centos-vault/)
> - [Linux @ CERN - CentOS Vault](http://linuxsoft.cern.ch/centos-vault/)
> - [NSC LIU (National Supercomputer Centre at Linkoping University) - CentOS Store](http://mirror.nsc.liu.se/centos-store/)

## Comandos Utiles
> Para buscar el nombre exacto de un paquete, usas:
```
rpm -qa | grep <package>
```

> Puedes desinstalar paquetes sin eliminar ni buscar dependencias
```
rpm -e --nodeps <package-name>
```

> Para instalar un paquete local y buscar sus dependencias
```
rpm -ivh <package-name.rpm>
```

> Con estos tres comandos puedes buscar las flags de la cpu
```
grep -E 'vmx|svm' /proc/cpuinfo
lscpu
grep flags /proc/cpuinfo
```

> Ver el espacio de disco utilizado
```
df -h | awk 'NR==1 || $1 ~ /^\/dev\//'
```

## Creacion del VM
**Desde QEMU/KVM (Virt-viewer)**

*Etapa 1*
- El medio de instalacion sera "Medio de Instalacion Local (Imagen ISO o CDROM)", seleccionas adelante
- Luego "exploras" el .iso a instalar "`CentOS-x86-64-Minimal-2207-02`" y eliges el sistema operativo a instalar "`Red Hat Enterprise Linux 7.9 (rhel7.9)`"
- Despues le das en "Adelante" nomas
- Creas el disco de 20GB (Por defecto)
- Le cambias el nombre a IOU-WEB-Icarus
- Le das click en "Personalizar configuracion antes de instalar" y click en Finalizar, abrira el menu de la Etapa 2

*Etapa 2*

Vista General
- Nombre: IOU-WEB-Icarus
- Titulo: IOU WEB Icarus
- Chipset: Q35
- Firmware: UEFI
CPU (Depende Obviamente de tu cantidad de CPU, en mi caso, tengo 1 CPU, con 2 Nucleo, 4 Hilos)
- Configuracion: Marcar "host-passthrough"
- Topologia
	- Marcar: "Establecer Manualmente la topologia de CPU"
	- Socket: 1
	- Centros: 2
	- Hilos: 2
Memoria (Depende cuanta memoria tengas)
- Asignacion Actual: `4096` (Minimo), Idealmente `8196`
- Asignacion Maxima: Se autoconfigura con el valor que escribiste arriba

Seleccionas "Iniciar"

**Desde Virtualbox**
Si utilizas Virtualbox, tambien puedes crearla usando los siguientes datos

Crea una nueva VM, le pongo un nombre creativo con los siguientes datos
- Tipo: IOU WEB Icarus
- Version: Red Hat (64-bits)
- Memoria Base: 8196MB (Minimo 4096MB)
- Procesadores: 2vCPU (La verdad podria ser el maximo posible)
- Habilitar EFI
- Creas un disco duro virtual ahora de 20GB
- Le das en "Terminar" y seleccionas "Configuracion"
Seleccionas las siguientes configuraciones
- Basico (Omitir ya esta configurado)
- Sistema
	- Placa Base
		- Chipset: ICH9
		- Dispositivo Apuntador: Tableta USB
		- Habilitar reloj hardware en tiempo UTC
	- Procesador
		- Habilitar PAE/NX
		- Forzar Habilitar VT-x/AMD-V anidado con "`VBoxManage modifyvm "IOU WEB Icarus" --nested-hw-virt on`"
		- Interfaz de paravirtualizacion: "KVM"
		- Activar Hardware de virtualizacion
	- Pantalla
		- Memoria de Video: 128MB
		- Controlador Grafico: VBoxSVGA (Default)
		- Activar Aceleracion 3D
	- Almacenamiento (Depende si tienes SSD o HDD, si tienes HDD ignora esta parte)
		- Controlador SATA
			- Tipo: AHCI
			- Cantidad de puertos: 3
			- Activa Usar cache de I/O anfitrion
		- VM-name.vdi
			- Activa Unidad de estado solido
	- Audio (Omitido por defecto)
	- Red
		- Adaptador 1
			- Activar esta interfaz
			- Conectado a: Adaptador Puente
			- Tipo de adaptador: Defecto (Luego de la instalacion de drivers, sera configurado a "virtio-net")
			- Modo Promiscuo: Permitir todo
	- Puertos Serie (Omitido por defecto)
	- USB
		- Activar controlador USB
		- Seleccionar Controlador USB 2.0 (OHCI + EHCI)

# Instalacion

**Inicia el .iso de Instalacion**

Cuando inicies la ISO de CentOS 7.9, se presentara un menu grafico de instalacion. Aqui tienes los pasos escenciales de la configuracion basica del instalador:
1. Idioma y Localizacion
	- Idioma: Selecciona `Español`
	- Localizacion: Selecciona `Español (Chile)`
2. Configuracion del sistema
	- Regionalizacion: Se configura Automaticamente
	- Software: Se configura Automaticamente
	- Sistema
		- Selecciona "Destino de la Instalacion"
		- Selecciona la opcion "Voy a configurar las particiones" y luego presiona "Listo"
			- Del menu de puntos de montaje, cambia "LVM" a "Particion Estandar" y apretamos el boton con el icono "Mas"
				- Punto de Montaje: `/grub`
				- Capacidad: 5MiB
				- Sistema de archivos: `vfat` y apreta "Actualizar Parametros"
			- Apretamos el boton "mas"
				- Punto de Montaje: `/boot/efi`
				- Capacidad: 512MiB
				- Sistema de archivos: EFI System Partition
			- Seleccionamos "Particion Estandar" y apretamos el boton "mas"
				- Punto de Montaje: `/boot`
				- Capacidad: 1GiB
				- Sistema de archivos: `ext4` y selecciona "Actualizar Parametros"
			- Apretamos "mas"
				- Punto de Montaje `/`
				- Capacidad: todo, vacio y presionar "Enter"
				- Sistema de archivos: `ext4` y selecciona "Actualizar Parametros"
		- Seleccion "Listo" 2 veces (Ignoramos el mensaje de advertencia sobre la falta de Swap", no se necesita)
		- Selecciona "Red y Nombre del Equipo"
			- Enciendes la interfaz de red y le das en "Listo"
3. Ahora apretas en "Empezar Instalacion" y mientras carga, continua
4. Ajustes de Usuario
	- Contraseña de Root: Configura `cisco` en ambas y apreta 2 veces Listo
	- Creacion de Usuario
		- Nombre Completo: `duoc`
		- Nombre de Usuario: `duoc`
		- Marcar en "Hacer que este usuario sea administrador"
		- Contraseña: `cisco`
		- Confirma Contraseña: `cisco`
	- Apretas 2 veces en Listo y esperas que la instalacion termine (Se demora unos 6 minutos en un SSD)
5. Cuando termine apreta "Reiniciar", ya que el instalador no lo hace automaticamente

Como recomendacion, te recomiendo conectarte por ssh directamente mediante el usuario root, de esta forma todo es mas sencillo

La instalacion base consume 1.2GB

## Instalar Paquetes

> Borra todos los repos `/etc/yum.repos.d/` o simplemente borrar con `rm -f *.repo`
```
sudo rm -f /etc/yum.repos.d/*.repo
```

> [!TIP] Notas sobre `vi`
> - Abres el editor con `vi` y la ruta del archivo como cualquier otro editor
> - Apretar `i` y abrira el modo interactivo, yo copie y pegue cada linea, 1 por 1
> - Luego apretas el boton "`Esc`" y luego escribes "`:wq`" y asi guarda y sales

> Crea un repo en `/etc/yum.repos.d/` llamado "`CentOS-Vault.repo`"
```
[base]
name=CentOS-Vault - Base
baseurl=http://archive.kernel.org/centos-vault/7.9.2009/os/x86_64/
enabled=1
gpgcheck=0

[updates]
name=CentOS-Vault - Updates
baseurl=http://archive.kernel.org/centos-vault/7.9.2009/updates/x86_64/
enabled=1
gpgcheck=0

[extras]
name=CentOS-Vault - Extras
baseurl=http://archive.kernel.org/centos-vault/7.9.2009/extras/x86_64/
enabled=1
gpgcheck=0

[RHLo]
name=CentOS-Vault - RHLo
baseurl=http://archive.kernel.org/centos-vault/7.9.2009/sclo/x86_64/rh
enabled=1
gpgcheck=0
```

> Actualiza los repositorios
```
yum repolist
```

> Instala Nano y wget
```
sudo yum install nano wget
```

**Install Micro 2.0.13 (Editor)**

> Descarga el lanzamiento desde [Github - zyedida/micro](https://github.com/zyedidia/micro/releases/tag/v2.0.13)
```
wget https://github.com/zyedidia/micro/releases/download/v2.0.13/micro-2.0.13-linux64.tar.gz
```

> Descomprime el archivo tar
```
tar xvf micro-2.0.13-linux64.tar.gz
```

> Mueve el archivo micro a los binarios
```
sudo mv micro-2.0.13/micro /bin
```

> Elimina archivo sobrantes
```
rm -drf micro-2.0.13 micro-2.0.13-linux64.tar.gz
```

> Instala Epel Release
```
sudo yum install epel-release
```

> Instalar Remi Release
```
sudo rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
```

> Ve a `/etc/yum.repos.d`
```
cd /etc/yum.repos.d/
```

> Luego descarga el repositorio
```
wget https://download.opensuse.org/repositories/shells:fish:release:3/CentOS_7/shells:fish:release:3.repo
```

> Vuelve al sistema
```
cd
```

> Actualiza la base de datos
```
yum repolist
```

> Instala DNF
```
yum install dnf
```

> Actualiza los paquetes
```
dnf update
```

> Rescata CentOS-Vault.repo
```
mv /etc/yum.repos.d/CentOS-Vault.repo .
```

> Elimina todos los repos de CentOS e instala el repo del principio otra vez
```
rm -drf /etc/yum.repos.d/CentOS*.repo
```

> Vuelve a mover el repositorio
```
mv CentOS-Vault.repo /etc/yum.repos.d/CentOS-Vault.repo
```

> Actualiza la base de datos de DNF
```
dnf repolist
```

> Instalar Grupos de Paquetes
```
dnf groupinstall "Compatibility Libraries" "Development Tools"
```

> Instalar Paquetes compatibilidad 32 bits
```
dnf install glibc.i686 libstdc++.i686 zlib.i686 openssl-libs.i686 libpcap.i686 libX11.i686 libXext.i686 glibc-static libstdc++-static glibc.i686 openssl-devel.i686 xulrunner.i686 libcurl.i686 glibc-devel.i686
```

> Instalar Paquetes Programacion
```
dnf install cmake cmake3 libxml2-devel sqlite-devel libcurl-devel libjpeg-devel libpng-devel freetype-devel dialog open-vm-tools net-tools psmisc dos2unix gmp-devel libmpc-devel mpfr-devel dbus-devel patchelf strace perl-IPC-Cmd perl-Test-Simple perl-Net-Pcap.x86_64 texinfo python3 python3-pip bzip2-devel ncdu
```

> Instalar Paquetes Sueltos
```
dnf install libvirt virt-viewer qemu-guest-agent telnet-server xinetd htop tmux screen byobu httpd-devel dkms xclip xsel libcap-devel syslog-ng tftp-server lsof perl-IO-Tty  perl-Authen-PAM terminus-fonts-* perl-LDAP ntp terminus-fonts bind-utils telnet ImageMagick tree fish iperf3 yum-utils zstd mlocate hdparm xorg-x11-server-Xorg xorg-x11-drv-qxl xorg-x11-drv-vmware spice-vdagent spice-protocol remmina-plugins-spice xorg-x11-drv-fbdev xorg-x11-server-Xvfb gdisk mdadm cryptsetup ntfs-3g cifs-utils dmraid device-mapper-multipath ncurses-static openssl-static vtun sysstat
```

> Instala Dependencias de Grub
```
dnf install grub2-efi grub2-efi-x64-modules grub2-efi-modules shim
```

> Instala un modulo con PIP3
```
pip3 install abnf
```

> Actualiza la base de datos de locate
```
sudo updatedb
```

TALVEZ NO SEAN NECESARIOS php-mbstring

> Instalar PHP
```
dnf install php php-common php-cli php-curl php-fpm php-mysqlnd php-gd php-xml php-pdo php-zip php-sqlite3 php-pspell
```

> Instala las herramientas devtoolset-11 (SE SUSPENDE ESTO)
```
dnf install devtoolset-11
```

**Instala Busybox**
> [!TIP] Lecturas Recomendadas
> - [Pagina Oficial - Busybox](https://busybox.net/)

> Descargare la version que me funcione
```
wget https://busybox.net/downloads/binaries/1.31.0-defconfig-multiarch-musl/busybox-x86_64
```

> Configura los permisos de Busybox
```
chmod 755 ./busybox-x86_64
```

> Mueve el archivo bonito
```
mv ./busybox-x86_64 /bin/busybox
```

## Instalar Kernel
**Actualizar Kernel Pre-Compilado**
> [!TIP] Lecturas Recomendadas
> - [Alpha GNU Forum - Install and Upgrade to Kernel 6.5 in CentOS 7](https://www.alphagnu.com/topic/53-install-and-upgrade-to-kernel-65-in-centos-7centos-8-stream-cwp7-aapanel/): Por la idea
> - [The Linux Kernel Archives](https://kernel.org/)
> - [ElRepo Mirror - Coreix](https://mirrors.coreix.net/elrepo-archive-archive/kernel/el7/x86_64/RPMS/): Un mirror de ElRepo que aun tiene los kernels para RHEL7
> - [Wtarreau Blog - Look back too EOL kernel 3.10](http://wtarreau.blogspot.com/2017/11/look-back-to-end-of-life-lts-kernel-310.html)
> - [Kernel.org - Changelog Kernel 3.10.1](https://www.kernel.org/pub/linux/kernel/v3.x/ChangeLog-3.10.1)
> - [Kernel.org - Changelog 6.9.7](https://www.kernel.org/pub/linux/kernel/v6.x/ChangeLog-6.9.7)
> - [EndOfLife Dates - Linux Kernel](https://endoflife.date/linux)
> - [Wikipedia - Linux Kernel Version History](https://en.wikipedia.org/wiki/Linux_kernel_version_history)
> - [DNF Docs - Command Ref](https://dnf.readthedocs.io/en/latest/command_ref.html#options): Revisar opciones como `--allowerasing` y `--best`

El kernel es el corzon de la maquina, mas alto mejor, pero en los lanzamientos versionado, quedan atras, en los repos oficiales solo llega hasta la version 3.10, la cual tiene fecha de publicacion 8 de julio de 2013, EOL a finales de 2013 y la rama 3.x llego a EOL en 2017
```
Linux 3.10.0-1160.119.1.el7.x86_64
```

Pero gracias a un mirror de ElRepo, podemos actualizar el kernel, este kernel tiene fecha de publicacion "27 de junio de 2024"
```
Linux 6.9.7-1.el7.elrepo.x86_64
```

Para una actualizacion completa, necesitamos los siguientes archivos
```
kernel-ml-6.9.7-1.el7.elrepo.x86_64.rpm
kernel-ml-devel-6.9.7-1.el7.elrepo.x86_64.rpm
kernel-ml-headers-6.9.7-1.el7.elrepo.x86_64.rpm
kernel-ml-tools-6.9.7-1.el7.elrepo.x86_64.rpm
kernel-ml-tools-libs-6.9.7-1.el7.elrepo.x86_64.rpm
```

> [!CAUTION] Estado del Mirror
> Debido a que el repositorio esta roto, no se instala a los repos de DNF, sino que se instala directamente desde el URL con DNF, debes probar si el [repositorio](https://mirrors.coreix.net/elrepo-archive-archive/kernel/el7/x86_64/RPMS/) ([Alternativo](https://dl.lamp.sh/kernel/el7/)) esta en linea aun o no.

> Instalamos el Kernel-ML 6.9.7-1
```
dnf install https://mirrors.coreix.net/elrepo-archive-archive/kernel/el7/x86_64/RPMS/kernel-ml-6.9.7-1.el7.elrepo.x86_64.rpm
```

> Actualizamos Grub para aceptar el kernel mas nuevo automaticamente
```
grub2-set-default 0
```

> Actualizamos configuracion de GRUB para sistemas BIOS
```
grub2-mkconfig -o /boot/grub2/grub.cfg
```

> Actualizamos configuracion de GRUB para sistemas UEFI
```
grub2-mkconfig -o /boot/efi/EFI/centos/grub.cfg
```

> [!WARNING] Sobre el Reinicio
> El kernel 3.10 ya esta en el sistema, si uno instala otro no afectaria hasta que se inicie el mas nuevo, pero para poder borrar los paquetes, si deberas de reiniciar, entonces es mejor antes que despues, deberas asegurarte de seleccionar el nuevo kernel `6.9.7-1` desde el menu avanzado de CentOS

> Revisa todos los paquetes de kernel que tienes instalados
```
rpm -qa kernel*
```

> Deberas borrar todos los paquetes de paquetes que no tengan dependencias (Los siguientes se sobreescriben)
```
dnf remove kernel-3.10.0-1160.71.1.el7.x86_64 kernel-3.10.0-1160.119.1.el7.x86_64 kernel-tools-libs-3.10.0-1160.119.1.el7.x86_64 kernel-tools-3.10.0-1160.119.1.el7.x86_64
```

> Instala Kernel-Headers, reemplazando la version del paquete anterior con `--alowerasing`
```
dnf install https://mirrors.coreix.net/elrepo-archive-archive/kernel/el7/x86_64/RPMS/kernel-ml-headers-6.9.7-1.el7.elrepo.x86_64.rpm --allowerasing
```

> Instala Kernel-ML-Tools-libs
```
dnf install https://mirrors.coreix.net/elrepo-archive-archive/kernel/el7/x86_64/RPMS/kernel-ml-tools-libs-6.9.7-1.el7.elrepo.x86_64.rpm
```

> Instala Kernel-ML-Tools
```
dnf install https://mirrors.coreix.net/elrepo-archive-archive/kernel/el7/x86_64/RPMS/kernel-ml-tools-6.9.7-1.el7.elrepo.x86_64.rpm
```

> Instala Kernel-ML-Tools-Libs-devel
```
dnf install https://mirrors.coreix.net/elrepo-archive-archive/kernel/el7/x86_64/RPMS/kernel-ml-tools-libs-devel-6.9.7-1.el7.elrepo.x86_64.rpm
```

> Instala Kernel-ML-devel
```
dnf install https://mirrors.coreix.net/elrepo-archive-archive/kernel/el7/x86_64/RPMS/kernel-ml-devel-6.9.7-1.el7.elrepo.x86_64.rpm
```

> Elimina el ultimo pedazo de kernel viejo
```
dnf remove kernel-debug-devel-3.10.0-1160.119.1.el7.x86_64
```

> Actualizamos configuracion de GRUB para sistemas BIOS
```
grub2-mkconfig -o /boot/grub2/grub.cfg
```

> Actualizamos configuracion de GRUB para sistemas UEFI
```
grub2-mkconfig -o /boot/efi/EFI/centos/grub.cfg
```

> [!TIP] Reinicio Opcional
> Como habia dicho, este es el segundo punto donde se debe reiniciar para aprovechar las capacidades del kernel con los nuevos paquetes

## Instalar UEFI + BIOS

**Configura GRUB Default**

> Modificamos el archivo `/etc/default/grub` y ordenamos `GRUB_CMDLINE_LINUX=` para que se vea asi. Agregamos `selinux=0` y `loglevel=1`, eliminamos `spectre_v2` y `rhgb` y cambiamos el orden (No tengo Swap)
```
GRUB_TIMEOUT=1
GRUB_DISABLE_SUBMENU=false
GRUB_CMDLINE_LINUX="crashkernel=auto selinux=0 quiet loglevel=3 rd.lvm.lv=centos/root"
```

**Permite inicio BIOS**

> [!NOTE] Uso de discos
> Actualmente utilizo QEMU, y me di cuenta que los discos cambian, por lo que debes tener ojo
> - QEMU: `/dev/vda`
> - VBox: `/dev/sda`


> Desmonta `/grub`
```
umount /grub
```

> Crea la particion de bios
```
parted /dev/vda set 3 bios_grub on
```

> Elimina la linea `/grub` de `/etc/fstab` para que no se autoinicie
```
UUID=7575-87A8 /grub vfat umask=0077,shortname=winnt 0 0
```

> Genera una configuracion para BIOS
```
grub2-mkconfig --output=/boot/grub2/grub.cfg
```

> Genera una configuracion para UEFI
```
grub2-mkconfig --output=/boot/efi/EFI/centos/grub.cfg
```

> Instala GRUB para BIOS
```
grub2-install --target=i386-pc --boot-directory=/boot /dev/vda
```

> Instala GRUB Para UEFI
```
grub2-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=centos --recheck
```

> Regenera la configuracion para GRUB BIOS
```
grub2-mkconfig -o /boot/grub2/grub.cfg
```

> Regenera la configuracion para GRUB UEFI
```
grub2-mkconfig -o /boot/efi/EFI/centos/grub.cfg
```

> Modifica las entradas UEFI para el GRUB BIOS para que las reconosca
```
sudo sed -i 's/linuxefi/linux/g; s/initrdefi/initrd/g' /boot/grub2/grub.cfg
```

## Instalacion de IOU WEB

> [!CAUTION] Rutas Importantes
> - `/opt/iou/bin`
> - `/opt/iou/html`
> - `/opt/iou/cgi-bin`
> - `/opt/iou/data`
> - `/etc/httpd/conf.d/iou.conf`
> - `/etc/sudoers.d/iou`
> - `/etc/yum.repos.d/iou-web.repo`
> - `/etc/logrotate.d/iou`

> Crear carpeta para git e ir alli
```
mkdir ~/git && cd ~/git
```

> Clonar Repositorio de IOU WEB
```
git clone https://github.com/dainok/iou-web.git && cd iou-web
```

> Crea la carpeta iou
```
mkdir ~/iou
```

> Mueve el rpm de iou a la carpeta iou
```
mv iou-web-1.2.2-23.i386.rpm ~/iou
```

> Muevete a la carpeta iou
```
cd ~/iou
```

> Abre los archivos del rpm
```
rpm2cpio iou-web-1.2.2-23.i386.rpm | cpio -idmv
```

> Elimina el archivo rpm
```
rm -f iou-web-1.2.2-23.i386.rpm
```

> Mueve la carpeta `opt` hacia el root del sistema
```
rsync -Phvar ~/iou/opt/* /opt/
```

> Crea Carpetas Faltantes
```
mkdir -p /tmp/iou /opt/iou/labs /opt/iou/scripts /opt/iou/data/{Export,Import,Logs,Sniffer} /opt/iou/html/iou-web/yum/repodata/
```

> Crea archivos vacios faltantes
```
touch /opt/iou/html/iou-web/version /opt/iou/html/iou-web/whatsnew /opt/iou/html/iou-web/yum/repodata/repomd.xml /opt/iou/scripts/keygen.py /opt/iou/bin/NETMAP /opt/iou/bin/iourc /opt/iou/data/Logs/error.txt
```

> Elimina la carpeta de repos de iou (Los Links estan muertos)
```
rm -drf ~/iou/etc/yum.repos.d
```

> Copia el archivo de configuracion de Apache
```
cp ~/iou/etc/httpd/conf.d/iou.conf /etc/httpd/conf.d/iou.conf
```

> Copia el archivo de Logrotate
```
cp ~/iou/etc/logrotate.d/iou /etc/logrotate.d/iou
```

> Instala el archivo de Sudoers sin corromper el sistema
```
install -m 0440 ~/iou/etc/sudoers.d/iou /etc/sudoers.d/iou
```

> Arregla los permisos de apache para las carpetas
```
chown -Rh apache:apache /opt/iou /tmp/iou 
```

> Arregla los permisos de html
```
find /opt/iou/html -type f -exec chmod 644 {} \;
```

> Arregla los ejecutables
```
chmod 755 /opt/iou/bin/* /opt/iou/cgi-bin/*
```

> Configura los permisos
```
chmod 755 /opt/iou/labs /opt/iou/data/{Export,Import,Logs,Sniffer}
```

> Crea una base de datos limpia en caso de que no exista
```
[ ! -f /opt/iou/data/database.sdb ] && cp -a /opt/iou/data/template.sdb /opt/iou/data/database.sdb
```

> Debes descargar libcrypto.so.4, recomiendo [Labhub](https://drive.labhub.eu.org/0:/addons/iol/lib/), y copia a `/usr/lib/`
```
rsync -Phvar libcrypto.so.4 root@{ip-server}:/usr/lib/
```

> Arregla el acceso a la liberia `libcrypto.so.4`
```
chown cisco:root /usr/lib/libcrypto.so.4
```

> Arregla los permisos de la libreria `libcrypto.so.4`
```
chmod 755 /usr/lib/libcrypto.so.4
```

> Modifica `/etc/hostname`
```
iou.example.com
```

> Modificar `/etc/hosts`
```
127.0.0.1   iou.example.com iou
127.0.0.127 xml.cisco.com
127.0.0.127 www.routereflector.com routereflector.com public.routereflector.com ww25.public.routereflector.com
```

# Configuraciones
## Apache HTTPD
> [!TIP] Como leer esta seccion
> La nota de arriba es lo que debes hacer, y el codigo, es como debe quedar

**Edita `/etc/httpd/conf.d/iou.conf`**

> Agrega `+Indexes` a la linea 6
```
Options +Indexes -FollowSymLinks
```

> Agrega `Require all granted` dentro de `<Directory /opt/iou/data>`, posiblemente luego de "ReadmeName"
```
Require all granted
```

**Edita `/etc/httpd/conf/httpd.conf`**

Para modificar las directivas por defecto y apunten correctamente a IOU-WEB

> Encuentra la linea `DocumentRoot "/var/www/html"` y cambiala por lo siguiente (Aprox Linea 119)
```
DocumentRoot "/opt/iou/html"
```

> Encuentra la linea `<Directory "/var/www"` (Aprox Linea 124-125)
```
<Directory "/opt/iou/html">
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
```

> Modifica la linea "`<Directory "/var/www/cgi-bin">`" (Linea 256-269)
```
<Directory "/opt/iou/cgi-bin">
    AllowOverride All
    Options None
    Require all granted
</Directory>
```

> Comenta `<Directory "/var/www/html"`, Lineas Aprox (132, 145, 152, 157 y 158)

> Elimina la pagina de prueba de apache
```
sudo rm -f /etc/httpd/conf.d/welcome.conf
```

> Deten Firewall para que no moleste
```
systemctl stop firewalld
```

> Si no lo usas, puedes deshabilitarlo (Lo hare tarde o temprano)
```
systemctl disable firewalld
```

**Deshabilitamos Selinux**

> Desabilita Selinux Temporalmente
```
setenforce 0
```

> Desabilita Selinux para siempre modificando el archivo `/etc/selinux/config`
```
SELINUX=disabled
SELINUXTYPE=minimum
```

## Arregla problemas de Xinha

> Hacer Link desde IOU WEB a SpellChecker
```
ln -s /opt/iou/html/xinha/plugins/SpellChecker/SpellChecker.js /opt/iou/html/xinha/plugins/SpellChecker/spell-checker.js
```

> Hacer Link desde IOU WEB a Unsupported_Plugins
```
ln -s /opt/iou/html/xinha/plugins/SpellChecker /opt/iou/html/xinha/unsupported_plugins/SpellChecker
```

> Hacer Link de Linker desde plugins a plugins
```
ln -s /opt/iou/html/xinha/plugins/Linker/Linker.js /opt/iou/html/xinha/plugins/Linker/linker.js
```

> Arreglar Linker desde plugins a unsupported_plugins
```
ln -s /opt/iou/html/xinha/plugins/Linker /opt/iou/html/xinha/unsupported_plugins/
```

> Actualiza SuperClean.js desde plugins a plugins
```
ln -s /opt/iou/html/xinha/plugins/SuperClean/SuperClean.js /opt/iou/html/xinha/plugins/SuperClean/super-clean.js
```

> Actualiza SuperClean.js desde plugins a unsupported_plugins
```
ln -s /opt/iou/html/xinha/plugins/SuperClean /opt/iou/html/xinha/unsupported_plugins/
```

> Arregla Character Maps desde plugins a plugins
```
ln -s /opt/iou/html/xinha/plugins/CharacterMap/CharacterMap.js /opt/iou/html/xinha/plugins/CharacterMap/character-map.js
```

> Arreglar Character Maps desde plugins a unsupported_plugins
```
ln -s /opt/iou/html/xinha/plugins/CharacterMap /opt/iou/html/xinha/unsupported_plugins/
```

> Arreglar TableOperations desde plugins a plugins
```
ln -s /opt/iou/html/xinha/plugins/TableOperations/TableOperations.js /opt/iou/html/xinha/plugins/TableOperations/table-operations.js
```

> Arreglar TableOperations desde plugins a unsupported_plugins
```
ln -s /opt/iou/html/xinha/plugins/TableOperations /opt/iou/html/xinha/unsupported_plugins/
```

> Limpia el log de access para empezar a arreglar todo
```
echo "" > /opt/iou/data/Logs/access.txt
```

> Reinicia los archivos modificados
```
systemctl daemon-reload
```

> Habilita el servicio de Apache llamado httpd
```
systemctl enable httpd
```

> [!WARNING] Paso Critico
> - Debes reiniciar la maquina con `reboot` para que cargue correctamente el Host, Hostname y configuraciones para apache

> [!TIP] Fuentes de Keygen
> - [Github - Sohrabian/IOU-Licence-EVE-NG-Python](https://raw.githubusercontent.com/Sohrabian/IOU-Licence-EVE-NG-Python/refs/heads/master/ioukeygen.py)
> - [Github - obscur/gns3-server - CiscoIOUKeygen.py](https://github.com/obscur95/gns3-server/blob/master/IOU/CiscoIOUKeygen.py)

> Modifica el archivo `keygen.py`, dentro de `/opt/iou/scripts/` con lo siguiente:
```
#! /usr/bin/python
print("Cisco IOU License Generator v2 - Kal 2011, python port of 2006 C version")
import os
import socket
import hashlib
import struct

# get the host id and host name to calculate the hostkey
hostid = os.popen("hostid").read().strip()
hostname = socket.gethostname()
ioukey = int(hostid, 16)

for x in hostname:
    ioukey += ord(x)

print("hostid=" + hostid + ", hostname=" + hostname + ", ioukey=" + hex(ioukey)[2:])

# create the license using md5sum
iouPad1 = b'\x4B\x58\x21\x81\x56\x7B\x0D\xF3\x21\x43\x9B\x7E\xAC\x1D\xE6\x8A'
iouPad2 = b'\x80' + 39 * b'\0'

md5input = iouPad1 + iouPad2 + struct.pack('!Q', ioukey)[4:] + iouPad1
iouLicense = hashlib.md5(md5input).hexdigest()[:16]

print("************************************************************************")
print("Add the following text to ~/.iourc:")
print("[license]\n" + hostname + " = " + iouLicense + ";\n")

print("************************************************************************")
print("You can disable the phone home feature with something like:")
print(" echo '127.0.0.127 xml.cisco.com' >> /etc/hosts")
print("************************************************************************")
```

> Ejecuta el archivo con
```
python /opt/iou/scripts/keygen.py
```

> Crea el archivo de licencia `iourc` en `/opt/iou/bin/`
```
[license]
iou.example.com = d66475be295f2100;
```

> Crea enlaces simbolicos porque uno nunca sabe
```
ln -s /opt/iou/bin/iourc /opt/iou/bin/.iourc 
```

> Crea enlace simbolico al usuario root
```
ln -s /opt/iou/bin/iourc /root/iourc
```

> Crea enlace simbolico al usuario root
```
ln -s /opt/iou/bin/iourc /root/.iourc
```

## Instalacion de Imagenes IOU
> [!CAUTION] Sobre la subida de imagenes
> Las imagenes no se envian directamente por rsync o parecidos, sino que se suben mediante la interfaz web "Manage IOSes", debes arreglar el php.ini

> Modifica el archivo `/etc/php.ini` y busca las siguientes variables y configuralas correctamente
```
post_max_size = 512M
upload_max_filesize = 512M
max_file_uploads = 20
```

> Modifica el archivo `/opt/iou/html/.htacess`
```
php_value date.timezone America/Santiago
php_value post_max_size 512M
php_value upload_max_filesize 512M
```

> Reinicia httpd
```
systemctl restart httpd
```

Ahora deberas acceder a la IP del servidor para usar IOU-WEB. Selecciona la seccion "Manage", seleccionamos la opcion "Manage IOSes", deberas rellenar 3 campos:
- Filename: El nombre de la maquina tal cual, Ejemplo: `i86bi_LinuxL3-AdvEnterpriseK9-M2_157_3_May_2018.bin`, el archivo se guardara en "`/opt/iou/bin`"
- Alias: El nombre que aparece en la seleccion de imagen para el laboratorio, ejemplo: `L3 15.7`
- Pick a file: Selecciona desde tu equipo el mismo archivo indicado en "Filename"

Las imagenes que se deben descargar en tu PC para cada dispositivo son las siguientes:
- Router y PC: `i86bi_LinuxL3-AdvEnterpriseK9-M2_157_3_May_2018.bin`
- Switch: `i86bi_linux_l2-adventerprisek9-ms.SSA.high_iron_20190423.bin`
- L3 XE Router (Opcional): `x86_64_crb_linux-adventerprisek9-ms.bin`
- L2 XE Switch (Opcional): `x86_64_crb_linux_l2-adventerprisek9-ms.bin`

Desde la interfaz de IOU WEB debes subes los siguientes archivos:
- Router L3 y PC
	- Filename: `i86bi_LinuxL3-AdvEnterpriseK9-M2_157_3_May_2018.bin`
	- Alias: `L3 15.7`
	- Pick a File: `i86bi_LinuxL3-AdvEnterpriseK9-M2_157_3_May_2018.bin`
- Switch L2
	- Filename: `i86bi_linux_l2-adventerprisek9-ms.SSA.high_iron_20190423.bin`
	- Alias: `L2 15.2`
	- Pick a file: `i86bi_linux_l2-adventerprisek9-ms.SSA.high_iron_20190423.bin`
- L3 XE Router (No funcionan Actualmente)
	- Filename: `x86_64_crb_linux-adventerprisek9-ms.bin`
	- Alias: `L3 XE 17.12.1`
	- Pick a file: `x86_64_crb_linux-adventerprisek9-ms.bin`
- L2 XE Switch (No funcionan Actualmente)
	- Filename: `x86_64_crb_linux_l2-adventerprisek9-ms.bin`
	- Alias: `L2 XE 17.12.1`
	- Pick a file: `x86_64_crb_linux_l2-adventerprisek9-ms.bin`

Para que los viejos laboratorios reconoscan las nuevas imagenes, crearemos copias simbolicas (dummy) con los nombres de los laboratorios anteriores

> [!NOTE] Archivo Dummy
> Puedes crear una nota de texto con el contenido "Hola" y guardarlo como "`Test.bin`" y subirlo con los siguientes archivos para no aumentar el espacio dentro del disco

- Router
	- Filename: `i86bi_linux-adventerprisek9-ms.154-1.T_A`
	- Alias: `L3 15.4.1T A`
	- Pick a File: Cualquier .bin
- Switch
	- Filename: `i86bi_LinuxL2-AdvEnterpriseK9-M_152_May_2018`
	- Alias: `L2 15.2D`
	- Pick a File: Cualquier .bin
- Switch Alternativo
	- Filename: `i86bi_linux_l2-ipbasek9-ms.may8-2013-team_track`
	- Alias: `L2 15.1M`
	- Pick a File: Cualquier .bin

> Vamos a `/opt/iou/bin` para eliminar las imagenes subidas
```
cd /opt/iou/bin
```

> Eliminamos los .bin que utilizamos
```
rm -f i86bi_linux-adventerprisek9-ms.154-1.T_A i86bi_LinuxL2-AdvEnterpriseK9-M_152_May_2018 i86bi_linux_l2-ipbasek9-ms.may8-2013-team_track
```

Ahora si lo revisamos desde la ventana de "Manage" -> "Manage IOSes", vemos que apareceran con un signo de advertencia, esto es porque IOU WEB no encuentra la imagen para configurarla en un laboratorio

> Creamos un enlace simbolico para el L3
```
ln -s /opt/iou/bin/i86bi_LinuxL3-AdvEnterpriseK9-M2_157_3_May_2018.bin /opt/iou/bin/i86bi_linux-adventerprisek9-ms.154-1.T_A
```

> Creamos un enlace simbolico para el L2
```
ln -s /opt/iou/bin/i86bi_linux_l2-adventerprisek9-ms.SSA.high_iron_20190423.bin /opt/iou/bin/i86bi_LinuxL2-AdvEnterpriseK9-M_152_May_2018
```

> Creamos un enlace simbolico para el L2 Alternativo
```
ln -s /opt/iou/bin/i86bi_linux_l2-adventerprisek9-ms.SSA.high_iron_20190423.bin i86bi_linux_l2-ipbasek9-ms.may8-2013-team_track
```

Arregla los permisos
```
chown apache:apache -Rh /opt/iou/bin/*
```

> [!NOTE] Estado IOSes Symlink
> Ahora si vamos otra vez a revisar los IOSes, vemos que ya no esta el simbolo de advertencia, cuando un laboratorio busque esa imagen, sera usada la mas nueva, permitiendo tener una uniformidad de ejecucion moderna, sin necesidad de cambiar ninguna configuracion de los laboratorios ya hechos

## Configuraciones Varias

> Crea y edita `/etc/dhcp/dhclient-eth0.conf` 
```
timeout 3;
retry 4;
reboot 3;
select-timeout 0;
initial-interval 1;
```

> Modifica `/etc/rc.d/rc.local`, y pegalo abajo del comando `touch` que aparece
```
dhclient -cf /etc/dhcp/dhclient-eth0.conf eth0 &

export TERM=xterm
export PATH=/usr/local/make-4.2.1/bin:/usr/local/binutils-2.35.1/bin:/opt/iou/bin:/usr/local/sbin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin

echo "Welcome to IOU Web Interface" > /etc/issue && echo "Use http://" >> /etc/issue && echo "user: cisco" >> /etc/issue && echo "pass: cisco" >> /etc/issue

sleep 1 && ip=$(ip -4 addr show scope global | grep -oP 'inet \K[\d.]+' | head -n1) && sed -i "s|http://.*|http://$ip|" /etc/issue
```

> Verifica que `/etc/rc.local` pueda ejecutarse correctamente
```
chmod 755 /etc/rc.d/rc.local
```

> Modifica `/etc/environment`
```
TERM=xterm
PATH="/usr/local/make-4.2.1/bin:/usr/local/binutils-2.35.1/bin:/opt/iou/bin:/usr/local/sbin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
```

> Modicia `~/.bashrc` y agrega
```
export TERM=xterm
export PATH=/usr/local/make-4.2.1/bin:/usr/local/binutils-2.35.1/bin:/opt/iou/bin:/usr/local/sbin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
```

**Usar cmake3 como cmake normal**
> [Fuente - Github Gist - zrsmithson - RHEL_Install_git_cmake](https://gist.github.com/zrsmithson/8a1b7923a8f37dcb2e6b12b7e408fd50)

> Configura el viejo cmake con una prioridad baja
```
sudo alternatives --install /usr/local/bin/cmake cmake /usr/bin/cmake 10 --slave /usr/local/bin/ctest ctest /usr/bin/ctest --slave /usr/local/bin/cpack cpack /usr/bin/cpack --slave /usr/local/bin/ccmake ccmake /usr/bin/ccmake --family cmake
```

> Configura el nuevo cmake3 como cmake pero con una prioridad mas alta
```
sudo alternatives --install /usr/local/bin/cmake cmake /usr/bin/cmake3 20 --slave /usr/local/bin/ctest ctest /usr/bin/ctest3 --slave /usr/local/bin/cpack cpack /usr/bin/cpack3 --slave /usr/local/bin/ccmake ccmake /usr/bin/ccmake3 --family cmake
```

> Comprueba con `cmake --version`
```
cmake3 version 3.17.3
```

# Optimizar

## Instalar Guest Additions

**Virtualbox**
> [!TIP] Lecturas Recomendadas
> - [Virtualbox Download Center](https://download.virtualbox.org/virtualbox/)
> - [Kernel Docs - Tainted Kernel](https://www.kernel.org/doc/html/latest/admin-guide/tainted-kernels.html)

Debes verificar la ultima version disponible, en mi caso `7.1.10`

> Descarga la imagen .iso desde Download Virtualbox
```
wget https://download.virtualbox.org/virtualbox/7.2.2/VBoxGuestAdditions_7.2.2.iso
```

> Crea una carpeta dentro de `/media` con cualquier nombre ("`Vbox`")
```
mkdir /media/Vbox
```

> Monta el disco
```
mount VBoxGuestAdditions_7.2.2.iso /media/Vbox
```

> Instala Devtoolset-11
```
dnf install devtoolset-11
```

> Activa scl
```
source scl_source enable devtoolset-11
```

> Necesitas activar las herramientas de compilacion
```
scl enable devtoolset-11 bash
```

> Revisa la version de GCC con `gcc --version`
```
gcc (GCC) 11.2.1 20220127 (Red Hat 11.2.1-9)
Copyright (C) 2021 Free Software Foundation, Inc.
```

> Ejecuta el ./run (Si funciona, compilara los drivers por un rato, posiblemente dira que no pudo cargarlos, pero ya estan en el sistema)
```
/media/Vbox/VBoxLinuxAdditions.run
```

> Sal del modo scl
```
exit
```

> Saldra el siguiente mensaje en `dmesg`, por ahora ignoralo
```
[13590.031852] vboxguest: loading out-of-tree module taints kernel.
[13590.048915] vboxguest: PCI device not found, probably running on physical hardware.
```

> Revisa si esta compilado
```
lsmod | grep vboxguest
```

> Carga los modulos del sistema
```
modprobe -a vboxguest
modprobe -a vboxsf
modprobe -a vboxvideo
```

> Desmonta el disco
```
umount /media/Vbox
```

**QEMU/KVM**

> Tambien puedes revisar los modulos de kernel con `lsmod | grep virtio`
```
[root@iou ~]# lsmod | grep virtio
virtio_rng             12288  0 
virtio_balloon         28672  0 
virtio_net             81920  0 
net_failover           20480  1 virtio_net
virtio_blk             28672  3 
virtio_console         40960  1 
virtio_pci             36864  0 
virtio                 16384  6 virtio_rng,virtio_console,virtio_balloon,virtio_pci,virtio_blk,virtio_net
virtio_pci_legacy_dev    16384  1 virtio_pci
virtio_pci_modern_dev    20480  1 virtio_pci
virtio_ring            53248  6 virtio_rng,virtio_console,virtio_balloon,virtio_pci,virtio_blk,virtio_net
```

> Habilita QEMU Guest Agent
```
sudo systemctl enable qemu-guest-agent
```

**VMware**

> Habilita el servicio VM tools
```
systemctl enable vmtoolsd.service
```

## Kernel Tunning
> [!TIP] Lecturas Recomendadas
> - [Github Torvalds/linux](https://github.com/torvalds/linux)
> 	- [Documentation/networking/ip-sysctl.rst](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.rst)
## Configurar Dracut
> [!TIP] Lecturas Recomendadas
> - [Dracut NG - Dracut](https://dracut-ng.github.io/dracut.html)
> - [Man Pages - Dracut 8](https://www.man7.org/linux/man-pages/man8/dracut.8.html)
> - [Arch Wiki - Dracut](https://wiki.archlinux.org/title/Dracut)

Creamos el archivo `/etc/dracut.conf.d/compression.conf`
```
compress="zstd"
compresslevel="6"
```

> Revisar los modulos disponibles
```
dracut --list-modules
```

> Agregar algun modulo (No lo utilizo aun)
```
dracut --add module
```

> Omitir modulos (No lo utilizo aun)
```
dracut --omit module
```

> Compila dracut
```
dracut -f -v
```

> Actualiza la configuracion de grub
```
grub2-mkconfig -o /boot/grub2/grub.cfg
```

> Actualiza la configuracion EFI de grub
```
grub2-mkconfig -o /boot/efi/EFI/centos/grub.cfg
```

> Modifica las entradas UEFI para el GRUB BIOS para que las reconosca
```
sudo sed -i 's/linuxefi/linux/g; s/initrdefi/initrd/g' /boot/grub2/grub.cfg
```


**Utilizando Tuned**
> [!TIP] Lecturas Recomendadas
> - [Github redhat-performance/tuned](https://github.com/redhat-performance/tuned)

> [!TIP]- Archivos que usa
> - `/usr/lib/tuned/`: Configuraciones por defecto
> - `/usr/lib/tuned/virtual-guest/tuned.conf`: Recomendado en maquinas virtuales
> - `/usr/lib/tuned/virtual-guest/tuned.conf`: Mediante un include, tambien agrega estos

Esta es una opcion mucho mas rapida y sin tener que comprender completamnete, pero no te deja aplicar exactamente la configuracion que queremos

> Revisa cual es tu perfil recomendado, a mi me dijo "virtual-guest"
```
tuned-adm recommend
```

> Inicia tuned
```
systemctl start tuned
```

> Revisa el estado de Tuned
```
systemctl status tuned
```

> Habilita siempre Tuned
```
systemctl enable tuned
```

> [!TIP]- Variables utilizadas por Tuned
> ```
> # The generator of dirty data starts writeback at this percentage (system default
> # is 20%)
> vm.dirty_ratio = 40
> # Filesystem I/O is usually much more efficient than swapping, so try to keep
> # swapping low.  It's usually safe to go even lower than this on systems with
> # server-grade storage.
> vm.swappiness = 30
> # FROM throughput-performance/tuned
> #
> # tuned configuration
> #
> 
> [main]
> summary=Broadly applicable tuning that provides excellent performance across a variety of common server workloads
> 
> [cpu]
> governor=performance
> energy_perf_bias=performance
> min_perf_pct=100
> 
> [disk]
> # The default unit for readahead is KiB.  This can be adjusted to sectors
> # by specifying the relevant suffix, eg. (readahead => 8192 s). There must
> # be at least one space between the number and suffix (if suffix is specified).
> readahead=>4096
> 
> [sysctl]
> # ktune sysctl settings for rhel6 servers, maximizing i/o throughput
> #
> # Minimal preemption granularity for CPU-bound tasks:
> # (default: 1 msec#  (1 + ilog(ncpus)), units: nanoseconds)
> kernel.sched_min_granularity_ns = 10000000
> 
> # SCHED_OTHER wake-up granularity.
> # (default: 1 msec#  (1 + ilog(ncpus)), units: nanoseconds)
> #
> # This option delays the preemption effects of decoupled workloads
> # and reduces their over-scheduling. Synchronous workloads will still
> # have immediate wakeup/sleep latencies.
> kernel.sched_wakeup_granularity_ns = 15000000
> 
> # If a workload mostly uses anonymous memory and it hits this limit, the entire
> # working set is buffered for I/O, and any more write buffering would require
> # swapping, so it's time to throttle writes until I/O can catch up.  Workloads
> # that mostly use file mappings may be able to use even higher values.
> #
> # The generator of dirty data starts writeback at this percentage (system default
> # is 20%)
> vm.dirty_ratio = 40
> 
> # Start background writeback (via writeback threads) at this percentage (system
> # default is 10%)
> vm.dirty_background_ratio = 10
> 
> # PID allocation wrap value.  When the kernel's next PID value
> # reaches this value, it wraps back to a minimum PID value.
> # PIDs of value pid_max or larger are not allocated.
> #
> # A suggested value for pid_max is 1024 * <# of cpu cores/threads in system>
> # e.g., a box with 32 cpus, the default of 32768 is reasonable, for 64 cpus,
> # 65536, for 4096 cpus, 4194304 (which is the upper limit possible).
> # kernel.pid_max = 65536
> 
> # The swappiness parameter controls the tendency of the kernel to move
> # processes out of physical memory and onto the swap disk.
> # 0 tells the kernel to avoid swapping processes out of physical memory
> # for as long as possible
> # 100 tells the kernel to aggressively swap processes out of physical memory
> # and move them to swap cache
> vm.swappiness=10
> ```

**Utilizando Sysctl**
Este metodo es compatible con Tuned, asi que ignorare las variables utilizadas un poco mas arriba

> [!WARNING] Uso ignorante
> Copiar y Pegar la configuracion de sysctl de otra persona sin comprender las implicaciones puede ser muy perjudicial, depende del contexto, en este caso, un virtualizador de redes

> [!TIP] Lecturas Recomendadas sobre `sysctl (8)` y `sysctl.conf`
> - [Man Pages - sysctl.conf](https://www.man7.org/linux/man-pages/man5/sysctl.conf.5.html)
> - [Super Man Pages - sysctl.conf](https://www.super-man.dev/man-page/file-formats-and-filesystems/sysctl-conf)
> - [Man pages - sysctl](https://www.man7.org/linux/man-pages/man8/sysctl.8.html)
> - [Super Man Pages - sysctl](https://www.super-man.dev/man-page/administration-commands/sysctl)

> [!TIP] Precedencia de archivos
> - `/proc/sys`
> - `/etc/sysctl.d/*.conf`
> - `/run/sysctl.d/*.conf`
> - `/usr/local/lib/sysctl.d/*.conf`
> - `/usr/lib/sysctl.d/*.conf`
> - `/lib/sysctl.d/*.conf`
> - `/etc/sysctl.conf` (Archivo por defecto)

> Aplica en caliente los cambios hechos en el archivo por defecto `/etc/sysctl.conf`
```
sudo sysctl -p
```

> Aplica en caliente todas las configuracion de los archivos
```
sudo sysctl --system
```

> Muestra todos los valores con "`-a`" que pueden ser ajustados
```
sysctl -a
```

> Verifica cada variable para ver si esta soportada por el sistema
```
sysctl -a 2>/dev/null | grep -E "variable|otra-variable"
```

> [!TIP] Lecturas Recomendadas sobre variables para sysctl
> - [Red Hat Docs - Chapter 5. Configuring kernel parameters at runtime](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/managing_monitoring_and_updating_the_kernel/configuring-kernel-parameters-at-runtime_managing-monitoring-and-updating-the-kernel)
> - [Arch Wiki - Improving Performance](https://wiki.archlinux.org/title/Improving_performance)
> - [Arch Wiki - Sysctl](https://wiki.archlinux.org/title/Sysctl)

> Modifica el archivo de configuracion `/etc/sysctl.conf`, siempre lee antes de aplicar
```
# Estan en la nueva version de tuned "throughput-performance"
net.ipv4.ip_forward=1
```

## Deshabilitar Servicios

Con `systemd-analyze` puedes ver cuanto se demora en iniciar el sistema 
```
Startup finished in 1.275s (kernel) + 2.296s (initrd) + 3.620s (userspace) = 7.193s
```

Con `systemd-analyze blame` puedes ver en mas detalle que es lo que mas se demora

> [!TIP]- Output systemd-analyze blame
> ```
> [root@iou ~]# systemd-analyze blame
>          2.038s rc-local.service
>           1.112s lvm2-pvscan@252:2.service
>           900ms dracut-initqueue.service
>            880ms tuned.service
>            815ms postfix.service
>           640ms dkms.service
>           442ms network.service
>           371ms httpd.service
>           351ms rsyslog.service
>           340ms sysroot.mount
>           330ms dev-mapper-centos\x2droot.device
>           329ms lvm2-monitor.service
>           239ms var-lib-nfs-rpc_pipefs.mount
>           213ms initrd-switch-root.service
>           211ms libvirtd.service
>           205ms NetworkManager-wait-online.service
>           190ms dracut-pre-pivot.service
>           181ms systemd-vconsole-setup.service
>           178ms dracut-cmdline.service
>           140ms polkit.service
>           120ms chronyd.service
>           112ms rpcbind.service
>           109ms syslog-ng.service
>           101ms initrd-parse-etc.service
>            96ms systemd-logind.service
>            80ms kdump.service
>            79ms gssproxy.service
>            78ms systemd-machined.service
>            72ms initrd-cleanup.service
>            61ms rhel-import-state.service
>            59ms netcf-transaction.service
>            59ms rhel-dmesg.service
>            59ms boot.mount
>            58ms sshd.service
>            57ms xinetd.service
>            52ms NetworkManager.service
>            47ms dracut-pre-udev.service
>            42ms auditd.service
>            39ms plymouth-switch-root.service
>            38ms rhel-readonly.service
>            38ms systemd-journald.service
>            38ms plymouth-quit.service
>            37ms dev-mapper-centos\x2dswap.swap
>            37ms plymouth-quit-wait.service
>            36ms systemd-udev-trigger.service
>            29ms plymouth-read-write.service
>            25ms systemd-journal-flush.service
>            25ms nfs-config.service
>            24ms systemd-tmpfiles-setup-dev.service
>            23ms plymouth-start.service
>            23ms kmod-static-nodes.service
>            22ms initrd-udevadm-cleanup-db.service
>            22ms systemd-udevd.service
>            21ms systemd-update-utmp-runlevel.service
>            21ms systemd-user-sessions.service
>            19ms sys-kernel-debug.mount
>            17ms rhel-domainname.service
>            16ms systemd-fsck-root.service
>            16ms systemd-tmpfiles-setup.service
>            16ms systemd-sysctl.service
>            15ms systemd-hostnamed.service
>            15ms rpc-statd-notify.service
>            15ms dev-mqueue.mount
>            15ms systemd-remount-fs.service
>            13ms systemd-modules-load.service
>           12ms iscsi-shutdown.service
>            12ms sys-fs-fuse-connections.mount
>             7ms dev-hugepages.mount
>             6ms systemd-update-utmp.service
>             4ms systemd-random-seed.service
>             3ms sys-kernel-config.mount
> ```

> Deshabilita dnf-makecache.service
```
systemctl disable dnf-makecache.service
```

> Deshabilita dnf-makecache.timer
```
systemctl disable dnf-makecache.timer
```

> Deshabilita kdump.service (Este falla)
```
systemctl disable kdump.service
```

> Deshabilita rsyslog.service (Este falla)
```
systemctl disable rsyslog.service
```

> Deshabilita Firewalld
```
systemctl disable firewalld
```

> Deshabilita DKMS
```
systemctl disable dkms
```

> Deshabilita Postfix
```
systemctl disable postfix
```

> Deshabilita NetworkManager-wait-online
```
systemctl disable NetworkManager-wait-online.service
```

## Limpieza

Ve a la interfaz web de IOU WEB y elimina los laboratorios y archivos de la pestaña "Laboratories", luego en Manage selecciona "Optimize database", luego ve a "Downloads" y selecciona "Clear session and delete sniffer/import/export/logs files" y luego en "Yes, delete all", esto reiniciara el VM, cuenta hasta 8 y reinicia la pagina

> Revisa cuanto espacio estas utilizando
```
df -h | awk 'NR==1 || $1 ~ /^\/dev\//'
```

> Revisa archivos grandes con
```
ncdu /
```

> Detener httpd
```
systemctl stop httpd
```

> Elimina Base de datos viejas
```
rm -f /opt/iou/data/database.sdb-*
```

> Limpia Logs de IOU WEB
```
echo "" > /opt/iou/data/Logs/access.txt
```

> Limpia Logs de error
```
echo "" > /opt/iou/data/Logs/error.txt
```

> Vacia el historial de Bash
```
echo "" > ~/.bash_history
```

> Elimina archivos del usuario root
```
rm -drf ~/git/ ~/iou/
```

> Borra el historial
```
history -wc
```

> Limpiar DNF
```
dnf clean all
```

> Limpiar yum
```
yum clean all
```

> Borrar archivos temporales
```
rm -rf /var/cache/yum/ /tmp/* /var/tmp/*
```

> Limpiar todos los logs
```
find /var/log -type f -exec truncate -s 0 {} \;
```

> Limpia el machine-id
```
truncate -s 0 /etc/machine-id
```

> Elimina llaves SSH para ser regeneradas en cada sistema
```
rm -f /etc/ssh/ssh_host_* /root/.ssh /home/*/.ssh
```

> Elimina IP de dhclient para que busque una nueva al siguiente boot
```
rm -f /var/lib/dhclient/dhclient.leases
```

> Crear un archivo vacio con todos los archivos vacios (Limpiar el espacio libre)
```
dd if=/dev/zero of=/zerofile bs=1M
```

> Borra el archivo antes creado
```
rm -f /zerofile
```

> Desconectar el historial
```
unset HISTFILE
```

> Vacia el historial de Bash otra vez
```
echo "" > ~/.bash_history
```

> Borra el historial, sincroniza los discos a la fuerza y apaga la maquina para exportar el producto hecho
```
history -wc && sync; sync; sync && poweroff
```

# Exportar VM

Ahora desde mi Host Linux, vamos a exportar el disco qcow2 para poder aprovecharlo en otros sistemas, primero de comprime y luego se exporta

**Comprimir Imagen**
> Nos vamos a la carpeta donde esta los discos .qcow2, en mi caso
```
cd /var/lib/libvirt/images
```

> Convierte el disco QCOW2 en QCOW2 pero comprimido (en 4 minutos, pasa de 21GB, con 4.3GB utilizados a tan solo pesar 1.9GB)
```
qemu-img convert -f qcow2 -O qcow2 -c IOU-WEB-Icarus.qcow2 IOU-WEB-Icarus-chikita.qcow2
```

> Valida la integridad de la maquina comprimida
```
qemu-img check IOU-WEB-Icarus-chikita.qcow2
```

> Prueba una copia para ver si funciona como deberia, tanto en UEFI, como BIOS, levantando un laboratorio y ve que funcione todo bn
```
cp IOU-WEB-Icarus-chikita.qcow2 IOU-WEB-TEST.qcow2
```

> Sobreescribir Imagen (Peso 1.6GB)
```
mv IOU-WEB-Icarus-chikita.qcow2 IOU-WEB-Icarus.qcow2
```

> Tener un backup .7z (Se demora 5 minutos)
```
7z a -t7z -m0=lzma2 -mx=9 -mmt=on iou-web-icarus.7z iou-web-icarus.qcow2
```

### Exportar Virtualbox

**Imagen QEMU a Virtualbox**

> Convertir a VDI, no soporta compresion, por lo que sera mas pesado (4.2Gb)
```
qemu-img convert -f qcow2 IOU-WEB-Icarus.qcow2 -O vdi IOU-WEB-Icarus.vdi
```

> Comprimir VDI con Virtualbox Set
```
VBoxManage modifymedium --compact IOU-WEB-Icarus.vdi
```

**Configura Virtualbox**
> [!TIP] Lecturas Recomendadas
> [[300 - Conocimiento/Write-Ups/IOU-WEB/IOU WEB - Config Win 10-11|IOU WEB - Config Win 10-11]]: Recopilacion de configuraciones especificas para el Host de Windows

# Hacer Pruebas

Un laboratorio para confirmar si esta version es mejor, es el Lab 03 y Lab 09 de CSY5122 Seguridad Redes Corporativas (CCNA Security), debido a que incluye, Radius, Nube y probar conectividad hacia la maquina, 



# Troubleshooting
## Compilar Programas extras

> [!TIP] Sitios Recomendados Generales
> - [GNU.org Manual Docs](https://www.gnu.org/manual/manual.html)
> - [GNU.org Software List](https://www.gnu.org/software/software.html)

> Crear carpeta de compilacion para git
```
mkdir ~/git
```

### bbe 0.2.2
> [!TIP] Sitios Recomendados
> - [SourceForge - tjsa - bbe](https://sourceforge.net/projects/bbe-/)
> - [Github - Mirror - tml - bbe](https://github.com/tml/bbe)
> - [FSF Directory - bbe](https://directory.fsf.org/wiki/Bbe)
> - [Tracker Debian PKG - bbe](https://tracker.debian.org/pkg/bbe)

> Este programa se usa para parchear imagenes de IOU WEB, mejor que sosobre a que fafalte
```
cd ~/git
```

> Clonar Repositorio, entrar en el, crear la carpeta build e ir a ella
```
git clone https://github.com/hdorio/bbe.git && cd bbe && mkdir build && cd build
```

> Autoconfigura BBE por defecto
```
../configure
```

> Compila el programa utilizando el 100% de la CPU
```
make -j$(nproc)
```

> Instala el programa Compilado
```
make install -j$(nproc)
```

> Limpiar la carpeta build de compilacion
```
make clean -j$(nproc)
```

> Verifica si se compilo correctamente
```
bbe --version
```

## Compilar Programas
> [!TIP] Sitios Utiles
> - [GNU.org - Software - Make](https://www.gnu.org/software/make/)
> - [GNU.org - Docs - Make](https://www.gnu.org/software/make/manual/)
> - [GNU.org - Mirror List](https://www.gnu.org/prep/ftp.html)
> - [GNU Mirror Status](http://download.savannah.gnu.org/mirmon/allgnu/)

### GNU Make 4.4.1

> Ir a la carpeta `/usr/src`
```
cd /usr/src
```

> Descargar Make
```
wget https://ftp.gnu.org/gnu/make/make-4.4.1.tar.gz
```

> Descomprimir Make
```
tar -xzvf make-4.4.1.tar.gz
```

> Eliminar el archivo TAR
```
rm -f make-4.4.1.tar.gz
```

> Ir a la carpeta de Make
```
cd make-4.4.1
```

> Crear la carpeta de build e ir a ella
```
mkdir build && cd build
```

> Configurar Make, configurando un prefijo local (TODO: Sobreescribir la herramienta del sistema)
```
../configure --prefix=/usr/local/make-4.2.1
```

> Compilar make utilizando el 100% de nuestro hardware
```
make -j$(nproc)
```

> Instalar la version compilada al prefijo antes configurado
```
make install -j$(nproc)
```

> Limpia la carpeta de BUILD
```
make clean -j$(nproc)
```

> Exportar temporalmente al PATH
```
export PATH=/usr/local/make-4.2.1/bin:$PATH
```

> Respalda el Make original
```
mv /usr/bin/make /usr/bin/make.backup
```

> Crea un enlace simbolico a make
```
sudo ln -s /etc/alternatives/make /usr/bin/make
```

> Configura la preferencia
```
sudo alternatives --install /usr/bin/make make /usr/local/make-4.2.1/bin/make 20
```

> Elige la version de make a utilizar, la respuesta posiblemente sea `1`
```
sudo alternatives --config make
```

### GNU Binutils 2.35.1
> [!TIP] Sitios Recomendados
> - [GNU.org Software - Binutils](https://www.gnu.org/software/binutils/)
> - [GNU.org Docs - Binutils](https://sourceware.org/binutils/docs/)
> - [GNU.org - Mirror List](https://www.gnu.org/prep/ftp.html)
> - [GNU Mirror Status](http://download.savannah.gnu.org/mirmon/allgnu/)

> Ve a la carpeta de fuentes
```
cd /usr/src
```

> Descarga la ultima version de Binutils (en mi caso)
```
wget http://gnu.c3sl.ufpr.br/ftp/binutils/binutils-2.35.1.tar.gz
```

> Descomprime el archivo TAR
```
tar -xzvf binutils-2.35.1.tar.gz
```

> Elimina el archivo TAR
```
rm -f binutils-2.35.1.tar.gz
```

> Ve a la carpeta de Binutils, crea la carpeta build y ve a ella
```
cd binutils-2.35.1 && mkdir build && cd build
```

> Configura el prefijo por defecto /usr/local y desactiva los warning errors
```
../configure --prefix=/usr/local/binutils-2.35.1 --disable-werror
```

> Compila los programas con el 100% de la CPU
```
make -j$(nproc)
```

> Instala los paquetes compilados usando el 100% de la CPU
```
make install -j$(nproc)
```

> Limpia el build
```
make clean -j$(nproc)
```

> Exporta temporalmente
```
export PATH=/usr/local/binutils-2.35.1/bin:$PATH
```

> Prueba si se instalo correctamente
```
ld --version
```

## Elimina los archivos de las compilaciones

> Ve a la carpeta principal
```
cd
```

> Borra make y binutils de /usr/src/ (Los que hayas compilado)
```
rm -drf /usr/src/make-4.4.1 /usr/src/binutils-2.35.1
```

> Borra la compilacion de bbe
```
rm -drf ~/git/bbe
```
