# Info

Este es un trabajo en progreso, actualmente me quede pegado en la compilacion de php56 desde AUR, debido a la libreria libxml (Tengo libxml2 instalado) si alguien sabe como solucionarlo paso a paso, encantado lo escucho

## ¿Que hay de Nuevo?

IOU WEB Interface 64 Bits
Modded by @Proxylivy

Version 1 (NIRVANA):
- NAH; ARCH LINUX + IOU WEB, INCREIBLE

## Datos
> [!TIP] Sitios Recomendados
> - 

IOU WEB, en su planificacion por 2011, tenia 2 ramas principales segun las familias Linux
- Debian
- RedHat
Pero luego de trabajar en distintos Write-Ups
- [[500 - Personal/500.3 - Write-Ups/IOU-WEB/IOU WEB - 32 Bits CentOS 6 (Original Upgrade)|IOU WEB - 32 Bits CentOS 6 (Original Upgrade)]]
- [[500 - Personal/500.3 - Write-Ups/IOU-WEB/IOU WEB - 64 Bits CentOS 7|IOU WEB - 64 Bits CentOS 7]]
- [[500 - Personal/500.3 - Write-Ups/IOU-WEB/IOU WEB - Intentos Fallidos|IOU WEB - Intentos Fallidos]]

Me di cuenta que IOU WEB es solo una pagina web, montada sobre PHP54 y las imagenes necesitan su entorno

Asi que me pregunte, sera compatible con Arch Linux??

Asi que con este write-up lo vamos a corroborar


# Instalacion
> [!TIP] Lecturas Recomendadas
> - [Github proxylivy/dotfiles-proxylivy](https://github.com/proxylivy/dotfiles-proxylivy)
> - [Arch Wiki - Installation Guide](https://wiki.archlinux.org/title/Installation_guide)

Hay 2 formas, utilizar la imagen QCOW2 directamente de Arch

> Descarga la ultima imagen de Qcow2 para ArchLinux
```
wget https://geo.mirror.pkgbuild.com/images/latest/Arch-Linux-x86_64-basic.qcow2
```

> En tu Host, mueve el .qcow2 para la carpeta de QEMU 
```
mv Arch-Linux-x86_64-basic.qcow2 /var/lib/libvirt/images/
```


Tienen un peso de ~500MB aprox, permite crear facilmente un entorno con limite 40GB, mas que suficiente

Configuracion de QEMU

Vista General
- Nombre: iou-web-en-arch-linux
- Titulo: IOU WEB en Arch Linux
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
- Asignacion Actual: 4096 o 8196
- Asignacion Maxima: 4096 o 8196

Le das en instalar nomas

## Instalar Paquetes
> [!TIP] Lecturas Recomendadas
> - [Minibots Blogs - Extraccion de Archivos RPM en Arch Linux](https://minibots.wordpress.com/2024/09/17/extraccion-de-archivos-rpm-en-archlinux/)

Al iniciar, te pedira contraseña
- User: arch
- Pass: arch

> Extrae la ip del servidor para hacer las configuraciones
```
ip -br a
```

> Accede via ssh desde otra maquina para poder copiar y pegar, la contraseña es `arch`
```
ssh arch@{ip}
```

> Actualizas los paquetes
```
sudo pacman -Syu
```

> Instala Kitty (mejora la compatibilidad con terminales kitty), recuerda salir de la sesion ssh e iniciar otra vez
```
sudo pacman -S kitty
```

> Instalas opcionales para la salud mental del creador
```
sudo pacman -S micro fish duf lsd ttf-hack-nerd
```

> Habilita el repositorio Multilib, editando `/etc/pacman.conf` y descomentando estas 2 lineas
```
[multilib]
Include = /etc/pacman.d/mirrorlist
```

> Actualiza los repositorios
```
sudo pacman -Syu
```

> Instala paquetes para la compatibilidad con la extraccion paquetes de RPM
```
sudo pacman -S rpm-tools cpio rpm-sequoia
```

> Instala librerias de compatibilidad para 32 bits
```
sudo pacman -S lib32-glibc lib32-zlib lib32-openssl lib32-libpcap lib32-libxml2
```

> Instala Utilidades del sistema y desarrollo
```
sudo pacman -S base-devel inetutils rsync git wget cmake gcc make htop tmux byobu screen strace tree lsof ncdu btop mlocate which go dma libxml2
```

> Instala todos los paquetes, me aburri
```
sudo pacman -S busybox 
```

> Instala paquetes para Python
```
sudo pacman -S python python-pip
```

> Instala HTTPD (Apache)
```
sudo pacman -S apache logrotate net-tools
```

NOTE: RECOMIENDA
```
mariadb-libs
db
lynx
```

> Actualiza la base de datos
```
sudo updatedb
```

**Instala YAY precompilado**
> Ve a la carpeta prinicipal
```
cd
```

> Instala yay desde un comando unico
```
sudo pacman -S --needed git base-devel && git clone https://aur.archlinux.org/yay-bin.git && cd yay-bin && makepkg -si
```

> Ve a la carpeta principal
```
cd
```

> Elimina la carpeta de Yay
```
rm -drf ~/yay
```

> Actualiza a yay
```
yay -Syu
```

> Actualiza bin
```
yay -S yay-bin
```

> Actualiza el sistema
```
yay -Syu
```

**Instala PHP**
> [!TIP] Lecturas Recomendadas
> - [Arch Wiki - PHP](https://wiki.archlinux.org/title/PHP)
> - [Arch AUR - PHP56](https://aur.archlinux.org/packages?O=0&SeB=nd&K=php56&outdated=&SB=p&SO=d&PP=50&submit=Go)

VAMOS CON PHP56
> Instala Dependencias de Pacman
```
sudo pacman -S unixodbc tidy libmcrypt libvoikko libxslt freetds enchant postgresql-libs gd libfbclient patchelf recode net-snmp libvpx hspell aspell hunspell nuspell
```

> Instala las dependencias de php56
```
yay -S c-client
```

> Instalar php56 (Sin output la principio, se demora un buen buen rato, paciencia)
```
yay -S php56
```

> Instalar Modulos de PHP56
```
yay -S php56-apache php56-cli php56-cgi php56-curl php56-fpm php56-gd php56-mbstring php56-mysql php56-pdo php56-pspell php56-sqlite php56-xml php56-zip
```

> Revisa los nombres de los modulos
```
pacman -Ql php56-apache | grep modules
```

> El comando anterior te mostrara algo parecido a
```
php56-apache /usr/lib/httpd/modules/libphp56.so
php56-apache /etc/httpd/conf/extra/php56-module.conf
```

php-common: No existe
php-mysqlnd: No existe, el mas parecido es php-mysql

> Modifica el archivo `/etc/httpd/conf/httpd.conf`, al final de `LoadModule`, en la linea 173
```
LoadModule php_module modules/libphp.so
AddHandler php-script .php
```

> Configura al final de
```
Include conf/extra/php_module.conf
```

**Configura PHP**
Edita `/etc/php/php.ini`

1. Modificar
```
open_basedir = /opt/iou/:/srv/http/:/var/www/:/home/:/tmp/:/var/tmp/:/var/cache/:/usr/share/pear/:/usr/share/webapps/:/etc/webapps/
```

2. Descomenta
```
extension=gd
extension=pdo_mysql
extension=mysqli
extension=mysql
extension=pdo_sqlite
extension=sqlite3
```

## Instala IOU WEB

> Crear carpeta para git
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

> Cambia los permisos en /opt
```
sudo chown -Rh arch:root /opt
```

> Mueve la carpeta `opt` hacia el root del sistema
```
rsync -Phvar ~/iou/opt/* /opt/
```

> Crea las carpetas faltantes de IOU
```
mkdir -p /tmp/iou /opt/iou/labs /opt/iou/scripts /opt/iou/data/{Export,Import,Logs,Sniffer} /opt/iou/html/iou-web/yum/repodata/
```

> Crea carpeta para Cisco XML a localhost
```
sudo mkdir -p /var/www/html
```

> Crea los siguientes archivos vacios
```
touch /opt/iou/html/iou-web/version /opt/iou/html/iou-web/whatsnew /opt/iou/html/iou-web/yum/repodata/repomd.xml
```

> Arregla los permisos de HTML
```
find /opt/iou/html -type f -exec chmod 644 {} \;
```

> Arregla los permisos de los Ejecutables
```
chmod 755 /opt/iou/bin/* /opt/iou/cgi-bin/*
```

> Ve a la carpeta ~/iou/etc/
```
cd ~/iou/etc
```

> Elimina yum.repos.d
```
rm -drf ~/iou/etc/yum.repos.d
```

> Copia el archivo de configuracion de HTTPD con rutas exactas
```
sudo cp ~/iou/etc/httpd/conf.d/iou.conf /etc/httpd/conf/conf.d/iou.conf
```

> Copia el archivo de confiugracion de Logrotate para rotar logs de apache
```
sudo cp ~/iou/etc/logrotate.d/iou /etc/logrotate.d/iou
```

> Instala el archivo de sudoers
```
sudo install -m 0440 ~/iou/etc/sudoers.d/iou /etc/sudoers.d/iou
```

> Desde el Host debes descargar libcrypto.so.4, (recomiendo [Labhub](https://drive.labhub.eu.org/0:/addons/iol/lib/)), y lo envias a `/home/arch/`
```
rsync -Phvar libcrypto.so.4 root@{ip-server}:/home/arch/
```

> Luego volviendo a la maquina, mueves libcrypto como sudo a `/usr/lib`
```
sudo cp ~/libcrypto.so.4 /usr/lib/
```

> Arregla los permisos a libcrypto.so.4
```
sudo chmod 755 /usr/lib/libcrypto.so.4
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

> Exporta el editor temporalmente, en mi caso `micro`
```
export EDITOR=micro
```

> Edita el archivo sudoers de iou con
```
sudo visudo -f /etc/sudoers.d/iou
```

> Se abrira el archivo para editar, deberas cambiar todos los `apache` por `http`, quedara algo asi
```
Defaults:http runas_default=root, !requiretty, setenv, umask_override, umask=0002
http ALL=(root) NOPASSWD: /opt/iou/bin/*
http ALL=(root) NOPASSWD: /sbin/ifconfig
http ALL=(root) NOPASSWD: /usr/bin/pkill
http ALL=(root) NOPASSWD: /bin/rm
http ALL=(root) NOPASSWD: /usr/sbin/apachectl
http ALL=(root) NOPASSWD: /sbin/fuser
http ALL=(root) NOPASSWD: /bin/kill
http ALL=(root) NOPASSWD: /sbin/reboot

# Ubuntu has different path
http ALL=(root) NOPASSWD: /bin/fuser
```

> Crea una base de datos desde el template en caso de que no exista ninguna
```
[ ! -f /opt/iou/data/database.sdb ] && cp -a /opt/iou/data/template.sdb /opt/iou/data/database.sdb
```

> Arregla permisos de apache dentro de las carpetas de IOU WEB
```
sudo chown -Rh http:http /opt/iou/ /tmp/iou
```

> Modifica el archivo `/opt/iou/html/.htaccess`
```
php_value post_max_size 512M
php_value upload_max_filesize 512M
```


CAPAZ QUE LE FALTA UN
```
chmod 755 -Rh /opt/iou
```


## Configura HTTP

**Edita `/etc/httpd/conf/conf.d/iou.conf`**

> Modifia la linea 6, `Indexes` a `+Indexes` la cual esta dentro de `<Directory /opt/iou/data>`
```
Options +Indexes -FollowSymLinks
```

> Agrega debajo de la linea 9, `Require all granted` que esta dentro de `<Directory /opt/iou/data>`
```
Require all granted
```

**Edita `/etc/httpd/conf/httpd.conf`**

Encuentra la linea `Directory` (Aprox Linea 239)
```
<Directory "/opt/iou/html">
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
```

> Encuentra la linea `DocumentRoot` (Aprox Linea 257)
```
DocumentRoot "/opt/iou/html"
```

> Busca "Directory", hasta encontrar la linea `<Directory "/srv/http/cgi-bin">` y cambiala para que se vea asi
```
<Directory "/opt/iou/cgi-bin">
    AllowOverride All
    Options None
    Require all granted
</Directory>
```

> [!IMPORTANT] Comentar distintos `Directory`
> Recomiendo Comentar todas las configuraciones en este archivo que no sean las de IOU WEB para que el sistema httpd solo busque las rutas necesias, el rango de las que pille son
> - `/srv/http`: Linea 258, 271, 278, 283 y 284

> Elimina la pagina de prueba de Apache
```
sudo rm -f /etc/httpd/conf
```

> Crea el archivo de logs
```
touch /opt/iou/data/Logs/error.txt
```

> Arregla los permisos de ese log
```
sudo chown http:http /opt/iou/data/Logs/error.txt
```

> Actualiza los archivos modificados
```
sudo systemctl daemon-reload
```

> Activa los servicios de httpd (Apache)
```
sudo systemctl status httpd
```

> Reinicia
```
reboot
```



### Soluciona Problemas de HTTP
> Revisa Logs de error en Apache
```
cat /var/log/httpd/error_log
```

> Agrega el usuario arch a http???
```

```