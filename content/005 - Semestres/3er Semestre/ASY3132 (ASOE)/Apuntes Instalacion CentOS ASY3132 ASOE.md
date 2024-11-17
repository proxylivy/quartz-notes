[Dotfiles Livy Arch](https://github.com/proxylivy/dotfiles-proxylivy)
Instalar [CentOS](https://www.centos.org/)
Descargar e instalar [CentOS minimal](http://isoredirect.centos.org/centos/7/isos/x86_64/) -> [Tutorial](https://comoinstalar.me/como-instalar-centos-7-en-virtualbox/) / `extras/Adjuntos/1_1_3_Instalacion_de_RedHat_Enterprise_Linux.pdf|Tutorial_Clase.pdf`
NOTA: En el primer reincio aparecera solo la consola, para el ejemplo es dafne:dafne
Revisar internet con "`ping -c 1 google.cl`"
conectar a wifi(Nota: eth se conecta automaticamente):
```
nmcli r wifi on
nmcli d wifi list
nmcli d wifi "Your\ Hostname" password "Your\ Password"
```

Instalar GUI
```
yum upgrade -y
yum install epel-release -y # Repositorio
## Elegir GUI
yum groupinstall "Server with GUI" -y
## OR
yum groupinstall "Xfce" "X Window System" -y
```

Configurar GUI
```
systemctl get-default
systemctl set-default graphical.target
systemctl get-default
systemctl isolate graphical.target
```

Instalar repositorios
Nota: Son Links completos
### RPM Fusion Free
```
yum localinstall --nogpgcheck https://download1.rpmfusion.org/free/el/rpmfusion-free-release-7.noarch.rpm
```
### RPM fusion Nonfree
```
yum localinstall --nogpgcheck https://download1.rpmfusion.org/nonfree/el/rpmfusion-nonfree-release-7.noarch.rpm
```

Instalar paquetes
```
yum upgrade -y
yum install htop vsftpd httpd ufw
```