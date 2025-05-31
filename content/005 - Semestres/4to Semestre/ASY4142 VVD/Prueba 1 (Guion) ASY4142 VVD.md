# "Instalación de Asterisk en Ubuntu Server"

Hola Profesor y Alumnos, hoy les voy a mostrar la instalación de una maquina virtual y asterisk

Para esto separe este vídeo en
1. [[005 - Semestres/4to Semestre/ASY4142 VVD/Prueba 1 (Guion) ASY4142 VVD#Materiales Necesarios|Materiales Necesarios]]
2. [[005 - Semestres/4to Semestre/ASY4142 VVD/Prueba 1 (Guion) ASY4142 VVD#Requisitos Necesarios|Requisitos Necesarios]]
4. Archivos de configuración y referencias esenciales
## Materiales Necesarios
Los materiales que vamos a requerir son
- [Ubuntu Server](https://ubuntu.com/download/server) (22.04.3 LTS)
- [Asterisk](https://www.asterisk.org/) | [Docs](https://docs.asterisk.org/) | [Asteriskdocs](http://asteriskdocs.org/en/3rd_Edition/asterisk-book-html-chunk/index.html) | [Asterisk Package Ubuntu](https://packages.ubuntu.com/jammy/asterisk) (v18) (La ultima es la [v21.0.0-rc1](http://downloads.asterisk.org/pub/telephony/asterisk/releases/asterisk-21.0.0-rc1.tar.gz))

## Requisitos Necesarios
- Requerimientos by [Asterisk](http://asteriskdocs.org/en/3rd_Edition/asterisk-book-html-chunk/asterisk-InstallationPlanning.html#asterisk-InstallationPlanning-TABLE-1) -> [Tarjeta Analogica Wildcard Digium](https://data2.manualslib.com/pdf4/89/8811/881030-digium/tdm410.pdf?f0504468e6f7f131f93f0c19e86291a3) y [CPU 2 GHz Dual Core](https://www.techpowerup.com/cpu-specs/?mobile=No&nCores=2&igp=Yes&sort=name)

# Instalacion
Instalamos la maquina, como vamos a usar
Instalamos Ubuntu Server como siempre
`!!User:asterisko`
`!!Pass:Asterisko`
Reiniciar la maquina y acceder

Comandos
> Actualizar el servidor a la ultima version y reiniciar
```
sudo apt-get update -y && sudo apt-get upgrade -y && sudo apt-get autoremove && sudo reboot
```

> Dar el env de sudo y la terminal
```
sudo -i
```

> Revisar los paquetes que tienen el nombre de asterisk
```
apt-cache search asterisk
```

> Instalamos Asterisk y sus dependencias
```
apt-get install net-tools asterisk asterisk-prompt-es asterisk-core-sounds-es asterisk-core-sounds-es-gsm asterisk-core-sounds-es-wav asterisk-core-sounds-es-g722 wget build-essential git autoconf subversion pkg-config libtool
```

> Revisar la version con asterisk instalada
```
astersik -v
```

> Comprobamos los paquetes instalados
```
dpkg -l asterisk*
```

> Vemos el status de asterisk
```
service asterisk status
```

> Si todo esta ok, saltar la parte de los error
> err1) revisar el directorio
```
ls -l /etc/radcli/
```

(OPT1 - RECOMMENDED)
> opt1) Crear la carpeta que pide asterisk
```
mkdir /etc/radiusclient-ng/
```

> opt1) Crear un enlace simbolico a la configuracion de radiuscfg
```
ln -s /etc/radcli/radiusclient.conf /etc/radiusclient-ng/radiusclient.conf
```


---
OPT2 - Not Recommended
> [!WARNING]- Peligro
> 1. Listamos y comprobamos el archivo de configuracion
> `nano /etc/asterisk/cel.conf`
> 2. Editamos la linea "`radiuscfg`", que sea:
> `/etc/radcli/radiusclient.conf`
> 3. Guardamos la configuracion y salimos del archivo
> 4. Abre el archivo de configuracion
> `nano /etc/asterisk/cdr.conf`
> 5. Editamos la linea "`radiuscfg`", que sea:
> `/etc/radcli/radiusclient.conf`
> 6. Guardamos la configuracion y salimos del archivo

---
> Reiniciamos el servicio
```
service asterisk restart
```

> Ver el estado de asterisk, avanzamos si todo esta ok, y revisamos otra vez error hasta que lo solucionemos
```
service asterisk status
```

## Creacion del archivo sip
> Eliminamos el archivo `/etc/asterisk/sip.conf`
```
rm -drf /etc/asterisk/sip.conf
```

> Modificamos el archivo con `nano /etc/asterisk/sip.conf`
```
[general]
port=5060
bindaddr=0.0.0.0
nat=force_rport,comedia
language=es
maxexpirey=3600
disallow=all
allow=ilbc
allow=ulaw
allow=gsm
alloW=speex


;extencion 6101
[6101]
type=friend
username=Kimberly
callerid=¨Kimberly¨ <6101>
secret=1234
port=5061
qualify=yes
host=dynamic
dtmfmode=info
dtmfmode=inband
dtmfmode=rfc2833
context=phones
disallow=all
allow=ulaw
allow=alaw


;extencion 6102
[6102]
type=friend
username=Paulina
callerid=¨Paulina¨ <6102>
secret=1234
qualify=yes
host=dynamic
dtmfmode=info
dtmfmode=inband
dtmfmode=rfc2833
context=phones
disallow=all
allow=ulaw
allow=alaw


;extension 6103
[6103]
type=friend
username=Gabo
callerid="Gabo" <6103>
secret=1234
qualify=yes
host=dynamic
dtmfmode=info
dtmfmode=inband
dtmfmode=rfc2833
context=phones
disallow=all
allow=ulaw
allow=alaw

;extension 6104
[6104]
type=friend
username=Alexis
callerid="Alexis"<6104>
secret=1234
qualify=yes
host=dynamic
dtmfmode=info
dtmfmode=inband
dtmfmode=rfc2833
context=phones
disallow=all
allow=ulaw
allow=alaw

;extension 6105
[6105]
type=friend
username=Livy
callerid="Livy" <6105>
secret=1234
qualify=yes
host=dynamic
dtmfmode=info
dtmfmode=inband
dtmfmode=rfc2833
context=phones
disallow=all
allow=ulaw
allow=alaw

;extension 6106
[6106]
type=friend
username=Diego
callerid="Diego"<6106>
secret=1234
qualify=yes
host=dynamic
dtmfmode=info
dtmfmode=inband
dtmfmode=rfc2833
context=phones
disallow=all
allow=ulaw
allow=alaw

;extension 6107
[6107]
type=friend
username=Matias
callerid="Matias"<6107>
secret=1234
qualify=yes
host=dynamic
dtmfmode=info
dtmfmode=inband
dtmfmode=rfc2833
context=phones
disallow=all
allow=ulaw
allow=alaw

;extension 6108
[6108]
type=friend
username=Carla
callerid="Carla"<6108>
secret=1234
qualify=yes
host=dynamic
dtmfmode=info
dtmfmode=inband
dtmfmode=rfc2833
context=phones
disallow=all
allow=ulaw
allow=alaw

;extension 6109
[6109]
type=friend
username=Leonardo
callerid="Leonardo"<6109>
secret=1234
qualify=yes
host=dynamic
dtmfmode=info
dtmfmode=inband
dtmfmode=rfc2833
context=phones
disallow=all
allow=ulaw
allow=alaw
```

> Recargar el servicio
`service asterisk reload`

> validamos en la consola de asterisk
`asterisk -rvvvvvvv`
```
sip show users
sip show peers
exit
```

> Recargamos el servicio
`service asterisk reload`

> Editamos el archivo `extensions.conf`
`rm -drf /etc/asterisk/extensions.conf`
`nano /etc/asterisk/extensions.conf`
```
[general]
static=yes
writeprotect=yes

[default]

[phones]
exten => 6100,1,NoOp(Llamada a grabacion)
exten => 6100,n,Answer()
exten => 6100,n,Playback(tt-monkeys)
exten => 6100,n,HangUp()

exten => 6101,1,Dial(SIP/6101)
exten => 6102,1,Dial(SIP/6102)
exten => 6103,1,Dial(SIP/6103)
exten => 6104,1,Dial(SIP/6104)
exten => 6105,1,Dial(SIP/6105)
exten => 6106,1,Dial(SIP/6106)
exten => 6107,1,Dial(SIP/6107)
exten => 6108,1,Dial(SIP/6108)
exten => 6109,1,Dial(SIP/6109)
```

> Validar en la consola de asterisk
`asterisk -rvvvvvvv`
```
dialplan reload
```