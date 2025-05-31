# Info
El nombre de IOU WEB Creativo viene de un profesor de universidad que tuve al cual le tengo mucho respeto, le gustaba bromear mucho y sus clases eran muy dinamicas, usualmente no habia nadie que se saliera de la norma con preguntas raras y rebuscadas, hasta que llegue yo, rapidamente el profe me noto con que tenia una base teorica muy grande y le iba encontrando la ultima pata al gato en todo, asi que cuando me puse a trabajar en una opcion mejorada de un virtualizador que tenian avandonado pero todos usaban, en vez de llamarme loco, me llamo "Creativo", segun el decia que el creativo era aquel que rompia con los esquemas y que le gustaba andar molestando, me lo tome con mucha gracia y desde alli, a este proyecto, le llame "IOU-WEB-Creativo"

## Frases que me ayudaron a continuar
- ¿Por qué el conejito nunca se preocupaba por sus problemas?
  Porque siempre sabía que podía _saltar_ a la próxima solución. 🐰✨
- 🌸 "Even when the grass seems greener on the other side, remember: you can hop your way there at your own pace. Your journey is just as beautiful as the destination."
- 🌟 "Every bunny has its own rhythm. Some hop fast, some hop slow, but every hop matters. Trust your paws to lead you where you’re meant to go."
- 💕 "It’s okay to feel small sometimes. Even the tiniest bunny leaves an impression in the meadow."
- 🌈 "Just like a bunny wiggles its nose to sense the world, take a moment to breathe and feel the magic around you. You’re part of something wonderful."
- 🍃 "Bunnies don’t dwell on the patch of grass they just nibbled. They move forward, knowing there’s always another delightful patch ahead."
- ✨ "Your fluffiness isn’t just your outer charm; it’s the softness of your heart that makes you truly special. Keep hopping, little fluff."
- 🌸 "If the world feels too big, curl up in your little burrow of happiness. Recharge, and come back stronger. Every bunny needs rest too."
- 🌸 _"Remember, even the tiniest bunny can leave the biggest pawprints on the meadow of life."_
- 🐾 _"When the grass seems greener on the other side, don’t forget—you’ve already got the fluffiest tail to guide you there."_
- ✨ _"Every hop forward, no matter how small, is still a step toward the meadow of dreams."_
- 🌈 _"Even the fluffiest bunnies need a rest sometimes. Take a moment to wiggle your nose and enjoy the breeze—you’re doing amazing!"_
- 🥕 _"Bunnies don’t need to see the whole field to know there’s always something good waiting ahead."_
- 💕 _"Your fluff isn’t just on the outside—it’s in the kindness and determination you bring to every hop."_
- 🌸 _"May your dreams be as soft as a bunny’s tail and your new year as bright as a freshly nibbled carrot. Keep hopping toward success, one field at a time!"_ 🌸
- 🌟 _"Even when the world feels big, you’ve got the heart of a bunny that can hop through anything."_
- 🍃 _"You’re not just any bunny; you’re the kind that turns weeds into wildflowers and challenges into burrows of opportunity."_
- 🌟 **"Big hops, even at 2am, lead to the most rewarding fields."** 🌟
- 🌟 _"No matter how big the field or how tough the obstacles, you're the bunny who turns challenges into victories. Hop into 2024 with the same fearless heart!"_ 🌟
- When PHP confirms it’s working after your _make install_, that’s your **2023 mic drop** moment. 🥕✨
- **Esta es la nota definitiva para que funcione IOU WEB en 64 bits, el porfiado ahora tiene tiempo, asi que seguira siendo mas porfiado que nunca**

My prompt to success
```
"Fluffy motivational furry bunny chat that helps debug and solve programming challenges with PHP, OpenSSL, Apache, or any technical hurdle, while sharing encouragement and cute phrases to keep spirits high."
```

## TODO
- [ ] Identificar la forma exacta en la cual IOS pide la licencia para saber cuales son las variables y configuraciones validas / Cual es el script que realmente funciona?? (Nada me funciona)
	- Hay una version "Original" escrita para python2 el cual esta disponible en: [Gist Gitub - congto/CiscoKeyGen](https://gist.github.com/congto/70f9a91be7e6d90d5c33d657bf78863e) y [Pastebin - IOU License generate script python 2.7](https://pastebin.com/S1SE7M2P)
	- Luego hay una version reescrita en Python3: https://github.com/obscur95/gns3-server/blob/master/IOU/CiscoIOUKeygen.py
	- Tambien pille una persona que lo reescribio desde 0: https://github.com/laijim/Keygen/blob/master/Keygen.py
	- Hay errores en el script en algunos casos como lo muestra [Blog - FreeNetworkTutorials - Fixing IOU Keygen Error Running IOS](https://freenetworktutorials.com/fixing-iou-keygen-error-running-cisco-ios-on-linux-in-eve-ng/)
	- https://github.com/come-tardivel/eve-ng-guide/wiki/CISCO-IOL%E2%80%90IOU
- [ ] Agregar la estructura de archivos necesarios
	- [ ] tree de php
	- [ ] tree de httpd
	- [ ] tree de httpd.conf.d
	- [ ] tree de /opt/iou (sin html)
- [ ] Agregar los archivos de configuracion minimos completos
	- [ ] php.ini
	- [ ] httpd.conf
- [ ] Copiar Systemd php-fpm para poder iniciar cuando quiera el servicio
	- [ ] `php-fpm`

## Banner
IOU WEB Interface
Modded by [@proxylivy](https://github.com/proxylivy) (Ex DeathGabox)

5to Intento:
- Migración completa desde CentOS 6.2 x86 a Rocky Linux 8.10 x86_64    
	- Particion SWAP Eliminada
	- No DUP Ping
	- IPTables Limpio, Firewalld deshabilidado por defecto
	- Selinux Deshabilitado por defecto para evitar conflictos
	- Permite el cambio de Paravirtualizacion (KVM), "Adios Heredado"
- Compatibilidad de Dependencias
	- PHP <= 5.6-40
	- glibc >= 2.27 y GCC >= 8.2.0
	- Multilib instalado para soporte de librerias 32 bits
- IOU WEB Actualizado (`1.2.2-23`)
	- IOU WEB, Base de datos limpia, logs limpios, y arregla tiempos de espera
- Drivers Graficos de Intel y AMD Instalados
- Rocky Linux Actualizado a `[8.10]` y Kernel a `[4.18.0-553.33.1]`
- Repo EPEL-Release y REMI
- Mejoras para Virtualbox (Guest-Additions), Qemu (qemu-guest-agent) VMWare (Repo Arreglado)
- Soporte para UEFI
- PHP: `php.ini` y `.htaccess` modificado para acceder cargas mas grandes (400MB) y poder recibir imagenes de Cisco IOS
- SSL: Puede resolver url https y ruta original a `libcrypto.so.4` 
- Ip Automatica y mostrando al iniciar sesion (WIP)

## Requerimientos
La instalacion original tiene los siguientes paquetes instalados
> php -v (Version) en 32 bits
```
[root@iou ~]# php -v
PHP 5.3.3 (cli) (built: Nov  1 2019 12:35:32) 
Copyright (c) 1997-2010 The PHP Group
Zend Engine v2.3.0, Copyright (c) 1998-2010 Zend Technologies
```

> php -m (modules) en 32 bits
```
[root@iou ~]# php -m
[PHP Modules]
bz2
calendar
Core
ctype
curl
date
dom
ereg
exif
fileinfo
filter
ftp
gd
gettext
gmp
hash
iconv
json
libxml
openssl
pcntl
pcre
PDO
pdo_sqlite
Phar
pspell
readline
Reflection
session
shmop
SimpleXML
sockets
SPL
sqlite3
standard
tokenizer
wddx
xml
xmlreader
xmlwriter
xsl
zip
zlib
```

> Archivo `/etc/php.ini` en 32 bits
```
[root@iou ~]# php --ini
Configuration File (php.ini) Path: /etc
Loaded Configuration File:         /etc/php.ini
Scan for additional .ini files in: /etc/php.d
Additional .ini files parsed:      /etc/php.d/curl.ini,
/etc/php.d/dom.ini,
/etc/php.d/fileinfo.ini,
/etc/php.d/gd.ini,
/etc/php.d/json.ini,
/etc/php.d/pdo.ini,
/etc/php.d/pdo_sqlite.ini,
/etc/php.d/phar.ini,
/etc/php.d/pspell.ini,
/etc/php.d/sqlite3.ini,
/etc/php.d/wddx.ini,
/etc/php.d/xmlreader.ini,
/etc/php.d/xmlwriter.ini,
/etc/php.d/xsl.ini,
/etc/php.d/zip.ini
```

> Rutas de bin ($PATH) en 32 bits
```
[root@iou ~]# echo $PATH
/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin
```


Esta maquina debe cumplir 2 requisitos para que funcione
- Librerias PHP >= [5.3.3](https://www.php.net/ChangeLog-5.php#5.3.3) (22/Jul/2010), <= [5.3.26](https://www.php.net/ChangeLog-5.php#5.3.26) (14/Ago/2014)
- Librerias Glibc >= 2.27
- Librerias GCC >= 8.2.0
```
gcc --version
ldd --version
php -v
php -m
php --ini
```

Mi enfoque va en restaurar la version basada en CentOS, que esta distribuida con CentOS 6.2 x86, basado en mi trabajo mejorando esa imagen mostrado en [Ghost - Proxylivy - Configura Correctamente IOU WEB](https://ghost.proxylivy.work/configura-correctamente-iou-web/), podrias descargar [OneDrive - IOU-WEB-Creativo (V. CentOS) (32 Bits).ova](https://duoccl0-my.sharepoint.com/:u:/g/personal/ga_zunigam_duocuc_cl/EfCKajx1c5hJvaOdFXB34qsB1x9PtwBYdWYFvu4AufBPaw?e=W4iDX6)

Para poder transformar un .OVA a Qemu debes hacer los siguientes pasos bassado en siguientes guias:
- [Blog - Dannyda - How to use qemu-img to convert between disk images](https://dannyda.com/2020/06/25/how-to-use-qemu-img-command-to-convert-between-vmdk-raw-qcow2-vdi-vhd-vhdx-formats-disk-images-qemu-img-create-snapshot-resize-etc/)
- [Blog - Mario Fischer - Convert VM from OVA to QCOW2](https://blog.mcfisch.com/virtualization/Convert-VM-from-OVA-to-QCOW2-and-run-on-QEMU-KVM/)
- [Blog - Kevindias - Running VMware Images in Qemu](https://www.kevindiaz.dev/blog/running-vmware-images-in-qemu.html)

> Extrae la Imagen OVA con TAR
```
tar xvf original.ova
```

> Eliminar archivos que no necesitamos
```
rm -f IOU-WEB-Creativo.mf IOU-WEB-Creativo.ovf
```

> Convierte el disco a qcow, comprimelo y ve el progreso (puede ser vmdk o otro)
```
qemu-img convert -f vmdk -O qcow2 -c IOU-WEB-Creativo-disk001.vmdk IOU-WEB-32-bits.qcow2 -p
```

> Mueve el archivo .qcow2 a la carpeta de qemu
```
mv IOU-WEB-32-bits.qcow2 /var/lib/libvirt/images/
```

Ahora desde qemu (libvirt-manager)
1. Lo instalaras desde el boton "`+`"
2. Seleccionas "`importar imagen de disco existente`" y luego en "Adelante"
3. Apretas "Explorar" y Seleccionas el disco "`IOU-WEB-32-bits.qcow2`", luego seleccionas el sistema a instalar que es `Generic Linux 2022`, le das en "Adelante"
4. Dejas los valores de Ram y CPU Por defecto (Luego se modifican), le das en "Adelante"
5. Le configuras el nombre `IOU-WEB-32-Bits`, le das el tick en `Personalizar configuracion antes de instalar` y seleccionas "Finalizar"
6. Ve al menu de "CPU", Abre el menu de "Topologia", dale un tick en "Establecer manualmente la topologia de la CPU" (Esta opcion dependera de tu CPU), yo tengo 2 nucleos, 4 hilos; En Socket va "`1`", Centros va "`2`", Hilos va "`2`"
7. Ve al menu "Virtio Disco 1" y cambia "Bus de disco" de "`VirtIO`" -> "`SATA`"
8. Ve al menu de "NIC", cambia el "Modelo de dispositivo" de "`virtio`" -> "`e1000e`" 
9. Inicia la instalacion y deberia funcionar correctamente
10. Para acceder por ssh desde un dispositivo mas moderno, usa el comando `ssh -oHostKeyAlgorithms=+ssh-rsa root@{ip-machine}`
11. Para enviar archivos por rsync desde un dispositivo mas moderno, usa el comando `rsync -druLPO -e "ssh -oHostKeyAlgorithms=+ssh-rsa" root@{ip-machine}:/remote-folder /local-folder`

Debido a que CentOS esta dentro del rango "RedHat Based", queria comprobar el funcionamiento con una version mas actual como Rocky Linux para x86_64

Usaremos
- [Rocky Linux 8.10](https://rockylinux.org) - Minimal ISO
- [Github - dainok/iou-web](https://github.com/dainok/iou-web) (Repositorio Oficial)
- LabHub (Conseguir Imagenes)
	- [LabHub - Vercel - USA - Backed by OneDrive](https://labhub.eu.org/es/)
	- [Mirror 1 - Drive LabHub - Cloudflare - Backed by Google Drive](https://drive.labhub.eu.org/)
	- [Mirror 2 - Legacy LabHub - Cloudflare - Backed by Google Drive](https://legacy.labhub.eu.org/)

## Lecturas
Blog sobre Cisco IOU (Imagenes) y IOU WEB (Interfaz Grafica)
- [FAQ - NetworkHaven - Cisco IOU](https://networkhaven.blogspot.com/2014/02/cisco-iou.html)
- [FAQ - Nabromov - IOS on UNIX IOU](https://nabromov.blogspot.com/2011/01/ios-on-unix-iou.html)
- [FAQ - Thomas Low - Using Cisco IOU](https://thomaslowblog.wordpress.com/2015/08/18/using-cisco-iou/)
- [Blog - My Howtos and Projects - Cisco IOS on UNIX: Installing And Running](https://myhowtosandprojects.blogspot.com/2013/08/installing-and-running-iou-checking_10.html)
- [Blog - Thomas Low - Cisco IOU Install on VMware ESX Server](https://thomaslowblog.wordpress.com/2015/08/18/cisco-iou-installation-steps-on-vmware/)
- [Blog - VMgeeks - Deploying Cisco IOU Web Interface on VMware ESXi](https://vmgeeks.wordpress.com/2012/07/21/deploying-cisco-iou-web-interface-on-vmware-esxi/)
- [Blog - Brezular - Cisco IOU Images Running on Fedora Linux](https://brezular.com/2011/04/30/iou-on-fedora-linux/)
- [Blog - Brezular - Create Cisco Switch using IOUL2 loaded on CentOS QEMU](https://brezular.com/2011/10/23/creating-a-cisco-switch-using-ioul2-loaded-on-centos-qemu-image/)
- [Blog - Brezular - Create Cisco Switch using IOUL2 Loaded on Linux Core](https://brezular.com/2011/11/01/cisco-network-device-based-on-iou-installed-on-core-linux/)
- [Blog - Brezular - Connect Cisco IOU to Real Cisco Gear using iou2net.pl](https://brezular.com/2013/10/09/how-to-connect-iou-to-a-real-cisco-gear-using-iou2net-pl/)
- [Blog - Brezular - Connect Cisco IOU to Real Cisco Gear using int2netio](https://brezular.com/2013/10/15/how-to-connect-iou-to-a-real-cisco-gear-using-iou-live-int2netio/)
- [Blog - Kovacs - Cisco IOU web interface](https://kovacsdaniel.blogspot.com/2015/02/cisco-iou-with-web-interface.html)

Brezular Blogs - Building Linux L3 Switch/Router on X86
- [Part 1 - Introduction](https://brezular.com/2011/09/01/building-linux-l3-switchrouter-on-x86-part1-introduction/)
- [Part 2 - CentOS 6.0](https://brezular.com/2011/09/01/building-linux-l3-switchrouter-on-x86-part2-centos-6-0-installation/)
- [Part 3 - Wireless Access Point Installation and Configuration](https://brezular.com/2011/09/01/building-linux-l3-switchrouter-on-x86-part3-wireless-access-point-installation-and-configuration/)
- [Part 4 - OpenvSwitch Installation and Configuration](https://brezular.com/2011/09/03/building-linux-l3-switchrouter-on-x86-part4-openvswitch-installation-and-configuration/)
- [Part 5 - Connecting Box to the Internet - PPPoE Configuration](https://brezular.com/2011/09/03/building-linux-l3-switchrouter-on-x86-part5-connecting-box-to-the-internet-pppoe-configuration/)
- [Part 6 - Connecting Box to the Internet - NAT and Firewall Configuration](https://brezular.com/2011/09/11/building-linux-l3-switchrouter-on-x8-part6-connecting-box-to-the-internet-nat-and-firewall-configuration/)
- [Part 7 - DDNS and NTP Installation and Configuration](https://brezular.com/2011/09/11/building-linux-l3-switchrouter-on-x8-part7-ddns-and-ntp-installation-and-configuration/) 
- [Part 8 - DNS Cache Server Installation and Configuration](https://brezular.com/2011/09/11/building-linux-l3-switchrouter-on-x86-part8-dns-cache-server-installation-and-configuration/)
- [Part 9 - DHCP and Samba server Installation and Configuration](https://brezular.com/2011/09/11/building-linux-l3-switchrouter-on-x86-part9-dhcp-and-samba-server-installation-and-configuration/)

Brezular Blogs - Installation Solaris 2.6 (SunOS 5.6) on Qemu
- [Part 1 - Qemu Installation on Fedora Linux](https://brezular.com/2012/02/12/qemu-installation-on-fedora-linux/)
	- [Blog - Artyom Tarasenko - Get BIOS Solaris Sparc Work in Qemu](http://tyom.blogspot.com/2009/12/solaris-under-qemu-how-to.html)
- [Part 2 - Solaris Installation](https://brezular.com/2012/02/17/installation-solaris-2-6-sparc-on-qemu-part2-solaris-installation/)
- [Part 3 - iou2net.pl Installation](https://brezular.com/2012/04/08/installation-solaris-sparc-2-6-sunos-5-6-on-qemu-part3-iou2net-pl-installation/)

Blog que no estan relacionados directamente con IOU o IOU WEB
- [Github - come-tardivel/eve-ng-guide: Wiki Cisco IOL IOU](https://github.com/come-tardivel/eve-ng-guide/wiki/CISCO-IOL%E2%80%90IOU)
- [Github - Nanue1/web-iou-docker Dockerfile DEBIAN VERSION](https://github.com/Nanue1/web-iou-docker/blob/master/Dockerfile)

# Instalacion
## Rocky Linux
Descarga la version "Minimal ISO" de [Rocky Linux 8.10 AMD/Intel (x86_64)](https://rockylinux.org/download), Instala en tu hipervisualizador preferido (Usare [Qemu](https://www.qemu.org/))

Requisitos de la maquina (Casi todo en Default):
- Chipset: UEFI Q35
- CPU: 2 Cores x86_64
- RAM: 4096MB
- Network: Driver -> Virtio

Cuando aparesca una GUI, selecciona el lugar e Idioma, yo elijo "Español" y "Español (Chile)", luego apreto "Siguiente"

Luego aparece el menu "Resumen de la Instalacion" donde entraremos a "Contraseña de root" y escribimos `cisco` y le damos dos veces a "`Hecho`"

Luego vamos a "Creacion de usuario" con la siguiente informacion
- Nombre Completo: `duoc`
- Nombre de usuario: `duoc`
- Hacer de este usuario un administrador: `tick`
- Contraseña: `cisco`
- Confirmar contraseña: `cisco`
- Nota: Recuerda darle 2 veces al boton "Hecho"

Ahora vamos al menu "Destino de la instalacion" y le damos en "Hecho" (Es algo automatico)

Seleccionamos el menu "Red y nombre del equipo" y encendemos desde el switch para que se pueda configurar una ip y precionamos "Hecho"

Ahora le podemos dar en "Comenzar la Instalacion"


Cuando termine la instalacion de Rocky, reinicia la maquina e inicia sesion con el user `root:cisco`

Cuando accedas de forma remota, exporta este env para que puedas escribir correctamente
```
export TERM=xterm
```

## Repos y Paquetes
> Actualiza la lista de paquetes disponibles
```
dnf update
```

> Instala Repo EPEL-Release
```
dnf install epel-release
```

> Habilita CRB
```
/usr/bin/crb enable
/usr/bin/crb status
```

> Instala Repo REMI
```
dnf install https://rpms.remirepo.net/enterprise/remi-release-8.rpm
```

> Actualiza los repositorios
```
dnf repolist
dnf makecache
dnf update
```

> Lista los grupos de paquetes
> Nota: Pueden no estar disponibles los grupos, como mi instalacion es en español, los paquetes se deben instalar en español
```
dnf grouplist
```

> Instalar Grupos de Paquetes
```
dnf groupinstall "Compatibilidad con legado de UNIX" "Herramientas del sistema" "Herramientas de desarrollo"
```

> Instala herramientas Multilib
```
dnf install multilib-rpm-config dnf-utils
```

> Instalar Paquetes compatibilidad 32 bits
```
dnf install glibc.i686 libstdc++.i686 zlib.i686 openssl-libs.i686 libpcap.i686 libX11.i686 libXext.i686 glibc-static libstdc++-static openssl-devel.i686 libnsl.i686 libnsl2.i686
```

> Instalar Paquetes Programacion
```
dnf install rsync openssl-devel tar git gcc cmake autoconf wget gzip libxml2-devel sqlite-devel libcurl-devel libjpeg-devel libpng-devel freetype-devel dialog open-vm-tools net-tools psmisc dos2unix gmp-devel libmpc-devel mpfr-devel libnsl texinfo telnet
```

> Instalar Paquetes Sueltos
```
dnf install libvirt virt-viewer qemu-guest-agent telnet-server xinetd htop tmux screen byobu man httpd-devel pcre-devel dkms xclip xsel libcap-devel dosfstools rsyslog syslog-ng tftp-server lsof perl-IO-Tty perl-Time-HiRes perl-Authen-PAM terminus-fonts-* perl-LDAP
```

> Descarga Repo Fish
> - Recuerda moverte a la carpeta `/etc/yum.repos.d`
```
wget https://download.opensuse.org/repositories/shells:fish:release:3/CentOS_8/shells:fish:release:3.repo
```

> Instalar Micro, Fastfetch y Fish (Opcional)
```
dnf install micro fastfetch fish
```

> Configura Micro
> Tienes que abrir `micro` Apreta `Ctrl+e`
```
set softwrap true
```

> Instalar Nerd Fonts (Opcional)
```
mkdir git && cd git
git clone --filter=blob:none --sparse https://github.com/ryanoasis/nerd-fonts.git
cd nerd-fonts
git sparse-checkout add patched-fonts/Hack
git sparse-checkout add patched-fonts/JetBrainsMono
./install.sh Hack
./install.sh JetBrainsMono
```

> Instala todos los paquetes de apache
```
dnf install httpd httpd-devel httpd-tools httpd-filesystem mod_http2 libdb-devel openldap-devel
```
### Compilar Openssl 1.0.2u
> Crear las carpetas
```
mkdir /opt/source
```

> Muevete a la carpeta
```
cd /opt/source
```

> Exporta los siguientes env
```
export CFLAGS="-fPIC -m64"
export CXXFLAGS="-fPIC -m64"
```

> Compila desde la version [1.0.2u](https://github.com/openssl/openssl/releases/tag/OpenSSL_1_0_2u)
```
wget https://github.com/openssl/openssl/releases/download/OpenSSL_1_0_2u/openssl-1.0.2u.tar.gz
tar -zxvf openssl-1.0.2u.tar.gz
cd openssl-1.0.2u
./config no-shared --prefix=/opt/openssl-1.0.2u --openssldir=/opt/openssl-1.0.2u -fPIC
make -j$(nproc)
sudo make install
```

### Compilar PHP 5.3.26
Gracias a [Blog - BlahBlah - Como Compilar PHP 5.3.x](https://blahblah.mx/como-compilar-php-5-3-xx-en-ubuntu-16-04/) para inspirarme a compilar php

> Instalar paquetes necesarios para compilar
> Nota: El paquete `libxslt-devel-1.1.32-6.el8.i686` entra en conflicto con su version de 64 bits, prefiero dejar solamente el de 64 bits en ese caso
```
dnf install libjpeg-turbo-utils-1.5.3-12.el8.x86_64 libjpeg-turbo-devel-1.5.3-12.el8.i686 libpng-devel-2:1.6.34-5.el8.i686 libXpm-devel libXpm-devel-3.5.12-11.el8.i686 libmcrypt-2.5.8-26.el8.x86_64 libmcrypt-devel-2.5.8-26.el8.x86_64 mysql mysql-devel-8.0.36-1.module+el8.10.0+1676+9b4b6e24.x86_64 aspell aspell-devel-12:0.60.6.1-22.el8.i686 aspell-devel-12:0.60.6.1-22.el8.x86_64 readline-devel-7.0-10.el8.x86_64 readline-devel-7.0-10.el8.i686 libsqlite3x-devel-20071018-26.el8.x86_64 libxslt-devel-1.1.32-6.el8.x86_64.rpm libevent-devel-2.1.8-5.el8.x86_64 libevent-devel-2.1.8-5.el8.i686 freetype-devel-2.9.1-9.el8.i686 libtool-ltdl-devel libtool-ltdl-devel-2.4.6-25.el8.i686 oniguruma-devel-6.8.2-3.el8.x86_64
```

> Descarga el codigo fuente de PHP (Prefiero 5.3.29 ya que es la version mas actual del branch)
> - https://www.php.net/releases/index.php#5.3.29
> - https://www.php.net/releases/index.php#5.3.3
```
wget https://www.php.net/distributions/php-5.3.29.tar.gz
```

> Descomprime y muevete a ese archivo
```
tar -zxvf php-5.3.29.tar.gz
cd php-5.3.29
```

> Modifica archivos los siguientes archivos buscando `my_bool` y cambialo por `bool`
> - `/opt/source/php-5.3.29/ext/pdo_mysql/php_pdo_mysql_int.h`
> - `/opt/source/php-5.3.29/ext/pdo_mysql/mysql_statement.c`
> - `/opt/source/php-5.3.29/ext/pdo_mysql/mysql_driver.c`

```
// Original:
my_bool *in_null;
my_bool *out_null;

// Cambiado a:
bool *in_null;
bool *out_null;
```

> Crear la carpeta php.d
```
mkdir /etc/php.d
```

> Configurar Configuracion de PHP
> NOTA: Recuerda que debe estar compilado con exito la version 1.0.2u de openssl y estar en la ruta correcta
> NOTA2: PHP estara instalado en 
> NOTA3: Posiblemente sea buena idea tambien usar: --with-libdir=lib64
```
./configure --with-config-file-path=/etc --with-config-file-scan-dir=/etc/php.d --with-zlib=/usr --enable-fpm --enable-inline-optimization --enable-gd-native-ttf --enable-mbregex --with-openssl=/opt/openssl-1.0.2u --with-pcre-regex --with-libxml-dir=/usr --with-bz2 --with-curl --with-gd --with-jpeg-dir=/usr --with-png-dir=/usr --with-zlib-dir=/usr --with-xpm-dir=/usr --with-freetype-dir=/usr --enable-calendar --enable-exif --enable-ftp --with-gettext --with-gmp --with-iconv --enable-mbstring --with-mcrypt --enable-pcntl --enable-sockets --with-pspell --with-readline --enable-shmop --enable-simplexml --enable-soap --enable-sysvsem --enable-sysvshm --enable-wddx --with-xsl --enable-zip --with-pear --with-pdo-mysql --with-pdo-sqlite --with-sqlite3 --with-mhash --enable-dom --enable-xmlreader --enable-xmlwriter --enable-json 
```

> Compila
> NOTA: Puede usar `make test` pero se demora un monton y falla debido a que es una version vieja, segun revise, hace 3000 test correctamente, luego me dio sueño seguir revisando
```
make -j$(nproc)
```

> Instala php compilado
```
sudo make install
```

> Las rutas instaladas son las siguientes
```
[root@localhost php-5.3.29]# sudo make install
Installing PHP SAPI module:       fpm
Installing PHP CLI binary:        /usr/local/bin/
Installing PHP CLI man page:      /usr/local/man/man1/
Installing PHP FPM binary:        /usr/local/sbin/
Installing PHP FPM config:        /usr/local/etc/
Installing PHP FPM man page:      /usr/local/man/man8/
Installing PHP FPM status page:      /usr/local/share/php/fpm/
Installing build environment:     /usr/local/lib/php/build/
Installing header files:          /usr/local/include/php/
Installing helper programs:       /usr/local/bin/
  program: phpize
  program: php-config
Installing man pages:             /usr/local/man/man1/
  page: phpize.1
  page: php-config.1
Installing PEAR environment:      /usr/local/lib/php/
[PEAR] Archive_Tar    - already installed: 1.3.12
[PEAR] Console_Getopt - already installed: 1.3.1
[PEAR] Structures_Graph- already installed: 1.0.4
[PEAR] XML_Util       - already installed: 1.2.3
[PEAR] PEAR           - already installed: 1.9.5
Wrote PEAR system config file at: /usr/local/etc/pear.conf
You may want to add: /usr/local/lib/php to your php.ini include_path
/opt/source/php-5.3.29/build/shtool install -c ext/phar/phar.phar /usr/local/bin
ln -s -f /usr/local/bin/phar.phar /usr/local/bin/phar
Installing PDO headers:          /usr/local/include/php/ext/pdo/
```

> Verificar PHP y modulos
```
php -v
/usr/local/php/bin/php -m
```

> Verificar si PHP usa la version correcta
> Deberias ver algo como:
> OpenSSL Library Version => OpenSSL 1.0.2u
> OpenSSL Header Version => OpenSSL 1.0.2u
```
php -i | grep OpenSSL
```

## Configura el Sistema
> Crea Carpetas para IOU y OVF
```
mkdir /opt/ovf
mkdir /opt/iou
mkdir -p /opt/iou/bin/lib
```

> Desde el Nuevo Rocky, mueve OVF del viejo CentOS a la carpeta
```
rsync -druLPO root@{old-ip-server}:/etc/init.d/ovfconfig /etc/init.d/ovfconfig
```

> Mueve de CentOS a Rocky, las carpetas de ovf
```
rsync -druLPO root@{old-ip-server}:/opt/ovf/ /opt/ovf/
```

> Mueve el `libcrypto.so.4` original desde [Drive LabHub](https://drive.labhub.eu.org/1:/addons/iol/lib/) en tu maquina Host y envialo a `/usr/lib/` dentro de Rocky Linux
```
rsync -druLPO root@{ip-server}:~/libcrypto.so.4 /opt/iou/bin/lib/

ln -s /opt/iou/bin/lib/libcrypto.so.4 /usr/lib/libcrypto.so.4
```

> Envia el archivo `iou.conf` desde CentOS y dejalo en `/etc/httpd/conf.d/`
```
rsync -druLPO root@{ip-old-server}:/etc/httpd/conf.d/iou.conf /etc/httpd/conf.d/
```

> Envia los archivos de configuracion de httpd
```
rsync -druLPO root@{old-ip-server}:/etc/httpd/ /etc/httpd/
```

> Enviar archivos principales de IOU WEB ubicados en `/opt/iou` desde CentOS a Rocky
```
rsync -druLPO root@{old-ip-server}:/opt/iou/ /opt/iou/
```

> Enviar archivos de php.ini
```
rsync -druLPO root@{old-ip-server}:/etc/php.ini /usr/local/lib/php.ini
```

> Enviar archivos de la carpeta php.d
```
rsync -druLPO root@{old-ip-server}:/etc/php.d /etc/
```

> Enviar archivos de la carpeta php.d
```
rsync -druLPO root@{old-ip-server}:/etc/php-fpm.conf /etc/php-fpm.conf
```

> Enviar archivos de la carpeta php.d
```
rsync -druLPO root@{old-ip-server}:/etc/php-fpm.d/ /etc/
```

> Ejecutar OVF
> - Nota: Las respuestas estan en el siguiente recuadro
> - Nota2: Esto Reiniciara la maquina
```
/etc/init.d/ovfconfig force-config
```

> - Contraseña: cisco
> - Repetir Contraseña: cisco
> - shot-hostname (default) = iou
> - dns-domain-name (default) = example.com
> - Los demas valores por defecto, solo apretar Enter
```
cisco
cisco
enter
enter
enter
enter
enter
```

> Modificar `/etc/hosts`
```
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
127.0.0.2   iou.example.com iou
127.0.0.127 xml.cisco.com
127.0.0.3   www.routereflector.com routereflector.com public.routereflector.com ww25.public.routereflector.com
```

> Modificar `/etc/hostname`
> NOTA: OVF Modifica esto
```
iou.example.com
```

> Limpiar IPTABLES
```
iptables -L
iptables -F
iptables -X
iptables -L
service iptables status
```

> Edita el archivo de sudoers ubicado en `/etc/sudoers` para agregar las lineas
```
user    ALL=(ALL:ALL)   ALL
apache  ALL=(ALL:ALL)   ALL
apache  ALL=(ALL) NOPASSWD: ALL
```

> Mejora el acceso de Apache (Inexacto)
```
usermod -aG root apache mysql
usermod -aG apache root 
```

> Modificar reglas de Firewall
> Nota: Se porta mal asi que un `systemctl disable --now firewalld` le hara reflexionar
```
firewall-cmd --add-service=https --permanent
firewall-cmd --add-service=https --permanent
firewall-cmd --reload
```

> Desabilita Selinux
> Nota: Modifica `etc/selinux/config`: `SELINUX` -> `disabled` y `SELINUXTYPE` -> `minimum`
```
setenforce 0
```

## IOU WEB CentOS a Rocky

NOTA: AUN ME FALTA HACER LO DE LA CHARLA CON CHATGPT PARA TENER HTTPD CORRECTAMENTE

> En `etc/httpd/conf.modules.d/00-base.conf` comenta `access_compat_module`
```
# LoadModule access_compat_module modules/mod_access_compat.so
```

> Modifica el archivo `/etc/httpd/conf.d/iou.conf` en Rocky, modifica `Indexes` -> `+Indexes` en la linea 6, para que quede asi
```
Options +Indexes -FollowSymLinks
```

> Modifica el archivo `/etc/httpd/conf.d/iou.conf` en Rocky, agrega `Require all granted` dentro `<Directory /opt/iou/data>` abajo de la linea 6, para que quede asi
```
Require all granted
```

> Modifica el archivo `/etc/httpd/conf/httpd.conf` en Rocky, modifica `DocumentRoot "/var/www/html"` -> `DocumentRoot "/opt/iou/html"` aproximandamente en la linea 122 para que quede asi
```
DocumentRoot "/opt/iou/html"
```

> Modifica el archivo `/etc/httpd/conf/httpd.conf` en Rocky, modifica la parte de `<Directory>` aproximadamente en la linea 127
```
<Directory "/opt/iou/html">
	AllowOverride All
    Options Indexes FollowSymLinks
    Require all granted
</Directory>
```

> Modifica el archivo `/etc/httpd/conf/httpd.conf` en Rocky, modifica la parte de `<Directory>` aproximadamente en la linea 135
```
<Directory "/opt/iou/html">
	Options Indexes FollowSymLinks
	AllowOverride All
	Require all granted
```

> CREO QUE EL PROBLEMA CON CGI-HANDLER
> https://localhorse.net/article/como-habilitar-y-configurar-mod_cgi-en-apache-para-ejecutar-scripts-cgi

> Arreglar Errores con Logs de apache
```
touch /opt/iou/data/Logs/error.txt
chown apache:apache /opt/iou/data/Logs/error.txt
ls -lZ /opt/iou/data/Logs/error.txt
sudo chcon -Rv system_u:object_r:httpd_log_t:s0 /opt/iou/data/Logs
chmod 755 -R /opt/iou
chown root:apache -R /opt/iou
chmod 777 /opt/iou/data/
chmod 7777 -R /opt/iou
```

> Crea el archivo `keygen.py` en `/opt/iou/scripts/` con el siguiente texto
> A veces cambia en la linea "`md5input = iouPad1 + iouPad2 + struct.pack('!Q', ioukey)[4:] + iouPad1`" el valor `Q` a `i` o `L`, cambia el resultado, pero no se cual funcionara, el mas comun en `i`
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

> Version Actualizada
```
#! /usr/bin/python3
import os
import socket
import hashlib
import struct
import sys
from pathlib import Path

hostid=os.popen("hostid").read().strip()
hostname = socket.gethostname()
ioukey=int(hostid,16)
for x in hostname:
 ioukey = ioukey + ord(x)

maxInt = 2 ** (struct.calcsize("i") * 8 - 1) - 1
if(ioukey > maxInt):
  print("\033[31m Your hostid " + hostid + " is too max. You can use sethostid command change to a smaller one.(less than 4 byte int) \033[0m")
  sys.exit(1)

print("This is your machine info hostid=" + hostid +", hostname="+ hostname + ", ioukey=" + hex(ioukey)[2:])

iouPad1 = b'\x4B\x58\x21\x81\x56\x7B\x0D\xF3\x21\x43\x9B\x7E\xAC\x1D\xE6\x8A'
iouPad2 = b'\x80' + 39*b'\0'
md5input=iouPad1 + iouPad2 + struct.pack('!i', ioukey) + iouPad1
iouLicense=hashlib.md5(md5input).hexdigest()[:16]

home = str(Path.home())
rcFile = home + "/.iourc"
iourc = "[license]\n" + hostname + " = " + iouLicense + ";\n"

fd = None
try:
    fd = open(rcFile, "wt")
    fd.write(iourc)

    print("\033[32m License info: \033[0m")
    print("\033[32m " + iourc + " \033[0m")
    print("\033[32m Writed to : " + rcFile +  " \033[0m")
except:
    print("\033[31m Write licence info fail, You can set it by your self \033[0m")
    print("\033[31m " + iourc + " \033[0m")

finally:
    if fd != None:
      fd.close()
```


> La licencia generada en el comando anterior se copia a `/opt/iou/bin/iourc`
> - Puede ser el primero o el segundo, sin este valor, las maquina nunca encenderan
```
[license]
iou.example.com = 145ef75ad4ea0ebd;

OR

[license]
iou.example.com = d66475be295f2100;

OR

[license]
localhost.localdomain = 717c564d5236d62d;
```

> Modifica `.htaccess` en `/opt/iou/` y `/opt/iou/html/`
```
# PHP.ini
post_max_size = 400M
upload_max_filesize = 400M
max_input_time = 180
max_input_vars = 5000
default_socket_timeout = 3

# The following are set under manage.php
max_execution_time = 180
memory_limit = 1024M

# supress php errors
display_startup_errors = off
display_errors = off
html_errors = off

# enable PHP error logging
log_errors = on
error_log  = /opt/iou/data/Logs/php_errors.txt
```

> Modifica valores php.ini en `/etc/php.ini` o `/usr/local/lib/php.ini`
> - Nota: Los valores `post_max_size` y `upload_max_filesize` controlan el limite para recibir archivos .bin
```
# post_max_size = 400M
upload_max_filesize = 400M
max_input_time = 180
# max_input_vars = 5000
default_socket_timeout = 60
max_execution_time = 180
memory_limit = 1024M
html_errors = off
error_log = /opt/iou/data/Logs/php_errors.txt
```

> Tambien agrega el siguiente valor
```
extension_dir = "./php.d/"
```

> Verifica configuracion apache correcta
```
apachectl configtest
```

> Recarga los cambios hechos en el sistema
```
systemctl daemon-reload
```

> Inicia HTTPD y php56
```
systemctl start httpd
systemctl enable httpd
systemctl enable --now php-fpm
systemctl status php-fpm
```

> Reinicia todos los servicios cuando realices un cambio
```
systemctl restart httpd php-fpm
```

## Probar Imagenes

> Revisa las librerias en busqueda del libcrypto.so.4
```
ls /usr/lib/libcrypto.so.4
```

> Crea el archivo NETMAP
```
touch /opt/iou/bin/NETMAP
```

> Probar imagenes que estan en `/opt/iou/bin`???
> - Nota: El puerto usado es el "2222" y el ID de app es el 200
```
./wrapper-linux -m ./i86bi_linux-adventerprisek9-ms.152-4.M1 -p 2222 -- -e 1 -s 1 200
```

> Conectar al puerto 2222
```
telnet localhost 2222
```

> Dejar de ejecutar imagenes de fondo
```
ps -aux | grep wrapper-linux | grep 200 | kill `echo $(cut -d " " -f2)`
```

# Optimizar
Configura Idioma
> Edita `/etc/sysconfig/i18n` y modifica `LANG="en_US.UTF-8"` por:
```
LANG="es_CL.UTF-8"
```

Instalar VirtualBox Additions
> Agrega el Disco correspondiente a la version, si falla, te enviara un mensaje por `dmesg`
> NOTA: Estos 2 comandos "`/sbin/rcvboxadd quicksetup all`" y "`/sbin/rcvboxadd setup`", son para hacer el mismo proceso que se hace al instalar, es innecesario si ya funciono 
```
mkdir /media/V
mount /dev/cdrom /media/V
sh /media/V/VBoxLinuxAdditions.run
dmesg
cat /var/log/vboxadd-setup.log
```

Revisa las flags de CPU (Emulacion Completa)
```
grep -E 'vmx|svm' /proc/cpuinfo
lscpu
grep flags /proc/cpuinfo
```

Optimiza VM con sysctl
> Modifica el archivo `/etc/sysctl.conf` con el siguiente contenido, luego lo aplicas con `sysctl -p`
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
dnf install -y qemu-guest-agent
systemctl enable qemu-guest-agent
systemctl start qemu-guest-agent
```

# Limpieza
```
dnf clean all
rm -rf /var/cache/yum/
rm -rf /tmp/*
rm -rf /var/tmp/*
find /var/log -type f -exec truncate -s 0 {} \;
df -BM
dd if=/dev/zero of=/zerofile bs=1M
rm -f /zerofile
dracut -f -v
grub2-mkconfig -o /boot/grub2/grub.cfg
grub2-mkconfig -o /boot/efi/EFI/centos/grub.cfg
unset HISTFILE
history -c
```

# TSHOOT
## Parchear Imagenes .bin
Las imagenes que he probado nunca han necesitado parchado, posiblemente tengas algo mal configurado, pero aqui esta

> Compila bbe para parchar binarios
```
mkdir /root/git
cd /root/git
git clone https://github.com/hdorio/bbe.git
cd bbe
./configure
make
make install
```

> ALTERNATIVO: Instalar bbe forzadamente
```
wget http://sourceforge.net/projects/bbe-/files/bbe/0.1.8/bbe-0.1.8-2.i386.rpm

sudo rpm -ihv ./bbe-0.1.8-2.i386.rpm --force
```

> Ir a `/opt/iou/bin`
```
cd /opt/iou/bin
```

> Crea un archivo de licencia vacio en `/opt/iou/bin`
```
echo -e "[license]\n$(uname -n) = 0000000000000000" > iourc
```

> Crea un archivo NETMAP
```
touch ./NETMAP
```

> Parchear Imagenes L3
```
for F in i86bi_linux-*;do bbe -b "/xfcxffx83xc4x0cx85xc0x75x14x8b/:10" -e  "r 7 x90x90" -o $F.x $F;mv $F.x $F;done;chmod +x ./i86bi_linux-*
```

> Parchear Imagenes L2
```
for F in i86bi_linuxl2*;do bbe -b "/xa1xffx83xc4x0cx85xc0x75x17x8b/:10" -e "r 7 x74" -o $F.x $F;mv $F.x $F;done;chmod +x ./i86bi_linuxl2*
```

## Errores de tiempo
IOU WEB esta hecho para funcionar en Europa/Roma, cambiar este valor podria hacer que los relojes del VM fallen, esto es solo una hipotesis, pero lo podria causar la siguiente configuracion

> Configurar reloj Santiago (Podria generar errores)
```
ln -sf /usr/share/zoneinfo/America/Santiago /etc/localtime
```


# Extra
## Script IP start (NO PROBADO)
> Crear Script Actualizar issue en la ruta `/usr/local/bin/update_issue.sh`
```
#!/bin/bash

# Archivo temporal para almacenar el banner original
ORIGINAL_ISSUE="/etc/issue.orig"

# Si el archivo original no existe, crearlo
if [ ! -f "$ORIGINAL_ISSUE" ]; then
    cp /etc/issue "$ORIGINAL_ISSUE"
fi

# Obtener la dirección IP de la interfaz activa
IP_ADDRESS=$(ip -4 -o addr show up primary scope global | awk '{print $4}' | cut -d/ -f1)

# Si no se encontró una dirección IP, usar "IP no disponible"
if [ -z "$IP_ADDRESS" ]; then
    IP_ADDRESS="IP no disponible"
fi

# Actualizar /etc/issue con el banner original y agregar la dirección IP
cat "$ORIGINAL_ISSUE" > /etc/issue
echo "Use http://$IP_ADDRESS/" >> /etc/issue
```

> Crear Script Apagado en `/usr/local/bin/restore_issue.sh`
```
#!/bin/bash

# Archivo temporal que almacena el banner original
ORIGINAL_ISSUE="/etc/issue.orig"

# Restaurar el banner original si existe
if [ -f "$ORIGINAL_ISSUE" ]; then
    cp "$ORIGINAL_ISSUE" /etc/issue
fi
```

> Hacer script ejecutable
```
sudo chmod +x /usr/local/bin/update_issue.sh
sudo chmod +x /usr/local/bin/restore_issue.sh
```

> Crear el Actualizador en `/etc/systemd/system/update_issue.service`
```
[Unit]
Description=Actualizar /etc/issue con la dirección IP
After=network-online.target

[Service]
Type=oneshot
ExecStart=/usr/local/bin/update_issue.sh

[Install]
WantedBy=multi-user.target
```

> Crear el apagado en `/etc/systemd/system/restore_issue.service`
```
[Unit]
Description=Restaurar /etc/issue al apagar
DefaultDependencies=no
Before=shutdown.target

[Service]
Type=oneshot
ExecStart=/usr/local/bin/restore_issue.sh

[Install]
WantedBy=shutdown.target
```

> Recargar y Habilitar el Servicio
```
sudo systemctl daemon-reload
sudo systemctl enable update_issue.service
sudo systemctl enable restore_issue.service
```

## Convertir imagen QCOW2
> A VDI para que sea compatible en Virtualbox
```
qemu-img convert -f qcow2 centos.qcow2 -O vdi centos-iouweb.vdi
```

## Actualizar Codigo Base (Pospuesto por tiempo indefinidamente)
Este seria el metodo definitivo para IOU WEB y su funcionamiento en plataformas modernas, algunas opciones son:
- https://www.php.net/eol.php
- https://github.com/monque/PHP-Migration
- https://github.com/rectorphp/rector
- https://github.com/phpstan/phpstan

## Paquetes de 32 bits
> DUMP Paquetes de 32 bits usando `rpm -qa --qf "%{NAME} "`
```
dnf install hicolor-icon-theme libSM jasper-libs systemtap-client gpg-pubkey libX11 libXi libXcursor rootfiles libXcomposite nano gdb libcap gettext kernel-headers popt libthai libselinux libgcj libsepol-devel libsepol compat-db krb5-libs sed libtool libss gcc-c++ libxml2 compat-glibc e2fsprogs libidn flex dhcp-common rcs yum-plugin-fastestmirror checkpolicy compat-readline5 ncurses-libs bison chkconfig compat-expat1 glib2 gpg-pubkey cyrus-sasl-lib libutempter dbus xz-libs device-mapper apr dmidecode cpio python-argparse perl-version dash lvm2-libs tcp_wrappers-libs ConsoleKit-libs p11-kit libaio gmp nfs-utils-lib libnih ebtables tar bridge-utils mysql-libs mingetty pm-utils coreutils-libs cyrus-sasl-md5 shadow-utils elfutils-libelf-devel hwdata fipscheck-lib db4-devel nss-sysinit libarchive rpm-libs dkms php-cli man ethtool telnet-server iptables crontabs xsel util-linux-ng openssh rsyslog cronie dracut-kernel ca-certificates openssh-server iptables-ipv6 php-pdo audit gpg-pubkey gpg-pubkey vmware-tools-core vmware-tools-plugins-hgfsServer vmware-tools-plugins-vmbackup libxslt autoconf freetype libproxy-python avahi-libs atk gnutls perl-IO-Compress-Zlib apr-util-ldap libstdc++-devel aspell libX11-common dos2unix redhat-rpm-config basesystem cups-libs kbd-misc elfutils systemtap-runtime libxcb libXfixes libXtst libXft neon libattr kernel-firmware cvs git keyutils-libs libart_lgpl bzip2-libs libselinux-devel ppl gamin yum gcc readline e2fsprogs-libs gcc-gfortran openssl-devel compat-xcb-util libselinux-utils ctags centos-release compat-libstdc++-33 glibc-common compat-libf2c-34 bash indent db4 diffstat libusb shared-mime-info ntp libsemanage htop sqlite device-mapper-libs libblkid libtirpc diffutils gdbm libcgroup perl-Module-Pluggable device-mapper-event libssh2 parted cracklib-dicts libstdc++ iscsi-initiator-utils p11-kit-trust keyutils upstart dnsmasq db4-utils ConsoleKit procps hdparm coreutils radvd module-init-tools apr-devel libpciaccess db4-cxx nss-tools apr-util-devel rpm httpd-devel libuser php-gd plymouth-core-libs xinetd iproute xclip libffi initscripts iou-web yum-metadata-parser selinux-policy cyrus-sasl newt selinux-policy-targeted efibootmgr openssh-clients gnupg2 sudo grubby attr dialog vmware-tools-foundation vmware-tools-services dbus-glib vmware-tools-plugins-deployPkg vmware-tools-vgauth gpg-pubkey automake fontconfig libproxy libpng unzip patch apr-util perl-Compress-Zlib perl-HTML-Tagset compat-glibc-headers filesystem perl-XML-Parser cpp libgfortran libXau libXext gdk-pixbuf2 libXinerama pakchois cairo kernel info perl-Git libcom_err gtk2 keyutils-libs-devel compat-db42 zlib-devel cloog-ppl libkadm5 systemtap make subversion libxml2-python lua cscope libgcc doxygen ncurses-base compat-libtermcap nss-softokn-freebl patchutils nspr pth compat-libcap1 audit-libs compat-opensm-libs gawk tmux dbus-libs libgssglue expat libnl nss-softokn yajl perl-Pod-Escapes which rpcbind perl-Pod-Simple groff cryptsetup-luks sysvinit-tools cracklib gnutls-utils pcre redhat-logos lvm2 less augeas-libs file polkit pinentry nc psmisc hal-info pam libvirt python gpgme libXpm gzip ustr openldap-devel libcurl byobu openldap cmake logrotate php-xml net-tools libXmu plymouth libcap-devel udev python-iniparse dracut postfix slang newt-python b43-openfwwf authconfig ntpdate passwd rpm-python acl libpcap vmware-tools-libraries-nox vmware-tools-plugins-vix vmware-tools-plugins-guestInfo vmware-tools-esx-nox vmware-tools-repo-RHEL6 perl-IO-Compress-Base libjpeg-turbo elfutils-libs libtiff libICE perl-Compress-Raw-Zlib mailcap glibc-headers perl-HTML-Parser compat-libgcc-296 xz-lzma-compat gettext-libs alsa-lib libXrender libXrandr libXdamage compat-db43 pixman zlib kernel-devel rsync pkgconfig pango libcom_err-devel gettext-devel python-urlgrabber intltool krb5-devel systemtap-devel openssl rpm-build yum-utils swig setup compat-openldap libgpg-error tzdata compat-libgfortran-41 bzip2 glibc byacc nss-util openssl098e libacl compat-libstdc++-296 file-libs libevent MAKEDEV elfutils-libelf libudev libuuid device-mapper-event-libs findutils numactl m4 perl-libs numad perl cryptsetup-luks-libs libtasn1 hal-libs grep device-mapper-persistent-data httpd-tools nfs-utils vim-minimal eggdbus libcap-ng binutils netcf-libs ncurses hal plymouth-scripts libvirt-client python-libs expat-devel fipscheck nss cyrus-sasl-devel curl screen php-common pcre-devel kbd libgcrypt dvtm libdrm libXt iputils telnet policycoreutils pygpgme httpd cronie-anacron pciutils-libs grub php dhclient php-pspell python-pycurl wget epel-release vmware-tools-guestlib vmware-tools-plugins-timeSync libedit vmware-tools-plugins-powerOps perl-URI perl-Error libproxy-bin mpfr xz zip libgomp glibc-devel perl-libwww-perl
```

> DUMP de paquetes que no se encontraron
```
gpg-pubkey libgcj compat-db compat-glibc yum-plugin-fastestmirror compat-readline5 compat-expat1 python-argparse ConsoleKit-libs nfs-utils-lib libnih mingetty pm-utils coreutils-libs db4-devel iptables-ipv6 vmware-tools-core vmware-tools-plugins-hgfsServer vmware-tools-plugins-vmbackup libproxy-python compat-xcb-util compat-libstdc++-33 compat-libf2c-34 db4 ntp upstart db4-utils ConsoleKit db4-cxx iou-web yum-metadata-parser vmware-tools-foundation vmware-tools-services vmware-tools-plugins-deployPkg vmware-tools-vgauth compat-glibc-headers compat-db42 cloog-ppl libxml2-python compat-libtermcap pth compat-libcap1 compat-opensm-libs libgssglue libnl sysvinit-tools hal-info python python-iniparse newt-python b43-openfwwf ntpdate rpm-python vmware-tools-libraries-nox vmware-tools-plugins-vix vmware-tools-plugins-guestInfo vmware-tools-esx-nox vmware-tools-repo-RHEL6 compat-libgcc-296 compat-db43 python-urlgrabber compat-openldap compat-libgfortran-41 openssl098e compat-libstdc++-296 MAKEDEV libudev hal-libs eggdbus hal python-libs dvtm pygpgme grub php-pspell python-pycurl vmware-tools-guestlib vmware-tools-plugins-timeSync vmware-tools-plugins-powerOps
```

## Paquetes para instalar con remi (Deprected)
> No se usa debido a que no hay forma de instalar php5.3 desde los repositorios
```
sudo dnf --enablerepo=remi install php php-bz2 php-calendar php-ctype php-curl php-dom php-exif php-fileinfo php-ftp php-gd php-gettext php-gmp php-iconv php-json php-libxml php-openssl php-pcntl php-pcre php-pdo php-pdo_sqlite php-phar php-pspell php-readline php-shmop php-simplexml php-sockets php-sqlite3 php-tokenizer php-wddx php-xml php-xmlreader php-xmlwriter php-xsl php-zip php-zlib
```

> Paquetes Instalados de php 5.6
> - No se encuentra `php-pecl-mysql` y `php-curl (php-common)` y `php-sqlite3 (pdo)` y `php-pear`
> - No se ve necesario el remi, pero cualquier cosa
> - dnf module reset php && dnf module enable php:{remi-version}
```
dnf install php56 php56-php-cli php56-php-common php56-php-fpm php56-php-gd php56-php-mysqlnd php56-php-mbstring php56-php-xml php56-php-pdo php56-php-zephir-parser php56-php-pspell php56-php-pecl-zip php56-libzip php56-php php56-php-mcrypt php56-php-lz4 php56-syspaths php56-php-pecl-binpack php56-php-pecl-chdb php56-php-pecl-http-devel php56-php-pecl-jsonc php56-php-process php56-xhprof php56-php-xmlrpc php56-php-pear
```

> Posible arreglo a PHP56
> Debido a que se descargan las version php56, debes hacer un link a la version normal para que funcionen correctamente
> Igual hay que revisar php-info.php para poder ver que librerias y donde las tiene agregadas para funcionar tan bien
```
ln -s /bin/php56 /bin/php
ln -s /bin/php56-cgi /bin/php-cgi
ln -s /bin/php56
```

