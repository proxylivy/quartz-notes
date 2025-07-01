# Info
## Que Mejoras Tienes?
Version 1 by Proxylivy:
- User: devasc
- Pass: devasc

Mejoras:
- Instalacion Limpia desde la maquina OVA original de Cisco
- Maquina Optimizada para funcionar en Virtualbox, Activando Para-virtualizacion(KVM), virtualbox-guest-image y driver red virtio
- Sistema actualizado de Ubuntu 20.04 LTS a 24.04 LTS
- Todos los paquetes han sido actualizados, Kernel `6.8.12`
- Python 3.8 y sus dependencias instaladas para funcionar correctamente en la libreria con venv
- Hora configurada correctamente a America/Santiago y con NTP
- Eliminado SNAPD y reemplazado por aplicaciones nativas (Mejor funcionamiento)
- Actualizado VSCode, Postman, Firefox, Chrome
- Espacio Disco Ajustado: 45GB (Usado 16GB)
- Modo oscuro activado

## Notas
- Recuerda activar la virtualizacion en nido
```
VBoxManage modifyvm "Virtual-Machine-Name" --nested-hw-virt on
```

- Verifica las instrucciones de la cpu
```
grep -E 'vmx|svm' /proc/cpuinfo
```

- Ver version de kernel
```
cat /proc/version_signature
```


# Configuracion
## Instalacion desde 0
1. Descargar la imagen original sin ninguna modificacion desde el portal [Legacy de Netacad](https://legacy.netacad.com/portal/node/4036)
2. Importa el .ova a Virtualbox, cambia la interfaz a "Puente" o "Host-Only" con modo promiscuo permitido
3. Pon el driver del adaptador de red "virtio-net"
4. Ve a "Administracion de Discos" y selecciona el disco "devasc" y sube el tamaño del disco de 31.5GB a 45GB
5. Enciende la maquina de manera normal
6. Acepta el contrato de uso por parte de cisco
7. Redimensiona la ventana para que se ajuste la ventana automaticamente a tu pantalla del VM
8. Ejecuta `apt update` y luego `apt upgrade`
9. Reinicia la maquina con `reboot`
10. Ejecuta `setxkbmap latam`
11. Ejecuta `sudo su`, luego `passwd devasc` y elige una contraseña para ese usuario
12. Habilita SSH con `sudo systemctl enable --now ssh`
13. Dentro de la sesion SSH ejecuta `export TERM=xterm-256color`
14. Abre "`Language Support`" y cambia el idioma de ingles a español
15. Abre "`Keyboard Preferences`", ve a "`layout`", selecciona "`Add`" y agrega "`Country:Chile`" y "`Variant:Spanish(Latin America)`", luego remueve "`English(US)`".
16. Selecciona el idioma español de chile con `sudo localectl set-locale LANG=es_CL.UTF-8`
17. Modifica `/etc/locale.alias` y deja comentado todas las entradas
18. Modifica `/etc/locale.gen` y solo deja descomentado `es_CL-UTF8.utf8`
19. (Talvez sea tu ultimo paso) Desactiva el bloqueo de fondo de pantalla desde "Control Center" > "Screensaver" > "Move to 2 Hour and deactivate Activate screen saver and Lock Screen when screensaver is active"
20. Desactiva las opciones de optimizacion de bateria desde "Control Center" > "Power Manager" > "Put in Never every option"

## Haz una copia de la libreria

> Instalar Paquetes especificos de Python 3.8
```
sudo apt install python3.8 python3.8-venv python3.8-distutils python3-pip iptables
```

> Modificar Permisos de la libreria
```
chmod 755 -R /opt/API-SIMULATOR
chown devasc:devasc -R /opt/API-SIMULATOR
```

> Copiar exactamente los elementos de PIP para despues
```
source /opt/API-SIMULATOR/venv/bin/activate
pip freeze > requeriments2.txt
deactivate
```

> Crear copia offline de la libreria para tener un respaldo
```
tar czvf api-simulator-backup.tar.gz /opt/API-SIMULATOR
```

> Mover venv a venv-old para tener un respaldo
```
mv venv/ venv-old
```

> Crear nuevo venv basado en python 3.8 exclusivamente
```
python3.8 -m venv venv
```

> Activar nuevo venv
```
source venv/bin/activate
```

> Instalar paquetes basicos de pip
```
pip install --upgrade pip setuptools wheel
```

> Instalar requerimientos del sistema de respaldo
```
pip install -r requeriments2.txt
```

> Desactivar venv
```
deactivate
```

> Opcional: Dar permisos a devasc para usar iptables
```
sudo chgrp devasc /usr/sbin/iptables
sudo chmod u+rxs,o= /usr/sbin/iptables
```

## Elimina SNAPD de raiz

[Guide](https://www.kevin-custer.com/blog/disabling-snaps-in-ubuntu-20-10-and-20-04-lts/)

> Lista los paquetes de Snap para eliminarlos 1 a 1
```
sudo snap list
```

> Elimina Chromium, Postman, Drawio, Cups, Core, Core20, Gnome3, Gnome42, Core22, GTK-Common-themes, Bare, Core18
```
sudo snap remove --purge chromium postman drawio cups core core20 gnome-3-28-1804 gnome-42-2204 core22 gtk-common-themes bare core18
```

> Elimina Snapd
```
sudo snap remove --purge snapd
```

> Elimina los paquetes instalados desde Snap
```
sudo apt purge snapd
```

> Limpia APT
```
sudo apt clean
```

> Haz que Snapd no se instale como una actualizacion
```
sudo apt-mark hold snapd
```

> Elimina Archivos residuales de Snap
```
rm -rf ~/snap
sudo rm -rf /snap
sudo rm -rf /var/snap
sudo rm -rf /var/lib/snapd
```

> Bloquea la instalacion de snap con el archivo `/etc/apt/preferences.d/nosnap.pref`
```
Package: snapd
Pin: release a=*
Pin-Priority: -10
```

> Bloquea tambien de forma extra con el archivo `/etc/apt/apt.conf.d/99snap`
```
APT::Get::Always-Include-Phased-Updates "false";
APT::Install-Suggests "false";
APT::Install-Recommends "false";
```

## Actualiza la version de Ubuntu
- [Fuente](https://documentation.ubuntu.com/server/how-to/software/upgrade-your-release/index.html)

El comando `do-release-upgrade` buscara una nueva version del sistema operativo Ubuntu que esta corriendo, hara una tabla resumen con los nuevos cambios, actualmente estas en la version "20.04 LTS", ejecutar el comando por primera vez te instalara la version "22.04.5 LTS" y luego a la version "24.04 LTS"

Estas descargas se demoran entre 10-30 minutos, todo depende de la velocidad de internet que tengas, hay momentos donde se queda pegado pero es normal, solo esta calculando cosas.

> [!IMPORTANT] Nota
> Si en algun momento te dice un mensaje "`Commands detached from windows`", te pedira activar la ventana y apretar "`r`", empezara otra vez, ni idea porque sucede, pero lo peor que hace es enlentecer la actualizacion


1. Siempre asegurate de actualizar el sistema con `sudo apt update` y `sudo apt upgrade`
2. Ejecutas `do-release-upgrade`
3. Escribes `y` y luego presionas `enter`
4. Empezara a descargar los nuevos paquetes para el sistema, para luego instalarlos
5. En medio de las instalaciones, te pedira modificar archivos de configuracion, simplemente apreta "`O`" para denegar todos los cambios
6. Luego que termine de instalar todos los paquetes, te pedira remover los paquetes que no tienen soporte, ES IMPORTANTE QUE LE DES A ESA OPCION "NO" con "`n`"
7. Ocasionalmente te pedira reiniciar sockets (Dockers, etc) y solo le das que si a todos los servicios que se reinician
8. Grub te pedira instalarse para poder actualizarse, es necesario instalarlo en `/dev/sda` solamente
9. Dice que debes reiniciar la maquina para terminar con la actualizacion, le dices `y`
10. Cuando la maquina termine de reiniciar, te dira que hay una actualizacion pendiente, cierra todas las pestañas
11. Abre una ventana del navegador chrome y verifica que "`library.demo.local`" continua funcionando
12. Abre una nueva terminal y vuelve al paso 1 hasta llegar a la version "24.04 LTS"
13. Cuando encienda, estaras en Ubuntu Mate 24.04 LTS. "!FELICIDADES!"
14. Ejecuta `sudo apt update`, `sudo apt upgrade` y `sudo apt autoremove`

## Instalar y Actualizar Paquetes
> Extra del Sistema
```
sudo apt install linux-base virtualbox-guest-additions-iso virtualbox-guest-utils virtualbox-guest-x11 autofs mom libvirt0 libvirt-clients resource-agents-base xinetd micro git tree xclip xsel neofetch kitty iperf3 mousepad wget gpg apt-transport-https btop duf fish
```

> **Instala CODE** - [Fuente](https://code.visualstudio.com/docs/setup/linux)

> Descarga las llaves gpg de Microsoft
```
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
```

> Instala y verifica las llaves gpg de Microsoft
```
sudo install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg
```

> Añade los repositorios basados en la llave gpg
```
echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" | sudo tee /etc/apt/sources.list.d/vscode.list > /dev/null
```

> Elimina las llaves locales de gpg
```
rm -f packages.microsoft.gpg
```

> Actualiza la base de datos e instala code
```
sudo apt update && sudo apt install code
```

> **Extensiones de VSCode**
- Python - [Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
- Python Debugger - [Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-python.debugpy)
- Postman VsCode - [Marketplace](https://marketplace.visualstudio.com/items?itemName=Postman.postman-for-vscode) / [Docs](https://learning.postman.com/docs/getting-started/basics/about-vs-code-extension/)

> **Instala Postman 11.45.5** - [Fuente](https://learning.postman.com/docs/getting-started/installation/installation-and-updates/#install-postman-on-linux) - [Release Notes](https://www.postman.com/release-notes/postman-app/) - [Guide](https://gist.github.com/prrao87/114933c4638a4f77aa3d4b2c5a3b2477) (Se demora como 1 minuto y medio en abril, se podra abrir mas rapido??) (Talvez tambien activando la aceleracion por hardware desde el menu "Help")

> Descarga el Tarball
```
wget https://dl.pstmn.io/download/latest/linux64 -O postman.tar.gz
```

> Extrae el Tarball
```
sudo tar -xzf postman.tar.gz -C /opt
```

> Haz un Link simbolico para el binario
```
sudo ln -s /opt/Postman/Postman /usr/bin/postman
```

> Elimina el Tarball
```
rm postman.tar.gz
```

> Crea un launcher de escritorio en la ruta `/usr/share/applications/postman.desktop`
```
[Desktop Entry]
Type=Application
Name=Postman
Icon=/opt/Postman/app/resources/app/assets/icon.png
Exec="/opt/Postman/Postman"
Comment=Postman Desktop App
Categories=Development;Code;
```

> Soluciona un posible error de lentitud - [Fuente](https://github.com/flathub/com.getpostman.Postman/issues/237#issuecomment-1737689928)
```
sudo openssl req -subj '/C=US/CN=Postman Proxy' -new -newkey rsa:2048 -sha256 -days 365 -nodes -x509 -keyout ~/.var/app/com.getpostman.Postman/config/Postman/proxy/postman-proxy-ca.key -out ~/.var/app/com.getpostman.Postman/config/Postman/proxy/postman-proxy-ca.crt
```


> **Instala Chromium-Browser** - [Fuente](https://askubuntu.com/questions/1204571/how-to-install-chromium-without-snap/1511695#1511695)

> Agrega el repositorio xtradeb
```
sudo add-apt-repository ppa:xtradeb/apps -y
```

> Actualiza la base de datos e instala chromium
```
sudo apt update && sudo apt install chromium chromium-sandbox
```

> **Instala Firefox** - [Fuente](https://support.mozilla.org/en-US/kb/install-firefox-linux)

> Instala las llaves dentro de los keyrings
```
sudo install -d -m 0755 /etc/apt/keyrings
```

> Descarga las llaves gpg de Mozilla
```
wget -q https://packages.mozilla.org/apt/repo-signing-key.gpg -O- | sudo tee /etc/apt/keyrings/packages.mozilla.org.asc > /dev/null
```

> Verififa las firmas gpg localmente
```
gpg -n -q --import --import-options import-show /etc/apt/keyrings/packages.mozilla.org.asc | awk '/pub/{getline; gsub(/^ +| +$/,""); if($0 == "35BAA0B33E9EB396F59CA838C0BA5CE6DC6315A3") print "\nThe key fingerprint matches ("$0").\n"; else print "\nVerification failed: the fingerprint ("$0") does not match the expected one.\n"}'
```

> Añade el repositorio de firefox basado en las llaves
```
echo "deb [signed-by=/etc/apt/keyrings/packages.mozilla.org.asc] https://packages.mozilla.org/apt mozilla main" | sudo tee -a /etc/apt/sources.list.d/mozilla.list > /dev/null
```

> Actualiza la base de datos e instala firefox
```
sudo apt update && sudo apt install firefox 
```

> **Instala Librewolf** - [Fuente](https://librewolf.net/installation/debian/)

> Añade el repositorio extrepo
```
sudo apt update && sudo apt install extrepo -y
```

> Activa el paquete Librewolf desde extrepo
```
sudo extrepo enable librewolf
```

> Actualiza el repo e instala Librewolf
```
sudo apt update && sudo apt install librewolf -y
```

> **Instala Nerd Fonts (Hack)** - [Fuente](https://cloudenv.io/how-to-install-nerd-fonts-on-ubuntu-linux/)

> Descarga la ultima version de las fuentes en el formato que mas te agrade
```
wget https://github.com/ryanoasis/nerd-fonts/releases/download/v3.3.0/Hack.zip
```

> Descomprime la fuente
```
unzip Hack.zip
```

> Crea la carpeta de las fuentes
```
sudo mkdir /usr/share/fonts/truetype/Hack-Nerd-Fonts
```

> Mueve todas las fuentes ttf a esa carpeta
```
sudo mv *.ttf /usr/share/fonts/truetype/Hack-nerd-Fonts
```

> Actualiza el cache de fuentes del sistema
```
fc-cache -fv
```

> **Instala Packet Tracer** :o

> Desinstala la antigua version de packettracer
```
sudo apt remove --purge packettracer
```

> Arregla unos problemitas de dependencias
```
sudo apt --fix-broken install
```

- Descarga Packet Tracer 8.2.2 desde [Netacad](https://www.netacad.com/es/resources/lab-downloads) ingresando tu cuenta estudiantil

> Instala Packet tracer .deb
```
sudo dpkg -i Packet_Tracer822_amd64_signed.deb 
```

- Lee y acepta el contrato de uso


> **Descarga DRAWIO**

> Descarga la ultima version de Drawio desde su [github](https://github.com/jgraph/drawio)
```
curl -s https://api.github.com/repos/jgraph/drawio-desktop/releases/latest | grep browser_download_url | grep 'amd64' | cut -d '"' -f 4 | wget -i -
```

> Instala Drawio
```
sudo dpkg -i drawio-amd64-26.2.15.deb
```

## Configurar Hora
> Haz un enlace a la zona local
```
sudo ln -sf /usr/share/zoneinfo/America/Santiago /etc/localtime
```

> Elije la zona desde timedatectl
```
sudo timedatectl set-timezone America/Santiago
```

> Habilita NTP
```
sudo timedatectl set-ntp true
```

**Actualiza Ansible Correctamente**
> Remueve la version instalada
```
sudo apt remove ansible
```

> Actualiza los repositorios
```
sudo apt update
```

> Instala el siguiente paquete
```
sudo apt install software-properties-common
```

> Instala y actualiza el repositorio
```
sudo apt-add-repository --yes --update ppa:ansible/ansible
```

> Instala Ansible correctamente
```
sudo apt install ansible
```


# Optimizacion del sistema
## Systemctl
> Desabilitar Apport - Crash Report
```
sudo systemctl disable apport.service
```

> Desabilitar Whoopsie - Crash Report
```
sudo systemctl disable whoopsie.service
```

> Desabilitar Avahi
```
sudo systemctl disable avahi-daemon avahi-daemon.socket
```

> Desabilitar Actualizador de Firmware
```
sudo systemctl disable fwupd
```

> Desabilitar SSD
```
sudo systemctl disable sssd
```

> Desabilitar APT-Daily
```
sudo systemctl disable apt-daily.service apt-daily.timer
```

> Desabilitar APT-daily-Upgrade
```
sudo systemctl disable apt-daily-upgrade.service apt-daily-upgrade.timer
```

> Desabilitar Vboxadd (Temporal)
```
sudo systemctl disable vboxadd.service
```

> Desabilitar Rsyslog
```
sudo systemctl disable rsyslog.service
```

> Desabilititar GPU-MANAGER
```
sudo systemctl disable gpu-manager.service
```

> Desabilitar MOTS news timer
```
sudo systemctl disable motd-news.timer
```

> Enmascarar MOTD news
```
sudo systemctl mask motd-news.service
```

## Bajar el Timer de Network-Online-Config
De 2:30 Minutos a 10 segundos

> Crea las carpetas necesarias
```
sudo mkdir -p /etc/systemd/system/systemd-networkd-wait-online.service.d
```

> Crea el archivo de sobreescritura en `/etc/systemd/system/systemd-networkd-wait-online.service.d/override.conf`
```
[Service]
ExecStart=
ExecStart=/lib/systemd/systemd-networkd-wait-online --timeout=10
```

> Actualiza los daemon
```
sudo systemctl daemon-reload
```

> Vuelve a ejecutar los daemon
```
sudo systemctl daemon-reexec
```

## Delete Packages

> Delete extra packages
```
sudo apt remove --purge apport whoopsie cups cups-* avahi-daemon fwupd ubuntu-advantage-tools popularity-contest sssd-*
```

> Autoremover Paquetes
```
sudo apt autoremove --purge
```

## Limpieza

> Borrar Archivos Temporales
```
sudo rm -drf /tmp/
```

> Borrar mas archivos temporales
```
sudo rm -drf /var/tmp/
```

> Poner todos los logs en 0
```
sudo find /var/log -type f -exec truncate -s 0 {} \;
```

> Limpiar espacio vacio en real 0
```
sudo dd if=/dev/zero of=/zerofile bs=1M
```

> Eliminar el archivo 0
```
sudo rm -f /zerofile
```

> Vaciar Historial
```
echo "" > ~/.bash_hostory
history -wc
```

## Configuracion de Virtualbox con la maquina
General
- Basico
	- Ubuntu (64 Bits)
- Avanzado
	- Portapapeles Compartido: Inhabilitado
	- Arrastrar y Soltar: Inhabilitado
Sistema
- Placa Base
	- Memoria Base
		- Ram: 4096MB
		- Chipset: ICH9
		- Dispositivo Apuntador: Tableta USB
		- Caracteristicas Extendidas
			- Habilitar I/O APIC
			- Habilitar reloj hardware en tiempo UTC
	- Procesador
		- Procesadores: Todos los que puedas
		- Habilitar PAE/NX
		- Habilitar VT-x/AMD-V Anidado
	- Aceleracion
		- Interfaz de paravirtualizacion: KVM
		- Hardware de virtualizacion: Habilitar paginacion anidada
Pantalla
- Pantalla
	- Memoria de Video: 128MB o 256MB
	- Controlador Grafico: VMSVGA
	- Caracteristicas extendidas: Habilitar Aceleracion 3D
Almacenamiento
- Dispositivos (Controlador)
	- Nombre: SATA
	- Tipo: AHCI
	- Cantidad de Puertos: 2
	- Usar cache de I/O anfitrion
- Disco (DEVASC-LABVM-disk001.vdi)
	- Unidad de estado solido (Solo si tienes SSD, HDD desmarcar)
Red
- Adaptador 1
	- Conectado a: Adaptador puente
	- Nombre: "Tu NIC de red"
	- Tipo de adaptador: virtio-net
	- Modo promiscuo: Permitir todo
	- Direccion MAC: "Aleatoria"
USB
- Habilitar controlador USB
	- Controlar USB 2.0 (OHCI + EHCI)