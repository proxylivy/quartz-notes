# Info

## Que tiene de nuevo?
IOU WEB Interface 32 Bits
Modded by Proxylivy

Version 4:
- Correcta Instalacion sin GUI
- IOU WEB Actualizado (1.2.2-23)
- IOU WEB, Base de datos limpia, logs limpios, y arregla tiempos de espera
- Permite el cambio de Paravirtualizacion (KVM), "Adios Heredado"
- Ip Automatica y mostrando al iniciar sesion
- SSL Funciona para resolver https
- Drivers Graficos de Intel Instalados
- CentOS Actualizado a `[6.10]` y Kernel a 6.7 `[2.6.32-573]` completamente funcional + Repo Epel-Release
- Mejoras de Virtualbox `[Guest-Additions ISO]` y VMWare `[Repo Arreglado]`
- No IPTABLES, No Firewall
- Hora y Fecha Sincronizadas con NTP Chile
- No DUP Ping desde la maquina
- Particion Swap Eliminada
- Extension del disco: 7GB -> 12GB desde Administrador de medios virtuales de Virtualbox | [Guia](https://edgarjayo.wordpress.com/2020/11/09/aumentar-el-espacio-del-disco-duro-en-oracle-vm-virtualbox/)
- Link Simbolico de las nuevas imagenes para remplazar las antiguas
- Documentacion Re-Escrita para ser entendida de mejor forma

## Notas Antes de Empezar

> Para acceder via SSH desde otra maquina, debes usar
```
ssh -oHostKeyAlgorithms=+ssh-rsa root@{ip}
```

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

No todas las versiones del Kernel parecen ser compatibles con CentOS 6, las siguientes NO son compatibles con GRUB, crean una pantalla en negro, no responde, problemas en `/lib/modules/[network,modeset,block,etc]`
- 6.10: `[2.6.32-754.35.1.el6.i686]`
- 6.9:  `[2.6.32-696.el6.i686]`
- 6.8:  `[2.6.32-642.el6.i686]`

Las siguientes version si son compatibles
- 6.7  `[2.6.32-573.el6.i686]` (lsmod -> 59)
- 6.6: `[2.6.32-504.el6.i686]`
- 6.5: `[2.6.32-431.el6.i686]`
- 6.4: `[2.6.32-358.el6.i686]`
- IOU: `[2.6.32-358.23.2.el6.i686]` (lsmod -> 27) (Directamente desde el OVA original)

# Configuracion
## Basica

> **Idioma**
> Edita `/etc/sysconfig/i18n` y cambia el valor `LANG=es_US.UTF-8` por
```
LANG="es_CL.UTF-8"
```

> Teclado
> Edita `/etc/sysconfig/keyboard` y cambia los valores a:
```
KEYTABLE="la-latin1"
MODEL="pc105"
LAYOUT="la"
KEYBOARDTYPE=""
```

> Reloj
> Con el primer comando configuras la zona horaria, el segundo configura tu mirror NTP
```
ln -sf /usr/share/zoneinfo/America/Santiago /etc/localtime
ntpdate pool.ntp.org
```

> Modifica `/etc/hosts` y agrega la linea de configuracion al final
> Contexto: Debido a que el tiempo es implacable, las paginas mueren, esta maquina intenta buscar contenido en "routereflector" que luego paso a ser "evilrouters", ambas caidas, asi que el dns local es mejor que las marque como local para que se resuelvan rapidamente
```
127.0.0.3	xml.cisco.com www.routereflector.com routereflector.com public.routereflector.com ww25.public.routereflector.com
```

> **Configura contraseña de root**
> Mi recomendacion es configurar `cisco` como la contraseña por defecto para no reconfigurar nada mas
```
passwd root
```

## Arregla el kernel y los paquetes

> **Crea Repositorio**

> [!IMPORTANT] Nota sobre la creacion del repositorio
> Este repositorio solo instalara el kernel de linux con sus componentes, nada mas. Los demas repositorios como "`iou-web`" y "`vmware`" deben ser comentados
```
cd /etc/yum.repos.d/
rm -f /etc/yum.repos.d/CentOS-*
touch kernel.repo
```

> Edita el archivo `/etc/yum.repos.d/kernel.repo`
```
[kernel]
name=CentOS-Vault - Base(Kernel)
baseurl=http://archive.kernel.org/centos-vault/6.7/os/i386/
enabled=1
gpgcheck=0
```

> Actualiza Repos
> Si tienes problemas de IP, intenta actualizarlas con `dhclient` y `dhclient -r`
```
yum clean all
yum repolist
```

> Instala el Kernel y sus componentes
```
yum install kernel kernel-firmware kernel-devel kernel-headers
```

> Modifica el archivo `/etc/yum.conf` para no actualizar el kernel
> Esta linea debe ser agregada despues de "obsoletes"
```
...
exclude=kernel*
...
```

> Elimina el archivo `/etc/yum.repos.d/kernel.repo`
> Esto es porque ya no se actualizara el kernel otra vez y genera conflictos con el repo `[base]` de VaultCentOS
```
rm -f /etc/yum.repos.d/kernel.repo
```

> Reinicia la maquina y haz el siguiente paso mientras enciende
```
reboot
```

> Cuando inicie GRUB, mueve las fechas de arriba o abajo `↑` o `↓`, se abrira un menu de seleccion y debes seleccionar el kernel mas nuevo, posiblemente sea `[573]`, talvez se vea asi
```
2.6.32-573.el6.i686 [Este deberias elejir]
2.6.32-123.el6.i686
2.6.32-345.el6.i686
```

> Luego cuando inicie, verifica la version del kernel con `uname -r`, deberia decir, si no es asi, verifica las actualizaciones y el kernel correcto
```
2.6.32-573.el6.i686
```

> Ahora podras borrar los kernels antiguos
> yum no es tonto, hara "skip" automaticamente a los kernels que esten ejecutandose, excepto por los viejos
```
yum remove kernel
```

> Modifica `/boot/grub/grub.conf` con la siguiente informacion
```
default=0
timeout=3
splashimage=(hd0,0)/grub/splash.xpm.gz
hiddenmenu

title CentOS (2.6.32-573.el6.i686)
        root (hd0,0)
        kernel /vmlinuz-2.6.32-573.el6.i686 ro root=/dev/sda1 rd_NO_LUKS rd_NO_LVM rd_NO_MD rd_NO_DM rd_NO_DMraid loglevel=3 crashkernel=auto SYSFONT=latarcyrheb-sun16 rhgb quiet audit=0
        initrd /initramfs-2.6.32-573.el6.i686.img
```

> **Actualiza repositorios**

> Elimina viejos repositorios que ya no necesitas (Talvez se tengan que borrar despues)
```
rm -rf /etc/yum.repos.d/CentOS-*
```

> Crea el archivo `/etc/yum.repos.d/Vault-CentOS.repo`
```
[base]
name=CentOS-Vault - Base
baseurl=http://archive.kernel.org/centos-vault/6.10/os/i386/
enabled=1
gpgcheck=0

[updates]
name=CentOS-Vault - Updates
baseurl=http://archive.kernel.org/centos-vault/6.10/updates/i386/
enabled=1
gpgcheck=0

[extras]
name=CentOS-Vault - Extras
baseurl=http://archive.kernel.org/centos-vault/6.10/extras/i386/
enabled=1
gpgcheck=0

[contrib]
name=CentOS-Vault - Contrib
baseurl=http://archive.kernel.org/centos-vault/6.10/contrib/i386/
enabled=1
gpgcheck=0
```

> Actualiza Repositorios con yum
```
yum clean all
yum repolist
```

> Actualiza aplicaciones y reinicia la maquina
```
yum install openssl-devel yum-utils
yum update
reboot
```

> Install Epel Repo
```
yum install epel-release
```

> Descomenta y habilita `epel-testing`
```
TO-DO
```

> VMWare Repo Config
> Edita `/etc/yum.repos.d/vmware.repo`
```
#
# Latest VMWare OSPs for RHEL6
#
[vmware-tools-collection]
name=vmware-tools-collection
baseurl=https://packages.vmware.com/tools/esx/latest/rhel6/i386/
enabled=1
gpgcheck=1
gpgkey=https://packages.vmware.com/tools/keys/VMWARE-PACKAGING-GPG-RSA-KEY.pub
```

> Actualiza Repositorios
```
yum cleanall
yum repolist
yum update
```

> Instala Micro
```
curl https://getmic.ro | bash
mv micro /bin/
```

> Instala Grupos de paquetes
```
yum groupinstall "Compatibility Libraries" "Development Tools"
```

> Instala Paquetes Sueltos
```
yum install libvirt virt-viewer qemu-guest-agent telnet-server xinetd cmake htop tmux screen byobu man php-gd php-xml httpd-devel pcre-devel dkms xclip xsel libcap-devel dosfstools rsyslog syslog-ng tftp-server lsof perl-IO-Tty perl-Time-HiRes perl-Authen-PAM terminus-fonts-* perl-LDAP
```

> Compilar OpenSSH desde la fuente oficial

> [!IMPORTANT] Notas sobre Compilar OpenSSH
> - Esta limitada la version por la version de OpenSSL (1.0.1) que tiene instalada el sistema
> - Puedes ver la fuente de las versiones de OpenSSH Portable desde su [pagina oficial de Descargas](https://cdn.openbsd.org/pub/OpenBSD/OpenSSH/portable/)
> El ultimo cliente OpenSSH Portable compatible para 32 bits es "openssh-9.3p1" de 2024
```
cd
mkdir git
cd git
wget https://cdn.openbsd.org/pub/OpenBSD/OpenSSH/portable/openssh-9.3p1.tar.gz
tar xvf openssh-9.3p1.tar.gz
rm -drf openssh-9.3p1.tar.gz
cd openssh-9-3p1
./configure
make
make install
reboot
```

> Solucionar problemas con libcrypto.so

> [!IMPORTANT] Contexto para poder solucionar el problema
> - Al actualizar los paquetes de CentOS, la libreria libcrypto que depende de OpenSSL cambia de nombre, su cambio es de "`libcrypto.so.4`" a "`libcrypto.so.10`", por lo que los router nunca la encontraran para lograr funcionar
> - Se debe hacer un Hard-Link para poder encontrarla correctamente dentro de `/usr/lib/`
- Lista las librerias, debes observar el nombre de la libreria original o sin enlace
```
ls /usr/lib/ | grep libcrypto
```
- Haz el link de la libreria
```
ln /usr/lib/libcrypto.so.10 /usr/lib/libcrypto.so.4
```

**Instalar Virtualbox Guest ISO**
> [!IMPORTANT] Notas sobre Guest ISO
> - Para saber la version actual que tienes instala, debes abrir Virtualbox, luego ir a "`Ayuda`" > "`Acerca de Virtualbox`", el requisito es que la imagen de guest debe ser la misma que la version instalada en el host, en mi caso `7.0.14`
> 1. Con la maquina apagada, en tu computador Host, entras a la [Pagina Oficial de Descargas](https://download.virtualbox.org/virtualbox/) y descargas el archivo ISO llamado "`VBoxGuestAdditions_{version}.iso`" y debe ser la misma version que tu virtualbox instalado
> 2. Luego en las configuracion de la maquina dentro de virtualbox, vas a la seccion de "Almacenamiento" > "Controlador:" y en la parte de abajo, le das a "Añadir Conexion" (Es el Floopy Disk con un signo "+"), > "Unidad Optica" > "VBoxGuestAdditions.iso"
> 3. Enciendes la maquina y ejecutas los siguientes comandos

> [!IMPORTANT] Nota sobre los comandos
> - Los comandos `/sbin/rcvboxadd quicksetup all` y `/sbin/rcvboxadd setup` son identicos para el proceso de instalacion, es innecesario usar ambos si ya funciono uno de los 2
```
mkdir /mnt/cdrom
mount /dev/cdrom /mnt/cdrom
cd /mnt/cdrom
./VBoxLinuxAdditions.run
dmesg
cat /var/log/vboxadd-setup.log
```

> Modifica el archivo `/etc/dhcp/dhclient-eth0.conf` 
> Cambia los tiempos de DHCLIENT para hacer un inicio mas rapido
```
timeout 5;
retry 5;
reboot 5;
select-timeout 0;
initial-interval 1;
```

## IOU-WEB
> Desactiva HTTPD Temporalmente
```
service httpd stop
```

> **Instala IOU WEB**

> [!IMPORTANT] Nota sobre Instalacion IOU WEB
> Debido a que cuando un servidor web se instala, las paginas de pruebas se instalan en `/var/html` o rutas parecidas, las cuales tendran alta prioridad de funcionamiento, es importante borrar las paginas de pruebas para que se ejecute IOU WEB

> Borra los archivos temporales
```
sudo rm -drf /var/tmp
```

> Copia el repositorio actual de IOU WEB desde Github
```
cd ~/git
git clone https://github.com/dainok/iou-web.git
```

> Abre la carpeta de IOU WEB e instala el archivo .rpm que contiene IOU WEB
```
cd iou-web
rpm -Uvh iou-web-1.2.2-23.i386.rpm
```

> Mueve las carpetas instaladas en `~/git/iou-web/` a `/opt/iou` 
```
rsync -Pavrh html /opt/iou/
cp bin/* /opt/iou/bin/
cp cgi-bin/* /opt/iou/cgi-bin/
```

> Arregla problemas con dependencia de carpetas y archivos vacios necesarios
```
mkdir /opt/iou/html/iou-web
touch /opt/iou/html/iou-web/version
touch /opt/iou/html/iou-web/whatsnew
mkdir -p /opt/iou/html/iou-web/yum/repodata/
touch /opt/iou/html/iou-web/yum/repodata/repomd.xml
```

> Comenta todas las lineas de `/etc/yum.repoos.d/iou-web.repo` debido a que el programa no esta disponible
```
micro /etc/yum.repos.d/iou-web.repo
```

> Actualiza los permisos para la carpeta de IOU-WEB y sus subcarpetas para que apache puede manejar y controlar las rutas correctamente
```
chmod 755 -R /opt/iou
```

> Solucionar problemas de rutas con hardlinks
```
cd /opt/iou/html/xinha/plugins/TableOperations
ln TableOperations.js table-operations.js
cd ../SuperClean
ln SuperClean.js super-clean.js
cd ../Linker
ln Linker.js linker.js
cd ../CharacterMap
ln CharacterMap.js character-map.js
cd ../SpellChecker
ln SpellChecker.js spell-checker.js
cd ../../../css/contextmenu
ln cmenu-gloss-semitrasparent-menu-item-hover.png cmenu-item-gloss-semitransparent-menu-item-hover.png
ln cmenu-xp-bg.gif cmenu-gloss-bg.gif
```

> Reinicia HTTPD
> De esta forma podras probar si es que esta funcionando
> Extra: Si accedes a la interfaz WEB de IOU, ve a "`Downloads`", luego apreta "`Clear Sesion...`", te saldra una eleccion y le das en "`Yes, Delete All`"
```
service httpd restart
```

---
> Modifica `/etc/xinetd.d/tftp` y corrige a
```
disable=no
```

> Modifica de `/etc/xinetd.d/tftp`
```
disable=no
```

---
> **Re-inicia servicios**
```
service rsyslog restart
chkconfig rsyslog on
service xinetd restart
chkconfig xinetd on
service syslog-ng restart
chkconfig syslog-ng on
```

---
> **Configura Interfaces de Red**

> **Modifica `/etc/sysconfig/network-scripts/ifcfg-eth0`**

> [!IMPORTANT] Notas sobre Interfaces
> Hay un truco de feria, tienes que cambiar el valor "`DEVICE=eth0`" por la interfaz que necesitas (Puedes revisar con `ip a`), luego ejecutar `service network start`, `service network reload`, `chkconfig network on`, luego cambiar ese valor temporal de " `DEVICE`" a `eth0`, ejecutar `service network reload` y luego reinciar la maquina con `reboot`
```
DEVICE="eth0"
BOOTPROTO="dhcp"
NM_CONTROLLED="no"
ONBOOT="yes"
TYPE="Ethernet"
USERCTL="no"
DNS1="1.1.1.1"
```

> Reinicia servicio de internet
```
service network restart
chkconfig network on
```

---
> Arreglar IP Automatica al iniciar
> Modifica `/etc/rc.local`

> [!IMPORTANT] Notas para Arreglar IP
> - El siguiente comando dentro de `/etc/rc.local` modificara el archivo `/etc/issue` cada vez la maquina se inicie, asi que no la modifiques a mano, su contenido normal es "`http:///`"
> - `/etc/udev/rules.d/*` los maneja `/lib/udev/write_net_rules`

```
touch /var/lock/subsys/local
rm -drf /etc/udev/rules.d/70-persistent-net.rules
dhclient &
loadkeys la-latin1
sleep 2 && ip=$(ip -4 a show | grep -oP 'inet \K[\d.]+' | grep -v 127.0.0.1); sed -i "s|http:///|http:///$ip|" /etc/issue
```

> Modifica `~/.bashrc`
Luego ejecuta `source ~/.bashrc`
```
export PATH=$PATH:/opt/iou/bin/
export PATH=$PATH:/usr/local/openssl/bin/
export TERM=xterm
```

> Modifica `/etc/environment`
```
TERM=xterm
```

> **Desactiva Selinux**
Modifica `/etc/selinux/config` y cambia el valor al siguiente
```
SELINUX=disabled
```

> Borrar Interfaces al apagar el computador
Edita `/etc/rc.d/rc0.d/K01reboot`
```
ip rule flush
rm -drf /etc/udev/rules.d/70-persistent-net.rules
```

> Dale permisos al K01reboot
```
chmod 777 /etc/rc.d/rc0.d/K01reboot
```

---
> **Cambia Opciones de GRUB**

> [!IMPORTANT] Notas sobre GRUB
> No recomiendo configurar el valor "`timeout=0`" porque en caso de fallo, no tendras acceso a grub para poder cambiar algun valor, el valor minimo deberia ser "`timeout=1`"

> Modifica `/boot/grub/grub.conf`, cambiando el valor `timeout=5` a
```
timeout=1
```

> Configura Telnet
```
service xinetd start
chkconfig xinetd on
chkconfig telnet on
```

> **Envia Nuevas Imagenes**

> [!IMPORTANT] Notas sobre las nuevas Imagenes
> - Puedes descargar las imagenes desde [Labhub](https://drive.labhub.eu.org/0:/addons/iol/bin/) | [Mirror1](https://labhub.eu.org/es/) | [Mirror2](https://legacy.labhub.eu.org/0:/) | [Mirror3](https://alist.labhub.eu.org)
> - Te puedes guiar por las recomendadas en la documentacion de [EVE-NG](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-cisco-iol-ios-on-linux/)

> Listado de maquinas configuradas en IOU WEB
```
L2 15.0: /opt/iou/bin/I86BI_LINUXL2-UPK9-M-15.0
L2 15.1: /opt/iou/bin/i86bi_linux_l2-ipbasek9-ms.jan24-2013-B
L2 15.1 NOV 2013: /opt/iou/bin/i86bi_linux_l2-adventerprise-ms.nov11-2013-team_track
L2 15.1J: /opt/iou/bin/i86bi_linux_l2-upk9-ms.june20_2012_golden_spike
L2 15.1M: /opt/iou/bin/i86bi_linux_l2-ipbasek9-ms.may8-2013-team_track
L2 15.2D: /opt/iou/bin/i86bi_LinuxL2-AdvEnterpriseK9-M_152_May_2018
L3 12.4 TPG: /opt/iou/bin/I86BI_LINUX-TPGEN-ADVENTERPRISEK9-M-12.4
L3 15.0: /opt/iou/bin/i86bi_linux-p-ms.251012_golden_spike
L3 15.2.3: /opt/iou/bin/I86BI_LINUX-ADVENTERPRISEK9-M-15.2.3
L3 15.2.4.M1: /opt/iou/bin/i86bi_linux-adventerprisek9-ms.152-4.M1
L3 15.3.1T: /opt/iou/bin/i86bi_linux-adventerprisek9-ms.153-1.3.T
L3 15.4.1T A: /opt/iou/bin/i86bi_linux-adventerprisek9-ms.154-1.T_A
L3 15.5: /opt/iou/bin/i86bi-linux-l3-adventerprisek9-15.5.2T
L3 154-1T_AntiGNS3: /opt/iou/bin/i86bi_linux-adventerprisek9-ms.154-1.T_AntiGNS3
```

> Desde el VM
```
cd /opt/iou/bin
rsync -Pavrh $USER@$IP:/ruta/imagenes/bin /opt/iou/bin
```

**Eliminar Viejas Imagenes desde `/opt/iou/bin`**

> Eliminar Imagen Router
```
rm -f /opt/iou/bin/i86bi_linux-adventerprisek9-ms.154-1.T_A
```

> Eliminar Imagen Switch L2
```
rm -f /opt/iou/bin/i86bi_LinuxL2-AdvEnterpriseK9-M_152_May_2018.bin
```

> Cambiar Permisos Nuevas Imagenes
```
chmod 755 *
chown -R apache:apache *
```

Posible fix
> Deberia modificar las imagenes y pasar de .bin a sin .bin?????

> **Link Nuevas Imagenes**

> Link Router L3 And PC: L3 15.4.1T A
```
ln -s /opt/iou/bin/i86bi_LinuxL3-AdvEnterpriseK9-M2_157_3_May_2018.bin /opt/iou/bin/i86bi_linux-adventerprisek9-ms.154-1.T_A
```

> Link Switch L2/L3: L3 15.2D
```
ln -s /opt/iou/bin/i86bi_linux_l2-adventerprisek9-ms.SSA.high_iron_20190423.bin /opt/iou/bin/i86bi_LinuxL2-AdvEnterpriseK9-M_152_May_2018
```

Cambiar el permiso a los enlaces
```
chmod 755 *
chown -R apache:apache *
```

---
# Limpiar

> Revisa las flags de CPU (Para que veas como evoluciona el sistema)
```
grep -E 'vmx|svm' /proc/cpuinfo
lscpu
grep flags /proc/cpuinfo
```


> Elimina los Logs con +7 dias de antiguedad
```
find /var/log -type f -mtime +7 -exec rm {} \;
```

> Limpia yum
```
yum clean all
```

> Elimina archivos temporales
```
rm -drf /tmp/*
```

> Desactiva IPTABLES
```
iptables -L
iptables -F
iptables -X
iptables -L
service iptables status
```

> Borra el Historial y Apaga la Maquina
```
echo "" > ~/.bash_hostory
history -wc && poweroff
```


---
Para configurar el Host de Windows, recomiendo leer [[500 - Personal/500.3 - Write-Ups/IOU-WEB/IOU WEB - Config Win 10-11|IOU WEB - Config Win 10-11]]
