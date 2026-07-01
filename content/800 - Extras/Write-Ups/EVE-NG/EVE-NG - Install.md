# Info
## Introduccion

EVE-NG (**E**mulated **V**irtual **E**nvironment - **N**ext **G**eneration) es una plataforma de emulacion de redes que permite virtualizar dispositivos como Router, Switches, Firewall, etc. utilizando imagenes reales de sus sistemas operativos.

Si te interesa, tengo un articulo que habla mas en profundidad sobre la [[800 - Extras/Articulos/Historia de la Emulacion|Historia de la Emulacion]]

## Licencias y Limites

> [!TIP] Lectura Recomendada
> - [EVE-NG Docs - EVE Licencing Model](https://www.eve-ng.net/index.php/documentation/eve-licensing-model/)
> - [EVE-NG Docs - Features Compare](https://www.eve-ng.net/index.php/features-compare/)
> - Buy License Links
> 	- [EVE-NG - Buy Professional](https://www.eve-ng.net/index.php/buy/)
> 	- [EVE-NG - Buy Corporate/Learning Center](https://www.eve-ng.net/index.php/buy-corporate/)
> - [EVE-NG Docs - Community](https://www.eve-ng.net/index.php/community/)
> - [Youtube - EVE-NG - EVE WEB UI features](https://youtu.be/EsmfepaYOL8?si=mjvLD99_o3qFQsWs)

Su modelo de licenciamiento se basa en 3 tiers descritos en la siguiente tabla

| Edicion         | Costo          | Soporte Docker | Usuarios Concurrentes      | Uso                |
| --------------- | -------------- | -------------- | -------------------------- | ------------------ |
| Community       | Gratis         | No             | 1                          | Personal           |
| Pro             | $205USD/year   | Si             | 1                          | Personal Pro       |
| Learning Center | $1.000USD/year | Si             | 12 (2 admin + 10 usuarios) | Academico/Empresas |

Nos centraremos en la version "`Community`", los limites son
- No soporta Nodos directos de Docker
- Solo lo puede usar una persona a la vez
- Solo puedes tener 63 nodos activos

> [!WARNING] Notas sobre licencia y Docker
> Mas al respecto: [Youtube - EVE-NG - EVE Pro embedded Docker Setup and Usage](https://www.eve-ng.net/index.php/documentation/howtos-video/eve-embedded-dockers-setup-and-usage/)
> La edicion Community **no soporta Docker** directamente. Para utilizar utilizar contenedores integrados (Paquete `eve-ng-dind`), se necesita tener la version PRO o Learning Center.
> Puedes ver un Workarround en [[#Uso de Docker]]

## Requisitos del servidor
> [!IMPORTANT] Documentacion Recomendada
> - [Requisitos del sistema](https://www.eve-ng.net/index.php/documentation/installation/system-requirement/)
> - [Calcular el Uso](https://www.eve-ng.net/index.php/download/#CALC)
> - [Cookbook](https://www.eve-ng.net/index.php/documentation/community-cookbook/)
> 	- Hoja 10: 2.1.4 Dedicated Server BM system requirements

Requisitos:
- OS: Ubuntu Focal Fossa 22.04.X LTS (Bare-Metal) or VMware ESXi 6.7 minimum (Hipervisor tipo 1)
- CPU: Intel Xeon con Soporte Intel VT-X/EPT(Extended Page Tables) or AMD-V/RVI | Recommended: 2x Intel E5-2650v4
- Storage: M.2 PCIe > SSD Sata > HDD Sata (2TB or more)
- RAM: 128GB or more
- Motherboard: Support virtualize IOMMU options (Optional)

En el host donde manejas el WEBUI recuerda configurar las [[#Consolas Nativas]]

## Soporte de Imagenes
> [!TIP] Lecturas recomendadas
> - [EVE-NG Docs - Supported Images](https://www.eve-ng.net/index.php/documentation/supported-images/)
> - [EVE-NG Docs - QEMU Image Namings](https://www.eve-ng.net/index.php/documentation/qemu-image-namings/)
> - [EVE-NG Docs - HowTos](https://www.eve-ng.net/index.php/documentation/howtos/)
> - Las imagenes estan dando vueltas por internet, te recomiendo buscar, te doy unas pistas
> 	- [Github ishare2-org](https://github.com/ishare2-org)
> 		- [Labhub](https://labhub.eu.org/es/), o [Drive Labhub](https://drive.labhub.eu.org/0:/), o [Legacy Labhub](https://legacy.labhub.eu.org/0:/), o [Alist Labhub](https://alist.labhub.eu.org/), o [Beta Labhub (Down?)](https://beta.labhub.eu.org/)
> 	- [Github - hegdepavankumar/Cisco-Images-for-GNS3-and-EVE-NG](https://github.com/hegdepavankumar/Cisco-Images-for-GNS3-and-EVE-NG)

Hay una gran variedad de imagenes, cambian sus funcionalidades segun el nombre que tengan, aqui tengo un pequeño Matrix, que muestra sus funciones

| Vendor              | Router<br>L3 | Switch<br>L2 / L3 | Firewall    | Load<br>Balancer | SD-WAN    | IDS/IPS |
| ------------------- | ------------ | ----------------- | ----------- | ---------------- | --------- | ------- |
| Cisco               | ✓ IOS        | ✓ IOS L2          | ✓ ASAv      | ✗                | ✓ Viptela | ✓ NGIPS |
| Fortinet            | ✓ FGT        | ✗                 | ✓ FGT       | ✓ FAD            | ✓ FGT     | ✓ FNDR  |
| Huawei              | ✓ AR1k       | ✓ CE12800         | ✓ USG6kv    | ✗                | ✓ USG6kv  | ✓ WAF5k |
| Juniper             | ✓ EVO        | ✓ EX              | ✓ vSRX 3.0  | ✗                | ✗         | ✗       |
| Arista              | ✓ vEOS       | ✓ vEOS            | ✓ NGFW      | ✗                | ✗         | ✗       |
| Extreme<br>Networks | ✓ VOSS       | ✓ EXOS            | ✗           | ✗                | ✗         | ✗       |
| Hillstone           | ✗            | ✗                 | ✓ CloudEdge | ✓ vADC           | ✗         | ✓ vIPS  |
| VyOS                | ✓            | ✓                 | ✓\*         | ✓\*              | ✗         | ✗       |
| Citrix              | ✗            | ✗                 | ✗           | ✓ NetScaler      | ✓ SD-WAN  | ✗       |
| MikroTik            | ✓ CHR        | ✗                 | ✗           | ✗                | ✗         | ✗       |
| Aruba               | ✗            | ✓ CX              | ✗           | ✗                | ✗         | ✗       |
| Palo Alto           | ✗            | ✗                 | ✓ PAN-OS    | ✗                | ✗         | ✓       |
| F5                  | ✗            | ✗                 | ✗           | ✓ Big-IP         | ✗         | ✗       |
| A10                 | ✗            | ✗                 | ✗           | ✓ vThunder       | ✗         | ✗       |

\*: Solo uso basico

**Detalle Imagenes Utilizadas**

Cisco IOL image list:
- Cisco IOL
	- L2/L3 Switch: i86bi_linux_l2-adventerprisek9-ms.SSA.high_iron_20190423.bin (15.2 - 2019-04-23)
	- L2/L3 Switch: i86bi_LinuxL2-AdvEnterpriseK9-M_152_May_2018.bin (15.2 - 2018-05-10)
	- L3 Router and PC: i86bi_LinuxL3-AdvEnterpriseK9-M2_157_3_May_2018.bin (15.7 - 2018-05-10)
	- L3 XE Router 64 bits: x86_64_crb_linux-adventerprisek9-ms.bin (17.16.1)
	- L2/L3 XE Switch 64 bits: x86_64_crb_linux_l2-adventerprisek9-ms.bin (17.16.1)

QEMU image list:
- Aruba AOS-CX 10.18 - [Free with Registration in HPE](https://networkingsupport.hpe.com/globalsearch#q=AOS-CX%20OVA&tab=Software&sortCriteria=date%20descending)
- Cisco
	- ASAv-9.22.1.1-PLR-Licenced
	- c9800cl-17.17.01
	- CSR1000vng-universalk9.17.03.05-serial
	- CSR1000v-universalk9.17.03.08a-serial
- Cisco vIOS Router
	- vios-adventerprisek9-m.SPA.159-3.M6 (Slow)
- Cisco vIOS Switch
	- viosl2-adventerprisek9-m.ssa.high_iron_20200929 (Slow)
- Extreme Networks
	- ExtremeVOSS 9.4.0.0 - [Free](https://github.com/extremenetworks/Virtual_VOSS)
	- ExtremeXOS 33.6.1.14 - [Free](https://github.com/extremenetworks/Virtual_EXOS)
- F5 BigIP 17.5.0-0.0.15
- Fortinet
	- FAC (FortiAuthentication) 6.6.2
	- FGT (Fortigate) 7.6.2.F-build3462
	- FNDR (Forti Network Detection and Response) v7.4-build0520
- Freebsd 14.2 [Open Source](https://www.freebsd.org/)
- Hillstone SG6000
	- CloudEdge 5.5R11P3.4-v6
	- vADC 5.5R10-4.2.3-v6
	- VW 5.5R8-3.6.2-v6
- Huawei
	- NE40e
	- CE12800
	- AR1000 5.170-V300R022C00SPC100
	- USG6000kv 5.1.7-2018
	- WAF5000k VV200R001C00
- Linux
	- Alpine 3.19.1 - [Open Source](https://www.alpinelinux.org/)
	- Arch Linux - [Open Source](https://archlinux.org/)
- Microtik RouterOS 7.18.2 - [Free](https://mikrotik.com/download)
- IP Fusion OcNOS 7.0.0 - [Free Demos with registration](https://www.ipinfusion.com/free-software-demos/) (Psst: Pon informacion falsa)
- OpenWRT 25.12.4 - [Open Source](https://openwrt.org/)
- OPNsense 25.1 - [Open Source](https://opnsense.org/)
- Palo Alto 11.2.5
- PfSense-pfs 2.7.2 - [Open Source](https://atxfiles.netgate.com/mirror/downloads/)
- vJunosEVO 24.4R1.8
- Vyos 1.5 Rolling Release - [Open Source](https://vyos.net/) - [Changelog](https://github.com/vyos/vyos-nightly-build/releases)
- Virtual PC (VPCS) - [Open Source](https://github.com/GNS3/vpcs)

# Fase 1: Instalacion de EVE-NG
> [!IMPORTANT] Importante
> - Al instalar [EVE-NG Community](https://www.eve-ng.net/index.php/community/) Usa automaticamente la version de Ubuntu 22.04.4 LTS (Jammy Jellyfish), no se debe actualizar la version o dejara de funcionar
> 	- Disponibilidad hasta Apr 2027 - ESM 2032 | [Ubuntu - Release Page](https://www.releases.ubuntu.com/22.04/)

> [!NOTE] Documentacion Recomendada
> - [Documentacion Eve-NG - First Boot](https://www.eve-ng.net/index.php/documentation/installation/howto-configure-eve-during-first-boot/)
> - [Arny Blog - Instalacion EVE-NG (Ru)](https://arny.ru/linux/ustanovka-eve-ng/)
> - [JD-Networks Blog - Eve NG Install](https://jd-networks.co.uk/blog/2019/05/24/eve-ng-community-edition/)
> - [Dainok - Installing Eve-NG](https://www.adainese.it/blog/2023/09/21/installing-eve-ng/)
> - [Youtube - David Bombal - EVE-NG Install](https://youtu.be/FDbgTlr-tnw?si=DaXk2pCoLcMtWIpg)
> - [EVE-NG Cookbook](https://www.eve-ng.net/index.php/documentation/community-cookbook/)
> 	- Hoja 24: 3.3 BM server install
> 	- Hoja 46: 3.7 Login to the EVE WEB GUI
> 	- Hoja 48: 4.2 EVE-NG Community Upgrade
> 	- Hoja 57: 6 EVE WEB GUI Managent

Requerimientos
- USB de al menos 8GB
- Descargar [Ventoy](https://www.ventoy.net/en/index.html) o [Rufus](https://rufus.ie) para configurar el Pendrive

1. Iniciar el LiveUSB
2. Elegir "`Bare Metal Option`" y luego "`install EVE NG Community 6.2.0-4`"
3. Selecciona el idioma `Español`
4. Selecciona el teclado Layout y Variant `Spanish (Latin America)`
5. Apreta "`Continuar`" este paso formateara todos los discos que encuentre automaticamente, y se reiniciara automaticamente
6. Luego instalara otras cosas se demora aproximadamente 900 segundos (15 minutos) y se reiniciara. No debes iniciar sesion

> Al iniciar, te saldra este prompt de inicio, y sera tapado por otras cosas, simplemente inicia con las credenciales
```
Eve-NG (default root password is 'eve')
Use http:///

eve-ng login:
```

> Default CLI Login Credentials
```
user: root
pass: eve
```
> Default WEB Login Credentials
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

> En este paso se reinicia automaticamente el VM, deberia aparecer la IP en el baneer para iniciar sesion

> [!INFO] Sobre Gestionar VM
> Teniendo la IP, ya puedes conectarte por ssh, su gestion es mucho mas comoda
> ```
> ssh root@{ip-eve-ng}
> ``` 

> Paso 3: Prueba de internet y Actualizar Paquetes Servidor. Si necesitas reiniciar servicios, reinicialos todos
```
ping -c 2 google.cl
apt update && apt upgrade
apt autoremove
```

> Paso 4: Hacer la vida mas sencilla a mi (Opcional)
```
apt install micro btop kitty weston git tree
```

> Paso 4.1: Instalar [Fish Shell](https://fishshell.com/) y [Fastfetch](https://github.com/fastfetch-cli/fastfetch) (Opcional)
```
sudo apt-add-repository ppa:fish-shell/release-3
sudo add-apt-repository ppa:zhangsongcui3371/fastfetch
sudo apt update
sudo apt install fish fastfetch
```

> Paso 4.2: Instalar [Fisher](https://github.com/jorgebucaran/fisher) (Opcional)
> Nota: Solo funciona cuando dentro de `fish`
```
curl -sL https://raw.githubusercontent.com/jorgebucaran/fisher/main/functions/fisher.fish | source && fisher install jorgebucaran/fisher
```

> Paso 4.3: Instalar [Tide](https://github.com/IlanCosman/tide) (Opcional)
```
fisher install IlanCosman/tide@v6
```

# Fase 2: Instalar Imagenes

Recuerda tener descargadas tus imagenes para pasarlas al servidor, puedes encontrar mas informacion en [[#Soporte de Imagenes]]

Existen 3 metodos para ejecutar imagenes: Dynamips, IOL y Qemu. Dynamips no lo veremos en este Write-Up (reemplaza su funcionamiento IOL). Por lo que es util saber la diferencia:
- IOL (IOS on Linux) son binarios que corren directamente en el kernel de linux, sin necesidad de emular hardware completo. Esto es lo que los hace liviandos en RAM y CPU
- QEMU: Emulacion completa de Hardware mediante imagenes de disco (qcow2), lo que permite correr sistemas operativos reales tal y como vienen del fabricante, por lo que gasta mas RAM y CPU

La mayoria de las imagenes que se usan en EVE-NG usan el metodo de QEMU, por eso es tan flexible, con la excepcion de Cisco IOL, por eso es el primero que explicare.

## Cisco IOL
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - Howto add Cisco IOL](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-cisco-iol-ios-on-linux/)
> - [BlackBox Blog (Ru) - Eve-NG Arreglar imagenes IOL](https://it-blackbox.blogspot.com/2018/06/eve-ng-cisco-iouiol.html)
> - [Github - ishare2-org/ishare2-cli](https://github.com/ishare2-org/ishare2-cli) | [Generate new iourc license](https://github.com/ishare2-org/ishare2-cli?tab=readme-ov-file#generate-a-new-iourc-license-for-bin-images)
> - Github CiscoIOUKeygen
> 	- [Github obscur95/CiscoIOUKeygen.py](https://raw.githubusercontent.com/obscur95/gns3-server/refs/heads/master/IOU/CiscoIOUKeygen.py)
> 	- [Github Gist twrandolphchen/CiscoIOUKeygen.py](https://gist.githubusercontent.com/twrandolphchen/ed3588e1128488868c243a432b4bcfb4/raw/de0391a2da1bf1da0e5ad6aee2702c6221ce7dd7/CiscoIOUKeygen.py)

> [!DANGER] Mira la version de las imagenes
> - Evitar usar la version `L3 15.5.2T` debido a que se congela en standby

Cisco IOL sigue un metodo propio heredado de los tiempos de WEBIOL (~2010), que luego copio IOU WEB, y despues se transformo en UNL y termino siendo EVE-NG. Su estructura ya viene integrada, solo hay que colocar los archivos en su lugar.

Las carpetas relevantes son:
- `/opt/unetlab/addons/iol/bin/`: Imagenes con extension "`.bin`", junto a un archivo "`iourc`" que actua como licencia
- `/opt/unetlab/addons/iol/lib/`: La libreria "`libcrypto.so.4`" (Openssl) necesaria para que los binarios de "`bin/`" funcionen

**Tabla IOL Imagen Recomendada**

| Type         | EVE Image Name                                                   | Version                                                                                                                                     | NVRAM | RAM  |
| ------------ | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- | ----- | ---- |
| L2/L3 Switch | i86bi_linux_l2-adventerprisek9-ms.<br>SSA.high_iron_20190423.bin | Cisco IOS Software, Linux Software <br>(I86BI_LINUXL2-ADVENTERPRISEK9-M),<br>Version 15.2(CML_NIGHTLY_20190423)                             | 1024  | 1024 |
| L3 Router    | i86bi_LinuxL3-AdvEnterpriseK9-<br>M2_157_3_May_2018.bin          | Cisco IOS Software, Linux Software (I86BI_LINUX-<br>ADVENTERPRISEK9-M), Version 15.7(3)M2,<br>Compiled Wed 28-Mar-18 11:18 by prod_rel_team | 1024  | 1024 |

**IOURC**

> [!NOTE] Sobre IOURC
> Este archivo varia segun los archivos `/etc/hostname` y `/etc/hosts` que esten configurados para el servidor, exactamente en `Hostname` y `Host_id`

> Usar `script.py` para conocer la licencia iourc
```
cd /opt/unetlab/addons/iol/bin/
python2 script.py
```

> El output del script copialo dentro de `/opt/unetlab/addons/iol/bin/iourc`
```
[license]
eve-ng = 972xxxxxxxxx1616;
```

**BIN**

> Crear Carpeta
```
mkdir /opt/unetlab/addons/iol/bin /opt/unetlab/addons/iol/lib
```

> Haz que las imagenes sean ejecutables
```
chmod 755 bin/*.bin
```

> Copia las imagenes desde tu pc al servidor
```
rsync -Phvr *.bin root@{ip-server}:/opt/unetlab/addons/iol/bin/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

**Probar las imagenes (Opcional)**

> Mover a la carpeta con los `.bin`
```
cd /opt/unetlab/addons/iol/bin
```

> Crea un `NETMAP`
```
touch NETMAP
```

> Ejecuta la imagen donde `{iosname.bin}` sea la imagen a probar
```
LD_LIBRARY_PATH=/opt/unetlab/addons/iol/lib/ /opt/unetlab/addons/iol/bin/{iosname.bin} 1
```

## Cisco IOS XE

> [!TIP] Lecturas Recomendadas
> - [Cisco CML-Free Docs](https://developer.cisco.com/docs/modeling-labs/cml-free/#installing-cml-free)
> - [Cisco Meraki - CML Free Register](https://mkto.cisco.com/cml-opt-in.html)
> - [Cisco Software](https://software.cisco.com/download/home/)
> 	- [CML Free](https://software.cisco.com/download/home/286193282/type/286326381/release/CML-Free)

Bueno, Cisco tiene una rama mas moderna de los IOS, llamada IOS XE, las cuales se distribuyen en CML, y no estan permitidas para su uso en EVE-NG, rompes la licencia al hacerlo, solo digo

Y es que Cisco cambio su forma de distribuir las imagenes, ahora las empaqueta en distintos YAML y apunta a blobs comprimidos identificados por su hash256... Lo cual no es nuevo, siempre han sido metodos confusos de hacer funcionar las cosas

Las ultima version que extraje fue la 17.18.02a del 12/May/2026, y la tabla de recomendaciones es la siguiente

| Type            | EVE Image Name                                 | Version                                                                                                                                       | NVRAM | RAM  |
| --------------- | ---------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- | ----- | ---- |
| L3 XE Router    | x86_64_crb_linux-adventerprisek9<br>-ms.bin    | IOL XE Router Cisco IOS Software [Dublin], Linux <br>Software (X86_64BI_LINUX-ADVENTERPRISEK9-M), <br>Version 17.12.1, RELEASE SOFTWARE (fc5) | 1024  | 1024 |
| L2/L3 XE Switch | x86_64_crb_linux_l2-adventerprisek9<br>-ms.bin | IOL XE Switch Cisco IOS Software [Dublin], Linux<br>Software (X86_64BI_LINUX_L2-ADVENTERPRISEK9-M), Version 17.12.1, RELEASE SOFTWARE (fc5)   | 1024  | 1024 |

Estas imagenes las puedes conseguir gratuitamente mediante los siguientes pasos
1. Crea una cuenta en Cisco Meraki para acceder a la tienda de descargas
2. Descarga el "reference platform" mas reciente, en mi caso "`refplat-20260409-free-iso.zip`" (12-May-2026). (Probablemente cuando lo leas haya uno mas nuevo, descarga obviamente el mas nuevo)
3. Descomprime el `.zip` y luego el `.iso` resultante
4. Entra a `virl-base-images` y busca la carpeta `iol-xe-17-18-02`
5. Descomprime el `.tar.gz` que encuentres
6. Abre la carpeta `blobs` y luego `sha256` y busca el archivo comprimido mas pesado del listado, en mi caso `ac697212b57ca1706f4a5618a2b11e42746eb8d6e11f797d2878999ca108c955`. Debes descomprimirlo y te extraerla la imagen con la extension `.iol`
7. Debes cambiar la extension de `.iol` a `.bin`
8. Y lo mueves a la carpeta magica de eve-ng, que no recuerdo cual es, oopsie
9. Repite lo mismo con la carpeta `ioll2-xe-17-18-02`

## Aruba CX Switch
> [!IMPORTANT] Documentacion Recomendada
> - [HPE Support](https://networkingsupport.hpe.com/home): Iniciar sesion con cuenta HPE Certificada
> 	- [Global Search - AOS-CX OVA](https://networkingsupport.hpe.com/globalsearch#q=AOS-CX%20OVA&tab=Software)
> - [HPE Aruba Networks - Techdocs - Changelog](https://arubanetworking.hpe.com/techdocs/AOS-CX/Consolidated_RNs/Portal_Home/Content/cx-home.htm)
> - [EVE-NG Docs - HowTo add Aruba CX Switch](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-aruba-cx-switch/)
> - [Via Internet Archive - My Ethernet Mind Blog - Adding Aruba AOS-CX to EVE-NG](https://web.archive.org/web/20240226171832/https://www.madari.co.il/2019/11/adding-aruba-aos-cx-to-eve-ng.html)

> [!NOTE] Sobre Imagen
> - Carpeta HPE Aruba CX Switch: `arubacx-{version}`
> 	- Disco QEMU: `virtioa`
> - Access
> 	- user: `admin`
> 	- pass: N/A

Como nota, utilizar Aruba CX Switch en EVE-NG rompe con su autorizacion de licencia adicional, solo digo...

Puedes descargar esta imagen gratuitamente desde HPE, solo debes crear una cuenta con dominio academico o corporativo, no permite iniciar desde un correo general como gmail, yahoo, outlook, icloud, etc.

Una vez con tu cuenta creada busca el termino "`AOS-CX OVA`" desde el buscador global de HPE Support. Veras varias ramas activas disponibles, al momento de escribir, estas son:
- 10.18.001
- 10.17.1020
- ...
- 10.13.1180 (LTS)

Te recomiendo ver el Changelog desde Aruba, para ver cuales son los ultimos cambios de los branchs

> Descomprime el .zip que contiene el OVA
```
7z x AOS-CX_Switch_Simulator_10_18_0001_ova.zip
```

> Luego descomprime el OVA
```
7z x AOS-CX_10_18_0001.ova
```

> Convierte el archivo vmdk a qcow2
```
qemu-img convert -f vmdk -O qcow2 arubaoscx-disk-image-genericx86-p4-20260521162224.vmdk virtioa.qcow
```

> Crea la carpeta en el servidor
```
mkdir /opt/unetlab/addons/qemu/arubacx-{version}
```

> Copia la imagen al servidor
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/arubacx-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

## Cisco
### ASAv

> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add Cisco ASAv](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-cisco-asav/)

> [!NOTE] Nombre Imagen
> - Carpeta ASAv: `asav-{version}`
> 	- Disco QEMU: `virtioa`

El metodo para conseguir las imagenes es igual que con los [[#Cisco IOS XE]], el nombre dentro de la carpeta `virl-base-images` son
- ASAv: `asav-9-24-1`

> Crear las carpetas para ASAv
```
mkdir /opt/unetlab/addons/qemu/asav-{version}
```

> Envia las imagenes a la carpeta ASA
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/asav-{version}/
```

> Arregla permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### Cisco vIOS (EX-VIRL)

> [!IMPORTANT] Documentacion Recomendada
> [EVE-NG Docs - HowTo add Cisco ViOS from virl](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-cisco-vios-from-virl/)

> [!NOTE] Sobre Imagen
> - Carpeta L3: `vios-{version}`
> 	- Disco QEMU: `virtioa`
> - Carpeta L2: `viosl2-{version}`
> 	- Disco QEMU: `virtioa`

El metodo para conseguir las imagenes es igual que con los [[#Cisco IOS XE]], el nombre dentro de la carpeta `virl-base-images` son
- L3: `iosv-159-3-m12`
- L2: `iosvl2-2020`

> Crea carpeta L3
```
mkdir /opt/unetlab/addons/qemu/vios-{version}
```

> Crea Carpeta L2
```
mkdir /opt/unetlab/addons/qemu/viosl2-{version}
```

> Envia las imagenes a la carpeta correspondiente
```
rsync -Phvr vios-{version}.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/vios-{version}/
```

> Cambia el nombre
```
mv vios-{version}.qcow2 virtioa.qcow2
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### C9800CL
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add Cisco Wireless C9800CL](https://www.eve-ng.net/index.php/documentation/howtos/cisco-wireless-c9800-cl/)
> - [JD-Networks Blog - C9800CL on EVE-NG](https://jd-networks.co.uk/blog/2019/09/30/cisco-9800-cl-on-eve-ng/)

> [!NOTE] Nombre Imagen
> - Carpeta Cisco Wireless C9800CL: `c9800cl-{version}`
> 	- Disco QEMU: `virtioa`

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/c9800cl-{version}
```

> Envia las imagenes a la carpeta
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/asa-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### CSR1000v y CSR1000vng

> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add CSR1000vng](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-cisco-csrv1000-16-x-denali-everest-fuji/)
> - [EVE-NG Docs - HowTo add CSR1000vng-sdwan](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-cisco-csrv1000-sd-wan/)

> [!NOTE] Nombre Imagen
> - Carpeta CSR1000v: `csr1000v-{version}`
> 	- Disco QEMU: `virtioa`
> - Carpeta CSR1000vng: `csr1000vng-{version}`
> 	- Disco QEMU: `virtioa`
> - Login
> 	- User: `admin`
> 	- Pass: `admin`

> Crea la carpeta para CSR1000v
```
mkdir /opt/unetlab/addons/qemu/csr1000v-{version}
```

> Crea la carpeta para CSR1000vng
```
mkdir /opt/unetlab/addons/qemu/csr1000vng-{version}
```

> Mueve la imagen para CSR1000v
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/csr1000v-{version}/
```

> Mueve la imagen para CSR1000vng
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/csr1000vng-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### Cisco Viptela SD-WAN

> [!WARNING] Peso y Requisitos
> El nodo de vtmgmt es extremadamente pesado, necesita 100GB de espacio extra y 32GB de ram para correr correctamente

> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add Cisco SDWAN Viptela image set](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-cisco-viptela-images-set/)
> - [Network Academy Blog - Cisco SD-WAN on EVE-NG](https://www.networkacademy.io/ccie-enterprise/sdwan/cisco-sd-wan-on-eve-ng)
> - [Youtube - Michael O'Briens CCIE Journal - How to create Smart Account and License file for Cisco SD-WAN](https://youtu.be/Caze1TZldCM?si=tqOw6fs_mmWNqM1Y)

> [!NOTE] Nombre Imagen
> - Carpeta Cisco Viptela SD-WAN vtbond: `vtbond-{version}`
> 	- Disco QEMU: `virtioa`
> - Carpeta Cisco Viptela SD-WAN vtedge: `vtedge-{version}`
> 	- Disco QEMU: `virtioa`
> - Carpeta Cisco Viptela SD-WAN vtsmart: `vtsmart-{version}`
> 	- Disco QEMU: `virtioa`
> - Carpeta Cisco Viptela SD-WAN vtmgmt: `vtmgmt-{version}`
> 	- Disco QEMU: `virtioa`
> - Login
> 	- User: `admin`
> 	- Pass: `admin`

> Crear carpetas (de los 4 en sus respectivos lugares)
```
mkdir /opt/unetlab/addons/qemu/vtbond-{version}
```

> Copia las imagenes (de los 4 en sus respectivos lugares)
```
rsync -Phvr vtbond-{version}.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/vtbond-{version}
```

> Corrige el nombre (de los 4 en sus respectivos lugares)
```
mv vtbond-{version}.qcow2 virtioa.qcow2
```

> Ve a la carpeta de Cisco Viptela SD-WAN vtmgmt
```
cd /opt/unetlab/addons/qemu/vtmgmt-{version}
```

> Crea un segundo disco de 100GB llamado "`virtiob.qcow2`"
```
/opt/qemu/bin/qemu-img create -f qcow2 virtiob.qcow2 100G
```

> Aregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

## Extreme Networks

### ExtremeVOSS
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add Extreme VOSS](https://www.eve-ng.net/index.php/documentation/howtos/extreme-voss/)
> - [Github - extremenetworks/Virtual_VOSS](https://github.com/extremenetworks/Virtual_VOSS)
> - [Extreme Networks Docs](https://supportdocs.extremenetworks.com/support/documentation/)
> 	- [VOSS](https://supportdocs.extremenetworks.com/support/documentation/vsp-operating-system-software-voss-document-collections/) (Legacy)
> 	- [Fabric Engine](https://supportdocs.extremenetworks.com/support/documentation/fabric-engine-document-collections/)

> [!NOTE] Nombre Imagen
> - Carpeta Extreme VOSS: `extremevoss-{version}`
> 	- Disco QEMU: `hda`
> - Credenciales
> 	- User: `rwa`
> 	- Pass: `rwa`

VOSS significa VSP Operating System Software -> Fabric Engine

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/extremevoss-{version}
```

> Mueve el archivo
```
rsync -Phvr hda.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/extremevoss-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### ExtremeXOS

> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add Extreme EXOS](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-extreme-exos/)
> - [Github - extremenetworks/Virtual_EXOS](https://github.com/extremenetworks/Virtual_EXOS)
> - [Extreme Networks Docs - ExtremeXOS](https://supportdocs.extremenetworks.com/support/documentation/extremexos-33-6-1/) (Legacy)
> - [Extreme Networks Docs - Switch Engine 33.6.1](https://supportdocs.extremenetworks.com/support/documentation/switch-engine-33-6-1/)

> [!NOTE] Nombre Imagen
> - Carpeta Extreme EXOS: `extremexos-{version}`
> 	- Disco QEMU: `hda`
> - Credenciales
> 	- User: `admin`
> 	- Pass: N/A

Desde la version 31.6, EXOS ahora pasa a ser Switch Engine

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/extremexos-{version}
```

> Mueve el archivo
```
rsync -Phvr hda.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/extremexos-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

## F5 BigIP
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add F5 BigIP](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-f5-bigip/)

> [!NOTE] Nombre Imagen
> Durante la instalacion, debes elegir VNC
> - Carpeta Big IP: `bigip-{version}`
> 	- Disco QEMU: `virtioa`
> - CLI Login
> 	- User: `root`
> 	- Pass: `default`
> - WEB Login
> 	- User: `admin`
> 	- Pass: `admin`

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/bigip-{version}
```

> Mueve la imagen
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/bigip-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

## Fortinet

> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - Fortinet](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-fortinet-images/)
> - [Fortinet Docs](https://docs.fortinet.com/)
> - [Fortinet Training](https://training.fortinet.com/)
> - [Fortinet Video](https://video.fortinet.com/)
> - [Fortinet Community](https://community.fortinet.com/)
> 	- [How to run a real-time Wireshark inside FortiGate](https://community.fortinet.com/t5/FortiGate/Technical-Tip-How-to-run-a-real-time-Wireshark-capture-on/ta-p/213805)
> - [Github - hegdepavankumar/Fortigate-Firewall-Complete-Guide](https://github.com/hegdepavankumar/Fortigate-Firewall-Complete-Guide) | [Web Version](https://hegdepavankumar.github.io/Fortigate-Firewall-Complete-Guide/)
> - [Reddit - Tricks and tips for new and old players](https://old.reddit.com/r/fortinet/comments/lnxv0h/fgtfazfmg_tricks_and_tips_for_new_and_old_players/)

> [!TIP] Significado Nombres
> Fuentes: [Fortinet Customer - Deciphering abbreviations for Fortinet products](https://community.fortinet.com/t5/Customer-Service/Technical-Tip-Deciphering-abbreviations-for-Fortinet-products/ta-p/196062)
> - FAZ: FortiAnalyzer
> - FAC: FortiAuthenticator
> - FGT: Fortigate
> - FMG: FortiManager
> - FNDR: FortiNDR (Network Detection and Response)
> - FWB: FortiWeb

> [!NOTE] Nombre Imagen
> - Carpeta FAC, FGT y FNDR: `fortinet-{version}`
> 	- Disco QEMU: `virtioa`
> - Login
> 	- User: `admin`
> 	- Pass: `N/A`
> - WebLogin
> 	- User: `admin`
> 	- Pass: `N/A`

> Crear carpeta para FAC
```
mkdir /opt/unetlab/addons/qemu/fortinet-FAC-{version}
```

> Crear carpeta para FGT
```
mkdir /opt/unetlab/addons/qemu/fortinet-FGT-{version}
```

> Crear carpeta para FNDR
```
mkdir /opt/unetlab/addons/qemu/fortinet-FNDR-{version}
```

> Mueve la imagen para FAC
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/fortinet-FAC-{version}/
```

> Mueve la imagen para FGT
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/fortinet-FGT-{version}/
```

> Mueve la imagen para FNDR
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/fortinet-FNDR-{version}/
```

> Arregla permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

## Hillstone
> [!WARNING] Sobre uso de lab
> Es un vendor Chino, y segun tengo entendido, te deja utilizarlo por 30 dias y luego se autodestruye, asi que puede ser un poco incomodo para laboratorios que duren mas de 30 segundos. Pero tendria que probarlo

> [!IMPORTANT] Documentacion Recomendada
> - [Passport Hillstone - Registrar Cuenta](https://passport.hillstonenet.com/Account/Register)
> - [Hillstone Images - Login](https://images.hillstonenet.com/index/user/login.html)
> - [Docs Tecnicos](https://docs.hillstonenet.com/web/) | [Chino (Mas completos)](https://docs.hillstonenet.com.cn/web/)

Debes crearte una cuenta y verificarla desde el correo, y luego iniciar sesion en el portar de imagenes, alli ya puedes descargar las ultimas versiones de cada imagen

Tambien EVE-NG solo tiene consideracion por un tipo de imagen, asi que supongo que se utilizara el selector, deberia de funcionar de igual forma

### CloudEdge

> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - Hillstone](https://www.eve-ng.net/index.php/documentation/howtos/hillstone-firewall/)
> - [Hillstone - CloudEdge Firewall Showcase](https://www.hillstonenet.com/products/cloud-protection/cloud-security-cloudedge/)
> - [Hillstone Images - CloudEdge NGFW](https://images.hillstonenet.com/index/index/content?cid=59)
> - [Hillstone Docs (CN) - NGFW A/B Series](https://docs.hillstonenet.com.cn/web/doc-list/30) | [En (A Series)](https://docs.hillstonenet.com/web/doc-list/13) | [En (E Series)](https://docs.hillstonenet.com/web/doc-list/14)

> [!NOTE] Nombre Imagen
> - Carpeta Hillstone CloudEdge: `hillstone-sg6000-{version}`
> 	- Disco QEMU: `hda`
> - Login
> 	- User: `hillstone`
> 	- Pass: `hillstone`

La ultima version que encontre fue
- `SG6000-CloudEdge-5.5R12P2.44-v6.qcow2 - Tamaño: 268,2 MB - Última actualización: 30/06/2026 17:53:24`

> Crear carpeta para CloudEdge
```
mkdir /opt/unetlab/addons/qemu/hillstone-sg6000-CloudEdge-{version}
```

> Mueve el archivo
```
rsync -Phvr SG6000-CloudEdge-5.5R12P2.44-v6.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/hillstone-sg6000-CloudEdge-{version}/hda.qcow2
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### vADC
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - Hillstone](https://www.eve-ng.net/index.php/documentation/howtos/hillstone-firewall/)
> - [Hillstone - vADC Showcase](https://www.hillstonenet.com/products/application-protection/application-delivery-controller/)
> - [Hillstone Images - vADC AX Series](https://images.hillstonenet.com/index/index/content?cid=81)
> - [Hillstone Docs (CN) - vADC](https://docs.hillstonenet.com.cn/web/doc-list/28) | [En](https://docs.hillstonenet.com/web/doc-list/9)

Yo encontre
- `SG6000-vADC-5.5R12-5.0-v6.qcow2 - Tamaño: 309,9 MB - Última actualización: 28/05/2026 22:26:44`

> Crear carpeta para vADC
```
mkdir /opt/unetlab/addons/qemu/hillstone-sg6000-vADC-{version}
```

> Mueve el archivo
```
rsync -Phvr SG6000-vADC-5.5R12-5.0-v6 root@{ip-server}:/opt/unetlab/addons/qemu/hillstone-sg6000-vADC-{version}/hda.qcow2
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### vIPS
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - Hillstone](https://www.eve-ng.net/index.php/documentation/howtos/hillstone-firewall/)
> - [Hillstone - NIPS/DIPS Showcase](https://www.hillstonenet.com/products/network-edge-protection/network-intrusion-prevention-system/)
> - [Hillstone Image - NIPS/DIPS A.K.A vIPS](https://images.hillstonenet.com/index/index/content?cid=60)
> - [Hillstone Docs (CN) - NIPS](https://docs.hillstonenet.com.cn/web/doc-list/33) | [En](https://docs.hillstonenet.com/web/doc-list/16)

Yo encontre
- `SG6000-vIPS-5.5R12-6.2-v6.qcow2 - Size: 288.6 MB - Última actualización: 2026-03-25 14:29:11`

> Crear carpeta para vIPS
```
mkdir /opt/unetlab/addons/qemu/hillstone-sg6000-vIPS-{version}
```

> Mueve el archivo
```
rsync -Phvr SG6000-vIPS-5.5R12-6.2-v6 root@{ip-server}:/opt/unetlab/addons/qemu/hillstone-sg6000-vIPS-{version}/hda.qcow2
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

## Huawei
> [!IMPORTANT] Documentacion Recomendada
> - [Networking Hints Blog - Huawei NE40 and CE12800 on EVE-NG](https://networking-hints.blogspot.com/2021/01/huawei-ne40-12800-on-eve-ng.html)
> - [Youtube - Deploy Huawei NE40E and CE12800 on EVE-NG](https://youtu.be/8XgdSGLODD4?si=TOyf1mVb7pr5LhUj)
> - [KevinJin - Huawei in Eve-NG](https://www.kevinjin.com/posts/eve-ng/eve-ng/)

### AR1000
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add Huawei AR1000v](https://www.eve-ng.net/index.php/documentation/howtos/huawei-ar1000v/)

> [!NOTE] Nombre Imagen
> - Carpeta Huawei AR1000: `huaweiar1k-{version}`
> 	- Disco QEMU: `hda`

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/huaweiar1k-5.170
```

> Mueve el archivo
```
rsync -Phvr hda.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/huaweiar1k-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### USG6000v
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add Huawei USG6000v](https://www.eve-ng.net/index.php/documentation/howtos/huawei-usg6000v/)

> [!NOTE] Nombre Imagen
> - Carpeta Huawei USG6000v: `huaweiusg6kv-{version}`
> 	- Disco QEMU: `hda`
> - Login (USG6kv):
> 	- User: `admin`
> 	- Pass: `Admin@123`

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/huaweiusg6kv-{version}
```

> Mueve el archivo
```
rsync -Phvr hda.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/huaweiusg6kv-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### NE40E
(WIP)
> [!IMPORTANT] Documentacion Recomendada
> - [Huawei Forums - NE40e image](https://forum.huawei.com/enterprise/intl/en/thread/ne40e-image-for-ensp-v100r003c00spc100/667245683289243648?blogId=667245683289243648)

> [!NOTE] Nombre Imagen
> - Carpeta Huawei NE40e: `???-{version}`
> 	- Disco QEMU: `???`

### CE12800
(WIP)
> [!IMPORTANT] Documentacion Recomendada
> - [Huawei Forums - Run CE12800 in EVE-NG](https://forum.huawei.com/enterprise/intl/en/thread/run-ce12800-ne40e-in-eve-ng/667237045992570881?blogId=667237045992570881)

> [!NOTE] Nombre Imagen
> - Carpeta Huawei CE12800: `???-{version}`
> 	- Disco QEMU: `???`

## Linux
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo create own Linux Host Image](https://www.eve-ng.net/index.php/documentation/howtos/howto-create-own-linux-host-image/)
> - [Youtube - The Network Berg - EVE-NG Importing a Linux host](https://youtu.be/ZLvdJa3MXTU?si=ud_AM3k1wUfK0UuC)
> - [EVE-NG Docs - Mega - Download Linux Images](https://mega.nz/folder/30p3TKob#42_S__9wwPVO0zHIfC4xow)

> [!TIP] Credenciales Generales
> - root/root
> - root/eve
> - user/Test123
> - root/Test123
> - root/toor (Kali 2019.3 with RDP)

### Alpine

> [!TIP] Lecturas Recomendadas
> - [Sitio Oficial](https://www.alpinelinux.org/)

> [!NOTE] Nombre Imagen
> - Carpeta Alpine: `linux-alpine-{version}`
> 	- Disco QEMU: `virtioa.qcow2`
> - Login
> 	- User: `alpine`
> 	- Pass: `???`

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/linux-alpine-{version}
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### Arch Linux
(WIP)
> [!IMPORTANT] Documentacion Recomendada
> - [Gitlab - archlinux/arch-boxes](https://gitlab.archlinux.org/archlinux/arch-boxes)
> 	- [Fastly Mirro - Latest Image](https://fastly.mirror.pkgbuild.com/images/latest/)
> 	- [Geo Mirror - Latest Image](https://geo.mirror.pkgbuild.com/images/latest/)

> [!NOTE] Nombre Imagen
> - Carpeta Arch Linux: `Linux-archlinux-{version}`
> 	- Disco QEMU: `virtioa`
> - Login
> 	- User: `arch`
> 	- Pass: `arch`

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/linux-archlinux-20260615
```

> Descarga la ultima imagen Base
```
wget https://fastly.mirror.pkgbuild.com/images/latest/Arch-Linux-x86_64-basic.qcow2 -O virtioa.qcow2
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```


### Freebsd
(WIP)
> [!IMPORTANT] Documentacion Recomendada
> - 

> [!NOTE] Nombre Imagen
> - Carpeta Freebsd: `asa-{version}`
> 	- Disco QEMU: `hda`

****

### Issabel
> [!TIP] Lecturas Recomendadas
> - [SourceForge - issabelofficial/IssabelPBX Files](https://sourceforge.net/projects/issabelpbx/files/)

Esta es una imagen creada desde 0 pero eve-ng

Primero descargas la ISO desde la pagina oficial de Sourgeforce

Cambias el nombre de "`issabel5-USB-DVD-x86_64-20240430.iso`" a "`cdrom.iso`"

Luego creas la carpeta en eve-ng en la ruta "`/opt/unetlab/addons/qemu/`" llamada "`linux-issabel-20240430`"

Envias el archivo "`cdrom.iso`" a la carpeta antes creada

Crea un disco de 10GB
```
/opt/qemu/bin/qemu-img create -f qcow2 virtioa.qcow2 10G
```

Conecta el nodo a la interfaz en la nube "Cloud0 Management" para que pueda conectarse a internet

Crea el nuevo nodo creado, y configura su instalacion

Apreta "Test this media and install" y espera un buen rato hasta que aparesca el instalador grafico

Luego selecciona el idioma y le das en siguiente

Seleccionas "Teclado" y le das en "Hecho"

Seleccionas "Contraseña de Root" y creas la contraseña "eve"

Creas el usuario "eve" con contraseña "eve"

Seleccionas "Internet" y deberia estar 

Luego que se instale, apagas la maquina, haces la magia del pod, y haces commit a la imagen

### Uso de Contenedores

EVE-NG Community no permite crear nodos Docker Nativos, pero eso no significa que Docker (o incluso Kubernetes) no funcionen. La solucion que veo es tratar al contenedor como lo que es: Software que corre sobre Linux, y Linux si es un nodo normal de EVE-NG.

La idea es construir una imagen base reutilizable:
1. Crea un nodo con la distribucion Linux de tu eleccion y conectalo temporalmente a `Cloud0` (o la red management bridge que desees) para que tenga salida directa a internet.
2. Realiza la instalacion normal de la distro
3. Instala los paquetes necesarios, como Docker, containerd, kubeadm o lo que desees
4. Una vez lista, apaga el nodo y haz commit de la imagen, ahora esa es tu plantilla base

Una vez que tengas tu nodo clonado y listo para la topologia, si necesitas imagenes adicionales de contenedores, desconectalo de la red del lab, conectalo temporalmente de vuelta a `Cloud0`, haz pull de lo que necesites, apaga el nodo y reconectalo a la topologia. De esta forma el lab no necesita conectividad permanente a internet y puedes explorar el comportamiento de los contenedores sin tener que tener una via a internet directa

## MS Windows
(WIP)
### MS Win Host (Win XP, 7, 8.1, 10, 11)
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add MS Windows Host](https://www.eve-ng.net/index.php/documentation/howtos/howto-create-own-windows-host-on-the-eve/)

> [!NOTE] Nombre Imagen
> - Carpeta MS Windows Host: `???-{version}`
> 	- Disco QEMU: `???`


### MS Win Server (2008-2025)
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add MS Windows Server](https://www.eve-ng.net/index.php/documentation/howtos/howto-create-own-windows-server-on-the-eve/)

> [!NOTE] Nombre Imagen
> - Carpeta MS Windows Server: `???-{version}`
> 	- Disco QEMU: `???`


## Mikrotik RouterOS
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add Microtik Cloud Router](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-mikrotik-cloud-router/)
> - [Microtik Download](https://mikrotik.com/download/chr): Ve a CHR (Cloud Hosted Router) -> Disco "`RAW Disk Image`"
> - [Microtik Manual](https://manual.mikrotik.com/docs/introduction)

> [!NOTE] Nombre Imagen
> - Carpeta Mikrotik: `mikrotik-{version}`
> 	- Disco QEMU: `hda`
> - Login
> 	- User: `admin`
> 	- Pass: N/A

> Creas la carpeta
```
mkdir /opt/unetlab/addons/qemu/mikrotik-{version}
```

> Enviar el archivo
```
rsync -Phvr chr-{version}.img root@{ip-server}:/opt/unetlab/addons/qemu/mikrotik-{version}/
```

> Mover el archivo
```
mv chr-{version}.img /opt/unetlab/addons/qemu/mikrotik-{version}/hda.qcow2
```

> Arreglar Permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

## OpenWRT

Okey, es posible instalarlo y hacerlo funcionar, pero las capacidades Wireless no son soportadas en EVE-NG, asi que luego de hacerlo funcionar, estas solo practicamente

> [!IMPORTANT] Documentacion Recomendada
> - [Github Gist - rodrigojusto/Eve-NG - OpenWRT x86](https://gist.github.com/rodrigojusto/684308f6d65ac86a3c845912cee86789)
> - [OpenWRT Download 25.12.4](https://downloads.openwrt.org/releases/25.12.4/targets/x86/64/)
> - [OpenWRT Docs - Run in QEMU x86-64](https://openwrt.org/docs/guide-user/virtualization/qemu#openwrt_in_qemu_x86-64)

> [!NOTE] Nombre Imagen
> - Carpeta OpenWRT: `???-{version}`
> 	- Disco QEMU: `???`


## OPNsense
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add OPNsense](https://www.eve-ng.net/index.php/documentation/howtos/opnsense-firewall/)
> - [Pagina Oficial](https://opnsense.org/)

> [!NOTE] Nombre Imagen
> - Carpeta OPNsense: `opnsense-{version}`
> 	- Disco QEMU: `virtioa`
> - Default Login CLI y Web
> 	- User: `root`
> 	- Pass: `opnsense`

Al configurar el nodo, dale 4GB de ram

VNET0 es LAN
VNET1 es WAN

Se debe pasar la cdrom.iso, crear un disco de 15G, e instalar OPNsense

Luego que se termine de instalar, detienes la maquina, eliminas el cdrom y haces commit a la imagen

## Palo Alto
> [!IMPORTANT] Documentacion Recomendada
> - [Eve-NG Docs - Palo Alto](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-palo-alto/)
> - [Endoflife - PAN-OS](https://endoflife.date/panos)

> [!NOTE] Nombre Imagen
> - Carpeta Palo Alto: `paloalto-{version}`
> 	- Disco QEMU: `virtioa`
> - Login
> 	- User: `admin`
> 	- Pass: `admin`

Encontre la version 11.2.5

Si eres mas exotico esta la version [Sysin - PAN-OS 12.1.7 KVM](https://sysin.org/blog/pan-os-12/) for 5USD in Alipay...

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/paloalto-{version}
```

> Envia el Qcow2 al servidor
```
rsync -Phvr PA-VM-KVM-{version}.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/PA-VM-KVM-{version}/virtioa.qcow2
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

## PFsense

Prefiere [[#OPNsense]]

> [!WARNING] Pobre NetGate
> Fuentes
> - [Netgate Blog - Release PFsense CE 2.8.0](https://www.netgate.com/blog/netgate-releases-pfsense-community-edition-version-2.8.0)
> - [Netgate Forums - PFsense 2.8.0 full iso img](https://forum.netgate.com/topic/197601/pfsense-2-8-0-full-iso-img)
> Un pequeño Ranteo, posiblemente a netgate no le guste que usen su software, pero cada vez se vuelve un poco mas horrible de utilizar, lanzaron la version 8.2.0 el 28 de Mayo de 2025, la cual solo permite instalacion Online, por lo que debes tener una cuenta, asociar tu tarjeta de debido/credito, poner informacion personal, aceptar el EULA, y alli recien puedes descargar una version Community Gratuita, me parecio extraño, se podia saltar en la version 2.7.2.
> Ahora la version 2.8.0 solo permite "Netgate Installer - AMD64 ISO IPMI/VM"

> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add PFsense](https://www.eve-ng.net/index.php/3380-2/)
> - [PFsense Direct Download Directory for 2.7.2](https://atxfiles.netgate.com/mirror/downloads/)

> [!NOTE] Nombre Imagen
> - Carpeta PFsense: `pfsense-{version}`
> 	- Disco QEMU: `virtioa`
> - Login
> 	- User: `admin`
> 	- Pass: `pfsense`

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/pfsense-{version}
```

> Copia el Archivo (Cambia "`netgate-installer-amd64.iso`" a "`cdrom.iso`")
```
rsync -Phvr cdrom.iso root@{ip-server}:/opt/unetlab/addons/qemu/pfsense-{version}/
```

> cd a la carpeta
```
cd /opt/unetlab/addons/qemu/pfsense-{version}
```

> Crea un disco de 8GB
```
/opt/qemu/bin/qemu-img create -f qcow2 virtioa.qcow2 8G
```

> [!IMPORTANT] Selecciona correctamente
> Debes crear un lab y nodo de pfsense, seleccionando el tipo de consola a "`VNC`"

> Empieza la instalacion, selecciona HDD e instala pfsense
```
OK
```

> Cuando termine el instalador, presiona YES y apagas la maquina
```
YES
```

> Apretas la barra lateral izquierda, eliges "Lab Details" y copias el Lab-UUID del laboratorio
```
ejemplo: ID: 85bd7141-e2a7-43e6-8307-bfa7b301a12b
```

> Debes reconocer tu POD ID, esta en la pestaña de usuarios, 0 es el default
```
ejemplo: POD-ID: 0
```

> El nodo se puede obtener haciendo click derecho en el nodo, el numero que salga al lado del nombre, ese es el NODE-ID
```
ejemplo: NODE-ID: 1
```

> Te mueves a la carpeta reuniendo los valores en el orden `/opt/unetlab/tmp/{POD-ID}/{Lab-UUID}/{NODE-ID}`
```
ejemplo: cd /opt/unetlab/tmp/0/85bd7141-e2a7-43e6-8307-bfa7b301a12b/1/
```

> Haces commit a la imagen
```
/opt/qemu/bin/qemu-img commit virtioa.qcow2
```

> Te vas otra vez a la carpeta de pfsense
```
cd /opt/unetlab/addons/qemu/pfsense-{version}
```

> Eliminas el archivo cdrom.iso
```
rm -f cdrom.iso
```

> Arreglas los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

## Juniper

> [!TIP] Documentacion Recomendada
> - [Juniper Learning Portal - Open Learning](https://learningportal.juniper.net/juniper/user_activity_info.aspx?id=JUNIPER-OPEN-LEARNING)

Hay unos reemplazos que aclaran el panorama, los viejos son considerados EOL
- vMX -> vJunos Router or Evolved
- vQFX -> vJunos EX Switch
- vSRX -> vSRX 3.0

No considero el uso de Apstra AOS ni SDWAN 128T

### vJunos Router
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - vJunos-Router](https://www.eve-ng.net/index.php/documentation/howtos/vjunos-router/)
> - [Juniper Support - Download vJunos-Router](https://support.juniper.net/support/downloads/?p=vjunos-router): Seleccionar el OS "vJunos-Router"
> - [Juniper Docs](https://www.juniper.net/documentation/)
> 	- [vJunos-Router Docs](https://www.juniper.net/documentation/product/us/en/vjunos-router/)
> 	- [vJunos-Router HW requirements](https://www.juniper.net/documentation/us/en/software/vjunos-router/vjunos-router-kvm/topics/vjunos-router-kvm-hw-requirements.html)

> [!NOTE] Nombre Imagen
> - Carpeta vJunos-Router: `vjunosrouter-{version}`
> 	- Disco QEMU: `virtioa`
> - Login
> 	- User: `root`
> 	- Pass: N/A

Este es un Router Clasico de proposito general para laboratorios donde trabajes con BGP, OSPF, MPLS basico, con un comportamiento basado en vMX

Descarga la ultima version disponible, en mi caso `26.2R1`

> En el servidor crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/vjunosrouter-{version}
```

> Envia la imagen Qcow2 descargada
```
rsync -Phvr vjunosrouter-{version}.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/vjunosrouter-{version}/virtioa.qcow2
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### vJunos Evolved
> [!IMPORTANT] Documentacion Recomendada
> - [Eve-NG Docs - vJunos Evolved Router](https://www.eve-ng.net/index.php/documentation/howtos/juniper-vjunos-evo-router/)
> - [Juniper Support - Download vJunos Evolved](https://support.juniper.net/support/downloads/?p=vjunos-evolved)
> - [Juniper Docs](https://www.juniper.net/documentation/)
> 	- [vJunos Evolved Docs](https://www.juniper.net/documentation/product/us/en/vjunosevolved/)
> 	- [vJunos Evolved HW Requeriments](https://www.juniper.net/documentation/us/en/software/vJunosEvolved/vjunos-evolved-kvm/topics/vjunos-evolved-hw-sw-requirements.html)
> - [Juniper Community - Shalini Mukherjee - Deploying and Using vJunos in a Bare Metal EVE-NG server](https://community.juniper.net/blogs/shalini-mukherjee/2023/05/11/deploying-vjunos-in-a-bare-metal-eve-ng-server)

> [!NOTE] Nombre Imagen
> - Carpeta vJunos Evolved: `vjunosevo-{version}`
> 	- Disco QEMU: `virtioa`
> - Login
> 	- User: `root`
> 	- Pass: N/A

Orientado a laboratorios modernos con eVPN o VXLAN mas cercano a Junos OS

vJunos-Evolved se construye tomando como referencia el PTX10001-36MR

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/vjunosevo-{version}
```

> Envia el Qcow2 al servidor
```
rsync -Phvr vjunosevo-{version}.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/vjunosevo-{version}/virtioa.qcow2
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

NOTA: Debes leer la documentacion de EVE-NG, debes crear 4 enlaces con nombres especificos para que pueda reconocer las interfaces correctamente
```
For **EVE Community** you need to add **two new bridge networks** per node named ‘RPIO’ and ‘PFE’, as seen below, and connect it to the node via dual links. These PFE and RPIO bridges are required for vJunosEvolved to map virtual eth interfaces correctly.
```

### vJunos EX Switch
> [!IMPORTANT] Documentacion Recomendada
> - [Eve-NG Docs - vJunos EX Switch](https://www.eve-ng.net/index.php/documentation/howtos/vjunos-ex-switch/)
> - [Juniper Support - Download vJunos EX Switch](https://support.juniper.net/support/downloads/?p=vjunos)
> - [Juniper Docs](https://www.juniper.net/documentation/)
> 	- [vJunos-Switch Docs](https://www.juniper.net/documentation/product/us/en/vjunos-switch/)
> 	- [vJunos-Switch Architecture](https://www.juniper.net/documentation/us/en/software/vJunos/vjunos-switch-deployment-guide-for-kvm/vJunos-switch-kvm/topics/vJunos-switch-architecture-concept.html)

> [!NOTE] Nombre Imagen
> - Carpeta vJunos EX Switch: `vjunosswitch-{version}`
> 	- Disco QEMU: `virtioa`
> - Login
> 	- User: `root`
> 	- Pass: N/A

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/vjunosswitch-{version}
```

> Envia el Qcow2 al servidor
```
rsync -Phvr vjunosswitch-{version}.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/vjunosswitch-{version}/virtioa.qcow2
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

> NOTA: Este switch se apaga desde CLI antes de apagarlo desde la WEBUI
```
request system power-off
```

### vSRX 3.0
> [!IMPORTANT] Documentacion Recomendada
> - [Eve-NG Docs - vSRX 3.0 or Later](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-juniper-vsrx-ng-15-x-and-later/)
> - [Juniper Support - Download vSRX 3.0](https://support.juniper.net/support/downloads/?p=vsrx3)
> - [Juniper Docs](https://www.juniper.net/documentation/)
> 	- [vSRX Docs](https://www.juniper.net/documentation/product/us/en/vsrx/)
> 	- [SW Licenses for vSRX vFirewall](https://www.juniper.net/documentation/us/en/software/license/juniper-licensing-user-guide/topics/concept/licenses-for-vsrx.html)
> 	- [Requeriments for vSRX vFirewall on KVM](https://www.juniper.net/documentation/us/en/software/vsrx/vsrx-consolidated-deployment-guide/vsrx-kvm/topics/concept/security-vsrx-system-requirement-with-kvm.html)

> [!NOTE] Nombre Imagen
> - Carpeta Juniper vSRX: `vsrxng-{version}`
> 	- Disco QEMU: `virtioa`

vSRX 3.0 es la version virtualizada de los Firewall SRX de Juniper, por lo que tiene la misma CLI y Junos OS que el HW fisico. Trabaja con zonas de seguridad (trust, untrust, dmz) y politicas entre zonas. SIn licencia puedes usar todo lo Standard, que es Stateful Firewall, NAT, VPN, IPsec/SSL y routing. las funciones avanzadas como IPS, antivirus y filtrado web necesitan una licencia para utilizarse, de igual forma, un laboratorio no necesita ser tan fancy.

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/vsrxng-{version}
```

> Envia el Qcow2 al servidor
```
rsync -Phvr junos-vsrx3-{version}.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/vsrxng-{version}/virtioa.qcow2
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```


## VyOS
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add VyOS](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-vyos-vyatta/)
> - [VyOS - Official Site](https://vyos.net/)
> - [VyOS Docs - Guia uso Rapido](https://docs.vyos.io/en/latest/quick-start.html)
> - [VyOS Docs - Ejemplo de Configuracion](https://docs.vyos.io/en/latest/configexamples/index.html)
> - [Nathan Paul Blog - Build Eve-NG with VyOS](https://npaul.uk/2021/01/build-the-best-free-network-learning-environment-with-eve-ng/)

> [!NOTE] Sobre Imagen
> - Carpeta VyOS: `vyos-{version}`
> 	- Disco QEMU: `virtioa`

Se demora en enciender en 113 segundos, paciencia

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

## OcNOS

> [!TIP] Lecturas Recomendadas
> [VM Demo Gratuitas con Registro](https://www.ipinfusion.com/free-software-demos/ocnos-eve/) - PSST: Puedes poner info falsa, no verifica nada
> [IPinfusion SP Docs 7.x](https://documentation.ipinfusion.com/ocnos-sp-release-notes-7.0/Content/Home.htm)
> [Youtube - Zero to Hero Course](https://www.youtube.com/playlist?list=PLMeBQ51gYDADN31R_Wga3VnOTvePIGR_4)

Creada por IPinfusion, la version mas nueva que vi es OcNOS-SP-PLUS-x86-7.0.0-262

Antes era ZebOS VTY :P

No esta soportado por EVE-NG, por lo que debes agregarlo a mano

> YAML
```
##############################################################################
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
# ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
# WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
# DISCLAIMED. IN NO EVENT SHALL IP Infusion BE LIABLE FOR ANY
# DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
# (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
# LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND
# ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
# (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
# SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
---
type: qemu
description: OcNOS Virtual Machine
name: ocnos
cpulimit: 1
icon: Ocnos.png
cpu: 2
ram: 4096
ethernet: 6
eth_name:
- eth0
eth_format: eth{1}
console: vnc
shutdown: 1
qemu_arch: x86_64
qemu_version: 2.12.0
qemu_nic: virtio-net-pci
qemu_options: -machine type=pc,accel=kvm -vga std -serial mon:stdio -usbdevice tablet -boot order=cd
...
```

## Comprimir imagenes

Si funciona la version comprimida, puedes borrar el original
1. Ir a la carpeta
```
cd /opt/unetlab/addons/qemu/{image}
```

2. Comprime `virt-sparsify`
```
virt-sparsify --compress virtioa.qcow2 cvirtioa.qcow2
```

3. Reescribe el archivo
```
mv cvirtioa.qcow2 virtioa.qcow2
```

# Fase 3: Configuraciones

## Interfaz bridge

> [!TIP] Lecturas recomendadas
> - [PeteNetLive - EVE-NG Connecting to the internet](https://www.petenetlive.com/KB/Article/0001432)

Desde un laboratorio se crea un `object` tipo `network`, seleccionando la opcion `Management(Cloud)` esta nube permite que un dispositivo acceda a internet a travez de la conexion al router fisico como gateway

## Consolas Nativas

> [!TIP] Lecturas Recomendadas
> - [EVE-NG - Download Client Side](https://www.eve-ng.net/index.php/download/): Debes bajar hasta las herramientas de tu OS
> - [Youtube - EVE-NG - EVE Install Telnet VNC Wireshark Local Management](https://youtu.be/Ea4U93991dw?si=ZJexX-GdwKTjVSS3)
> - [Putty Features SSH Handler .reg config (Ru)](https://putty.org.ru/features/ssh-handler): En caso de necesitar un .reg base para modificar

Para Windows, debes descargar la el pack oficial desde EVE-NG, este verifica las instalaciones y ademas instala los wrappers, registros y configuraciones extras

En caso de fallar por ejemplo Putty al iniciar, deberas modificar un archivo .reg y apuntar las rutas correctamente

## Actualiza Templates

> [!TIP] Lecturas Recomendadas
> - [EVE-NG Docs - Update Template](https://www.eve-ng.net/index.php/documentation/howtos/template-icons-and-config-scripts-update-from-git/)
> - [Gitlab - eve-ng-dev](https://gitlab.com/eve-ng-dev)

> Ve a la carpeta de templates
```
cd /opt/unetlab/html/
```

> Evita cagasos
```
mv templates templates.bak
```

> Clona el repositorio desde Gitlab
```
git clone https://gitlab.com/eve-ng-dev/templates.git
```

> Ve a los Config Script
```
cd /opt/unetlab/
```

> Evita Cagasos
```
mv config_scripts config_script.bak
```

> Clona el repositorio desde Github
```
git clone https://gitlab.com/eve-ng-dev/config_scripts.git
```

> Ve a la carpeta principal
```
cd
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

## Importar y Exportar

Para que los laboratorios funcionen, debes tener siempre las mismas imagenes se utilizaron al exportar

## Ideas de Laboratorio

**HA en K8s**

Un cluster de Kubernetes (K8s) en alta disponibilidad requiere minimo tres nodos para el control plane, de forma que si uno cae, el cluster sigue operando sin intervencion manual. EVE-NG simula tener esos 3 dispositivos interconectados

Asi que creas 3 VMs Linux como nodos del cluster mas un nodo controlador, todos conectados entre si dentro de la topologia.

Algo interesante es que al hacerlo en EVE-NG sobre QEMU es que puedes usar interfaces "`virtio-net`" y si tu NIC lo permite, utilizar offloading real, por lo que podrias explorar las funcionalidades de Cilium con XDP y eBPF.

**Labs de Internet**
> [!TIP] Lecturas Recomendadas
> - [Github - hegdepavankumar/cisco-asa-firewall-training](https://github.com/hegdepavankumar/cisco-asa-firewall-training)

Siempre hay ideas dando vueltas, cursos para certificaciones, etc.


# Extra

Ahora es momento de utilizar tu version de EVE-NG, si no tienes ideas, puedes leer [[800 - Extras/Write-Ups/EVE-NG/EVE-NG - Labs|EVE-NG - Labs]]

## Porque no PNETLab?
> [!TIP] Fuente
> - [EVE-NG Forums - SCAMMERS PNETLAB](https://eve-ng.net/forum/viewtopic.php?t=16925)

PNETLab es un fork de EVE-NG Community, el cual extendio las funciones de EVE-NG Pro sin contar con una licencia oficial, lo que desencadeno polemicas por posible uso de codigo cerrado.

## Licencia EVE-NG

> [!TIP] Lecturas Recomendadas
> - [EVE-NG Docs - License](https://www.eve-ng.net/index.php/documentation/license/)
> - [EVE-NG Docs - EULA](https://www.eve-ng.net/index.php/documentation/eula/)

> La Licencia general hasta 2023
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

La licencia actual se separa en 4, BSD, UI, Go, y el monitoreo de trafico
```
**BSD LICENSE**  
Copyright (c) 2016, Andrea Dainese  
Copyright (c) 2017-2025, Alain Degreffe  
All rights reserved.

**SCOPE OF THIS LICENSE**  
This BSD license applies ONLY to PHP source code and C binaries distributed as part of EVE-NG. This license does NOT apply to:  
– EVE-NG UI components (see LICENSE.UI)  
– Go language binaries and source code (see LICENSE.GO)  
– lab-traffic-monitor and related components (see LICENSE.LAB-TRAFFIC-MONITOR)

The UI components and Go binaries are proprietary and subject to separate license agreements that require a valid EVE-NG Professional Edition subscription.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:  
* Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.  
* Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.  
* Neither the name of UNetLab Ltd nor the name of EVE-NG Ltd nor the names of its contributors may be used to endorse or promote products derived from this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS “AS IS” AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED.  
IN NO EVENT SHALL THE COPYRIGHT HOLDERS OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY.  
  
Nothing in this license requires the distribution of PHP source code as part of EVE-NG Professional Edition.
```

Esto es solo para que veas que EVE-NG NO es Open Source, ni tiene una licencia agradable de convivir, pero es un buen software

# Deprecated

NOTA: Deberia reconsiderar este punto, debido a que Proxmox ahora esta siendo soportado :O

**Virtualizador Tipo 1 (Ignorado por ahora)**
Permite usar otras maquinas

**Instalacion Soportada de VMware ESXi (Ignorado por ahora)**
Valor: $1.000.000 Al año
- [Youtube - VirtualizationHowTo - VMware ESXi do first](https://www.youtube.com/watch?v=-1BMiYZfz38)
- [Youtube - NetworkChuck - VMware ESXi Setup and Install](https://www.youtube.com/watch?v=apC1bOLbzbY&t=822s)
- [Github - hegdepavankumar/VMware Workstation Pro 17 Licence Keys](https://github.com/hegdepavankumar/VMware-Workstation-Pro-17-Licence-Keys)
- [Github - hegdepavankumar/VMware-ESXI-Licese-Keys](https://github.com/hegdepavankumar/VMware-ESXi-License-Keys)

**Alternativa Soportada para EVE-NG-PRO: Proxmox**
Valor: Gratis
- [Guia - Adam From The Future - Running EVE NG under Proxmox](https://adamfromthefuture.wordpress.com/2018/08/30/running-eve-ng-under-proxmox/)
- [Guia - Yzguy - EVE-NG in LXC on Proxmox](https://yzguy.dev/posts/eve-ng-in-lxc-on-proxmox/)
- [Youtube - Gerard O'Brien - EVE-NG on Proxmox](https://www.youtube.com/watch?v=BmuZHjkNCt0)