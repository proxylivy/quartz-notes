# Info
## Informacion General
[EVE-NG](https://www.eve-ng.net/) es una plataforma que permite virtualizar dispositivos de red como Routers o Switch en laboratorios, los cuales tienes 3 ediciones:
- Community: Gratuita, con limitaciones
- Pro: Pago, uso personal
- Learning Center: Pago, uso corporativo
Mas detalles en su [Modelo de Licencia](https://www.eve-ng.net/index.php/documentation/eve-licensing-model/) y [Comparacion de funciones](https://www.eve-ng.net/index.php/features-compare/)

Sobre la licencia Learning Center de EVE-NG tiene un costo de 1.000.000 CLP al año, permite usar 10 usuarios simultaneamente a un laboratorio, ademas de 2 usuarios administradores, para un total de 12 usuarios concurrentes
> [!IMPORTANT] Importante
> [EVE-NG Learning Center License](https://www.eve-ng.net/index.php/buy-corporate/)
> - 1x Base License (includes 2x Administrator users)
> - 10x Lab User licenses
> - Allows you to have 12 users connect and work with EVE at same time. 2 Administrators and 10 Lab Users.

> [!WARNING] Importante
> La edicion Community **no soporta Docker**. Para utilizar utilizar contenedores integrados (Paquete `eve-ng-dind`), se necesita tener la version PRO o Learning Center
> Mas al respecto: [Youtube - EVE-NG - EVE Pro embedded Docker Setup and Usage](https://www.eve-ng.net/index.php/documentation/howtos-video/eve-embedded-dockers-setup-and-usage/)
> 
	> Una alternativa seria usar una conexion hacia tu host y que se conecte a otra maquina que si utilize docker para crear la conexion hacia dentro de los laboratorios

Image List: 
- VyOS 1.5 Rolling
- Microtik RouterOS 7.15.2
- Cisco IOL ([Fuente de Eleccion](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-cisco-iol-ios-on-linux/))
	- i86bi-linux-l3-adventerprisek9-15.4.2T4.bin
	- i86bi_LinuxL2-AdvEnterprisek9-M_152_May_2018.bin
	- i86bi_LinuxL3-AdvEnterprisek9-M2_157_3_May_2018.bin
	- i86bi_Linux_l2-adventerprisek9-ms.SSA.high_iron_20190423.bin
	- x86_64_crb_linux-adventerprisek9-ms.bin (17.12)
	- x86_64_crb_linux_l2-adventerprisek9-ms.bin (17.12)
- Cisco vIOS Router
	- vios-adventerprisek9-m.SPA.159-3.M6 (Slow)
- Cisco vIOS Switch
	- viosl2-adventerprisek9-m.ssa.high_iron_20200929 (Slow)
- Linux Alpine - 3.18.4
- POSIBLEMENTE Huawei CE12800 and NE40e [Huawei Forums](https://forum.huawei.com/enterprise/intl/en/thread/run-ce12800-ne40e-in-eve-ng/667237045992570881?blogId=667237045992570881)
- ExtremeVOSS-8.10.1
- ExtremeXOS-32.6.3
- Virtual PC (VPCS)
Disk Usage: Aproximadamente 18GB

Maquina de Prueba
- CPU: I5-6200 2,4Ghz 2 Nucleos 4 Hilos

StandBy
- CPU Usage: 1% 
- Ram Usage: 1GB

Booting 11 Cisco IOL and 4 Linux Alpine, no config
- CPU Usage: 100%
- Ram Usage: 3.6GB

Standby With 11 Cisco IOL and 4 Linux Alpine, no config
- CPU Usage: 30%
- Ram Usage: 3.4GB

## Requisitos del servidor
> [!IMPORTANT] Nota 1
> [Requisitos del sistema](https://www.eve-ng.net/index.php/documentation/installation/system-requirement/) o [Calcular el Uso](https://www.eve-ng.net/index.php/download/#CALC) | [Cookbook](https://www.eve-ng.net/index.php/documentation/community-cookbook/) Hoja 10 (2.1.4 Dedicated Server BM system requirements)

Requisitos:
- OS: Ubuntu Focal Fossa 22.04.X LTS or VMware ESXi 6.7 minimum
- CPU: Intel Xeon con Soporte Intel VT-X/EPT(Extended Page Tables) or AMD-V/RVI | Recommended: 2x Intel E5-2650v4
- Storage: M.2 PCIe > SSD Sata > HDD Sata (2TB or more)
- RAM: 128GB or more
- Motherboard: Support virtualize IOMMU options (Optional)

Para los clientes usar la `Consola Nativa` clientless, se debe instalar el paquete el cual esta disponible para [Windows](https://www.eve-ng.net/index.php/download/#DL-WIN), [MacOS](https://www.eve-ng.net/index.php/download/#DL-OSX) y [Linux](https://www.eve-ng.net/index.php/download/#DL-LIN)

Extra: En caso de problemas en Windows con putty, aqui un [.reg](https://putty.org.ru/features/ssh-handler) mas completo
# Instalacion
> [!IMPORTANT] Importante
> - Al instalar [EVE-NG Community](https://www.eve-ng.net/index.php/community/) Usa automaticamente la version de Ubuntu 22.04.4 LTS (Jammy Jellyfish)
> - Disponibilidad hasta Apr 2027 - ESM 2032 | [Info version](https://www.releases.ubuntu.com/22.04/)

Requerimientos
- USB de al menos 8GB
- Descargar [Ventoy](https://www.ventoy.net/en/index.html) o [Rufus](https://rufus.ie) para tener un Live USB

## Fase 1: Instalar de Ubuntu
Informacion: [Documentacion Eve-NG - First Boot](https://www.eve-ng.net/index.php/documentation/installation/howto-configure-eve-during-first-boot/) | [EVE-NG Cookbook Hoja 24: 3.3 BM server install](https://www.eve-ng.net/index.php/documentation/community-cookbook/)
1. Iniciar el LiveUSB
2. Elegir "`Bare Metal Option`" y luego "`install EVE NG Community 6.0.1-12`"
3. Selecciona el idioma `Español`
4. Selecciona el teclado Layout y Variant `Spanish (Latin America)`
5. Apreta "`Continuar`" este paso formateara los discos seleccionados
6. Luego de instalar, la phase 2 iniciara automaticamente, NO INICIES SESION, el servidor se reiniciara automaticamente y saldra un menu diciendo, alli podras continuar
```
Eve-NG (default root password is 'eve')
Use http://{ip}

eve-ng login:
```

> Default CLI Login Credentials
```
user: root
pass: eve
```
> Default WEB Login Credentials | [Cookbook](https://www.eve-ng.net/index.php/documentation/community-cookbook/) Hoja 46 (3.7 Login to the EVE WEB GUI)
```
user: admin
pass: eve
```

> Paso 2: Configuracion TUI Basica
```
- Nueva Contraseña: eve
- Hostname: eve-ng
- DNS Domain Name: example.com 
- IP/DHCP: DHCP
- NTP Server: empty
- Proxy Server: Direct Connection
```

> Paso 3: Prueba de internet y Actualizar Paquetes Servidor | [Cookbook](https://www.eve-ng.net/index.php/documentation/community-cookbook/) Hoja 48 (4.2 EVE-NG Community Upgrade)
```
ping -c 2 google.cl
apt-get update && apt-get upgrade
apt autoremove
```
> Paso 3.1: Reestablecer servicios
```
# Selecciona todos #
```

> Paso 4: Instalar paquetes para el administrador
```
apt install micro btop kitty weston git tree
```
> Paso 4.1: Instalar [Fish Shell](https://fishshell.com/) y [Fastfetch](https://github.com/fastfetch-cli/fastfetch)
```
sudo apt-add-repository ppa:fish-shell/release-3
sudo add-apt-repository ppa:zhangsongcui3371/fastfetch
sudo apt update
sudo apt install fish fastfetch
```
> Paso 4.2: Instalar [Fisher](https://github.com/jorgebucaran/fisher)
> Nota: Solo funciona cuando dentro de `fish`
```
curl -sL https://raw.githubusercontent.com/jorgebucaran/fisher/main/functions/fisher.fish | source && fisher install jorgebucaran/fisher
```
> Paso 4.3: Instalar [Tide](https://github.com/IlanCosman/tide)
```
fisher install IlanCosman/tide@v6
```
> Paso 4.4: Configurar `.config/weston.ini` (GUI)
```
[keyboard]
keymap_layout=latam
```
Uso de la interfaz web en [Cookbook](https://www.eve-ng.net/index.php/documentation/community-cookbook/) Hoja 57 (6 EVE WEB GUI Managent)

## Fase 2: Instalar Imagenes
> [!IMPORTANT] Nota
> - Lista de [Imagenes Soportadas](https://www.eve-ng.net/index.php/documentation/supported-images/), se puede obtener en [Labhub](https://labhub.eu.org/es/), y leer la [Guia - Instalacion EVE-NG Rusa](https://arny.ru/linux/ustanovka-eve-ng/)

### VyOS
> [!IMPORTANT] Importante
> Enciende en 113 segundos
> Informacion: [EVE-NG - Documentacion](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-vyos-vyatta/), [VyOS - Official Site](https://vyos.net/get/), [VyOS - Ejemplo de Configuracion](https://docs.vyos.io/en/latest/configexamples/index.html) y [Guia Quick Start](https://docs.vyos.io/en/latest/quick-start.html)
> [Guia de Instalacion](https://npaul.uk/2021/01/build-the-best-free-network-learning-environment-with-eve-ng/)

1. Crear Carpeta
```
mkdir /opt/unetlab/addons/qemu/vyos-{version}
```
2. Mover las imagenes a esa carpeta
```
rsync -Phvr vyos-{version}-amd.iso root@{ip-server}:/opt/unetlab/addons/qemu/vyos-{version}/cdrom.iso
```
3. Ir a la carpeta
```
cd /opt/unetlab/addons/qemu/vyos-{version}/
```
4. Crear disco Qcow2
```
/opt/qemu/bin/qemu-img create -f qcow2 virtioa.qcow2 10G
```
5. Agrega un nodo a EVE-NG e inicialo, cuando inicie, usa vyos/vyos
6. Instala vyos en el disco duro
```
install image
```
7. Las preguntas son
- `y`
```
This command will install VyOS to your permanent storage.
Would you like to continue? [y/N]
```
- **Enter**
```
What would you like to name this image? (Default: 1.5-rolling-202407171706)
```
-  `vyos`
```
Please enter a password for the "vyos" user:
```
- `vyos`
```
Please confirm password for the "vyos" user:
```
- `S`
```
What console should be used by default? (K: KVM, S: Serial)? (Default: S)
```
- **Enter**
```
Probing disks
1 disk(s) found
The following disks were found:
Drive: /dev/vda (10.0 GB)
Which one should be used for installation? (Default: /dev/vda)
```
- `y`
```
Installation will delete all data on the drive. Continue? [y/N]
```
- `y`
```
Would you like to use all the free space on the drive? [Y/n]
```
- `2`
```
The following config files are available for boot:
    1: /opt/vyatta/etc/config/config.boot
    2: /opt/vyatta/etc/config.boot.default
Which file would you like as boot config? (Default: 1)
```
8. Apaga el nodo
```
poweroff
```

Parte 2: Commit la imagen para el uso futuro
9. Ve a la terminal de EVE-NG y mueve a la carpeta de UUID y POD
- `user-POD`:  En la interfaz web, en la administracion de usuarios, aparece `{user-POD}`
- `UUID`: En la interfaz web, en la seccion izquierda elige "`Lab Details`"
- `node-POD-ID`: Se encuentra apretando el click derecho en el nodo del laboratorio
```
cd /opt/unetlab/tmp/{user-POD}/{UUID}/{node-POD-ID}
```
Ejemplo:
```
cd /opt/unetlab/tmp/0/3491e0a7-25f8-46e1-b697-ccb4fc4088a2/1/
```
10. Hace commit a la imagen
```
qemu-img commit virtioa.qcow2
```
11. Elimina la imagen `cdrom.iso` de la carpeta raiz
```
cd /opt/unetlab/addons/qemu/vyos-{version}
rm cdrom.iso
```
12. Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### RouterOS
> [!IMPORTANT] Importante
> [Eve-NG - Documentacion agregar imagen](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-mikrotik-cloud-router/)

- Credenciales por defecto
```
user: admin
no password
```
Descarga desde la pagina de [Descarga](https://mikrotik.com/download) en la seccion Cloud Hosted Router seleccionamos la version "`Stable`" mas nueva, y eliges el disco "`RAW disk Image`"
0. Crear carpeta en el servidor donde "`version`" sea la version descargada
```
mkdir -p /opt/unetlab/addons/qemu/mikrotik-{version}
```
1. Enviar al servidor desde tu pc
Nota: Experimental, nunca he modificado un archivo en su termino directamente desde rsync
```
rsync -Phvr chr-{version}.img root@{ip-server}:/opt/unetlab/addons/qemu/mikrotik-{version}/
```
2. Mover el archivo
```
mv chr-{version}.img /opt/unetlab/addons/qemu/mikrotik-{version}/hda.qcow2
```
3. Arreglar Permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### Cisco IOL
> [!IMPORTANT] Importante
> [EVE-NG - Documentacion agregar imagen IOL](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-cisco-iol-ios-on-linux/)
> Las imagenes mas nuevas deben tener la extension `.bin`
> Versiones viejas posiblemente no funcionen
> Evitar usar la version `L3 15.5.2T` debido a que se congela en standby
#### Tabla IOL Imagen Recomendada

| Type            | EVE Image Name                                                   | Version                                                                                                                                       | NVRAM | RAM  |
| --------------- | ---------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- | ----- | ---- |
| L2/L3 Switch    | i86bi_linux_l2-adventerprisek9-ms.<br>SSA.high_iron_20190423.bin | Cisco IOS Software, Linux Software <br>(I86BI_LINUXL2-ADVENTERPRISEK9-M),<br>Version 15.2(CML_NIGHTLY_20190423)                               | 1024  | 1024 |
| L2/L3 Switch    | i86bi_LinuxL2-AdvEnterpriseK9-M<br>_152_May_2018.bin             | Cisco IOS Software, Linux Software (I86BI_LINUXL2-<br>ADVENTERPRISEK9-M), Version 15.2(CML_NIG <br>HTLY_20180510)FLO_DSGS7                    | 1024  | 1024 |
| L3 Router       | i86bi_LinuxL3-AdvEnterpriseK9-<br>M2_157_3_May_2018.bin          | Cisco IOS Software, Linux Software (I86BI_LINUX-<br>ADVENTERPRISEK9-M), Version 15.7(3)M2,<br>Compiled Wed 28-Mar-18 11:18 by prod_rel_team   | 1024  | 1024 |
| L3 Router       | L3-ADVENTERPRISEK9<br>-M-15.4-2T.bin                             | Cisco IOS Software, Linux Software (I86BI_LINUX-<br>ADVENTERPRISEK9-M), Version 15.4(2)T4, <br>Compiled Thu 08-Oct-15 21:21 by prod_rel_team  | 1024  | 1024 |
| L3 XE Router    | x86_64_crb_linux-adventerprisek9<br>-ms.bin                      | IOL XE Router Cisco IOS Software [Dublin], Linux <br>Software (X86_64BI_LINUX-ADVENTERPRISEK9-M), <br>Version 17.12.1, RELEASE SOFTWARE (fc5) | 1024  | 1024 |
| L2/L3 XE Switch | x86_64_crb_linux_l2-adventerprisek9<br>-ms.bin                   | IOL XE Switch Cisco IOS Software [Dublin], Linux<br>Software (X86_64BI_LINUX_L2-ADVENTERPRISEK9-M), Version 17.12.1, RELEASE SOFTWARE (fc5)   | 1024  | 1024 |
#### IOURC
Nota: Este archivo es diferente segun `hostname` y `domain-name` del servidor
Usar `script.py` para parchear como se ve [aqui](https://it-blackbox.blogspot.com/2018/06/eve-ng-cisco-iouiol.html)
```
cd /opt/unetlab/addons/iol/bin/
python2 script.py
```
El output del script copialo dentro de `/opt/unetlab/addons/iol/bin/iourc`
```
[license]
eve-ng = 972f30267ef51616;
```
#### BIN
Los archivos necesitan terminar en `.bin` o no funcionaran
0. Crear Carpeta
```
mkdir /opt/unetlab/addons/iol/bin /opt/unetlab/addons/iol/lib
```
1. Haz que las imagenes sean ejecutables
```
chmod 755 bin/*.bin
```
3. Copia las imagenes desde tu pc al servidor
```
rsync -Phvr bin/*.bin root@{ip-server}:/opt/unetlab/addons/iol/bin
```
3. Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```
#### Probar las imagenes
1. Mover a la carpeta con los `.bin`
```
cd /opt/unetlab/addons/iol/bin
```
2. Crea un `NETMAP`
```
touch NETMAP
```
3. Ejecuta la imagen donde `{iosname.bin}` sea la imagen a probar
```
LD_LIBRARY_PATH=/opt/unetlab/addons/iol/lib /opt/unetlab/addons/iol/bin/{iosname.bin} 1
```

#### Parchear .bin
Bueno, lo hare si es necesario, pero me dio flojera
Una licencia no valida lanza el siguiente mensaje
```
IOS On Unix - Cisco Systems confidential, internal use only
IOU License Error: invalid license
License for key 7f0343 required on host "eve-ng".
Obtain a license for this key and host from the following location:
http://wwwin-enged.cisco.com/ios/iou/license/index.html
Place in your iourc file as follows (see also the web page
for further details on iourc file format and location)
```

### Cisco vIOS (EX-VIRL)
Veo que funciona lento
Imporante: Las imagenes de [Qemu](https://www.eve-ng.net/index.php/documentation/qemu-image-namings/) segun EVE-ng se nombran de forma especial
[Documentacion EVE-ng](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-cisco-vios-from-virl/)
1. Crea carpeta (L3 = vios- | L2 = viosl2-)
```
mkdir /opt/unetlab/addons/qemu/vios-{version}
```
2. Envia las imagenes a la carpeta correspondiente
```
rsync -Phvr vios-{version}.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/vios-{version}/
```
3. Cambia el nombre
```
mv vios-{version}.qcow2 virtioa.qcow2
```
4. Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### Linux Ready Images
[Documentacion Oficial EVE-NG](https://www.eve-ng.net/index.php/documentation/howtos/howto-create-own-linux-host-image/) y [Videotutorial](https://www.youtube.com/watch?v=ZLvdJa3MXTU)
- Credenciales Generales
```
root/root
root/eve
user/Test123
root/Test123
root/toor #For Kali
```

Credenciales de Alpine
```
root/eve
```
0. Descarga la imagen preferida del link de [Mega](https://mega.nz/folder/30p3TKob#42_S__9wwPVO0zHIfC4xow)
1. Descomprime el archivo
```
tar xvzf linux-alpine-3.18.4.tar.gz
rm -f linux-alpine-3.18.4.tar.gz
```
2. Enviar el archivo descargado al servidor
```
rsync -Phvr linux-alpine-{version} root@{ip-server}:/opt/unetlab/addons/qemu/
```

### Comprimir imagenes
Si funciona la version comprimida, puedes borrar el original
1. Ir a la carpeta
```
cd /opt/unetlab/addons/qemu/{linux}
```
2. Comprime `virt-sparsify`
```
virt-sparsify --compress virtioa.qcow2 cvirtioa.qcow2
```
3. Reescribe el archivo
```
mv cvirtioa.qcow2 virtioa.qcow2
```

### SD-WAN en EVE-NG
Estos nodos son muy pesados, cada uno necesita 100GB de espacio y 32 GB de ram
[Documentacion](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-cisco-viptela-images-set/)
[Descargar Imagenes](https://drive.google.com/drive/u/0/folders/1mAHu1MCOSc-QDKZQxn71wqAT-_zzDska) o [Aqui](https://networkrare.com/free-download-cisco-viptela-images-vmanage-vsmart-vbond-vedge-cedge-for-eve-ng/)
[Guia](https://www.networkacademy.io/ccie-enterprise/sdwan/cisco-sd-wan-on-eve-ng) - [Video](https://www.youtube.com/watch?v=Caze1TZldCM)
1. Crear carpetas
```
mkdir /opt/unetlab/addons/qemu/vtbond-{version}
```
2. Envia las imagenes
```
rsync -Phvr viptela-{model}-{version}.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/vtbond-{version}
```
3. corrige el nombre
```
mv viptela-{model}-{version}.qcow2 virtioa.qcow2
```

```
!
!
!
```

# Fase 3: Configuraciones
## Importar Configuraciones
Necesita tener las mismas imagenes de L2 y L3 que los laboratorios que quiere importar
## Exportar Configuraciones
Se guarda en la nand del dispositivo y de alli se exporta hacia el espacio tmp de la maquina

## Acceso Red Host
Se crea un `object` de `network` con el tipo `Management(Cloud)` esta nube permite que un dispositivo acceda a internet a travez de la conexion al router fisico como gateway
[aqui hay mas info](https://www.petenetlive.com/KB/Article/0001432)

## Laboratorios de ejemplo
Nota: Los labs de EVE-NG PRO no son compatibles con [EVE-NG Community](https://www.eve-ng.net/index.php/community/)
- [Eve-NG Lab](https://www.eve-ng.net/index.php/lab-library/)
- [Troubleshoot](https://www.networktut.com/practice-tshoot-tickets-with-packet-tracer)
- [Cisco CCIE Practice](https://learningnetwork.cisco.com/s/article/ccie-enterprise-infrastructure-practice-labs)

Tabla dispositivos CCIE

| Devices                                | Type            | Version                         |
| -------------------------------------- | --------------- | ------------------------------- |
| cEdges, pe11, pe12, pe21, pe22, r1, r2 | Catalyst 8000v  | IOS-XE 17.9.x                   |
| All other routers                      | vIOL            | IOS 15.8(3)                     |
| Catalyst Center                        | Cisco           | 2.3.x                           |
| Hosts                                  | Debian          | N/A                             |
| Identity Services Engine (ISE)         | Cisco           | 3.1.x                           |
| sw11, sw21, sw22, sw23                 | Catalyst C9324T | IOS-XE 17.9.x                   |
| All other switches                     | vIOS-L2         | IOS 15.2, build 20200924:215240 |
| vManage, vSmart, vBond                 | Viptela         | Viptela 20.9.x                  |
# Extra - Aprender
- [Lectura en profundidad CCIE](https://www.reddit.com/r/ccie/comments/6bwc2a/ccie_rsv5_ocg_further_reading_links/)
- David Bombal - EVE NG Installation: [Youtube](https://www.youtube.com/watch?v=FDbgTlr-tnw)
- Se Permite el uso de [clusters](https://www.eve-ng.net/index.php/documentation/eve-ng-cluster/) para usar replicas en diferentes servidores y permitir laboratorios mas grandes

# Extra - Ideas Topologias
## Routing y Switching Avanzado 
- Crear una red MPLS VPN Capa 3 (L3VPN)
	- Configurar Multiples routers en topologia "PE-CE"
	- Probar rutas aisladas por VRF y BGP
	- Añadir un Route Reflector para optimizar la convergencia
- MPLS VPN de Capa 2 (L2VPN / VPLS)
	- Simular un servicio de Ethernet WAN sobre MPLS
	- Revisar las tramas Capa 2 en la red IP/MPLS
- Spine-Leaf con VXLAN EVPN
	- Usando (Cisco NX-OSv, Arista vEOS o Cumulus Linux)
	- Configurar VXLAN para extender L2 sobre una red IP underlay
	- Explorar BGP EVPN como control plane
- Redundancia de Primer Salto (HSRP/VRRP/GLBP)
	- Configurar 2 o 3 routers Cisco en una VLAN
	- Probar la tolerancia de fallos y la conmutacion entre gateway virtuales
- Multicast Routing (PIM-SM, PIM-DM)
	- Tener varios routers que soporten multicast (Cisco, VyOS, etc..)
	- Usar un servidor de streaming (por ejemplo, VLC en Linux) para probar flujos multicast
- QoS
	- Simular congestion en una red con varios nodos
	- Configurar colas, politicas de marcado (DSCP), shaping y policy

## Laboratorios de Seguridad
- Firewall Multi-Vendor (Palo Alto, FortiGate, Cisco ASA, etc.)
	- Configurar varias zonas (Inside, DMZ, Outside)
	- Probar NAT, ACL y politicas de seguridad para filtrar trafico
- VPN Site-to-Site IPsec
	- Conectar dos redes simuladas a travez de un tunel IPsec
	- Usar distintos firewall y routers
- Remote Access VPN (SSL VPN, AnyConnect)
	- Configurar acceso remoto con clientes VPN
	- Probar la autenticacion con servidor RADIUS/LDAP
- IDS/IPS con Snort o Suricata
	- Implementar un IDS/IPS en Linux para monitorear trafico
	- Simular ataques o paquetes maliciosos para ver las alertas generadas
- Monitoreo y Logging
	- Incluir un servidor de monitoreo (Zabbix, Nagios, LibreNMS) y un Syslog Centralizado
	- Revisar Estadisticas SNMP y Syslog para la topologia completa
- SIEM
	- Usar Wazuh / Elastic Stack / Splunk para recolectar eventos
	- Monitorear logs de routers, firewalls y sistemas Linux/Windows

## Integracion con Cloud y Laboratorios Hibridos
- Hybrid Cloud / Multi-Cloud (AWS, Azure, GCP)
	- Simular una conexion desde un router on-premises a distintas nubes a la vez
	- Emular BGP con routers virtuales conectados a "ip cloud" en EVE-NG
- DMVPN Multi-Hub
	- Configurar un lab con hub en diferentes "nubes" y permitir que hablen entre ellos
	- Explorar la escabilidad de DMVPN fase 2 o 3

## Servicios de Acceso Remoto y Gestion
- OpenVPN "Client-to-Lab"
	- Aislar la topologia en subredes y que se interconecten con redes VPN
- Configurar Bastion Host o Proxy Inverso
	- Montar un servidor Linux por ejemplo, un Ubuntu que sirva de punto de salto
	- Gestionar equipos en la topologia sin exponerlos todos a internet
- Entorno Zero Trust / Microsegmentacion
	- Combinar maquinas virtuales Linux/Windows con un firewall Palo Alto o Cisco ISE
	- Etiquetar trafico y aplicar politicas basadas en identidades de usuarios o dispositivos

## Automatizacion y DevOps
- Laboratorio con Ansible / Python
	- Crear un inventario dinamico de dispositivos Cisco, Juniper, Linux, etc
	- Automatizar configuraciones con Ansible o Netmiko / NAPALM
- GitOps
	- Almacenar configuraciones en repositorios Git
	- Aplicar cambios con pipelines que ejecuten playbooks de Ansible
- NETCONF / RESTCONF / YANG
	- Usar IOS-XE, JunOS o Nokia SR OS con soporte NETCONF
	- Explorar la gestion de dispositivos via modelos YANG

## Colaboracion VoIP
- Lab con CUCM
	- Probar llamadas entre softphones (Cisco IP Communicator)
	- Agregar gateways Cisco IOS como CUBE (SIP Trunk con PSTN simulado)
- Issabel / Asterisk / FreePBX
	- Integrar con gateways SIP en routers
	- Configurar IVR, extensiones, llamadas entrantes y salientes

## Balanceadores
- F5 BIG-IP LTM
	- Simular clientes e instancias web en Linux Alpine
	- Configurar Virtual Server, Pools, Monitores y perfiles de SSL
- NGINX / HAProxy en Linux
	- Lab simple de balanceo de carga HTTP/HTTPS
	- Comparar rendimiento y configuracion con F5 u otros load balancers

## Over IPv6
- IPv6 Purista
	- Configurar una red totalmente IPv6 (sin dual-stack)
	- Explorar SLAAC, DHCPv6 NAT64 y tecnicas de transicion (6to4, NAT-PT, etc)
- Segmentacion y Routing IPv6
	- Probar OSPFv3
	- Configurar BGP4 para intercambiar rutas IPv6












## Licencia Para poder escribir esto :P
```
EVE-NG Copyrights License

Copyright (c) 2016, Andrea Dainese  
Copyright (c) 2017-2023 Alain Degreffe  
All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:  
* Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.  
* Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.  
* Neither the name of the UNetLab Ltd nor the name of EVE-NG Ltd nor the  names of its contributors may be used to endorse or promote products derived from this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS “AS IS” AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL <COPYRIGHT HOLDER> BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
```

# Deprecated
## Virtualizador Tipo 1 (Ignorado por ahora)
Permite usar otras maquinas
### Instalacion Soportada de VMware ESXi (Ignorado por ahora)
Valor: $1.000.000 Al año
- [Youtube - VirtualizationHowTo - VMware ESXi do first](https://www.youtube.com/watch?v=-1BMiYZfz38)
- [Youtube - NetworkChuck - VMware ESXi Setup and Install](https://www.youtube.com/watch?v=apC1bOLbzbY&t=822s)

### Alternativa no soportada: Proxmox (Ignorado por ahora)
Valor: Gratis
- [Guia - Adam From The Future - Running EVE NG under Proxmox](https://adamfromthefuture.wordpress.com/2018/08/30/running-eve-ng-under-proxmox/)
- [Guia - Yzguy - EVE-NG in LXC on Proxmox](https://yzguy.dev/posts/eve-ng-in-lxc-on-proxmox/)
- [Youtube - Gerard O'Brien - EVE-NG on Proxmox](https://www.youtube.com/watch?v=BmuZHjkNCt0)
