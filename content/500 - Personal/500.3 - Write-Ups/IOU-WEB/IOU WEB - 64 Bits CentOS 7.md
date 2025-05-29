# Info
Necesitas:
- CentOS 7 - Minimal ISO
- Gitub - [dainok/iou-web](https://github.com/dainok/iou-web)

Haz una instalacion minima, con 2 usuarios, permite el uso de ssh a root
- `root:cisco`
- `duoc:cisco`
Inicia sesion con `root:cisco`, sera mas sencillo, evitandote escribir sudo a cada rato

## Ayuda
- [CISCO IOL-IOU](https://github.com/come-tardivel/eve-ng-guide/wiki/CISCO-IOL%E2%80%90IOU)
- [My Howtos and Projects - Cisco IOU - Installing and Running](https://myhowtosandprojects.blogspot.com/2013/08/installing-and-running-iou-checking_10.html)
- https://networkhaven.blogspot.com/2014/02/cisco-iou.html
- [Instalacion via ovf](https://thomaslowblog.wordpress.com/2015/08/18/cisco-iou-installation-steps-on-vmware/)
- [Install via VMWare](https://vmgeeks.wordpress.com/2012/07/21/deploying-cisco-iou-web-interface-on-vmware-esxi/)

Post Importantes
- [Blog - Brezular - IOU on Fedora Linux](https://brezular.com/2011/04/30/iou-on-fedora-linux/)
- [Blog - Brezular - Create Cisco IOUL2 QEMU](https://brezular.com/2011/10/23/creating-a-cisco-switch-using-ioul2-loaded-on-centos-qemu-image/)
- [Blog - Brezular - Connect IOU with iou2net.pl](https://brezular.com/2013/10/09/how-to-connect-iou-to-a-real-cisco-gear-using-iou2net-pl/)
- [Blog - Kovacs - Cisco IOU web interface](https://kovacsdaniel.blogspot.com/2015/02/cisco-iou-with-web-interface.html)


Revisa esto desde la instalacion de IOU-WEB
Blogs de Brezular que te pueden ayudar, parece todo ser muchisimo mas sencillo con esta info
- https://brezular.com/2011/09/01/building-linux-l3-switchrouter-on-x86-part1-introduction/
- https://brezular.com/2011/09/01/building-linux-l3-switchrouter-on-x86-part2-centos-6-0-installation/
- https://brezular.com/2011/10/23/creating-a-cisco-switch-using-ioul2-loaded-on-centos-qemu-image/
- https://brezular.com/2012/02/17/installation-solaris-2-6-sparc-on-qemu-part2-solaris-installation/
- https://brezular.com/2011/11/01/cisco-network-device-based-on-iou-installed-on-core-linux/
- https://brezular.com/2012/04/08/installation-solaris-sparc-2-6-sunos-5-6-on-qemu-part3-iou2net-pl-installation/
- https://brezular.com/2013/10/15/how-to-connect-iou-to-a-real-cisco-gear-using-iou-live-int2netio/
- https://brezular.com/2013/10/09/how-to-connect-iou-to-a-real-cisco-gear-using-iou2net-pl/
- https://brezular.com/2011/04/30/iou-on-fedora-linux/

Errores:
- Talvez me ayude a entender que se debe modificar [DockerFile Dedian](https://github.com/Nanue1/web-iou-docker/blob/master/Dockerfile)
- [This is so hard](https://myhowtosandprojects.blogspot.com/2013/08/installing-and-running-iou-checking_10.html)
- [Dockerize IOU WEB](https://democracyresourcecenter.com/cisco-iou-web-interface-license)
- https://vmgeeks.wordpress.com/2012/07/21/deploying-cisco-iou-web-interface-on-vmware-esxi/
- https://thomaslowblog.wordpress.com/2015/08/18/cisco-iou-installation-steps-on-vmware/


Gracias a:
- [Repositorio IOU WEB Oficial Github](https://github.com/dainok/iou-web)
- [Instalacion IOU WEB en Fedora](https://brezular.com/2011/04/30/iou-on-fedora-linux/)
- [Usar IOU WEB](https://thomaslowblog.wordpress.com/2015/08/18/using-cisco-iou/)
- [Crea una imagen de IOU WEB con VMWARE](https://vmgeeks.wordpress.com/2012/07/21/deploying-cisco-iou-web-interface-on-vmware-esxi/)

## Paso a Paso

Marcar Repositorios como Backup desde `/etc/yum.repos.d/`
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

Crea el repo en `etc/yum.repos.d/`
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

[contrib]
name=CentOS-Vault - Contrib
baseurl=http://archive.kernel.org/centos-vault/7.9.2009/contrib/x86_64/
enabled=1
gpgcheck=0
```

Instalar Remi
```
rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
yum repolist
```

Instalar Grupos de Paquetes
```
yum groupinstall "Compatibility Libraries" "Development Tools"
```

Instalar DNF
```
yum install dnf
```

Instalar Paquetes compatibilidad 32 bits
```
dnf install glibc.i686 libstdc++.i686 zlib.i686 openssl-libs.i686 libpcap.i686 libX11.i686 libXext.i686 glibc-static libstdc++-static glibc.i686 openssl-devel.i686 
```

Instalar Paquetes Programacion
```
dnf install rsync openssl-devel tar git gcc cmake autoconf wget gzip gunzip libxml2-devel sqlite-devel libcurl-devel libjpeg-devel libpng-devel freetype-devel dialog open-vm-tools net-tools psmisc dos2unix gmp-devel libmpc-devel mpfr-devel
```

Instalar Paquetes Sueltos
```
dnf install libvirt virt-viewer qemu-guest-agent telnet-server xinetd cmake htop tmux screen byobu man php-gd php-xml httpd-devel pcre-devel dkms xclip xsel libcap-devel dosfstools rsyslog syslog-ng tftp-server lsof perl-IO-Tty perl-Time-HiRes perl-Authen-PAM terminus-fonts-* perl-LDAP ntp terminus-fonts bind-utils telnet
```

Instalar PHP
> No se encuentra `php-pecl-mysql`
> - No se ve necesario el remi, pero cualquier cosa
> - dnf module reset php && dnf module enable php:{remi-version}
```
dnf install php php-common php-cli php-curl php-fpm php-mysqlnd php-gd php-xml php-mbstring php-pdo php-zip php-sqlite3 php-pspell
```

Instalar Fish
> Recuerda ir a la carpeta `/etc/yum.repos.d`
```
wget https://download.opensuse.org/repositories/shells:fish:release:3/CentOS_7/shells:fish:release:3.repo

dnf install fish
```

Instalar Nerd Fonts (Opcional)
```
mkdir git && cd git
git clone --filter=blob:none --sparse https://github.com/ryanoasis/nerd-fonts.git
cd nerd-fonts
git sparse-checkout add patched-fonts/Hack
git sparse-checkout add patched-fonts/JetBrainsMono
./install.sh Hack
./install.sh JetBrainsMono
```

Arreglar enlaces simbolicos
> Tienes 2 Metodos, recomiendo el primero:
> - 1: descarga el que esta [aqui](https://drive.labhub.eu.org/1:/addons/iol/lib/) y envialo a `/usr/lib/` 
> - 2: **Enlace simbolico a Libcrypto sistema**: `ls /usr/lib/libcrypto.so*`
```
rsync root@{ip-server}:~/libcrypto.so.4 /usr/lib/
OR
ln -s /usr/lib/libcrypto.so /usr/lib/libcrypto.so.4
```

Modificar `/etc/hosts`
```
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
127.0.0.2   iou.example.com iou
127.0.0.127 xml.cisco.com
127.0.0.1   www.routereflector.com routereflector.com public.routereflector.com ww25.public.routereflector.com
```

Crea Carpetas
> Para IOU y OVF
```
mkdir /opt/iou
mkdir /opt/ovf
```

Envia el archivo `iou.conf` y dejalo en `/etc/httpd/conf.d/`
```
rsync -druLPO root@{ip-old-server}:/etc/httpd/conf.d/iou.conf /etc/httpd/conf.d/
```

Modifica el archivo `iou.conf`, agregando `+Indexes` a la linea 6, para que quede asi
```
Options +Indexes -FollowSymLinks
```

Modifica el archivo `iou.conf`, agregando `Require all granted` dentro `<Directory /opt/iou/data>`
```
Require all granted
```

Modifica las directivas de la configuracion por defecto
- Encuentra la linea `DocumentRoot`(124) desde `/etc/httpd/conf/httpd.conf` y modifica
```
DocumentRoot "/opt/iou/html"
```
- Encuentra la linea `Directory` desde `/etc/httpd/conf/httpd.conf` y modifica
```
<Directory "/opt/iou/html">
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
```


Enviar archivos de IOU WEB
```
rsync -druLPO root@{old-ip-server}:/opt/iou/ /opt/iou/
```

Arreglar Error Logs
> Para evitar problemas con selinux se podria poner `setenforce 0`, pero parece que estos comandos lo arreglan
```
setenforce 0
touch /opt/iou/data/Logs/error.txt
chown apache:apache /opt/iou/data/Logs/error.txt
ls -lZ /opt/iou/data/Logs/error.txt
sudo chcon -Rv system_u:object_r:httpd_log_t:s0 /opt/iou/data/Logs
chmod 755 -R /opt/iou
chown root:apache -R /opt/iou
chmod 777 /opt/iou/data/
```

Modificar reglas de Firewall
> Se porta mal asi que un `systemctl disable --now firewalld` le hara reflexionar
```
firewall-cmd --add-service=https --permanent
firewall-cmd --add-service=https --permanent
firewall-cmd --reload
```

Borrar `/etc/hostname`
> Se creara uno con ovf
```
rm -f /etc/hostname
```

Cambio de Hostname de la forma correcta
> shot-hostname = iou
> dns-domain-name = example.com
> ESTE PASO SE DEBE HACER CON OVF, NO CON SYSTEMD, FALLARA
```
rsync -druLPO root@{old-ip-server}:/etc/init.d/ovfconfig /etc/init.d/ovfconfig

rsync -druLPO root@{old-ip-server}:/opt/ovf/ /opt/ovf/
```

Ejecutar OVF
> Reiniciara la maquina
```
/etc/init.d/ovfconfig force-config
```

Las respuestas son
> La contraseña es cisco, los demas por defecto, solo enter
```
cisco
cisco
enter
enter
enter
enter
enter
```

Keygen Update to Python3
- vas a `/opt/iou/scripts/`, luego ejecutas `python keygen.py`
```
#! /usr/bin/python3
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

Crea llave licencia
> Modifica `/opt/iou/bin/iourc`
```
[license]
iou.example.com = 145ef75ad4ea0ebd;

OR
[license]
iou.example.com = d66475be295f2100;
```

Edita el archivo de sudoers !!!
- Agrega las lineas
```
user    ALL=(ALL:ALL)   ALL
apache  ALL=(ALL:ALL)   ALL
apache  ALL=(ALL) NOPASSWD: ALL
```

Desabilita Selinux
> Modifica `SELINUX` -> `disabled` y `SELINUXTYPE` -> `minimum`
```
setenforce 0
```

Inicia HTTPD y Toca madera
```
systemctl daemon-reload
systemctl start httpd
systemctl enable httpd
```

Modifica Grub
> Debes Modificar el archivo `/etc/default/grub`, no parece funcionar
- Para iniciar mas rapido, modifica `GRUB_TIMEOUT` -> `1`
- Para no mostrar el menu, modifica `GRUB_DISABLE_SYVMENU` -> `false`
```
grub2-mkconfig -o /boot/grub2/grub.cfg
grub2-mkconfig -o /boot/efi/EFI/centos/grub.cfg
```

---
---

# Extra

## Testear Imagenes
Nota 2222 puerto TCP ; 200 ID-APP
```
./wrapper-linux -m ./i86bi_linux-adventerprisek9-ms -p 2222 -- -e 1 -s 1 200
```
### Conectate al router puerto TCP
```
telnet localhost 2222
```
### Dejar de correr imagenes
```
ps -aux | grep wrapper-linux | grep 200 | kill `echo $(cut -d " " -f2)`
```

## Parchear Imagenes
Si fallan el test de encendido, posiblemente tendras que parcharlas

Compila bbe
> Para poder parchear las imagenes
```
cd ~/git
git clone https://github.com/hdorio/bbe.git
cd bbe
./configure
make
make install
```
## Parchea Imagenes L3
```
for F in i86bi_linux-*;do bbe -b "/xfcxffx83xc4x0cx85xc0x75x14x8b/:10" -e  "r 7 x90x90" -o $F.x $F;mv $F.x $F;done;chmod +x ./i86bi_linux-*
```
## Parchea Imagenes L2
```
for F in i86bi_linuxl2*;do bbe -b "/xa1xffx83xc4x0cx85xc0x75x17x8b/:10" -e "r 7 x74" -o $F.x $F;mv $F.x $F;done;chmod +x ./i86bi_linuxl2*
```


# Optimizar

Instalar VirtualBox Additions
> Agrega el Disco correspondiente a la version, si falla, te enviara un mensaje por `dmesg`
```
mkdir /media/V
mount /dev/cdrom /media/V
sh /media/V/VBoxLinuxAdditions.run
```

Optimiza VM con sysctl
> Modifica el archivo `/etc/sysctl.conf` con el siguiente contenido, luego lo aplicas con `sudo sysctl -p`
```
vm.swappiness = 10
vm.dirty_ratio = 15
vm.dirty_background_ratio = 5
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216
net.ipv4.tcp_rmem = 4096 87380 16777216
net.ipv4.tcp_wmem = 4096 65536 16777216
net.ipv4.tcp_window_scaling = 1
```

Optimiza Qemu
> Tambien puedes revisar los modulos de kernel con `lsmod | grep virtio`
```
sudo yum install -y qemu-guest-agent
sudo systemctl enable qemu-guest-agent
sudo systemctl start qemu-guest-agent
```

# Limpieza
```
dnf clean all
rm -rf /var/cache/yum/
rm -rf /tmp/*
rm -rf /var/tmp/*
find /var/log -type f -exec truncate -s 0 {} \;
dd if=/dev/zero of=/zerofile bs=1M
rm -f /zerofile
dracut -f -v
grub2-mkconfig -o /boot/grub2/grub.cfg
grub2-mkconfig -o /boot/efi/EFI/centos/grub.cfg
unset HISTFILE
history -c
```

Convertir a VDI
```
qemu-img convert -f qcow2 centos.qcow2 -O vdi centos-iouweb.vdi
```

