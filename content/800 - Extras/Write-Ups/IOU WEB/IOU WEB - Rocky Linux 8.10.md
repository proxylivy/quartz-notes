# Info
Version modernizada de IOU WEB ejecutandose sobre Rocky Linux 8.10 (64 bits) con mejoras de seguridad, estabilidad y rendimiento frente a las versiones legacy [[800 - Extras/Write-Ups/IOU WEB/IOU WEB - 32 Bits - CentOS 6 (Legacy)|IOU WEB - 32 Bits - CentOS 6 (Legacy)]] y [[800 - Extras/Write-Ups/IOU WEB/IOU WEB - CentOS 7 (Legacy)|IOU WEB - CentOS 7 (Legacy)]]

Basado en el trabajo de **Andrea Dainese** ([@dainok](https://github.com/dainok)) y mejorado en un fork por [@proxylivy](https://github.com/proxylivy) en [Github - proxylivy/iou-web](https://github.com/proxylivy/iou-web).

> [!WARNING] Sobre IOS XE
> - NO soporta switches y routers IOS XE `x86_64_crb_linux`. Si necesitas estas imagenes, prefiere [[800 - Extras/Write-Ups/EVE-NG/EVE-NG - Install|EVE-NG]] o [GNS3](https://gns3.com/)

## Requisitos
- CPU: 2vCPU
- RAM: 4096MB
- Storage: 8GB

## ¿Que hay de nuevo?
7mo Intento:
- Basado en Rocky Linux 8.10 Minimal 64 bits
- Paravirtualizacion activada para instrucciones VT-X y emulacion KVM
- Integracion de Drivers Virtio y Qemu Guest Agent
- Integracion `VBoxGuestAdditions` en su version `7.2.4`
- Repositorios Epel, Remi Release y ElRepo
- Kernel actualizado a `7.0.10-1` (23 May 2026)
- Servicios innecesarios desactivados para reducir consumo
- Instalacion con soporte BIOS
- IP mostrada automaticamente en el login
- OpenSSL 32 Bits actualizado a "1.0.2zl" (2025), reemplazando "[0.9.8o](https://www.mail-archive.com/openssl-announce%40openssl.org/msg00089.html)" (2010)
- Paquetes PHPs actualizado a `7.4.33`
- Hardening HTTPD
- Enlaces Simbolicos creados para las nuevas imagenes, reemplazando nombres antiguas de labs

> [!TIP] Lecturas Recomendadas
> - [EndOfLife - Rocky Linux](https://endoflife.date/rocky-linux)
> - [Cisco - IOS OS Software](https://www.cisco.com/c/en/us/support/ios-nx-os-software/index.html)
> - [Docker Hub - Rocky Linux](https://hub.docker.com/r/rockylinux/rockylinux)

> [!IMPORTANT] Sobre Selinux y Firewalld
> La integracion con Firewalld es insegura, y nunca logre hacer funcionar con Selinux, se recomienda dejar desactivados ambos. Si conoces una forma de hacerlos funcionar, envia un PR ^^

Luego de 2 años investigando en experimentos fallidos, documentacion abandonada y muchas horas de troubleshooting, cada pieza termina calzando y el resultado es mejor de lo que pude pensar y logre con versiones anteriores.

Esta version continua donde quedo [[800 - Extras/Write-Ups/IOU WEB/IOU WEB - CentOS 7 (Legacy)|IOU WEB - CentOS 7 (Legacy)]], avandonando varias ideas como compilaciones extras, compatibilidad con glibc superior y enfoques que no ayudaban en nada. Utilizar una base como lo es *Rocky Linux 8.10* es mucho 


La base se reconstruyo desde cero sobre `Rocky Linux 8.10`, una distribucion moderna y firme, nacida tras la caida de CentOS.

El nombre de este VM, **Atlas**, refleja lo que busca representar: Firmeza, estabilidad, seguridad y continuidad.

# Creacion del VM
Antes de comenzar, descarga la ISO minimal de [Rocky Linux](https://rockylinux.org) 8.10 (64 bits): `Rocky-8.10-x86_64-minimal.iso`. 

La rama 8.x tiene soporte hasta el 31 de mayo de 2029.

Debes elegir una opcion
- [[#Opcion 1 QEMU/KVM]]: Host Linux (Recomendado)
- [[#Opcion 2 Virtualbox]]: Host Windows o entornos mixtos sin soporte completo KVM
- Opcion 3 Otros hipervisores (Sin probar): No estan documentados aqui pero igualmente podrias probarlos sin problemas, podrias probar
	- Proxmox VE (Virt Tipo 1)
	- Hyper-V (Nativo Windows)
	- VMware (Si es que ya tienes un stack, Broadcomm hace todo mas complicado)

### Opcion 1: QEMU/KVM 

Desde virt-manager

*Etapa 1 - Asistente de creacion*
- Selecciona "Medio de Instalacion Local (Imagen ISO o CDROM)" y haz click en "Adelante"
- Explora y selecciona la ISO "`Rocky-8.10-x86_64-minimal.iso`"
- En tipo de sistema operativo, elige "`Rocky Linux 8`" y continua con "Adelante"
- Asigna un disco virtual de 20GB (Por defecto y suficiente para el proyecto)
- Cambia el nombre del VM a algo descriptivo, por ejemplo: "*IOU-WEB-ROCKY-8*"
- Marca la casilla "Personalizar configuracion antes de instalar" y haz click en "Finalizar" para abrir la configuracion avanzada (Etapa 2)

*Etapa 2 - Ajuste de Hardware*

Vista General
- Nombre: IOU-WEB-Rocky
- Titulo: IOU WEB Rocky
- Chipset: Q35
- Firmware: BIOS (UEFI me da problemas con esta version)
CPU (La asignacion depende de tu host; esta seria referencia)
- Habilita "host-passthrough"
- En "Topologia" marca "Establecer manualmente la topologia de CPU"
	- Socket: `1` (CPUs Fisicas)
	- Centros: `2` (Nucleos)
	- Hilos: `2`
Memoria (Depende cuanta memoria tengas)
- Asignacion Actual: `4096` (Minimo), Idealmente `8196` si tu host lo permite

Tras aplicar estos cambios, guarda la configuracion, inicia la maquina virtual y continuas con la [[#Instalacion Base]]

### Opcion 2: Virtualbox
Si prefieres Virtualbox, puedes crear un VM equivalente con la siguiente configuracion

*Etapa 1 - Creacion Basica*

Crea una nueva VM y define:
- Nombre: `IOU WEB Atlas`
- ISO: `Rocky-8.10-x86_64-minimal.iso`
- Desactivas "Proceder con instalacion desatendida"
- Tipo de OS: `Linux`
- Distribucion: `Red Hat`
- Version: `Red Hat 8.x (64-bits)`

Haz click en "Terminar" y abre la configuracion de la maquina antes de iniciar

*Etapa 2 - Configuracion Detallada*

- General -> Descripcion
	- Documenta tu usuario y contraseña por defecto, por ejemplo, "`cisco / cisco`" y asi no olvidarlos
- Sistema
	- Placa Base
		- Memoria Base: 8196MB (Minimo 4096MB)
		- Dispositivo Apuntador: Tableta USB
		- Activa "Habilitar reloj hardware en tiempo UTC"
	- Procesador
		- Habilita el Mayor numero de CPU que puedas, minimo 2 vCPU
		- Habilitar PAE/NX
		- Forzar VT-x/AMD-V anidado con "`VBoxManage modifyvm "IOU WEB Atlas" --nested-hw-virt on`"
	- Aceleracion
		- Interfaz de paravirtualizacion: "KVM"
		- Activar Hardware de virtualizacion
- Pantalla
	- Memoria de Video: 128MB
	- Controlador Grafico: VBoxSVGA (Default)
	- Activar Aceleracion 3D
- Almacenamiento (Depende si tienes SSD o HDD, si tienes HDD ignora esta parte)
	- Controlador: SATA
		- Nombre: SATA
		- Tipo: AHCI
		- Cantidad de puertos: 3
		- Activa "Usar cache de I/O anfitrion"
	- `VM-name.vdi`
		- Activa "Unidad de estado solido"
- Audio (Omitido por defecto)
- Red
	- Adaptador 1
		- Activa "Habilitar adaptador de red"
		- Conectar a: Adaptador Puente
		- Nombre: *Tu Interfaz*
		- Tipo de Adaptador: Intel Pro/1000 MT Server (82545EM)
		- Modo Promiscuo: Permitir todo
- Puertos Serie (Omitido por defecto)
- USB
	- Activar controlador USB
	- Seleccionar Controlador USB 2.0 (OHCI + EHCI)
- Interfaz de usuario
	- Desactiva "Mini ToolBar - Mostrar en pantalla completa/fluido"

# Instalacion Base

Al arrancar con la ISO de Rocky Linux 8.10, aparece el menu de instalacion grafico. Detallo los pasos a seguir:
1. Idioma y Localizacion
	- Idioma: `Español`
	- Localizacion: `Español (Chile)`
2. Configuracion del sistema
	- Selecciona "Destino de la Instalacion"
		- En "Configuracion del Almacenamiento", selecciona la opcion "Personalizada" y luego presiona "Hecho"
			- Del menu de puntos de montaje, cambia "LVM" a "Particion Estandar"
			- Apretamos el boton con el icono "Mas"
				- Punto de Montaje: `biosboot`
				- Capacidad: 2MiB
				- Tipo de dispositivo: `Particion estandar`
			- Apretamos "mas"
				- Punto de Montaje: `/boot`
				- Capacidad: 1GiB
				- Sistema de archivos: `ext4` y selecciona "Actualizar Parametros"
			- Apretamos "mas"
				- Punto de Montaje `/`
				- Capacidad: todo, vacio y presionar "Enter"
				- Sistema de archivos: `ext4` y selecciona "Actualizar Parametros"
		- Seleccion "Listo" 2 veces (Ignoramos el mensaje de advertencia sobre la falta de Swap", no se necesita)
	- Selecciona "KDUMP"
		- Desactiva la opcion "Habiitar KDUMP" y le das en "Hecho"
	- Selecciona "Red y Nombre del Equipo"
		- Enciendes la interfaz de red y le das en "Hecho"
	- Selecciona "Contraseña de Root"
		- Configuras `cisco` y `cisco` y le das 2 veces en "Hecho"
	- Selecciona "Creacion de usuario"
		- Configura `cisco` en todos los espacios y marca en "Hacer de este usuario un administrador" y le das 2 veces en "Hecho"
	- Selecciona "Seleccion del Software" y elije "Instalacion Minima" y le das en "Hecho"
3. Ahora apretas en "Empezar Instalacion" y mientras carga, continua
	- Apretas 2 veces en Listo y esperas que la instalacion termine (Se demora unos 6 minutos en un SSD)
4. Cuando termine apreta "Reiniciar", ya que el instalador no lo hace automaticamente

> [!TIP] Consejo SSH
> Tras la instalacion, es recomendable acceder a la VM via SSH desde tu host principal, en lugar de la consola grafica del virtualizador. Esto facilita copiar/pegar comandos y trabajar comodamente en una terminal de tamaño completo. Si no reconoce el tipo de terminal, exporta una variable mas generica
> ```
> export TERM=xterm
> ```

Despues de Instalar Rocky Linux, preparamos el sistema con los repositorios y paquetes necesarios

## Repositorios y Paquetes

> [!TIP] Sobre los repositorios
> - [Rocky Linux Wiki - repo](https://wiki.rockylinux.org/rocky/repo/)
> - [Pkgs - RHEL Repos](https://rhel.pkgs.org/)
> - [EPEL](https://docs.fedoraproject.org/en-US/epel/) proporciona paquetes de software adicionales mantenidos por la comunidad
> - [RHEL Docs - CRB](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/package_manifest/codereadylinuxbuilder-repository) (CodeReady Builder) se debe activar para usar EPEL
> - [Remi repo](https://rpms.remirepo.net/) ofrece versiones actualizadas de PHP y archiva antiguas versiones, nosotros usaremos PHP 7.4
> - [ElRepo](https://elrepo.org/wiki/doku.php?id=start) ofrece kernels mas nuevos y drivers extras

> Actualiza todos los paquetes del sistema a la ultima version
```
sudo dnf update --refresh
```

> Instala repositorio EPEL
```
sudo dnf install epel-release
```

> Habilita CRB
```
sudo /usr/bin/crb enable
sudo /usr/bin/crb status
```

> Deshabilita el modulo de freeradius (Generaba problemas)
```
sudo dnf module disable freeradius
```

> Instala repositorio Remi
```
sudo dnf install https://rpms.remirepo.net/enterprise/remi-release-8.rpm
```

> Importa las llaves GPG v2 de ElRepo
```
sudo rpm --import https://www.elrepo.org/RPM-GPG-KEY-v2-elrepo.org
```

> Instala repositorio ElRepo
```
sudo dnf install https://www.elrepo.org/elrepo-release-8.el8.elrepo.noarch.rpm
```

> Habilita de forma permanente el repositorio ElRepo-kernel
```
sudo dnf config-manager --enable elrepo-kernel
```

> Añade el repositorio de Fish Shell
```
sudo dnf config-manager --add-repo https://download.opensuse.org/repositories/shells:/fish:/release:/3/CentOS_8/shells:fish:release:3.repo
```

> Actualiza modulos y paquetes desde los nuevos repos
```
sudo dnf distro-sync --allowerasing
```

A continuacion, instalaremos los paquetes necesarios para IOU WEB y sus dependencias, asi como algunas utilidades utiles para la administracion

> Instala herramientas Multilib y Dependencias 32 Bits
```
sudo dnf install multilib-rpm-config dnf-utils glibc.i686 libgcc.i686 libstdc++.i686 glibc-devel.i686 glibc-static.i686 libpcap.i686
```

> Paquetes principales
```
sudo dnf install httpd telnet logrotate dhclient wget git rsync python3 dkms qemu-guest-agent libvirt dos2unix tcltls pam-devel net-tools libtool sqlite
```

> Paquetes Utiles
```
sudo dnf install micro fish btop xclip xsel zstd bat tree lsof kitty ncdu
```

> Instalar [duf](https://github.com/muesli/duf) | Revisa una nueva [Version](https://github.com/muesli/duf/releases) (0.9.1 al 2025)
```
sudo dnf install https://github.com/muesli/duf/releases/download/v0.9.1/duf_0.9.1_linux_amd64.rpm
```

> Borra paquetes que sobran
```
sudo dnf remove nginx-filesystem
```

> Configura Micro, abre `micro` y presiona `Ctrl+e`, escribe
```
set softwrap true
```

> [!NOTE] Sobre ejecucion PHP
> - Segun [PHPstan](https://phpstan.org/user-guide/getting-started), la mayoria de codigo escrito para PHP 5.x puede ser ejecutado en PHP 7.4.33 casi sin modificaciones.

> Resetea los modulos de PHP
```
sudo dnf module reset php
```

> Cargas el modulo de PHP 7.4 desde remi
```
sudo dnf module enable php:remi-7.4
```

> Instala PHP 7.4 y extensiones para IOU WEB
```
sudo dnf install composer php php-fpm php-cli php-common php-curl php-pecl-crypto php-gd php-mbstring php-mysqlnd php-pdo php-process php74-runtime php-xml php-pecl-zip graphviz graphviz-gd php-pear mod_ssl
```

> EXPERIMENTAL: Instalar GraphViz usando pear
```
sudo pear install Image_GraphViz
```

> [!TIP] Sobre MPM en PHP
> - La configuracion con `php-fpm` activado, necesita el modulo `mod_mpm_event.so`
> - La configuracion antigua de `mod_php` utiliza `mod_mpm_prefork.so`
> Nosotros usaremos `mod_mpm_event.so` por lo que no hay que configurar el archivo `/etc/httpd/conf.modules.d/00-mpm.conf`. Se deja por defecto

> [!TIP] Sobre Kernel
> - [Kernel Archives](https://kernel.org/)
> - [CrownCloud - How to Install Kernel on RockyLinux](https://wiki.crowncloud.net/?How_to_Install_Kernel_5_x_on_RockyLinux_8)
> - [The ElRepo Project Doku Wiki](https://elrepo.org/wiki/doku.php?id=start)

Yo prefiero actualizar desde la rama 4.18.x backport RHEL a 7.x Upstream de la mano de ELREPO

> Actualizar los kernels
```
sudo dnf install kernel-ml kernel-ml-core kernel-ml-devel kernel-ml-headers kernel-ml-modules kernel-ml-modules-extra kernel-ml-tools kernel-ml-tools-libs --allowerasing
```

> [!TIP]- Opcional: Instala RUST para instalar LSD
> > Instala Rustup como indica la [Documentacion](https://rust-lang.org/tools/install/)
> ```
> curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
> ```
> 
> > Sal de la sesion ssh y vuelve a ingresar
> ```
> exit
> ssh user@ip
> ```
> 
> > Opcional: Instala lsd mediante cargo
> ```
> cargo install lsd
> ```
> 
> > Opcioonal: Permite el uso de LSD para todo el sistema
> ```
> sudo mv /home/cisco/.cargo/bin/lsd /bin/lsd
> ```
> 
> Elimina Rustup
> ```
> rustup self uninstall
> ```

## Configuraciones

> [!IMPORTANT] Sobre la configuracion de GRUB
> 1. GRUB aparecera por 1 segundo en forma de submenus, y se encendera en forma silenciosa
> 2. `GRUB_ENABLE_BLSCFG=false` deshabilita BLS (Boot Loader Spec) para usar un grub.cfg clasico (Solo compatible con BIOS)
> 3. `net.ifnames=0` fuerza los nombres tradicionales de interfaces de red (eth0, eth1, etc.) en lugar de nombres predictivos (enp4s0f1, enp4s0f2, etc.) para simplificar la configuracion
> 4. Si al reiniciar la nueva configuracion de interfaz no da IP, cambia la MAC desde el virtualizador

> Creas las carpetas necesarias
```
mkdir ~/git
```

> Crea carpetas para el sistema (Usadas luego)
```
sudo mkdir -p /media/Vbox /tmp/iou /etc/pki/tls/iou/
```

> Edita `/etc/default/grub` y ajusta las siguientes opciones
```
GRUB_TIMEOUT=1
...
GRUB_DISABLE_SUBMENU=false
GRUB_CMDLINE_LINUX="quiet loglevel=3 net.ifnames=0"
#### TALVEZ ESTE NO ####
GRUB_ENABLE_BLSCFG=false
```

> Establece el nuevo kernel como predeterminado para Grub
```
sudo grub2-set-default 0
```

> Actualizar configuracion de GRUB para sistemas BIOS
```
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

> Edita `/etc/hostname` y pon el siguiente contenido
```
iou.example.com
```

> Edita `/etc/hosts` y pon el siguiente contenido
```
127.0.0.1   iou.example.com iou localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         iou.example.com iou localhost localhost.localdomain localhost6 localhost6.localdomain6
127.0.0.127 xml.cisco.com
127.0.0.127 www.routereflector.com routereflector.com public.routereflector.com ww25.public.routereflector.com
```

> [!TIP] Sobre DHCLIENT
> - [HugeMan - dhclient conf](https://huge-man-linux.net/man5/dhclient.conf.html)

> edita `/etc/dhcp/dhclient.conf` para agilizar el proceso de DHCP
```
timeout 3;
retry 4;
reboot 3;
initial-interval 1;
```

> Elimina el mensaje de Cockpit
```
sudo rm -f /etc/motd.d/cockpit /etc/issue.d/cockpit.issue
```

> Edita `/etc/environment` para no exportar cada sesion SSH
```
TERM=xterm
GPG_TTY=$(tty)
EDITOR=micro
```

> Edita `/etc/httpd/conf.d/ssl.conf` con las rutas para los futuros archivos crt y key de las lineas `85` y `93`
```
SSLCertificateFile /etc/pki/tls/iou/iou.crt

SSLCertificateKeyFile /etc/pki/tls/iou/iou.key
```

> Crea llaves para la instalacion
```
sudo openssl req -x509 -newkey rsa:4096 -nodes -keyout /etc/pki/tls/iou/iou.key -out /etc/pki/tls/iou/iou.crt -sha256 -days 3650 -subj "/CN=iou.local" -addext "subjectAltName=DNS:iou.local,IP:127.0.0.1"
```

> Configura el permiso de las llaves
```
sudo chmod 600 /etc/pki/tls/iou/iou.key /etc/pki/tls/iou/iou.crt
```

**Firewall**

> [!TIP] Sobre Configuracion del Firewall
> 1. Permitir SSH (22/TCP)
> 2. Permitir HTTP (80/TCP) y HTTPS (443/TCP)
> 3. Permitir Telnet (23/TCP)
> 4. Permitir rangos de puertos dinamicos para cada Nodo usados por IOU WEB en TCP y UDP (2000-3000/TCP) (2000-3000/UDP)
> 5. Permitir conexiones a puertos dinamicos TCP (32768-60999/TCP)
> 6. Marcar la interfaz `eth0` como confiable para no filtrar el trafico local

> Permitir puerto SSH 22/TCP
```
sudo firewall-cmd --permanent --zone=public --add-port=22/tcp
```

> Permitir Puerto HTTP 80/TCP
```
sudo firewall-cmd --permanent --zone=public --add-port=80/tcp
```

> Permitir Servicio HTTP
```
sudo firewall-cmd --permanent --zone=public --add-service=http
```

> Permitir Puerto HTTPS 443/TCP
```
sudo firewall-cmd --permanent --zone=public --add-port=443/tcp
```

> Permitir Servicio HTTPS
```
sudo firewall-cmd --permanent --zone=public --add-service=https
```

> Permitir Puerto Telnet 23/TCP
```
sudo firewall-cmd --permanent --zone=public --add-port=23/tcp
```

> Permitir puertos 2000 - 3000 (Dinamic Telnet) TCP
```
sudo firewall-cmd --permanent --zone=public --add-port=2000-3000/tcp
```

> Permitir puertos 2000 - 3000 (Dinamic Telnet) UDP
```
sudo firewall-cmd --permanent --zone=public --add-port=2000-3000/udp
```

> Permitir Puertos Dinamicos TCP
```
sudo firewall-cmd --permanent --add-port=32768-60999/tcp
```

> Permitir Servicio Telnet
```
sudo firewall-cmd --permanent --zone=public --add-service=telnet
```

> Marcar la interfaz eth0 como confiable
```
sudo firewall-cmd --permanent --zone=trusted --add-interface=eth0
```

> Marcar las conexiones que haga la interfaz eth0 como confiables
```
sudo firewall-cmd --permanent --zone=trusted --set-target=ACCEPT
```

> Recargar configuracion Firewalld
```
sudo firewall-cmd --reload
```

> Listar los servicios permitidos
```
sudo firewall-cmd --zone=public --list-services
```

> Listar los puertos permitidos
```
sudo firewall-cmd --zone=public --list-ports
```

> Lista todo
```
sudo firewall-cmd --list-all
```

> Habilitar Servicio
```
sudo systemctl enable firewalld.service
```

> Reinicia la maquina
```
sudo reboot
```

> [!IMPORTANT] REINICIO NECESARIO
> En este punto para utilizar el nuevo kernel y que se apliquen el nuevo nombre de hostname, es mejor reiniciar, si no lo haces, no podras borrar los viejos kernel

> Verifica que estes con el nuevo kernel
```
uname -r
```

> Borra los antiguos kernels
```
sudo dnf remove kernel kernel-devel kernel-modules kernel-core linux-firmware
```

**OpenSSL libcrypto**

> [!CAUTION] Sobre OpenSSL Libcrypto
> Los binarios de IOU WEB requieren la libreria `libcrypto.so.4` (OpenSSL 1.0.x), se comparte la libreria precompilada en base a `OpenSSL 0.9.8` (01 Junio 2010) disponible en [Labhub](https://alist.labhub.eu.org/LabHub/addons/iol/lib).
> 
> Puedes compilar tu version mas actual desde el repositorio [Gitub - alsyundawy/openssl-1.0.2](https://github.com/alsyundawy/openssl-1.0.2), siguiendo las instrucciones en [[#Compilar Libcrypto]]

> Descarga el archivo `libcrypto.so.4` desde labhub
```
sudo wget -O /usr/lib/libcrypto.so.4 "https://alist.labhub.eu.org/d/LabHub/addons/iol/lib/libcrypto.so.4?sign=5v0a9PvzSxQyA0hZdmRiqh_cm_m60sHCiPJEf9j-iH0=:0"
```

> Cambia el propietario de `libcrypto.so.4`
```
sudo chown root:root /usr/lib/libcrypto.so.4
```

> Establece los permisos de ejecucion a `libcrypto.so.4`
```
sudo chmod 755 /usr/lib/libcrypto.so.4
```

> Actualiza el cache de las librerias
```
sudo ldconfig
```

> [!TIP] Corrobora el archivo
> > Ejecuta `file /usr/lib/libcrypto.so.4`
> ```
> /usr/lib/libcrypto.so.4: ELF 32-bit LSB shared object, Intel 80386, version 1 (SYSV), dynamically linked, BuildID[sha1]=0cb13a46d6a6e068052daa4bb85e0c18ed835533, stripped
> ```

# Instalar IOU WEB

> Ve a la carpeta de git
```
cd ~/git
```

> Clona el repositorio de IOU WEB modificado por Proxylivy
```
git clone https://github.com/proxylivy/iou-web.git && cd iou-web
```

## Archivos de configuracion

> [!TIP] Versiones Probadas de las configuraciones
> - Apache `2.4` (`2.4.37-65`)
> - Logrotate `3.14.0`
> - Sudo `1.9.5p2`
> - Sudoers file grammar version `48`
> - Systemd `239`

> Copia el archivo de configuracion de Apache especifico de IOU WEB
```
sudo cp conf/httpd/conf.d/iou.conf /etc/httpd/conf.d/iou.conf
```

> Copia el archivo de Logrotate especifico de IOU WEB
```
sudo cp conf/logrotate.d/iou /etc/logrotate.d/iou
```

> Mueve el archivo de configuracion de fpm-php
```
sudo cp conf/php-fpm.d/iou.conf /etc/php-fpm.d/iou.conf
```

> Instala el archivo de Sudoers para IOU
```
sudo install -m 0440 conf/sudoers.d/iou /etc/sudoers.d/iou
```

> Instala los archivos de systemd
```
sudo cp conf/systemd/* /etc/systemd/system/
```

> Actualiza los permisos
```
sudo chmod 644 /etc/systemd/system/iou*
```

> Actualiza systemctl
```
sudo systemctl daemon-reload
```

> Inicia los servicios de systemd creados
```
sudo systemctl enable iou-issue.service iou-cert.service
```

> Copia la carpeta `opt` hacia el root del sistema
```
sudo rsync -ah --info=progress2 iou /opt/
```

> Elimina la pagina de bienvenida de apache para que no interfiera
```
sudo rm -f /etc/httpd/conf.d/welcome.conf
```

## Arregla Permisos

> Cambia el propietario de todos los archivos IOU a apache (httpd)
```
sudo chown -Rh apache:apache /opt/iou /tmp/iou 
```

> Configura directorios donde apache trabaja
```
sudo install -d -m 0755 /opt/iou/data/Export /opt/iou/data/Import /opt/iou/data/Logs /opt/iou/data/Sniffer
```

> Permisos de escritura en directorios de labs, Binarios, CGI y scripts
```
sudo chmod -R 755 /opt/iou/labs /opt/iou/bin/ /opt/iou/cgi-bin/ /opt/iou/scripts/
```

> Cambia el dueño del viejo console
```
sudo chown root:root /opt/iou/cgi-bin/console.old
```

> Cambia los permisos para que no sea ejecutado
```
sudo chmod 400 /opt/iou/cgi-bin/console.old
```

> Configura los directorios dentro de html
```
sudo find /opt/iou/html -type d -exec chmod 0755 {} \;
```

> Configura los archivos dentro de html
```
sudo find /opt/iou/html -type f -exec chmod 0644 {} \;
```

> Prueba la sintaxis de apache, deberia decir `Syntax OK`
```
sudo apachectl configtest
```

> Recarga los cambios hechos en el sistema
```
sudo systemctl daemon-reload
```

> Habilita el servicio de Apache llamado httpd
```
sudo systemctl enable httpd php-fpm
```

## Selinux

> [!CAUTION] Sobre Selinux
> Para la seguridad, Selinux es muy importante, pero no encuentro una forma plausible de integrarlo al funcionamiento de IOU WEB, solo solucionar un par de errores, mi idea es hacer una version con Selinux funcional

**Deshabilitamos Selinux**

> Desabilita Selinux Temporalmente
```
sudo setenforce 0
```

> Desabilita Selinux para siempre modificando el archivo `/etc/selinux/config`
```
SELINUX=permissive
SELINUXTYPE=minimum
```

> Reinicia los archivos modificados
```
sudo systemctl daemon-reload
```

> Reinicia httpd
```
sudo systemctl restart httpd php-fpm
```

## Licencia Imagenes

> [!TIP] Sobre CiscoIOUKeygen
> - [Github - Sohrabian/IOU-Licence-EVE-NG-Python](https://raw.githubusercontent.com/Sohrabian/IOU-Licence-EVE-NG-Python/refs/heads/master/ioukeygen.py)
> - [Github - obscur/gns3-server - CiscoIOUKeygen.py](https://github.com/obscur95/gns3-server/blob/master/IOU/CiscoIOUKeygen.py)

Cisco IOU requiere una clave de licencia vinculada al hostname y hostid de la maquina donde se ejecuta. Un generador de licencias escrito en C fue publicado en 2006, luego adaptado a Python, y esta version adapta ese codigo para funciona en Python3.

> Edita `/opt/iou/scripts/keygen.py` y añade el siguiente codigo:
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

> Ejecuta el script keygen con
```
python3 /opt/iou/scripts/keygen.py
```

> Output ejemplo script keygen
```
Cisco IOU License Generator v2 - Kal 2011, python port of 2006 C version  
hostid=xxxxxxxx, hostname=iou, ioukey=xxxxxx<hextrim>
************************************************************************  
Add the following text to ~/.iourc:
[license]
iou = xxxxxxxxxxxx2100;

************************************************************************  
You can disable the phone home feature... (etc)
```

> Edita `/opt/iou/bin/iourc` y agrega lo que diga el keygen con el siguiente formato:
```
[license]
iou.example.com = xxxxxxxxxxxxxxxx;
```

> Crea enlaces simbolicos a `.iourc`
```
sudo ln -s /opt/iou/bin/iourc /opt/iou/bin/.iourc
```

## Imagenes IOS
> [!CAUTION] Sobre la subida de imagenes
> Las imagenes NO se envian directamente por rsync o parecidos, sino que se suben mediante la interfaz web "Manage IOSes". Y te recuerdo otra vez que las imagenes x86_64 no son compatibles, por lo que no apareceran.

Una vez en la interfaz de IOU WEB, necesitamos cargar las imagenes .bin de IOS que se ejecutaran.

En IOU WEB, ve a la pestaña "Manage" y luego "Manage IOSes" y deberas completar los 3 campos, aqui te los explico
- **Filename**: El nombre completo del archivo tal cual, por ejemplo: `i86bi_LinuxL3-AdvEnterpriseK9-M2_157_3_May_2018.bin`. Este sera el nombre con el que se guardara en `/opt/iou/bin` en el servidor
- **Alias**: Un nombre corto descriptivo que aparecera en la lista de seleccion de imagenes al configurar un nodo, por ejemplo: "`L3 15.7`".
- **Pick a file**: Selecciona el archivo .bin desde tu equipo local.

Haces click en "Uplodad". Repite ese proceso con todas las imagenes deseadas.

Estas son las imagenes que subire:
- Router L3
	- Filename: `i86bi_LinuxL3-AdvEnterpriseK9-M2_157_3_May_2018.bin`
	- Alias: `L3 15.7`
	- Pick a File: *Selecciona el Archivo*
- Switch L2
	- Filename: `i86bi_linux_l2-adventerprisek9-ms.SSA.high_iron_20190423.bin`
	- Alias: `L2 15.2`
	- Pick a file: *Selecciona el Archivo*

Para mantener una compatibilidad con laboratorios antiguos y no tener que reajustar las imagenes con nombres antiguo (ej. `i86bi_linux_l2-ipbasek9-ms.may8-2013-team_track`), crearemos enlaces simbolicos en `/opt/iou/bin` que apunten de los viejos nombres a las nuevas imagenes

Para lograrlo, subiremos una "dummy image" para reservar el nombre, y luego reemplazarlas, en tu computador, crea un archivo llamado `fake.bin` y escribe algun dato dentro. Esta sera nuestra "dummy image" que usaremos para subir nombres a IOU WEB

Segun los laboratorios que he tenido, estas son las imagenes que mas se usan:
- Router
	- Filename: `i86bi_linux-adventerprisek9-ms.154-1.T_A`
	- Alias: `L3 15.4.1T A`
	- Pick a File: fake.bin
- Switch
	- Filename: `i86bi_LinuxL2-AdvEnterpriseK9-M_152_May_2018`
	- Alias: `L2 15.2D`
	- Pick a File: fake.bin
- Switch Alternativo
	- Filename: `i86bi_linux_l2-ipbasek9-ms.may8-2013-team_track`
	- Alias: `L2 15.1M`
	- Pick a File: fake.bin

> De vuelta al VM, ve a `/opt/iou/bin` para eliminar las imagenes subidas
```
cd /opt/iou/bin
```

> Eliminamos los .bin falsos segun el nombre que utilizamos
```
sudo rm -f i86bi_linux-adventerprisek9-ms.154-1.T_A i86bi_LinuxL2-AdvEnterpriseK9-M_152_May_2018 i86bi_linux_l2-ipbasek9-ms.may8-2013-team_track
```

> Creamos un enlace simbolico para el L3
```
sudo ln -s /opt/iou/bin/i86bi_LinuxL3-AdvEnterpriseK9-M2_157_3_May_2018.bin /opt/iou/bin/i86bi_linux-adventerprisek9-ms.154-1.T_A
```

> Creamos un enlace simbolico para el L2
```
sudo ln -s /opt/iou/bin/i86bi_linux_l2-adventerprisek9-ms.SSA.high_iron_20190423.bin /opt/iou/bin/i86bi_LinuxL2-AdvEnterpriseK9-M_152_May_2018
```

> Creamos un enlace simbolico para el L2 Alternativo
```
sudo ln -s /opt/iou/bin/i86bi_linux_l2-adventerprisek9-ms.SSA.high_iron_20190423.bin /opt/iou/bin/i86bi_linux_l2-ipbasek9-ms.may8-2013-team_track
```

> Arregla los permisos
```
sudo chown apache:apache -Rh /opt/iou/bin/*
```

> [!NOTE] Verificacion de Imagenes IOU WEB
> Ahora si vamos otra vez a revisar los IOSes, vemos que ya no esta el simbolo de advertencia, cuando un laboratorio busque esa imagen, sera usada la mas nueva, permitiendo tener una uniformidad de ejecucion moderna, sin necesidad de cambiar ninguna configuracion de los laboratorios ya hechos

# Optimizar

## Instalar Guest Additions

**Virtualbox**
> [!TIP] Lecturas Recomendadas
> - [Virtualbox Download Center](https://download.virtualbox.org/virtualbox/) | [Virtualbox - Latest Release.TXT](https://download.virtualbox.org/virtualbox/LATEST.TXT)
> - [Kernel Docs - Tainted Kernel](https://www.kernel.org/doc/html/latest/admin-guide/tainted-kernels.html)

Ve a git
```
cd ~/git
```

Descarga la ultima version disponible de Guest Additions, en mi caso `7.2.4`
```
wget https://download.virtualbox.org/virtualbox/7.2.4/VBoxGuestAdditions_7.2.4.iso
```

> Monta el disco
```
sudo mount -o loop VBoxGuestAdditions_7.2.4.iso /media/Vbox
```

> Ejecuta el ./run (compilara los drivers, se demora un par de minutos
```
sudo /media/Vbox/VBoxLinuxAdditions.run
```

> [!NOTE] Sobre dmesg
> El comando de arriba compilara e instalara los modulos en el kernel para Virtualbox (vboxguest, vboxsf, vboxvideo).
> 
> Si usas virtualizacion QEMU/KVM saldran mensajes en `dmesg` indicando modulos que manchan el kernel, esto es normal y esperable, ya que son modulo de virtualbox
> ```
> [13590.031852] vboxguest: loading out-of-tree module taints kernel.
> [13590.048915] vboxguest: PCI device not found, probably running on physical hardware.
> ```

> Revisa si esta compilado
```
lsmod | grep vboxguest
```

> Desmonta el disco
```
sudo umount /media/Vbox
```

> Eliminar iso de Virtualbox
```
rm VBoxGuestAdditions_7.2.4.iso
```

**QEMU/KVM**

> Habilita QEMU Guest Agent
```
sudo systemctl enable --now qemu-guest-agent
```

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

## Deshabilitar Servicios

Para mejorar tiempos de arranque y rendimiento general del VM, podemos deshabilitar servicios que no sean necesarios. Tambien afinaremos algunos detalles de configuracion

Con `systemd-analyze` puedes ver cuanto se demora en iniciar el sistema 
```
Startup finished in 1.344s (kernel) + 3.373s (initrd) + 4.048s (userspace) = 8.766s
multi-user.target reached after 4.008s in userspace
```

Con `systemd-analyze blame` puedes ver en mas detalle cuanto se demora en iniciar cada servicio en iniciar y estar funcionando

> [!TIP]- Output systemd-analyze blame
> ```
>          1.477s rc-local.service
>          1.089s firewalld.service
>           836ms dracut-initqueue.service
>           723ms initrd-switch-root.service
>           588ms php84-php-fpm.service
>           558ms libvirtd.service
>           557ms php-fpm.service
>           555ms php74-php-fpm.service
>           431ms systemd-vconsole-setup.service
>           347ms systemd-udev-trigger.service
>           315ms var-lib-nfs-rpc_pipefs.mount
>           274ms chronyd.service
>           269ms initrd-parse-etc.service
>           202ms netcf-transaction.service
>           188ms systemd-machined.service
>           184ms user@1000.service
>           176ms systemd-logind.service
>           150ms sysroot.mount
>           134ms systemd-tmpfiles-setup.service
>           112ms httpd.service
>           107ms auditd.service
>            98ms systemd-udevd.service
>            83ms gssproxy.service
>            83ms sshd.service
>            79ms lvm2-monitor.service
>            75ms dracut-cmdline.service
>            72ms polkit.service
>            72ms systemd-tmpfiles-clean.service
>            72ms user-runtime-dir@1000.service
>            71ms dracut-pre-pivot.service
>            69ms import-state.service
>            67ms NetworkManager.service
>            65ms systemd-tmpfiles-setup-dev.service
>            64ms systemd-journald.service
>            55ms dracut-shutdown.service
>            55ms sys-kernel-debug.mount
>            52ms rpc-statd-notify.service
>            52ms systemd-fsck@dev-disk-by\x2duuid-f8bf8d2c\x2d4e70\x2d4e73\x2db45b\x2d940e8f375c0f.service
>            51ms dev-mqueue.mount
>            49ms systemd-remount-fs.service
>            40ms systemd-journal-flush.service
>            37ms kmod-static-nodes.service
>            37ms dracut-pre-udev.service
>            36ms initrd-cleanup.service
>            34ms dev-hugepages.mount
>            33ms systemd-user-sessions.service
>            32ms systemd-update-utmp.service
>            31ms systemd-fsck-root.service
>            30ms systemd-sysctl.service
>            29ms systemd-update-utmp-runlevel.service
>            25ms dracut-pre-trigger.service
>            21ms systemd-random-seed.service
>            14ms initrd-udevadm-cleanup-db.service
>            12ms boot.mount
>             5ms sys-kernel-config.mount
> ```


> Deshabilita dnf-makecache.service
```
sudo systemctl disable dnf-makecache.service
```

> Deshabilita dnf-makecache.timer
```
sudo systemctl disable dnf-makecache.timer
```

> Deshabilita kdump.service
```
sudo systemctl disable kdump.service
```

> Deshabilita rsyslog.service
```
sudo systemctl disable rsyslog.service
```

> Deshabilita DKMS
```
sudo systemctl disable dkms
```

> Deshabilita NetworkManager-wait-online
```
sudo systemctl disable NetworkManager-wait-online.service
```

> Deshabilita Tuned
```
sudo systemctl disable tuned.service
```

> Deshabilita RPCBind
```
sudo systemctl disable --now rpcbind
```

> Deshabilita RPCBind Socket
```
sudo systemctl disable --now rpcbind.socket
```

> Deshabilita sssd
```
sudo systemctl disable --now sssd.service
```

> Deshabilita nisdomainname
```
sudo systemctl disable --now nis-domainname.service
```

> Dehabilita iscsi
```
sudo systemctl disable --now iscsi.service
```

> Deshabilita iscsi socket
```
sudo systemctl disable --now iscsid.socket
```

> Deshabilita iscsiuio socket
```
sudo systemctl disable --now iscsiuio.socket
```

# Arreglar CVEs
## Instalacion Escaner

Para revisar las vulnerabilidades, utilizare Kali Linux con Nessus y OpenVAS como un escaner de seguridad, para detener una superficie de ataque rapida

Los requisitos minimos son
- CPU: 4vCPU (Talvez necesite hasta 8vcpu para OpenVAS)
- RAM: 12GB
- Storage: 130GB

Aunque tiene fallos de diseño que no seran solucionados, el mas grande es el usuario y contraseña por defecto, siendo `cisco:cisco`, y hacer un escalamiento directo al usuario `root` mediante `sudo`. Esto puede ser prevenido configurando un usuario y contraseña distintos desde una instalacion desde 0.

> [!TIP] Consejos en Kali
> 1. Cada vez que uses Kali, descarga la imagen mas nueva, preferiblemente la actualizada por semana, asi se demora menos en actualizar
> 2. Siempre actualiza los paquetes y reinicia la maquina antes de empezar a utilizarla
> 3. Kali Linux se apaga cada 5 minutos de inactividad, para solucionarlo, presiona el icono de bateria el cual abre un menu, seleccionas "Modo Presentacion" y de esa forma no se suspendera
> 4. Puedes configurar el idioma en español con `setxkbmap -layout latam`

> Expande el disco qcow2 de Kali Linux (Recuerda ajustar el nombre), para agregar 50G
```
qemu-img resize kali-linux-2025-W44-qemu-amd64.qcow2 +50G
```

> Verifica el nuevo tamaño de Kali
```
❯ qemu-img info kali-linux-2025-W44-qemu-amd64.qcow2
image: kali-linux-2025-W44-qemu-amd64.qcow2
file format: qcow2
virtual size: 120 GiB (128949672960 bytes)
disk size: 15.4 GiB
cluster_size: 65536
Format specific information:
    compat: 1.1
    compression type: zlib
    lazy refcounts: false
    refcount bits: 16
    corrupt: false
    extended l2: false
Child node '/file':
    filename: kali-linux-2025-W44-qemu-amd64.qcow2
    protocol type: file
    file length: 15.4 GiB (16525428736 bytes)
    disk size: 15.4 GiB
```

> Si tienes problemas con la actualizacion de Kali edita `/etc/apt/sources.list` y cambia `http://http.kali.org/kali` Por
```
https://elmirror.cl/kali/
```

> Actualiza el sistema
```
sudo apt update && sudo apt upgrade
```

> Expande las particiones usando cfdisk
```
cfdisk > Resize > "Enter" > Write > "yes" > Quit
```

> Aplica los cambios al sistema de ficheros
```
sudo resize2fs /dev/vda1
```

> Instalar Docker ([Metodo Kali](https://www.kali.org/docs/containers/installing-docker-on-kali/))
```
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings

sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc

sudo chmod a+r /etc/apt/keyrings/docker.asc

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian trixie stable" | sudo tee /etc/apt/sources.list.d/docker.list

curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo systemctl enable --now docker

sudo usermod -aG docker kali
```

> Edita `/etc/sysctl.conf` para evitar problemas con OpenVAS
```
vm.overcommit_memory = 1
```

> Aplica los cambios de sysctl
```
sudo sysctl -p
```

> Reinicia la maquina
```
reboot
```

> [!TIP] Sobre Nessus
> - [Tenable - Nessus Essentials](https://www.tenable.com/products/nessus/nessus-essentials) | [Tenable for Education](https://www.tenable.com/tenable-for-education)
> 	- [Tenable Download Center](https://www.tenable.com/downloads)
> 	- [Tenable Nessus - Download](https://www.tenable.com/downloads/nessus) (Confirma la ultima version, con la opcion "`Download by curl`")
> - Para el uso de Nessus, me apoye en la licencia de estudiante para [Nessus Essentials](https://www.tenable.com/products/nessus/nessus-essentials)

La instalacion toma aproximadamente 1 hora, el mayor tiempo compilando los plugings basicos

> Descarga Nessus (10.11.1) para "Linux - Ubuntu - AMD64" con curl
```
curl --request GET --url 'https://www.tenable.com/downloads/api/v2/pages/nessus/files/Nessus-10.11.1-ubuntu1604_amd64.deb' --output 'Nessus-10.11.1-ubuntu1604_amd64.deb'
```

> Instala el paquete de Nessus
```
sudo dpkg -i {nessus.deb}
```

> Inicia Nessus luego de cada inicio
```
systemctl enable --now nessusd
```

> Conectate a la IP de Kali con el puerto por defecto
```
https://{ip-kali}:8384
```

- Sigue los siguientes pasos con tu licencia
	- Presiona `Continue` SIN DARLE EL TICK EN "register offline"
	- Presiona `Register for Nessus Essentials` y dale en `Continue`
	- Rellena `First Name`, `Last Name` y `Email` con un correo institucional, si te inscribiste de la pagina web y te llego un correo con un codigo de activacion, dale en "`Skip`" (Si falla el registro tambien dale en skip)
	- Luego escibe tu clave de activacion ONLINE, que te llego al correo al iniciar sesion en Nessus
	- Crea un usuario `admin` o `kali` con alguna contraseña segura y presiona en "Submit"
	- Se empezara a compilar los plugins, es el paso final de la instalacion, se demora aproximadamente 45 minutos en completar
	- Luego que se compilen los plugins, se instalar, esto toma 5 minutos, saldra un mensaje de "Finalizo la instalacion de plugins" y podras analizar hasta 16 IPs de forma gratuita

> [!TIP] Sobre OpenVAS
> - [Github - immauss/openvas (Version que me funciono)](https://github.com/immauss/openvas)
> - [Github - greenbone/openvas-scanner](https://github.com/greenbone/openvas-scanner)
> - [Greenbone Docs - OpenVAS](https://greenbone.github.io/docs/latest/)
> 	- [Containers](https://greenbone.github.io/docs/latest/22.4/container/index.html)
> - [Github - Greenbone/greenbone-feed-sync](https://github.com/greenbone/greenbone-feed-sync/)
> 	- [Greenbone Docs - Understand Sync programs](https://greenbone.github.io/docs/latest/faq.html#i-still-fail-to-see-understand-the-concept-of-greenbone-feed-sync-type-vs-greenbone-nvt-sync-greenbone-certdata-sync-greenbone-scapdata-sync-vs-gvm-feed-update)
> - [Devto - Installing and Using OpenVAS on Kali Linux](https://dev.to/terminaltools/installing-and-using-openvas-on-kali-linux-a-complete-guide-10d3)
> - [Pronetic - Como instalar y configurar OpenVAS](https://pronetic.geeknetic.es/Guia/3134/OpenVAS-Como-instalar-y-configurar-este-Escaner-de-Vulnerabilidades-gratuito.html)

La instalacion toma aproximadamente 3 hora, el mayor tiempo descargando y configurando las fuentes

> Crea las carpetas para OpenVAS
```
mkdir ~/openvas && cd ~/openvas
```

> Descarga el `docker-compose.yml` de immauss/docker y modifica el tag de la imagen de `beta` a `latest`
```
wget https://raw.githubusercontent.com/immauss/openvas/refs/heads/master/compose/docker-compose.yml
```

> Extrae el contenido de la imagen
```
docker compose pull
```

> Levanta la imagen
```
docker compose up -d
```

> Revisa los logs para ver el estado de la instalacion, deberia demorar unos 30 minutos
```
docker compose logs -f
```

> Accede a la interfaz web
```
http://127.0.0.1:8080/login
```

**Pasos para analizar el VM**
1. "Configuration" > "Credentials" > "New Credentials"
	- Name: IOU WEB Atlas
	- Comment: Rocky Linux 8.10
	- Type: Username + Password
	- Username: cisco
	- Password: cisco
	- Presiona "Save"
2. "Administration" > "Feed Status"
	- Los feed deben estar actualizados (Current) o (Days Old), espera a que se sincronizen, se demora un buen buen rato
3. "Configuration" > "Target" > "New Target"
	- Name: IOU WEB Atlas
	- Comment: Rocky Linux 8.10
	- Hosts: Manual
		- `{ip de tu VM}`
	- Port List: "All IANA assigned TCP"
	- Alive Test: ICMP Ping
	- SSH: SSH IOU WEB Atlas
		- Elevate Privileges: Create new Credential
			- Name: cisco -> root
			- Comment: IOU WEB privileges
			- Username: root
			- Password: cisco
			- "Presiona Save"
	- Presiona "Save"
4. "Scans" > "Tasks" > "New Tasks"
	- Name: Scan
	- Comment: Scan
	- Scan Target: IOU WEB Atlas
	- Scan Config: Full and fast
	- Maximum concurrently executed NVTs per Host: 7
	- Presiona "Save"
5. "Scans" > "Tasks" > "Scan: Play"
6. El Scan se demoro aproximadamente 4 horas...

## SSH Server CBC Mode Ciphers Enabled
> [!TIP] info
> - [RedHat Customer Portal - Disabling SHA-1 HMAC and CBC Algorithms](https://access.redhat.com/solutions/7077964)
> - [NIST - CVE-2008-5161](https://nvd.nist.gov/vuln/detail/CVE-2008-5161)

> Actualiza las politicas
```
sudo dnf update -q crypto-policies -y
```

> Edita `/etc/crypto-policies/policies/modules/NO-SHA1-HMAC.pmod` y agrega
```
mac@SSH = -HMAC-SHA1
```

> Edita `/etc/crypto-policies/policies/modules/NO-CBC.pmod` y agrega
```
cipher@SSH = -AES-128-CBC -AES-256-CBC -CHACHA20* -AES-256-GCM
ssh_etm = 0
```

> Edita la politica actual agregando las politicas creadas
```
sudo update-crypto-policies --set DEFAULT:NO-SHA1:NO-SHA1-HMAC:NO-CBC
```

> Reinicia la maquina
```
sudo reboot
```

## Hardening SSH
> [!TIP] Lecturas Recomendadas
> - [Github - jtesta/ssh-audit](https://github.com/jtesta/ssh-audit)
> - [SSH Audit - SSH Hardening Guides](https://www.ssh-audit.com/hardening_guides.html)
> 	- [RHEL 8](https://www.ssh-audit.com/hardening_guides.html#rhel8)

SSH es el principal metodo de administracion que usare, es lo minimo que debe ser seguro

> Edita como admin `/etc/ssh/sshd_config` y ve a la linea 44 para modifica `PermitRootLogin` por
```
PermitRootLogin no
```

> Edita como admin `/etc/ssh/sshd_config` y ve a la linea 46 y descomenta `MaxAuthTries` y cambia a `3`
```
MaxAuthTries 3
```

> Temporalmente sube los privilegios a root
```
sudo su
```

> Remueve los modulos de menos de 3071 bits de Diffie-Hellman
```
awk '$5 >= 3071' /etc/ssh/moduli > /etc/ssh/moduli.safe
```

> Carga el nuevo archivo como el por defecto
```
mv -f /etc/ssh/moduli.safe /etc/ssh/moduli
```

> Desactiva ECDSA quitando la mediacion de la llaves para `/etc/ssh/sshd_config`
```
sudo sed -i 's/^HostKey.*ecdsa.*/#&/' /etc/ssh/sshd_config
```

> Respalda la configuracion original en caso de errores
```
sudo cp /etc/crypto-policies/back-ends/opensshserver.config /etc/crypto-policies/back-ends/opensshserver.config.orig
```

> Restringe el soporte de intercambio de llaves a los mas seguros (NOTA: *Esta cadena no es la original, esta modificada por mi para dar soporte a un sistema un poco mas antiguo*)
```
echo -e "CRYPTO_POLICY='-oCiphers=chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr -oMACs=hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,umac-128-etm@openssh.com -oKexAlgorithms=curve25519-sha256@libssh.org,curve25519-sha256,diffie-hellman-group18-sha512,diffie-hellman-group16-sha512,diffie-hellman-group14-sha256,diffie-hellman-group-exchange-sha256 -oHostKeyAlgorithms=ssh-ed25519,ssh-ed25519-cert-v01@openssh.com,rsa-sha2-512,rsa-sha2-256 -oPubkeyAcceptedKeyTypes=ssh-ed25519,ssh-ed25519-cert-v01@openssh.com,rsa-sha2-512,rsa-sha2-256'" > /etc/crypto-policies/back-ends/opensshserver.config
```

> Reinicia el servidor OpenSSH
```
systemctl restart sshd.service
```

> Quitate los privilegios de root
```
exit
```

## ICMP Timestamp Request Remote Date Disclosure
> [!TIP] Info
> - [RedHat Customer Portal - How to Block ICMP Timestamp Request](https://access.redhat.com/solutions/2441531)
> - [NIST - CVE-1999-0524](https://nvd.nist.gov/vuln/detail/CVE-1999-0524)

> Ingresa la regla
```
sudo firewall-cmd --zone=public --add-rich-rule 'rule family="ipv4" icmp-type name="timestamp-request" drop' --permanent
```

> Ingresa la regla
```
sudo firewall-cmd --zone=public --add-rich-rule 'rule family="ipv4" icmp-type name="timestamp-reply" drop' --permanent
```

> Refresca el Firewall
```
sudo firewall-cmd --reload
```

## HTTP TRACE / TRACK Methods Allowed
> [!TIP] Info
> - [Nessus - HTTP TRACE](https://www.tenable.com/plugins/nessus/11213)
> - [Nessus CVE-2003-1567](https://www.tenable.com/cve/CVE-2003-1567) | [NIST](https://nvd.nist.gov/vuln/detail/CVE-2003-1567)
> - [Nessus CVE-2004-2320](https://www.tenable.com/cve/CVE-2004-2320)
> - [Nessus CVE-2010-0386](https://www.tenable.com/cve/CVE-2010-0386) | [NIST](https://nvd.nist.gov/vuln/detail/CVE-2010-0386)

Agrega la siguiente linea al final de `/etc/httpd/conf/httpd.conf`, aproximadamente linea 358
```
TraceEnable off
```

## Missing "HttpOnly" Cookie Attribute (HTTP)
> [!TIP] Info
> [RFC-Editor - RFC6265 - HTTP State Management Mechanism - section-5.2.6](https://www.rfc-editor.org/rfc/rfc6265#section-5.2.6)

> Ve a `/etc/php.ini` y edita la linea 1295, escribiendo el valor 1
```
session.cookie_httponly = 1
```

## TCP Timestamps Information Disclosure

> Modifica `/etc/sysctl.conf` y agrega 
```
net.ipv4.tcp_timestamps = 0
```

## CVEs relacionados con PHP 8.0

> [!TIP] Lecturas Recomendadas
> - Sobre EOL
> 	- [PHP - Release 7.4.33](https://www.php.net/releases/7_4_33.php)
> 	- [PHP - Changelog 7.x](https://www.php.net/ChangeLog-7.php#7.4.33)
> 	- [PHPwatch - PHP 7.4.33 Info](https://php.watch/versions/7.4/releases/7.4.33)
> - Sobre REMI (Security Fixes)
> 	- [Remirepo Blog - PHP 7.4 is Retired](https://blog.remirepo.net/post/2022/11/29/PHP-7.4-is-retired)
> 	- [Github - remicollet/php-src-security - Branch PHP-7.4.security-backports](https://github.com/remicollet/php-src-security/tree/PHP-7.4-security-backports) | [NEWS](https://github.com/remicollet/php-src-security/blob/PHP-7.4-security-backports/NEWS)
> 	- [Remirepo - RHEL 8 php74 pkgs](https://rpms.remirepo.net/enterprise/8/php74/x86_64/)

OpenVAS realiza la deteccion de vulnerabilidades de PHP basandose unicamente en el numero de version. En caso del repositorio REMI, las versiones incluyen backports de seguridad, por lo que la instalacion actual (`7.4.33-24` del `03-Jul-2025`) ya contiene las mitigaciones correspondientes. Por este motivo, los siguientes CVE y NVT reportados por OpenVAS pueden considerarse falsos positivos y omitirse, ya que las mitigaciones se encuentran integradas en la rama de mantenimiento.

A continuacion se listan las vulnerabilidades detectadas junto con la referencia al archivo `NEWS` donde se confirma la aplicacion del parche respectivo:
- NVT: Moderate: php:8.0 security, bug fix, and enhancement update (RLSA-2022:7624)
	- [NIST - CVE-2021-21708](https://nvd.nist.gov/vuln/detail/CVE-2021-21708) | [NEWS - Linea 178](https://github.com/remicollet/php-src-security/blob/d52bcc1e66edd421dfea1698b1f897ad26c5f15f/NEWS#L178)
	- [NIST - CVE-2022-31625](https://nvd.nist.gov/vuln/detail/CVE-2022-31625) | [NEWS - Linea 163](https://github.com/remicollet/php-src-security/blob/d52bcc1e66edd421dfea1698b1f897ad26c5f15f/NEWS#L163)
- NVT: Important: php:8.0 security update ([RLSA-2022:5468](https://access.redhat.com/errata/RHSA-2022:5468))
	- CVE-2022-31626 | [NEWS - Linea 158](https://github.com/remicollet/php-src-security/blob/d52bcc1e66edd421dfea1698b1f897ad26c5f15f/NEWS#L158)
- NVT: Moderate: php:8.0 security update ([RLSA-2023:0848](https://access.redhat.com/errata/RHSA-2023:0848))
	- CVE-2022-31628 | [NEWS - Linea 151](https://github.com/remicollet/php-src-security/blob/d52bcc1e66edd421dfea1698b1f897ad26c5f15f/NEWS#L151)
	- CVE-2022-31629 | [NEWS - Linea 152](https://github.com/remicollet/php-src-security/blob/d52bcc1e66edd421dfea1698b1f897ad26c5f15f/NEWS#L152)
	- CVE-2022-31630 | [NEWS - Linea 141](https://github.com/remicollet/php-src-security/blob/d52bcc1e66edd421dfea1698b1f897ad26c5f15f/NEWS#L141)
	- CVE-2022-31631 | [NEWS - Linea 135](https://github.com/remicollet/php-src-security/blob/d52bcc1e66edd421dfea1698b1f897ad26c5f15f/NEWS#L135)
	- CVE-2022-37454 | [NEWS - Linea 145](https://github.com/remicollet/php-src-security/blob/d52bcc1e66edd421dfea1698b1f897ad26c5f15f/NEWS#L145)

# Limpieza

Desde la Interfaz de IOU WEB
1. En la pestaña "`Laboratorios`", elimina todos los archivos
2. En la pestaña "`Manage`", selecciona "`Optimize Database`"
3. En la pestaña "`Downloads`" elige "`Clear session and delete sniffer/import/export/logs files`" y confirma con "`Yes, delete all`"

*(Esto reinicia el VM; cuando levante, recarga la paginas)*

> Revisa el espacio utilizado con duf
```
duf --style ascii -only local
```

> Revisa archivos grandes con
```
ncdu /
```

> Detener httpd
```
sudo systemctl stop httpd
```

> Elimina Base de datos viejas
```
sudo rm -f /opt/iou/data/database.sdb-*
```

> Permiso Root Temporal
```
sudo su
```

> Limpia Logs de IOU WEB
```
echo "" > /opt/iou/data/Logs/access.txt
```

> Limpia Logs de error
```
echo "" > /opt/iou/data/Logs/error.txt
```

> Quita los permisos
```
exit
```

> Limpiar DNF
```
sudo dnf clean all
```

> Elimina archivos del usuario root y archivos temporales e IP por DHCP
```
sudo rm -drf ~/git/ ~/iou/ /var/cache/yum/ /tmp/* /var/tmp/* /var/lib/dhclient/dhclient.leases
```

> Limpiar todos los logs
```
sudo find /var/log -type f -exec truncate -s 0 {} \;
```

> Limpia el machine-id
```
sudo truncate -s 0 /etc/machine-id
```

> Elimina llaves SSH para ser regeneradas en cada sistema
```
sudo rm -drf /etc/ssh/ssh_host_* /root/.ssh /home/*/.ssh
```

> Borra los certificados auto-firmados
```
sudo rm -f /etc/pki/tls/iou/iou*
```

> Limpia los espacios libres (Reemplaza a dd zerofile) (Solo si tienes nVME en el host y usas Qemu)
```
sudo fstrim -av
```

> Borra el historial, Desconecta el historial, sincroniza los discos a la fuerza y apaga la maquina para exportar el producto hecho
```
sudo history -wc && sudo unset HISTFILE && sudo sync; sudo sync && sudo poweroff
```

# Exportar

## Qemu

Ahora desde mi Host Linux, vamos a exportar el disco qcow2 para poder aprovecharlo en otros sistemas, primero de comprime y luego se exporta

**Comprimir Imagen**
> Nos vamos a la carpeta donde esta los discos .qcow2, en mi caso
```
cd /var/lib/libvirt/images
```

> Convierte el disco QCOW2 en QCOW2 pero comprimido (en 4 minutos, pasa de 21GB, con 4.3GB utilizados a tan solo pesar 1.9GB)
```
sudo qemu-img convert -f qcow2 -O qcow2 -c IOU-WEB-Atlas.qcow2 IOU-WEB-Atlas-chikita.qcow2
```

> Valida la integridad de la maquina comprimida
```
qemu-img check IOU-WEB-Atlas-chikita.qcow2
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

## Virtualbox

**Imagen QEMU a Virtualbox**

> Convertir a VDI, no soporta compresion, por lo que sera mas pesado (4.2Gb)
```
qemu-img convert -f qcow2 IOU-WEB-Icarus.qcow2 -O vdi IOU-WEB-Icarus.vdi
```

> Comprimir VDI con Virtualbox Set
```
VBoxManage modifymedium --compact IOU-WEB-Icarus.vdi
```



# Extra

## Creacion de llaves seguras

Durante el [[#Hardening SSH]], puede parecer logico generar llaves para asegurarse que estan configuradas correctamente y con permisos restrictivos. Sin embargo, en este entorno de maquinas virtuales que se replican en base a este write-up, no conviene para nada dejar llaves persistentes

Si las llaves permanecen iguales en todas las copias del sistema, cada VM tendria la misma huella SSH, lo que representa un riesgo grave de seguridad, cualquier atacante que obtenga una llave privada de un host podria hacerse pasar por cualquiera de las demas maquina clonadas

Por eso, antes de apagar la maquina base para exportarla, se eliminan las llaves del sistema. Luego, cuando el sistema arranca por primera vez, el servicio `sshd` genera automaticamente nuevas llaves `RSA` y `ED25519`, tal como esta configurado en `sshd_config` con los parametros `HostKey`.

Esto garantiza que cada host tenga su propia identidad criptografica unica, lo que evita colisiones y vulnerabilidades de clonacion. Puedes generar llaves seguras manualmente como se recomienda en la guia original de Hardening o simplemente generar las llaves por cuenta propia, estos son los pasos

> Borra previamente creadas
```
sudo rm -f /etc/ssh/ssh_host_*
```

> Genera llaves ED25519
```
sudo ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key -N ""
```

> Genera llaves RSA
```
sudo ssh-keygen -t rsa -b 4096 -f /etc/ssh/ssh_host_rsa_key -N ""
```

> Cambia el grupo en las llaves
```
chgrp ssh_keys /etc/ssh/ssh_host_ed25519_key /etc/ssh/ssh_host_rsa_key
```

> Cambia el permisos para las llaves
```
chmod g+r /etc/ssh/ssh_host_ed25519_key /etc/ssh/ssh_host_rsa_key
```

## Compilar Libcrypto

> [!TIP] Sobre OpenSSL
> - [OpenSSL Roadmap](https://www.openssl-library.org/roadmap/index.html)
> - OpenSSL Vulnerabilities
> 	- [Branch 0.9.8](https://www.openssl-library.org/news/vulnerabilities-0.9.8/)
> 	- [Branch 1.0.2](https://www.openssl-library.org/news/vulnerabilities-1.0.2/)
> - [Mail Archive - OpenSSL 0.9.8o release](https://www.mail-archive.com/openssl-announce%40openssl.org/msg00089.html)
> - [OpenSSL Wiki - Compilation And Installation](https://wiki.openssl.org/index.php/Compilation_and_Installation)
> - [Github - openssl/openssl](https://github.com/openssl/openssl)
> 	- [NOTES-UNIX.md](https://github.com/openssl/openssl/blob/master/NOTES-UNIX.md)
> 	- [Install.md](https://github.com/openssl/openssl/blob/master/INSTALL.md)

> [!IMPORTANT] Entorno Compilacion
> Yo utilize un entorno de 32 bits basado en [[800 - Extras/Write-Ups/IOU WEB/IOU WEB - 32 Bits - CentOS 6 (Legacy)|IOU WEB - 32 Bits - CentOS 6 (Legacy)]]. No he probado compilar en un entorno multilib de 64 bits
> > [!TIP] Entorno en 32 bits
> > Automaticamente, al ejecutar `./config`, selecciona `linux-elf` para su compilacion

Actualmente esta basado en OpenSSL 1.0.2zq (14 Junio 2026)

> Ve a la carpeta Git
```
cd ~/git
```

> Copias el repositorio y abres la carpeta
```
git clone https://github.com/alsyundawy/openssl-1.0.2.git && cd openssl-1.0.2/
```

> Configura el entorno de compilacion para 32 bits
```
./Configure linux-elf -m32 shared
```

> Compilas la version (Toma unos 3 minutos)
```
make -j$(nproc)
```

> Verifica la compilacion
```
file libcrypto.so.1.0.0
```

> Output file
```
libcrypto.so.1.0.0: ELF 32-bit LSB shared object, Intel 80386, version 1 (SYSV), dynamically linked, BuildID[sha1]=fd9dd54da65f776049bcb48298ca2c6ef91a4cdb, not stripped
```

> Verificacion SHA256: `aff72163a2ec580ee085b5ce49f8aa9640256f339ab778b3e7bbe0fcfce177e2`
```
sha256sum libcrypto.so.1.0.0
```

> Verifica las librerias
```
ldd libcrypto.so.1.0.0
```

> Output Librerias
```
linux-gate.so.1 (0xf7ed4000)
libdl.so.2 => /lib/libdl.so.2 (0xf7ca6000)
libc.so.6 => /lib/libc.so.6 (0xf7afe000)
/lib/ld-linux.so.2 (0xf7ed6000)
```

> Renombra el archivo
```
mv libcrypto.so.1.0.0 libcrypto.so.4
```

> Metelo a la libreria del sistema
```
sudo mv libcrypto.so.4 /usr/lib/libcrypto.so.4
```

El archivo `libcrypto.so.1.0.0` estara listo para copiar al VM siguiendo los pasos de instalacion

## Entorno de Ejecucion

Esta informacion se extrajo del sistema el `01-Nov-2025`

> PHP version
```
[root@iou iou]# php -v
PHP 7.4.33 (cli) (built: Jul  3 2025 13:43:34) ( NTS )
Copyright (c) The PHP Group
Zend Engine v3.4.0, Copyright (c) Zend Technologies
    with Zend OPcache v7.4.33, Copyright (c), by Zend Technologies
```

> GCC Version
```
[root@iou ~]# gcc --version
gcc (GCC) 8.5.0 20210514 (Red Hat 8.5.0-28)
Copyright (C) 2018 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

> LDD Version
```
[cisco@iou ~]$ ldd --version
ldd (GNU libc) 2.28
Copyright (C) 2018 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
Written by Roland McGrath and Ulrich Drepper.
```

> PHP Modules
```
[cisco@iou ~]$ php -m
[PHP Modules]
bz2
calendar
Core
crypto
ctype
curl
date
dom
exif
fileinfo
filter
ftp
gd
gettext
hash
iconv
intl
json
libxml
mbstring
mysqli
mysqlnd
openssl
pcntl
pcre
PDO
pdo_mysql
pdo_sqlite
Phar
posix
readline
Reflection
session
shmop
SimpleXML
sockets
sodium
SPL
sqlite3
standard
sysvmsg
sysvsem
sysvshm
tokenizer
xml
xmlreader
xmlwriter
xsl
Zend OPcache
zip
zlib

[Zend Modules]
Zend OPcache
```

> PHP INI
```
[cisco@iou ~]$ php --ini
Configuration File (php.ini) Path: /etc
Loaded Configuration File:         /etc/php.ini
Scan for additional .ini files in: /etc/php.d
Additional .ini files parsed:      /etc/php.d/10-opcache.ini,
/etc/php.d/20-bz2.ini,
/etc/php.d/20-calendar.ini,
/etc/php.d/20-ctype.ini,
/etc/php.d/20-curl.ini,
/etc/php.d/20-dom.ini,
/etc/php.d/20-exif.ini,
/etc/php.d/20-fileinfo.ini,
/etc/php.d/20-ftp.ini,
/etc/php.d/20-gd.ini,
/etc/php.d/20-gettext.ini,
/etc/php.d/20-iconv.ini,
/etc/php.d/20-intl.ini,
/etc/php.d/20-json.ini,
/etc/php.d/20-mbstring.ini,
/etc/php.d/20-mysqlnd.ini,
/etc/php.d/20-pdo.ini,
/etc/php.d/20-phar.ini,
/etc/php.d/20-posix.ini,
/etc/php.d/20-shmop.ini,
/etc/php.d/20-simplexml.ini,
/etc/php.d/20-sockets.ini,
/etc/php.d/20-sodium.ini,
/etc/php.d/20-sqlite3.ini,
/etc/php.d/20-sysvmsg.ini,
/etc/php.d/20-sysvsem.ini,
/etc/php.d/20-sysvshm.ini,
/etc/php.d/20-tokenizer.ini,
/etc/php.d/20-xml.ini,
/etc/php.d/20-xmlwriter.ini,
/etc/php.d/20-xsl.ini,
/etc/php.d/30-mysqli.ini,
/etc/php.d/30-pdo_mysql.ini,
/etc/php.d/30-pdo_sqlite.ini,
/etc/php.d/30-xmlreader.ini,
/etc/php.d/30-zip.ini,
/etc/php.d/40-crypto.ini
```

> Paquetes de HTTPD
```
[root@iou iou-web]# rpm -qa | grep httpd
httpd-tools-2.4.37-65.module+el8.10.0+2061+8d03fdec.5.x86_64
httpd-filesystem-2.4.37-65.module+el8.10.0+2061+8d03fdec.5.noarch
httpd-2.4.37-65.module+el8.10.0+2061+8d03fdec.5.x86_64
```

# GRACIAS
A Todas las personas que han hecho esto posible ^^



# Deprecated

## Limpiar configuracion Apache

ESTAS LINEAS NO SON IMPORTANTES; DEBIDO A QUE iou.conf LAS SOBREESCRIBE, VEAMOS QUE TAL, O TALVEZ LO MEJOR SEA COMENTARLAS

> Edita `/etc/httpd/conf/httpd.conf`, busca la linea `DocumentRoot "/var/www/html"` (Linea 122) y cambiala por:
```
DocumentRoot "/opt/iou/html"
```

> Edita `/etc/httpd/conf/httpd.conf`, busca `<Directory "/var/www"` (Linea 127-128)
```
<Directory "/opt/iou/html">
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
```

> Edita `/etc/httpd/conf/httpd.conf`, busca "`<Directory "/var/www/cgi-bin">`" (Linea 259-263)
```
<Directory "/opt/iou/cgi-bin">
    AllowOverride All|
    Options None
    Require all granted
</Directory>
```

> Edita `/etc/httpd/conf/httpd.conf`, busca `<Directory "/var/www/html"` y comenza las lineas de esa seccion
```
135   │ #<Directory "/var/www/html">
148   │ #    Options Indexes FollowSymLinks
155   │ #    AllowOverride None
160   │ #    Require all granted
161   │ #</Directory>
```

## Probar imagenes

> Nota: El puerto usado es el "2222" y el ID de app es el 200
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
