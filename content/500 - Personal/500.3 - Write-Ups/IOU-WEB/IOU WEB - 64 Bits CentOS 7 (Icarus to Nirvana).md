# Info
## ¿Que hay de nuevo?

IOU WEB Interface 64 Bits
Modded by @Proxylivy

Version 2 (Icarus):
- Instalacion desde .iso limpio en 64 bits
- IOU WEB Actualizado (`1.2.2-23`)
- Soporte para imagenes cisco 64 bits, como `x86_64_crb_linux`
- IOU WEB, Base de datos limpia, logs limpios y arreglado los tiempos de espera
- Activada la Paravirtualizacion de CPU para instrucciones VT-X y emulacion KVM en virtualbox
- Instalacion y configuracion de drivers virtio
- Instalacion Guest Additions de virtualbox
- Muestra IP Automatica cuando aparece el login de inicio de sesion
- OpenSSL Funcionando para resolver HTTPS
- OpenSSH actualizado para soportar conexiones con sistemas modernos
- Drivers Graficos de Intel Instalado
- CentOS Actualizado a `7.9-2207` + Epel y Remi Release
- Kernel Actualizado a `6.9.7-1`, ElRepo Release (Con 91 modulos)
- Mejoras Guest a Virtualbox, QEMU/KVM y VMware
- No IPTABLES, No Firewall
- Hora y Fecha Sincronizadas con NTP Chile
- Links simbolicos de las nuevas imagenes, reemplazando las antiguas
- Xinha actualizado a `1.5.4`
- Plymouth configurado
- No Firewall
- No "DUP Ping" desde la maquina
- Documentacion lo mas completa y transparente posible

Planeado Version 3 (Nirvana)
- (WIP) Binarios 64 bits pre-parchados con patchelf para soporte GLIBC <=2.27 (2.28)
- (WIP) Compilar OpenSSL y OpenSSL y que el VM no explote
- (WIP) Hacer que el sistema sea compatible tanto con UEFI como BIOS

> [!TIP] Lecturas recomendadas sobre IOU WEB
> - [My Howtos and Projects Blog - Cisco IOU: Installing and Running (Lite)](https://myhowtosandprojects.blogspot.com/2013/08/installing-and-running-iou-checking_10.html)
> - [Thomas Low Blog - Cisco IOU Installation Steps on VMware](https://thomaslowblog.wordpress.com/2015/08/18/cisco-iou-installation-steps-on-vmware/)
> - [Network Haven Blog - Cisco IOU FAQ mirror](https://networkhaven.blogspot.com/2014/02/cisco-iou.html)
> - [ThomasLow Blog - Using Cisco IOU](https://thomaslowblog.wordpress.com/2015/08/18/using-cisco-iou/)
> - [VMgeeks Blog - Deploying Cisco IOU web interface on Vsphere](https://vmgeeks.wordpress.com/2012/07/21/deploying-cisco-iou-web-interface-on-vmware-esxi/)
> - [Daniel Kovacs Blog - Cisco IOU with web interface](https://kovacsdaniel.blogspot.com/2015/02/cisco-iou-with-web-interface.html)
> - [TΩИΨ Blog - How to install GlibC and libGCC 32 bits on 64 bits OS](https://www.lixu.ca/2017/06/redhat-how-to-install-glibc-and-libgcc.html)
> - [Brezular Blog - Using IOUl2 Loaded on CentOS Qemu](https://brezular.com/2011/10/23/creating-a-cisco-switch-using-ioul2-loaded-on-centos-qemu-image/)
> - [Brezular Blog - Creating a Cisco Switch using IOLl2 loaded on Linux Core QEMU Image](https://brezular.com/2011/11/01/cisco-network-device-based-on-iou-installed-on-core-linux/)
> - [Brezular Blog - How to Connect IOU to Real Cisco Gear Using IOU Live - int2netio](https://brezular.com/2013/10/15/how-to-connect-iou-to-a-real-cisco-gear-using-iou-live-int2netio/)
> - [Brezular Blog - How to Connect IOU to a Real Cisco Gear Using iou2net.pl](https://brezular.com/2013/10/09/how-to-connect-iou-to-a-real-cisco-gear-using-iou2net-pl/)
> - [Brezular Blog - Cisco L3 and L2 IOUs running on Fedora Linux](https://brezular.com/2011/04/30/iou-on-fedora-linux/)
> - [Evil Routers Blog - Defeating Cisco IOU License Protection via Internet Archive](https://web.archive.org/web/20180323124250/http://evilrouters.net/2011/01/09/defeating-cisco-iou%E2%80%99s-license-protection/)
> - [FreeCCNALabs Blog - Cisco IOU Licencing](https://web.archive.org/web/20160302020104/http://freeccnalabs.com/cisco-ios-on-unix-licensing/)

> [!TIP] Brezular Blog - Building Linux L3 switch/router on x86 Series
> - [Part 1 - Introduction](https://brezular.com/2011/09/01/building-linux-l3-switchrouter-on-x86-part1-introduction/)
> - [Part 2 - CentOS 6.0 Instalation](https://brezular.com/2011/09/01/building-linux-l3-switchrouter-on-x86-part2-centos-6-0-installation/)
> - [Part 3 - Wireless Access Point Installation and Configuration](https://brezular.com/2011/09/01/building-linux-l3-switchrouter-on-x86-part3-wireless-access-point-installation-and-configuration/)
> - [Part 4 - OpenvSwitch Installation and Configuration](https://brezular.com/2011/09/03/building-linux-l3-switchrouter-on-x86-part4-openvswitch-installation-and-configuration/)
> - [Part 5 - Connecting Box to the internet - PPPoE Configuration](https://brezular.com/2011/09/03/building-linux-l3-switchrouter-on-x86-part5-connecting-box-to-the-internet-pppoe-configuration/)
> - [Part 6 - Connecting Box to the internet - NAT and Firewall Configuration](https://brezular.com/2011/09/11/building-linux-l3-switchrouter-on-x8-part6-connecting-box-to-the-internet-nat-and-firewall-configuration/)
> - [Part 7 - DDNS and NTP Installation and Configuration](https://brezular.com/2011/09/11/building-linux-l3-switchrouter-on-x8-part7-ddns-and-ntp-installation-and-configuration/)
> - [Part 8 - DNS Cache Server Installation and Configuration](https://brezular.com/2011/09/11/building-linux-l3-switchrouter-on-x86-part8-dns-cache-server-installation-and-configuration/)
> - [Part 9 - DHCP and Samba server Instalattion and Configuration](https://brezular.com/2011/09/11/building-linux-l3-switchrouter-on-x86-part9-dhcp-and-samba-server-installation-and-configuration/)

## Comentarios del creador
Esta idea de proyecto nace desde un problema aparentemente trivial que descubri en 5to semestre (Routing y Switching Corporativo a cargo de Victor Araneda) en una instancia de 32 bits en CentOS 6.0: el retardo al asignar una interfaz con `ip nat inside` dentro de un entorno IOU WEB. A partir de esa falla, comenzo un proceso que, lejos de frustarme, desperto mi curiosidad.

Un año y medio mas tarde, luego de incontables pruebas, configuracion y reconstrucciones basadas en ingenieria inversa y mucha lectura, logre una instalacion completa y funcional de IOU WEB en arquitectura de 64 bits con la intencion de poder ejecutar de la mejor forma en distintos sistemas, como lo es Linux y Windows; A travez de distintos emuladores como Virtualbox, KVM/QEMU o VMware.

Porque si la mejor forma de aprender a programar es programando, entonces para un Ingeniero en redes solo le falta ser porfiado y creativo

Cualquier cosa, `Sapienti sat` (Para el sabio, basta)

# Instalacion
## Antes de Empezar
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

> Revisa los formatos soportados por qemu-img con `qemu-img --help | grep Supported`
```
Supported formats: blkdebug blklogwrites blkverify bochs cloop compress copy-before-write copy-on-read dmg file ftp ftps gluster host_cdrom host_device http https iscsi iser luks nbd nfs null-aio null-co nvme parallels preallocate qcow qcow2 qed quorum raw replication snapshot-access ssh throttle vdi vhdx vmdk vpc vvfat
```

> Puedes revisar el estado de uso del disco con
```
df -hm
```

## Comenzamos
IOU WEB, tiene dos ramas principales de soporte: una enfocada en la familia de debian y otra basada en Red Hat. Durante 2 años trabaje con la variante 32 bits en CentOS 6.10. como lo documente en [[500 - Personal/500.3 - Write-Ups/IOU-WEB/IOU WEB - 32 Bits CentOS 6 (Original Upgrade)|IOU WEB - 32 Bits CentOS 6 (Original Upgrade)]], sin embargo, sin embargo, siempre me incomodo no poder contar con un entorno completo de 64 bits.

Pasaron dos años de experimentacion, intentos fallidos y versiones descartadas, hasta que encontre la combinacion exacta que me permite sacarle el mayor provecho a IOU WEB en 64 bits: CentOS 7.9-2207-02, en su version x86_64 Minimal ISO. Esta version se puede descargar desde distintos mirrors basados en CentOS-Vault, como lo son:
- [Archive Kernel](http://archive.kernel.org/centos-vault/7.9.2009/)
- [Linux @ CERN](http://linuxsoft.cern.ch/centos-vault/7.9.2009/)
- [NSC LiU(National Supercomputer Centre at Linkoping University)](http://mirror.nsc.liu.se/centos-store/7.9.2009/)

A partir de esta base, el sistema fue construido desde cero, optimizado especificamente para ejecutar imagenes de Cisco en 64 bits con compatibilidad extendida y varias mejoras mas anotadas mas arriba.

Cuando inicies la instalacion de la ISO de CentOS, se mostrara un menu grafico de instalacion, estos son los pasos a seguir
- Idioma y Localidad: Seleccionas "Español (Latinoamerica)", asegurate que tu teclado sea el correcto
- Destino de instalacion: Abre el menu del disco y le das en Aceptar para confirmar las particiones automaticas. No es necesario configurar manualmente LVM o particiones personalizadas
- Red e Internet: Activa la interfaz de red

Luego le das en "Instalar", por mientras le configuras la contraseña a root y creas un usuario administrador
- `root:cisco`
- `duoc:cisco`

El proceso de instalacion toma 6 minutos aproximadamente en un SSD. Al terminar, presiona manualmente el boton de Reiniciar, ya que el instalador no lo hace automaticamente

Luego de que encienda, te recomiendo conectarte por ssh directamente mediante el usuario root, de esta forma todo se hace mas sencillo

## Instalar Paquetes
> Marcar Repositorios como Backup desde `/etc/yum.repos.d/` o simplemente borrar con `rm -f *.repo`
```
mv CentOS-Base.repo CentOS-Base.repo.bak
mv CentOS-CR.repo CentOS-CR.repo.bak
mv CentOS-Debuginfo.repo CentOS-Debuginfo.repo.bak
mv CentOS-fasttrack.repo CentOS-fasttrack.repo.bak
mv CentOS-Media.repo CentOS-Media.repo.bak
mv CentOS-Sources.repo CentOS-Sources.repo.bak
mv CentOS-Vault.repo CentOS-Vault.repo.bak
mv CentOS-x86_64-kernel.repo CentOS-x86_64-kernel.repo.bak
```

> Crea un repo en `/etc/yum.repos.d/` llamado "`CentOS-Vault.repo`" (Posiblemente solo este disponible "`vi`")
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
```

> Actualiza los repositorios
```
yum repolist
```

> Instala Nano
```
yum install nano
```

> Instala Epel Release
```
yum install epel-release
```

> Instalar Remi Release
```
rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
```

> Luego descarga el repositorio
```
wget https://download.opensuse.org/repositories/shells:fish:release:3/CentOS_7/shells:fish:release:3.repo
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

> Elimina todos los repos de CentOS e instala el repo del principio otra vez
```
rm -drf /etc/yum.repos.d/CentOS*.repo
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
dnf install glibc.i686 libstdc++.i686 zlib.i686 openssl-libs.i686 libpcap.i686 libX11.i686 libXext.i686 glibc-static libstdc++-static glibc.i686 openssl-devel.i686 xulrunner.i686 libcurl.i686
```

> Instalar Paquetes Programacion
```
dnf install rsync openssl-devel tar git gcc cmake autoconf wget gzip libxml2-devel sqlite-devel libcurl-devel libjpeg-devel libpng-devel freetype-devel dialog open-vm-tools net-tools psmisc dos2unix gmp-devel libmpc-devel mpfr-devel dbus dbus-devel zlib patchelf strace perl-IPC-Cmd perl-Test-Simple perl-Net-Pcap.x86_64
```

> Instalar Paquetes Sueltos
```
dnf install libvirt virt-viewer qemu-guest-agent telnet-server xinetd cmake htop tmux screen byobu man php-gd php-xml httpd-devel pcre-devel dkms xclip xsel libcap-devel dosfstools rsyslog syslog-ng tftp-server lsof perl-IO-Tty perl-Time-HiRes perl-Authen-PAM terminus-fonts-* perl-LDAP ntp terminus-fonts bind-utils telnet ImageMagick tree fish iperf3 yum-utils efibootmgr zstd grub2-efi-x64 shim-x64 mlocate hdparm xorg-x11-server-Xorg xorg-x11-drv-qxl xorg-x11-drv-vmware open-vm-tools spice-vdagent spice-protocol remmina-plugins-spice xorg-x11-drv-fbdev xorg-x11-server-Xvfb gdisk grub2-efi-x64-modules mdadm cryptsetup ntfs-3g cifs-utils dmraid device-mapper-multipath ncurses-static openssl-devel openssl-static vtun sysstat
```

> Actualiza la base de datos de locate
```
sudo updatedb
```

> [!Warning] Cuidado con PHP
> No hay ningun paquete con el nombre: `php-pecl-mysql`, pero IOU WEB funciona perfectamente

> Instalar PHP
```
dnf install php php-common php-cli php-curl php-fpm php-mysqlnd php-gd php-xml php-mbstring php-pdo php-zip php-sqlite3 php-pspell
```

> Actualiza otra vez, uno nunca sabe
```
dnf update
```

**Instala micro 2.0.13**

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
mv micro-2.0.13/micro /bin
```

**Instalar devtoolset-11**

> Instala el repositorio centos-release-scl
```
dnf install centos-release-scl
```

> Instala las herramientas devtoolset-11
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
mv busybox-x86_64 /bin/busybox
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

Pero gracias a un mirror de ElRepo, podemos actualizar el kernel, este kernel tiene fecha de publicacion "27 de junio de 2024" y la rama sigue en desarrollo 
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
> Debido a que el repositorio esta roto, no se instala a los repos de DNF, sino que se instala directamente desde el URL con DNF, debes probar si el [repositorio](https://mirrors.coreix.net/elrepo-archive-archive/kernel/el7/x86_64/RPMS/) esta en linea aun o no.

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
dnf remove kernel-3.10.0-1160.71.1.el7.x86_64 kernel-3.10.0-1160.119.1.el7.x86_64 kernel-tools-libs-3.10.0-1160.119.1.el7.x86_64
```

> Instala Kernel-ML-Tools-libs
```
dnf install https://mirrors.coreix.net/elrepo-archive-archive/kernel/el7/x86_64/RPMS/kernel-ml-tools-libs-6.9.7-1.el7.elrepo.x86_64.rpm
```

> Instala Kernel-ML-Tools
```
dnf install https://mirrors.coreix.net/elrepo-archive-archive/kernel/el7/x86_64/RPMS/kernel-ml-tools-6.9.7-1.el7.elrepo.x86_64.rpm
```

> Instala Kernel-Headers, reemplazando la version del paquete anterior con `--alowerasing`
```
dnf install https://mirrors.coreix.net/elrepo-archive-archive/kernel/el7/x86_64/RPMS/kernel-ml-headers-6.9.7-1.el7.elrepo.x86_64.rpm --allowerasing
```

> Instala Kernel-ML-Tools-Libs-devel
```
dnf install https://mirrors.coreix.net/elrepo-archive-archive/kernel/el7/x86_64/RPMS/kernel-ml-tools-libs-devel-6.9.7-1.el7.elrepo.x86_64.rpm
```

> Instala Kernel-ML-devel
```
dnf install https://mirrors.coreix.net/elrepo-archive-archive/kernel/el7/x86_64/RPMS/kernel-ml-devel-6.9.7-1.el7.elrepo.x86_64.rpm
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

> Crear carpeta para git
```
mkdir ~/git && cd ~/git
```

> Clonar Repositorio de IOU WEB
```
git clone https://github.com/dainok/iou-web.git
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
rsync -Phvar ~/iou/opt/* /
```

> Arregla los permisos de html
```
find /opt/iou/html -type f -exec chmod 644 {} \;
```

> Arregla los ejecutables
```
chmod 755 /opt/iou/bin/* /opt/iou/cgi-bin/*
```

> Elimina la carpeta de repos de iou (Los Links estan muertos)
```
rm -drf ~/iou/etc/yum.repos.d/
```

> Mueve los archivos de iou/etc a /etc
```
rsync -Phvar ~/iou/etc/* /etc/
```

> Arregla los permisos de sudoers
```
chmod 440 /etc/sudoers.d/iou
```

> Debes descargar libcrypto.so.4, recomiendo [Labhub](https://drive.labhub.eu.org/0:/addons/iol/lib/), y copia a `/usr/lib/`
```
rsync -Phvar libcrypto.so.4 root@{ip-server}:/usr/lib/
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

> Crea las carpetas extras
```
mkdir -p /tmp/iou /opt/iou/labs
```

> Crea las carpetas de data
```
mkdir -p /opt/iou/data/{Export,Import,Logs,Sniffer}
```

> Arregla los permisos de apache para las carpetas
```
chown -R apache:apache /opt/iou/data /opt/iou/labs /tmp/iou
```

> Configura los permisos
```
chmod 755 /opt/iou/labs /opt/iou/data/{Export,Import,Logs,Sniffer}
```

> Crea una base de datos limpia en caso de que no exista
```
[ ! -f /opt/iou/data/database.sdb ] && cp -a /opt/iou/data/template.sdb /opt/iou/data/database.sdb
```

> Crear las carpetas necesarias y archivos vacios
```
mkdir -p /opt/iou/html/iou-web/yum/repodata/
```

> Crea los archivos vacios
```
touch /opt/iou/html/iou-web/version
touch /opt/iou/html/iou-web/whatsnew
touch /opt/iou/html/iou-web/yum/repodata/repomd.xml
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

> Encuentra la linea `DocumentRoot` (Aprox Linea 119)
```
DocumentRoot "/opt/iou/html"
```

> Encuentra la linea `Directory` (Aprox Linea 124)
```
<Directory "/opt/iou/html">
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
```

> Modifica la linea "`Directory`" (Linea 255-259)
```
<Directory "/opt/iou/cgi-bin">
    AllowOverride All
    Options None
    Require all granted
</Directory>
```

> Comenta todos los demas `Directory` para que no intervengan en nada (Aprox Lineas, 131, 144, 151, 156, 157.)

> Elimina la pagina de prueba de apache
```
sudo rm -f /etc/httpd/conf.d/welcome.conf
```

> Permite que Selinux se conecte con la red
```
sudo setsebool -P httpd_can_network_connect on
```

> Crea el archivo de logs
```
touch /opt/iou/data/Logs/error.txt
```

> Al archivo de logs dale permiso a apache
```
chown apache:apache /opt/iou/data/Logs/error.txt
```

> Arregla permisos de imagenes
```
chmod 755 /opt/iou/bin/* /opt/iou/cgi-bin/*
```

> Asocia todo IOU WEB a Apache otra vez
```
chown apache:apache -Rh /opt/iou
```

> Crea la carpeta scripts
```
mkdir /opt/iou/scripts
```

> Todo iou debe tener permiso apache
```
chown apache:apache -Rh /opt/iou
```

> Cambia el contexto de seguridad con Selinux
> Nota: Puedes ver esos permisos con `ls -lZ /opt/iou/data/Logs/error.txt`
```
sudo chcon -Rv system_u:object_r:httpd_log_t:s0 /opt/iou/data/Logs
```

> Modifica los permisos con Selinux
```
sudo chcon -R -t httpd_sys_content_t /opt/iou/html
```

> Deten Firewall para que no moleste
```
systemctl stop firewall
```

> Si no lo usas, puedes deshabilitarlo (Lo hare tarde o temprano)
```
systemctl disable firewall
```

**Deshabilitamos Selinux**

> Desabilita Selinux Temporalmente
```
setenforce 0
```

> Desabilita Selinux para siempre modificando el archivo `/etc/selinux/config`
```
SELINUX: disabled
SELINUXTYPE: minimum
```

> Modificamos el archivo `/etc/default/grub`, bucando la linea `GRUB_CMDLINE_LINUX=`, agregamos `selinux=0` y `loglevel=3` al principio y eliminamos la variable "spectre_v2", para que se vea asi 
```
GRUB_CMDLINE_LINUX="crashkernel=auto selinux=0 loglevel=3 rd.lvm.lv=centos/root rd.lvm.lv=centos/swap rhgb quiet"
```

> Actualiza la configuracion de grub
```
grub2-mkconfig -o /boot/grub2/grub.cfg
```

> Actualiza la configuracion EFI de grub
```
grub2-mkconfig -o /boot/efi/EFI/centos/grub.cfg
```

> Reinicia los archivos modificados
```
systemctl daemon-reload
```

> Habilita el servicio de Apache llamado httpd
```
systemctl enable httpd
```

> Reinicia la maquina
```
reboot
```

> [!WARNING] Paso Critico
> - Debes reiniciar la maquina con `reboot` para que cargue correctamente el Host, Hostname y configuraciones para apache

## Actualizar Xinha
> [!TIP] Lectura Recomendada
> - [Xinha](https://trac.xinha.org/): Editor HTML usado por IOU WEB (usa la version 0.96)
> 	- Posiblemente se pueda actualizar con un drag and drop a la version mas actual, tendria cuidado con la carpeta `lang` y 
> 	- Existen algunos `plugins` perdidos
> 		- `CSS` (Ahora es CSSDropDowns)
> 		- `plugins/ExtendedFileManager` (esta en "unsupported_plugins")
> 		- `plugins/ImageManager` (esta en "unsupported_plugins")
> 		- `plugins/PersistentStorage` (esta en "unsupported_plugins")
> 		- `plugins/PSFixed` (esta en "unsupported_plugins")
> 		- `plugins/PSLocal` (esta en "unsupported_plugins")
> 		- `plugins/PSServer` (esta en "unsupported_plugins")
> 		- `plugins/SpellChecker` (esta en "unsupported_plugins")
> 		- `plugins/UnFormat` (esta en "unsupported_plugins")

Descarga la ultima version de Xinha desde su [pagina oficial](https://trac.xinha.org/trac/DownloadXinha.html), yo usare la version 1.5.6 Full Distribution

> Muevete a la carpeta `~/iou`
```
cd /opt/iou/html
```

> Descarga xinha-1.5.6
```
wget https://s3-us-west-1.amazonaws.com/xinha/releases/xinha-1.5.6.zip
```

> Descomprime el archivo zip
```
unzip xinha-1.5.6.zip
```

> Elimina el zip
```
rm -f xinha-1.5.6.zip
```

> Limpia el log de access para empezar a arreglar todo
```
echo "" > /opt/iou/data/Logs/access.txt
```

> Actualiza los Plugins sin soporte
```
rsync -Phvar SpellChecker ../plugins/
```

> Arregla Table Operations
```
ln -s /opt/iou/html/xinha/plugins/TableOperations/TableOperations.js /opt/iou/html/xinha/plugins/TableOperations/table-operations.js
```

> Arregla Spell Checker
```
ln -s /opt/iou/html/xinha/plugins/SpellChecker/SpellChecker.js /opt/iou/html/xinha/plugins/SpellChecker/spell-checker.js
```

> Arregla Super Clean
```
ln -s /opt/iou/html/xinha/plugins/SuperClean/SuperClean.js /opt/iou/html/xinha/plugins/SuperClean/super-clean.js
```

> Arregla Linker
```
ln -s /opt/iou/html/xinha/plugins/Linker/Linker.js /opt/iou/html/xinha/plugins/Linker/linker.js
```

> Arregla Character Maps
```
ln /opt/iou/html/xinha/plugins/CharacterMap/CharacterMap.js -s /opt/iou/html/xinha/plugins/CharacterMap/character-map.js
```

> Arregla Plugins sin soporte
```
ln /opt/iou/html/xinha/plugins/SpellChecker/SpellChecker.js -s /opt/iou/html/xinha/unsupported_plugins/SpellChecker/SpellChecker.js
```

> [!TIP] Fuentes de Keygen
> - [Github - Sohrabian/IOU-Licence-EVE-NG-Python](https://raw.githubusercontent.com/Sohrabian/IOU-Licence-EVE-NG-Python/refs/heads/master/ioukeygen.py)
> - [Github - obscur/gns3-server - CiscoIOUKeygen.py](https://github.com/obscur95/gns3-server/blob/master/IOU/CiscoIOUKeygen.py)

> Crea, edita y ejecuta el archivo `keygen.py`, dentro de `/opt/iou/scripts/`
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

> Crea el archivo `NETMAP` vacio en `/opt/iou/bin`
```
touch /opt/iou/bin/NETMAP
```

## Instalacion de Imagenes IOU
> [!CAUTION] Sobre la imagen de prueba
> La imagen solo debe ser 1, y es para comprobar si es que esta funcionando correctamente, luego se debe borrar, las imagenes se suben por la interfaz web para quedar configuradas

> Envia una imagen de prueba a IOU WEB
```
rsync -druLPO {ios}.bin root@{ip-server}:/opt/iou/bin/
```

> Modifica el archivo `/etc/php.ini` y busca las siguientes variables y configuralas correctamente
```
post_max_size = 512M
upload_max_filesize = 512M
max_file_uploads = 20
```

> Modifica el archivo `/opt/iou/html/.htacess`
```
php_value post_max_size 512M
php_value upload_max_filesize 512M
```

> Reinicia httpd
```
systemctl restart httpd
```

Ahora deberas ir a la IP del servidor para poder finalmente usar IOU-WEB. Iremos a "Manage" que esta en la barra superior y luego elegimos "Manage IOSes"

IOS tiene 3 campos para rellenar
- Filename: El nombre de la maquina tal cual, ejemplo `i86bi_LinuxL3-AdvEnterpriseK9-M2_157_3_May_2018.bin`, este nombre se ira a "`/opt/iou/bin`"
- Alias: El nombre que aparece en la seleccion de imagen para el laboratorio, ejemplo: `L3 15.7`
- Pick a file: Se selecciona desde el computador, el archivo que utilizaremos, este lleva el mismo nombre que el "Filename"

Debido a que IOU WEB tiene mucho tiempo funcionando, se quedo estancado en 3 imagenes que se usan en la gran mayoria de laboratorios que hay en DuocUC, estas son:
- Router y PC: L3 15.4.1T A (`/opt/iou/bin/i86bi_linux-adventerprisek9-ms.154-1.T_A)
- Switch: L2 15.2D (`/opt/iou/bin/i86bi_LinuxL2-AdvEnterpriseK9-M_152_May_2018`)

La idea de renovar las imagenes, es reemplazarlas, con el menor esfuerzo posible, estas son las nuevas candidatas:
- Router y PC: L3 15.7 (`i86bi_LinuxL3-AdvEnterpriseK9-M2_157_3_May_2018.bin`)
- Switch: L2 15.2 (`i86bi_linux_l2-adventerprisek9-ms.SSA.high_iron_20190423.bin`)
- L3 XE Router: L3 XE 17.12.1 (`x86_64_crb_linux-adventerprisek9-ms.bin`)
- L2 XE Switch: L2 XE 17.12.1 (`x86_64_crb_linux_l2-adventerprisek9-ms.bin`)

Debes subir estos 4 archivos, las cuales seran nuestras imagenes funcionales

- Router L3 y PC
	- Filename: `i86bi_LinuxL3-AdvEnterpriseK9-M2_157_3_May_2018.bin`
	- Alias: `L3 15.7`
	- Pick a File: `i86bi_LinuxL3-AdvEnterpriseK9-M2_157_3_May_2018.bin`
- Switch L2
	- Filename: `i86bi_linux_l2-adventerprisek9-ms.SSA.high_iron_20190423.bin`
	- Alias: `L2 15.2`
	- Pick a file: `i86bi_linux_l2-adventerprisek9-ms.SSA.high_iron_20190423.bin`
- L3 XE Router
	- Filename: `x86_64_crb_linux-adventerprisek9-ms.bin`
	- Alias: `L3 XE 17.12.1`
	- Pick a file: `x86_64_crb_linux-adventerprisek9-ms.bin`
- L2 XE Switch
	- Filename: `x86_64_crb_linux_l2-adventerprisek9-ms.bin`
	- Alias: `L2 XE 17.12.1`
	- Pick a file: `x86_64_crb_linux_l2-adventerprisek9-ms.bin`

Luego para que los laboratorios reconoscan estas imagenes, haremos copias dummy o tontas, con enlaces simbolicos, aqui esta el proceso, debe tener las mismas caracteristicas que el IOSes que esta en el viejo lab

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
chown apache:apache -Rh *
```

Ahora si vamos otra vez a revisar los IOSes, vemos que ya no esta el simbolo de advertencia, cuando un laboratorio busque esa imagen, sera usada la mas nueva, permitiendo tener una uniformidad de ejecucion moderna, sin necesidad de cambiar ninguna configuracion de los laboratorios ya hechos

## Configuraciones Varias

> Crea y edita `/etc/dhcp/dhclient-eth0.conf` 
```
timeout 3;
retry 4;
reboot 3;
select-timeout 0;
initial-interval 1;
```

> Modifica `/etc/rc.local`
```
dhclient -cf /etc/dhcp/dhclient-eth0.conf eth0 &

export TERM=xterm

echo "Welcome to IOU Web Interface" > /etc/issue
echo "Use http://" >> /etc/issue

sleep 0.5 && ip=$(ip -4 addr show scope global | grep -oP 'inet \K[\d.]+' | head -n1) && sed -i "s|http://.*|http://$ip|" /etc/issue
```

> Modifica `/etc/environment`
```
TERM=xterm
```

**GRUB**

> Modifica el archivo `/etc/default/grub` para que se vean estos parametros asi
```
GRUB_TIMEOUT=1
GRUB_DISABLE_SUBMENU=false
```

> Actualiza la configuracion de grub
```
grub2-mkconfig -o /boot/grub2/grub.cfg
```

> Actualiza la configuracion EFI de grub
```
grub2-mkconfig -o /boot/efi/EFI/centos/grub.cfg
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
wget https://download.virtualbox.org/virtualbox/7.1.10/VBoxGuestAdditions_7.1.10.iso
```

> Crea una carpeta dentro de `/media` con cualquier nombre ("`Vbox`")
```
mkdir /media/Vbox
```

> Monta el disco
```
mount VBoxGuestAdditions_7.1.10.iso /media/Vbox
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

> Ejecuta el ./run
```
/media/Vbox/VBoxLinuxAdditions.run
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
modprobe vboxguest
modprobe vboxsf
modprobe vboxvideo
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

Creamos el archivo `/etc/dracut.conf.d/compression.conf`
```
compress="zstd"
compresslevel="6"
```

Compila dracut
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

> Aplica en caliente todas las configuracion de los archivos que
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
cpu.energy_performance_preference=performance
cpu.platform_profile=performance
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

## Limpieza

Ve a la interfaz web de IOU WEB y elimina los laboratorios y archivos de la pestaña "Laboratories", luego en Manage selecciona "Optimize database", luego ve a "Downloads" y selecciona "Clear session and delete sniffer/import/export/logs files" y luego en "Yes, delete all", esto reiniciara el VM, cuenta hasta 8 y reinicia la pagina

> Elimina Base de datos viejas
```
rm -f /opt/iou/data/database.sdb-*
```

> Detener httpd
```
systemctl stop httpd
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

> Recrea los modulos del kernel
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

> Elimina llaves SSH para ser regeneradas en cada sistema
```
rm -f /etc/ssh/ssh_host_*
```

> Elimina IP de dhclient para que busque una nueva al siguiente boot
```
rm -f /var/lib/dhclient/dhclient.leases
```

> Limpia Journalctl
```
journalctl --vacuum-time=1s
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

> Borra el historial otra vez
```
history -wc
```

> Apaga la maquina para exportar el producto hecho
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
qemu-img convert -f qcow2 -O qcow2 -c rhel7.9.qcow2 iou-web-icarus-chikita.qcow2
```

> Valida la integridad de la maquina comprimida
```
qemu-img check iou-web-icarus-chikita.qcow2
```

> Sobreescribir Imagen (Peso 1.6GB)
```
mv iou-web-icarus-chikita.qcow2 iou-web-icarus.qcow2
```

> Tener un backup .7z (Se demora 5 minutos)
```
7z a -t7z -m0=lzma2 -mx=9 -mmt=on iou-web-icarus.7z iou-web-icarus.qcow2
```

### Exportar Virtualbox

**Imagen QEMU a Virtualbox**

> Convertir a VDI, no soporta compresion, por lo que sera mas pesado (4.2Gb)
```
qemu-img convert -f qcow2 iou-web-icarus.qcow2 -O vdi iou-web-icarus.vdi
```

**Configura Virtualbox**
> [!TIP] Lecturas Recomendadas
> [[500 - Personal/500.3 - Write-Ups/IOU-WEB/IOU WEB - Config Win 10-11|IOU WEB - Config Win 10-11]]: Recopilacion de configuraciones especificas para el Host de Windows