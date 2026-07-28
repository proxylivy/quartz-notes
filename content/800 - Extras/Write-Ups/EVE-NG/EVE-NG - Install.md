# Introduccion

> [!DANGER] Estado de EVE-NG
> La ultima version de EVE-NG Community es la version `6.2.0-4` basada en Ubuntu 22.04LTS (Jammy), desde 7.0.0, EVE-NG Community dejara de ser desarrollado para desarrollar solamente EVE-NG Pro, el cual pasa a tener una version gratuita llamada Freemium, la peor caracteristica es que solo soporta 7 nodos activos... Mi recomendacion es utilizar la version Community hasta que deje de existir
> 
> Encontre un metodo para actualizar el Kernel y QEMU a sus ultimas versiones, va en contra de las intenciones del equipo de EVE-NG y quedas sin soporte oficial (Del que ya no existe), disponible en [[800 - Extras/Write-Ups/EVE-NG/EVE-NG - QEMU & Kernel|EVE-NG - QEMU & Kernel]]

EVE-NG (**E**mulated **V**irtual **E**nvironment - **N**ext **G**eneration) es una plataforma de emulacion de redes que permite virtualizar dispositivos como Router, Switches, Firewall, Load Balancer, IDS/IPS, etc. utilizando imagenes reales de sus sistemas operativos.

La version con la cual escribo esta guia es `EVE-NG Community Edition 6.2.0-4`

Si te interesa, tengo un articulo que habla mas en profundidad sobre la [[800 - Extras/Articulos/Historia de la Emulacion|Historia de la Emulacion]]

## Licencias y Limites

> [!TIP]- Lectura Recomendada
> - [EVE-NG Docs - EVE Licencing Model](https://www.eve-ng.net/index.php/documentation/eve-licensing-model/)
> - [EVE-NG Docs - Features Compare](https://www.eve-ng.net/index.php/features-compare/)
> - Buy License Links
> 	- [EVE-NG - Buy Professional](https://www.eve-ng.net/index.php/buy/)
> 	- [EVE-NG - Buy Corporate/Learning Center](https://www.eve-ng.net/index.php/buy-corporate/)
> - [EVE-NG Docs - Community](https://www.eve-ng.net/index.php/community/)
> - [Youtube - EVE-NG - EVE WEB UI features](https://youtu.be/EsmfepaYOL8?si=mjvLD99_o3qFQsWs)

Su antiguo modelo de licenciamiento se basa en 3 tiers descritos en la siguiente tabla

| Edicion         | Costo          | Nodos Activos | Soporte Docker | Usuarios Concurrentes      | Uso                |
| --------------- | -------------- | ------------- | -------------- | -------------------------- | ------------------ |
| Community       | Gratis         | 63            | No             | 1                          | Personal           |
| Pro             | $205USD/year   | 1024          | Si             | 1                          | Personal Pro       |
| Learning Center | $1.000USD/year | 1024          | Si             | 12 (2 admin + 10 usuarios) | Academico/Empresas |

> [!WARNING]- Sobre uso de Docker
> [Youtube - EVE-NG - EVE Pro embedded Docker Setup and Usage](https://www.eve-ng.net/index.php/documentation/howtos-video/eve-embedded-dockers-setup-and-usage/)
> 
> La edicion Community **no soporta Docker** directamente. Para utilizar utilizar contenedores integrados (Paquete `eve-ng-dind`), se necesita tener la version PRO o Learning Center.
> 
> Puedes ver un Workarround en [[#Uso de Contenedores]]

## Requisitos del servidor
> [!IMPORTANT] Documentacion Recomendada
> - [Requisitos del sistema](https://www.eve-ng.net/index.php/documentation/installation/system-requirement/)
> 	- [Supported](https://www.eve-ng.net/index.php/supported-hardware-and-software-systems/)
> 	- [Not Supported](https://www.eve-ng.net/index.php/not-supported-systems-or-hw/)
> - [Cookbook](https://www.eve-ng.net/index.php/documentation/community-cookbook/)
> 	- Hoja 10: 2.1.4 Dedicated Server BM system requirements

> [!TIP]- Lecturas sobre uso Hipervisores
> - ESXi
> 	- [Youtube - VirtualizationHowTo - VMware ESXi do first](https://www.youtube.com/watch?v=-1BMiYZfz38)
> 	- [Youtube - NetworkChuck - VMware ESXi Setup and Install](https://www.youtube.com/watch?v=apC1bOLbzbY&t=822s)
> 	- [Github - hegdepavankumar/VMware Workstation Pro 17 Licence Keys](https://github.com/hegdepavankumar/VMware-Workstation-Pro-17-Licence-Keys)
> 	- [Github - hegdepavankumar/VMware-ESXI-Licese-Keys](https://github.com/hegdepavankumar/VMware-ESXi-License-Keys)
> - Proxmox
> 	- [Guia - Adam From The Future - Running EVE NG under Proxmox](https://adamfromthefuture.wordpress.com/2018/08/30/running-eve-ng-under-proxmox/)
> 	- [Guia - Yzguy - EVE-NG in LXC on Proxmox](https://yzguy.dev/posts/eve-ng-in-lxc-on-proxmox/)
> 	- [Youtube - Gerard O'Brien - EVE-NG on Proxmox](https://www.youtube.com/watch?v=BmuZHjkNCt0)

**HW**
- CPU: Debe soportar Intel VT-X/EPT (**E**xtended **P**age **T**ables) o AMD-V/RVI
	- Minimo: 4vCPU (2 Nucleos / 2 Hilos) con VTX/EPT o AMD-V/RVI
	- Recomendado: 2xE5-2650v4 | [TechPowerUp](https://www.techpowerup.com/cpu-specs/xeon-e5-2650-v4.c3791)
- RAM: Entre mas RAM, mas nodos funcionando
	- Minimo: 16GB
	- Recomendado: 64GB o mas
- Storage: M.2 PCIe > SSD Sata > HDD Sata
	- Algunas imagenes necesitan 100GB de espacio libre por si solas
	- Minimo: 250GB
	- Recomendado: 1TB o mas
- Motherboard: El soporte IOMMU es opcional pero ayuda a mejorar la paravirtualizacion

**Recomendacion de HW**

> [!TIP] Servidores
> - [Wikipedia - Proliant @ Product Lines](https://en.wikipedia.org/wiki/ProLiant#Product_lines)
> - [Wikipedia - List of Dell PowerEdge @ Gen13](https://en.wikipedia.org/wiki/List_of_PowerEdge_servers#Generation_13)

Si estas pensando en comprar un servidor (de segunda mano) dedicado para EVE-NG, una buena referencia son las siguientes plataformas:
- HPE: Gen 9 o superior (ej. DL360 G9, DL380 G9, DL580 G9)
- Dell: Gen 13 o superior (ej. R530, R730, R730xd)

Sin embargo, **no necesitas** un servidor empresarial para comenzar. Con cualquier computador que soporte virtualizacion por hardware (VT-X/AMD-V) es suficiente para aprender, practicar o preparar certificaciones. La unica diferencia real es cuantos nodos podras utilizar simultaneamente

**SW**

EVE-NG puede desplegarse de distintas formas (Algunas Soportadas por el Team EVE-NG, y otras no), dependiendo del uso que quieras darle:
- Bare Metal:
	- EVE-NG: Se instala directamente sobre el servidor fisico utilizando todos sus recursos. Es la opcion mas sencila pero el equipo queda dedicado exclusivamente a EVE-NG
- Hipervisor: Se instala en un VM sobre un OS enfocado a la virtualizacion
	- VMware ESXi: Requiere ESXi 6.7 o superior. Desde la adquisicion por Broadcom dejo de ofrecer una edicion gratuita para nuevos usuarios
	- Proxmox VE: Plataforma libre y de codigo abierto basada en KVM
- OS de escritorio: 
	- QEMU/KVM (Linux)
	- VMware Workstation 17.5.2 o superior (Windows). Lee este [Post](https://knowledge.broadcom.com/external/article/368667/download-and-license-vmware-desktop-hype.html)
	- VMware Fusion 13.5.2 o superior (Mac). Lee este [Post](https://knowledge.broadcom.com/external/article/368667/download-and-license-vmware-desktop-hype.html)

En el host donde manejas el WEBUI recuerda configurar las [[#Consolas Nativas]]

## Soporte de Imagenes
> [!TIP] Lecturas recomendadas
> - [EVE-NG Docs - Supported Images](https://www.eve-ng.net/index.php/documentation/supported-images/)
> - [EVE-NG Docs - QEMU Image Namings](https://www.eve-ng.net/index.php/documentation/qemu-image-namings/)
> - [EVE-NG Docs - HowTo](https://www.eve-ng.net/index.php/documentation/howtos/)
> - Las imagenes estan dando vueltas por internet, te recomiendo buscar, te doy unas pistas
> 	- [Github ishare2-org](https://github.com/ishare2-org)
> 		- [Labhub](https://labhub.eu.org/es/), o [Alist Labhub](https://alist.labhub.eu.org/)
> 	- [Github - hegdepavankumar/Cisco-Images-for-GNS3-and-EVE-NG](https://github.com/hegdepavankumar/Cisco-Images-for-GNS3-and-EVE-NG)

La siguiente tabla busca romper un poco la burbuja de utilizar solo Cisco, es una invitacion a explorar mas soluciones multi-vendor

| Vendor              | Router<br>L3     | Switch<br>L2 / L3 | Firewall              | Load<br>Balancer        | SD-WAN    | IDS/IPS                |
| ------------------- | ---------------- | ----------------- | --------------------- | ----------------------- | --------- | ---------------------- |
| Cisco               | ✓ IOS            | ✓ IOS L2          | ✓ ASAv                | ✗                       | ✓ Viptela | ✓ NGIPS                |
| Fortinet            | ✓ FGT            | ✗                 | ✓ FGT                 | ✓ FAD                   | ✓ FGT     | ✓ FNDR                 |
| Huawei              | ✓ AR1000v        | ✓ CE12800         | ✓ USG6kv              | ✗                       | ✗         | ✗                      |
| Extreme<br>Networks | ✓ VOSS           | ✓ EXOS            | ✗                     | ✗                       | ✗         | ✗                      |
| Hillstone           | ✗                | ✗                 | ✓ CloudEdge           | ✓ vADC                  | ✗         | ✓ vIPS                 |
| VyOS                | ✓                | ✓                 | ✓                     | ✓                       | ✗         | ✗                      |
| Citrix              | ✗                | ✗                 | ✗                     | ✓ NetScaler             | ✓ SD-WAN  | ✗                      |
| MikroTik            | ✓ CHR            | ✗                 | ✗                     | ✗                       | ✗         | ✗                      |
| Aruba               | ✗                | ✓ CX              | ✗                     | ✗                       | ✗         | ✗                      |
| Palo Alto           | ✗                | ✗                 | ✓ PAN-OS              | ✗                       | ✗         | ✓                      |
| F5                  | ✗                | ✗                 | ✗                     | ✓ Big-IP                | ✗         | ✗                      |
| A10                 | ✗                | ✗                 | ✗                     | ✓ vThunder              | ✗         | ✗                      |
| Open Source         | ✓\* FRR/<br>Bird | ✓ SONiC           | ✓ OPNsense/<br>IPFire | ✓\* HAproxy/<br>Traefik | ✗         | ✓\* Snort/<br>Suricata |

`*`: Se instala sobre Linux, no existe como Appliance Independiente

**Detalle Imagenes Utilizadas**

Cisco IOS image list:
- Cisco IOS
	- L2/L3 Switch: `i86bi_linux_l2-adventerprisek9-ms.SSA.high_iron_20190423.bin` (15.2 - 2019-04-23)
	- L3 Router: `i86bi_LinuxL3-AdvEnterpriseK9-M2_157_3_May_2018.bin` (15.7 - 2018-05-10)
- Cisco IOS XE - [Free with Registration](https://developer.cisco.com/docs/modeling-labs/cml-free/)
	- L3 XE Router (64 bits): `x86_64_crb_linux-adventerprisek9-ms.bin` (17.18.1)
	- L2/L3 XE Switch (64 bits): `x86_64_crb_linux_l2-adventerprisek9-ms.bin` (17.18.1)

QEMU image list:
- Aruba AOS-CX 10.18 - [Free with Registration in HPE](https://networkingsupport.hpe.com/globalsearch#q=AOS-CX%20OVA&tab=Software&sortCriteria=date%20descending)
- Cisco
	- ASAv 9.24.1 - [Free with Registration](https://developer.cisco.com/docs/modeling-labs/cml-free/)
	- ASAv-9.22.1.1-PLR-Licenced
	- csr1000vng-universalk9.17.03.08a-serial
- Extreme Networks
	- ExtremeVOSS 9.4.0.0 - [Free](https://github.com/extremenetworks/Virtual_VOSS)
	- ExtremeXOS 33.6.1.14 - [Free](https://github.com/extremenetworks/Virtual_EXOS)
- Freebsd 15.2 - [Open Source](https://www.freebsd.org/)
- Hillstone SG6000 - [Free with Registration](https://images.hillstonenet.com/index/user/login.html)
	- CloudEdge-5.5R12P2.44-v6
	- vADC-5.5R12-5.0-v6
	- vIPS-5.5R12-6.2-v6
- Huawei
	- AR1000 5.170-V300R022C00SPC100
	- NE40e
	- CE12800
	- USG6000kv 5.1.7-2018
- Linux
	- Arch Linux - [Open Source](https://archlinux.org/)
	- Alpine Linux 3.24.1 - [Open Source](https://www.alpinelinux.org/)
	- Rocky Linux 8.10 - [Open Source](https://rockylinux.org/)
	- Kali Linux 2026.02 - [Open Source](https://www.kali.org/get-kali/)
	- Debian 13 - [Open Source](https://www.debian.org/)
	- Ubuntu Server 26.04 LTS - [Open Source](https://ubuntu.com/download/server)
	- IPFire 2.29 - Core Update 202 - [Open Source](https://www.ipfire.org/)
	- Issabel 5 - [Open Source](https://www.issabel.org/)
- MS Windows
	- Host (XP, 7, 10, 11)
	- Server (2008-2025)
- Microtik RouterOS 7.23.2 - [Free](https://mikrotik.com/download)
- OpenWRT 25.12.5 - [Open Source](https://openwrt.org/)
- OPNsense 25.1 - [Open Source](https://opnsense.org/)
- Palo Alto 11.2.5
- PfSense-pfs 2.7.2 - [Open Source](https://atxfiles.netgate.com/mirror/downloads/)
- Vyos 1.5 Rolling Release - [Open Source](https://vyos.net/) - [Changelog](https://github.com/vyos/vyos-nightly-build/releases)
- IP Fusion OcNOS 7.0.0 - [Free Demos with registration](https://www.ipinfusion.com/free-software-demos/) (Psst: Pon informacion falsa)
- Virtual PC (VPCS) - [Open Source](https://github.com/GNS3/vpcs)

### No recomiendo

Este podio son vendor que no recomiendo, un espacio de rant

**Cisco vIOS**

- Cisco vIOS - [Free with Registration](https://developer.cisco.com/docs/modeling-labs/cml-free/)
	- Router: vios-adventerprisek9-m.SPA.159-3.M12
	- Switch: viosl2-adventerprisek9-m.ssa.high_iron_20200929

Estas imagenes son notablemente lentas en EVE-NG, especialmente vIOSl2, l3 es mas utilizable. Recomiendo utilizar directamente imagenes [[#Cisco IOS XE]] que son mas rapidas y modernas, si quieres el comportamiento de IOS 15.x, pues tienes el clasico [[#Cisco IOL]]

**Dinamips**

Las imagenes dinamips, son IOS pero del 2000, son mañosos y requieren un setup especial para no consumir el 100% de tu CPU, puedes perfectamente utilizar un [[#Cisco IOS]] 

**F5**

Licencias limitadas y ademas se demoran un monton en entregartelas, desagradable
- F5 BigIP 21.1.0-0.0.38 - [Download - Ultra limited needs license](https://my.f5.com/manage/s/downloads)

> [!WARNING] Sobre la Licencia
> - [F5 Trials - BIG-IP Showcase](https://www.f5.com/trials/big-ip-virtual-edition)
> - [F5 Manage - Trials](https://my.f5.com/manage/s/trials)
> 
> Requiere una licencia para funcionar, aunque existen trials de 30 dias que puedes pedir y se demoran en entregarla de 24 a 48 horas.
> 
> Las licencias de evaluacion son individuales de cada instancia y no pueden reutilizarse en otras VMs. Por lo que por cada instancia debes hacer ese proceso.
> 
> Para obtener la licencia, accede a Trials y solicita la oferta para BIP-IP, luego de darle click, cargara unos 20 minutos y luego aparecera un mensaje diciendo "Pending Approval, Trial fulfillment on hold, pending approval from F5 Inc.". Y luego deberias esperar de 1 a 2 dias para que te entreguen la licencia.
> 
> Se demoraron en aprovar mi solicitud en: ?? (Llevo 120 horas al momento de escribir, (Jueves -> Martes) hagan este tramite con tiempo, no pense que se demoraba tanto) y no parece avisarte al correo


**Fortinet**

Es famoso, tiene sus certificados NSE y cosas, ademas de dominar el mercado, pero la arrogancia mata, y han matado poco a poco sus imagenes, no puedes hacer nada sin una licencia limitada a una cuenta, tu vez que quieres hacer con estas imagenes

- Fortinet - [Download - Ultra limited needs license](https://support.fortinet.com/support/#/downloads/vm)
	- FAC (FortiAuthentication) 6.6.2
	- FGT (Fortigate) 7.6.2.F-build3462
	- FNDR (Forti Network Detection and Response) v7.4-build0520

> [!TIP] Significado Nombres
> - Fuentes: [Fortinet Customer - Deciphering abbreviations for Fortinet products](https://community.fortinet.com/t5/Customer-Service/Technical-Tip-Deciphering-abbreviations-for-Fortinet-products/ta-p/196062)
> - FAD: FortiADC
> - FAZ: FortiAnalyzer
> - FAC: FortiAuthenticator
> - FGT: Fortigate
> - FMG: FortiManager
> - FNDR: FortiNDR (Network Detection and Response)
> - FWB: FortiWeb

EVE-NG solo tiene una plantilla para Fortinet, por lo que se diferencia por versiones en vez de carpetas

## Instalacion de EVE-NG

> [!NOTE] Documentacion Recomendada
> - [Documentacion Eve-NG - First Boot](https://www.eve-ng.net/index.php/documentation/installation/howto-configure-eve-during-first-boot/)
> - [Jose Juan Sanchez - Instalacion y Configuracion de EVE-NG en VMware](https://josejuansanchez.org/bastionado/eve-ng/index.html)
> - [Arny Blog - Instalacion EVE-NG (Ru)](https://arny.ru/linux/ustanovka-eve-ng/)
> - [JD-Networks Blog - Eve NG Install](https://jd-networks.co.uk/blog/2019/05/24/eve-ng-community-edition/)
> - [Dainok - Installing Eve-NG](https://www.adainese.it/blog/2023/09/21/installing-eve-ng/)
> - [Youtube - David Bombal - EVE-NG Install](https://youtu.be/FDbgTlr-tnw?si=DaXk2pCoLcMtWIpg)
> - [EVE-NG Cookbook](https://www.eve-ng.net/index.php/documentation/community-cookbook/)
> 	- Hoja 24: 3.3 - BM server install
> 	- Hoja 46: 3.7 - Login to the EVE WEB GUI
> 	- Hoja 48: 4.2 - EVE-NG Community Upgrade
> 	- Hoja 57: 6 - EVE WEB GUI Managent

EVE-NG esta basado en Ubuntu 22.04.4 LTS (Jammy Jellyfish), no se debe actualizar a una version mas nueva o EVE-NG dejara de funcionar. Su disponibilidad sera hasta Apr 2027 y luego comienza el soporte extendido de seguridad hasta 2032

Requerimientos
- Imagen ISO de EVE-NG
- USB de al menos 8GB
- Descargar [Ventoy](https://www.ventoy.net/en/index.html) (Linux/Windows) o [Rufus](https://rufus.ie) (Windows) para configurar el Pendrive

Instrucciones
1. Iniciar el LiveUSB
2. Inicia Grub y se autoselecciona "Install EVE-NG Community 6.2.0-4"
3. Selecciona el idioma (`Español`)
4. Selecciona la disposicion y variante del teclado (`Spanish (Latin American)`)
5. Selecciona "`Continuar`", formateara el disco instalado e instalara el sistema, se demora 5 minutos y se reiniciara automaticamente al terminar
6. Iniciara EVE-NG para la segunda etapa de instalacion, no debes iniciar sesion, automaticamente instalara el resto del sistema, se demora unos 15 a 20 minutos

| CLI User | CLI Pass | WEB User | Web Pass |
| -------- | -------- | -------- | -------- |
| `root`   | `eve`    | `admin`  | `eve`    |


Al iniciar, te saldra este prompt de inicio, y sera tapado por otras cosas, simplemente inicia con las credenciales
```md
Eve-NG (default root password is 'eve')
Use http:///

eve-ng login:
```

> Configuracion TUI Basica
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

> Prueba de internet y Actualizar Paquetes Servidor (Si necesitas reiniciar servicios, reinicialos todos)
```
ping -c 2 google.cl
apt update && apt upgrade
apt autoremove
```

> Hacer la vida mas sencilla
```
apt install micro btop kitty git tree imagemagick p7zip-full qemu-guest-agent lm-sensors ffmpeg jq fzf resvg xclip xsel fd-find libpoppler118 poppler-utils ripgrep zoxide
```

> Inicia el servicio de Guest Agent
```
sudo systemctl enable qemu-guest-agent --now
```

> Agrega los reopositoris para instalar [Fish Shell](https://fishshell.com/) y [Fastfetch](https://github.com/fastfetch-cli/fastfetch)
```
sudo apt-add-repository ppa:fish-shell/release-3
sudo add-apt-repository ppa:zhangsongcui3371/fastfetch
sudo apt update
sudo apt install fish fastfetch
```

> Instalar [Fisher](https://github.com/jorgebucaran/fisher)
> 
> Nota: Solo funciona cuando dentro de `fish`
```sh
curl -sL https://raw.githubusercontent.com/jorgebucaran/fisher/main/functions/fisher.fish | source && fisher install jorgebucaran/fisher
```

> Instalar [Tide](https://github.com/IlanCosman/tide)
```
fisher install IlanCosman/tide@v6
```

> Instala [LSD](https://github.com/lsd-rs/lsd)
```
wget https://github.com/lsd-rs/lsd/releases/download/v1.2.0/lsd_1.2.0_amd64.deb

sudo apt install ./lsd_1.2.0_amd64.deb

rm lsd_1.2.0_amd64.deb
```

> Instala [Yazi](https://github.com/sxyazi/yazi) | Comprueba la ultima version desde [Releases](https://github.com/sxyazi/yazi/releases) y debe ser la version MUSL
```
wget https://github.com/sxyazi/yazi/releases/download/v26.5.6/yazi-x86_64-unknown-linux-musl.deb

apt install ./yazi-x86_64-unknown-linux-musl.deb
```

Te recomiendo [[#Actualizar Templates]]

Te vuelvo a recordar que puedes actulizar el Kernel y QEMU a sus versiones mas actuales siguiendo [[800 - Extras/Write-Ups/EVE-NG/EVE-NG - QEMU & Kernel|EVE-NG - QEMU & Kernel]]

# Instalacion de Imagenes

Recuerda tener descargadas tus imagenes para pasarlas al servidor, puedes encontrar mas informacion en [[#Soporte de Imagenes]]

En este Write-UP no hablare sobre Dynamips (ej: `c7200.image`), ya que fue el metodo original de emular IOS (Por los años 2000) y hoy en dia, IOL cumple con el mismo proposito de forma mas eficiente

Por lo que quedan 2 metodos para ejecutar imagenes:
- IOL (**I**OS **o**n **L**inux): Son binarios que corren directamente en el kernel de Linux, sin necesidad de emular hardware completo. Esto es lo que los hace liviandos en RAM y CPU
- QEMU: Emulacion completa de Hardware mediante imagenes de disco (qcow2), lo que permite correr sistemas operativos reales tal y como vienen del fabricante, por lo que consume mas RAM y CPU

La mayoria de las imagenes que se usan en EVE-NG usan el metodo de QEMU, a excepcion, de obviamente, [[#Cisco IOS]] y [[#Cisco IOS XE]]

## Aruba CX Switch

> [!IMPORTANT] Documentacion Recomendada
> - [HPE Support](https://networkingsupport.hpe.com/home): Iniciar sesion con cuenta HPE Certificada
> 	- [Global Search - AOS-CX OVA](https://networkingsupport.hpe.com/globalsearch#q=AOS-CX%20OVA&tab=Software)
> - [HPE Aruba Networks - Techdocs - Changelog](https://arubanetworking.hpe.com/techdocs/AOS-CX/Consolidated_RNs/Portal_Home/Content/cx-home.htm)
> - [EVE-NG Docs - HowTo add Aruba CX Switch](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-aruba-cx-switch/)
> - [Via Internet Archive - My Ethernet Mind Blog - Adding Aruba AOS-CX to EVE-NG](https://web.archive.org/web/20240226171832/https://www.madari.co.il/2019/11/adding-aruba-aos-cx-to-eve-ng.html)

| Carpeta             | Disco   | User    | Pass | Boot  |
| ------------------- | ------- | ------- | ---- | ----- |
| `arubacx-{version}` | virtioa | `admin` | N/A  | 2 min |

HPE Aruba CX, es la linea de switches empresariales orientada a datacenter y campus con soporte de VXLAN, EVPN y automatizacion, nace de la linea ArubaOS.

Su licencia dice que no puedes utilizarla con herramientas de 3ros como EVE-NG, solo digo...

Puedes descargar esta imagen desde HPE, debes crear una cuenta con dominio academico o corporativo, ya que no permite un correo general (Gmail, Yahoo, Outlook, iCloud, etc.)

Una vez con tu cuenta creada, busca el termino "`AOS-CX OVA`" desde el buscador global de HPE Support. Veras varias ramas activas disponibles, al momento de escribir, estas son:
- 10.18.001
- 10.17.1020
- ...
- 10.13.1180 (LTS)

Te recomiendo revisar el Changelog de Aruba para ver cuales son los ultimos cambios y lanzamientos de cada rama.

> Crea la carpeta en el servidor
```
mkdir /opt/unetlab/addons/qemu/arubacx-{version}
```

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

> Copia la imagen al servidor
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/arubacx-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

> [!TIP] Notas Iniciar
> Da una pantalla en negro por 2 minutos, luego aparecera el login

## Cisco

**Descargas y Licencias**

Descargar imagenes directamente desde Cisco puede ser confuso. Existe un portal publico de descargas, aunque las imagenes requieren un "*Service Contract*" asociado a tu cuenta de Cisco. Tener una cuenta gratuita o de educacion no garantiza el acceso a las descargas.

Si intentas descargar una imagen sin los permisos necesarios, el portal te dira
> you must have a valid service contract associated to your Cisco.com profile.

Y para obtener acceso debes cumplir con uno de los requerimientos
- Tu tipo de cuenta tiene un acuerdo de compra directa con Cisco
- Eres socio o distribuidor de Cisco

**CML-P**

> [!IMPORTANT] Documentacion Recomendada
> - [Cisco - CML Index Showcase](https://www.cisco.com/site/us/en/learn/training-certifications/training/modeling-labs/index.html)
> - [Cisco U - Comprar Licencia CML-P](https://u.cisco.com/labs/cisco-modeling-labs-personal-1)
> - [Cisco Dev Docs](https://developer.cisco.com/docs/)
> 	- [CML](https://developer.cisco.com/docs/modeling-labs/)
> 		- [FAQ](https://developer.cisco.com/docs/modeling-labs/faq/)
> 		- [VM Images for CML Labs](https://developer.cisco.com/docs/modeling-labs/vm-images-for-cml-labs/)

Existe una alternativa para conseguir imagenes de Cisco de buena manera, y se llama "CML-P" (Cisco Modeling Labs Personal), tiene un costo de 200USD/año e incluye un conjunto de imagenes para laboratorio, entre ellas
- ASAv
- CAT8000v
- CAT9000v
- CSR1000v
- FMCv
- FTDv
- IOL XE L3
- IOL XE L2
- IOSv L3
- IOSv L2
- IOS XRv 9000
- NX OS 9000
- Catalyst SD-WAN
- Catalyst 9800-CL
- Algunas distros de Linux

Aunque estas imagenes incluidas con CML estan licenciadas exlusivamente para utilizarse en CML, son tecnicamente compatibles con EVE-NG y otros emuladores. Aunque debes estar claro que rompes la licencia al hacerlo.

**CML-Free**

> [!IMPORTANT] Documentacion Recomendada
> - [Cisco CML-Free Docs](https://developer.cisco.com/docs/modeling-labs/cml-free/#installing-cml-free)
> - [Cisco Meraki - CML Free Register](https://mkto.cisco.com/cml-opt-in.html)
> - [Cisco Software Download](https://software.cisco.com/download/home/)
> 	- [CML Free](https://software.cisco.com/download/home/286193282/type/286326381/release/CML-Free)

Si no deseas adquirir una licencia de CML-P, Cisco ofrece un plan gratuito mas limitado llamado "CML-Free" y contiene un set de imagenes igual interesante:
- ASAv (Sin licencia)
- IOS XE L3 (IOL)
- IOS XE L2 (IOL-L2)
- IOSv L3
- IOSv L2

Para obtener estas imagenes, debes registrarte en *Cisco CML-Free* (Puedes a travez del formulario de Meraki). Una vez verificado, podras descargar el archivo `refplat-{version}-free-iso.zip` desde el portal *Cisco Software Download*.

Extrae el contenido del archivo ZIP y luego la imagen ISO. Dentro encontraras el directorio `virl-base-images`, que contiene las imagenes qcow2 o binarios que necesitamos

A partir de este punto, las siguientes secciones asumen que ya tienes acceso al contenido de esta carpeta para poder instalar las imagenes de Cisco que lo requieran para EVE-NG

### Cisco IOS
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - Cisco IOL](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-cisco-iol-ios-on-linux/)
> - [BlackBox Blog (Ru) - Eve-NG Arreglar imagenes IOL](https://it-blackbox.blogspot.com/2018/06/eve-ng-cisco-iouiol.html)
> - [Github - ishare2-org/ishare2-cli](https://github.com/ishare2-org/ishare2-cli) | [Generate new iourc license](https://github.com/ishare2-org/ishare2-cli?tab=readme-ov-file#generate-a-new-iourc-license-for-bin-images)
> - Github CiscoIOUKeygen
> 	- [sbanszky - CiscoIOL](https://github.com/sbanszky/CiscoIOL/blob/main/CiscoIOUKeygen.py)
> 	- [lxcau - CiscoIOUKeygen.py](https://github.com/lxcau/script/blob/master/CiscoIOUKeygen.py)
> 	- [guishade - ciscoIOUkeygen.py](https://github.com/guishade/ciscoIOUkey/blob/main/ciscoIOUkeygen.py)
> 	- [robin113x - keygen](https://github.com/robin113x/keygen/blob/main/CiscoIOUKeygen.py)

> [!BUG] Evita este Router
> Evita usar la version `L3 15.5.2T` (Router) debido a que se congela en standby y no podras acceder a la consola

Cisco IOS (**I**nternetwork **O**perative **S**ystem) o IOL (**I**OS **O**n **L**inux) utilizan un formato distinto al de las maquinas virtuales tradicionales. En lugar de ejecutarse como una imagen de QEMU, IOL consiste en binarios compilados especificamente para Linux, una arquitectura heredada de los primeros laboratorios internos de Cisco (WebIOL) y fue posteriormente adoptada pro herramientas como IOU WEB, UNL y finalmente EVE-NG.

Las carpetas relevantes son:
- `/opt/unetlab/addons/iol/bin/`: Imagenes con extension "`.bin`", junto a un archivo "`iourc`" que actua como licencia
- `/opt/unetlab/addons/iol/lib/`: La libreria "`libcrypto.so.4`" (Openssl) necesaria para que los binarios de "`bin/`" funcionen

Las versiones que recomiendo son
- IOS Router L3: `i86bi_LinuxL3-AdvEnterpriseK9-M2_157_3_May_2018.bin` - 15.7 (28/MAR/2018)
- IOS Switch L2/L3: `i86bi_linux_l2-adventerprisek9-ms.SSA.high_iron_20190423.bin` - 15.2 (23/ABR/2019)

**IOURC**

IOURC es el archivo de licencia que leen los binarios de IOS para poder ejecutarse, dependen de los valores `hostname` y `Host_id` por lo que es importarte rehacerlo cuando cambias los archivos `/etc/hostname` y `/etc/hosts`.

> Crea un archivo `NETMAP` y `iourc`
```
touch /opt/unetlab/addons/iol/bin/NETMAP /opt/unetlab/addons/iol/bin/iourc
```

> Te recomiendo buscar el Keygen y copiarlo en un archivo `script.py` y ejecutalo usando Python
```
python3 script.py
```

> El output del script copialo dentro de `/opt/unetlab/addons/iol/bin/iourc`
```
[license]
eve-ng = 972xxxxxxxxx1616;
```

**BIN**

> Copia las imagenes desde tu pc al servidor
```
rsync -Phvr *.bin root@{ip-server}:/opt/unetlab/addons/iol/bin/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### Cisco IOS XE

> [!TIP] Lecturas Recomendadas
> - [Cisco CML-Free Docs](https://developer.cisco.com/docs/modeling-labs/cml-free/#installing-cml-free)
> - [Cisco Meraki - CML Free Register](https://mkto.cisco.com/cml-opt-in.html)
> - [Cisco Software](https://software.cisco.com/download/home/)
> 	- [CML Free](https://software.cisco.com/download/home/286193282/type/286326381/release/CML-Free)

Cisco IOS XE es la evolucion de IOS, diseñado con una arquitectura modular basada en Linux. Se puede obtener a travez de CML-Free y su metodo de instalacion se basa en binarios, de forma similar a [[#Cisco IOS]], aunque el proceso es un poco mas complejo debido a como Cisco empaqueta estos binarios

La version que utilize es: `17.18.02a (12/May/2026)`

Las versiones no tienen descripciones, siempre tienen el mismo nombre
- IOS XE Router L3: `x86_64_crb_linux-adventerprisek9-ms.bin`
- IOS XE Switch L2/L3: `x86_64_crb_linux_l2-adventerprisek9-ms.bin`

Para extraer las imagenes desde CML-Free para Router L3:
1. Entra a `virl-base-images` y busca la carpeta `iol-xe-{version}`
2. Descomprime el archivo `iol-xe-{version}.tar.gz`
3. Abre la carpeta `blobs`, luego `sha256` y descomprime el archivo comprimido mas pesado del listado, en mi caso `SHA256: ac697212b57ca1706f4a5618a2b11...`.
4. Extraera el archivo `x86_64_crb_linux-adventerprisek9-ms.iol`.
5. Debes cambiar la extension de `.iol` a `.bin`

Para extraer las imagenes desde CML-Free para Switch L2/L3
1. Entra a `virl-base-images` y busca la carpeta `ioll2-xe-{version}`
2. Descomprime el archivo `ioll2-xe-{version}.tar.gz`
3. Abre la carpeta `blobs`, luego `sha256` y descomprime el archivo comprimido mas pesado del listado, en mi caso `SHA256: 59fc7486ad1682cf83253a...`
4. Extraera el archivo `x86_64_crb_linux_l2-adventerprisek9-ms.iol`.
5. Debes cambiar la extension de `.iol` a `.bin`

> Envia el binario del router L3 a la carpeta de IOL
```
rsync -Phvr x86_64_crb_linux-adventerprisek9-ms.bin root@{ip-server}:/opt/unetlab/addons/iol/bin/
```

> Envia el binario del Switch L2/L3 a la carpeta de IOL
```
rsync -Phvr x86_64_crb_linux_l2-adventerprisek9-ms.bin root@{ip-server}:/opt/unetlab/addons/iol/bin/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### ASAv
**Metodo: CML-Free**
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - Cisco ASAv](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-cisco-asav/)
> - [Cisco CML-Free Docs](https://developer.cisco.com/docs/modeling-labs/cml-free/#installing-cml-free)
> - [Cisco Meraki - CML Free Register](https://mkto.cisco.com/cml-opt-in.html)
> - [Cisco Software](https://software.cisco.com/download/home/)
> 	- [CML Free](https://software.cisco.com/download/home/286193282/type/286326381/release/CML-Free)

| Carpeta              | Disco   | User | Pass       | Boot  |
| -------------------- | ------- | ---- | ---------- | ----- |
| `asav-{version}`     | virtioa | N/A  | N/A        | 2 min |
| `asav-plr-{version}` | virtioa | N/A  | `cisco132` | 2 min |

Puedes conseguir esta imagen actualizada con el metodo de [[#Cisco IOS XE]] (Mediante CML-Free), pero estan sin licencia.

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

> [!TIP] Nota Inicio
> La primera vez se instala y se reinicia, luego inicia normalmente.

**Metodo: PLR**

Las imagenes "**P**ermanent **L**icense **R**eservation (PLR)" traen la licencia integrada y expanden las funcionalidades del ASAv. El truco es que deben configurarse de forma especifica, de lo contrario el sistema detecta que es una imagen clonada y eliminara la licencia automaticamente

> Caracteristicas Licencia ASAv Universal v10
```
License mode: Smart Licensing
License reservation: Enabled
ASAv Platform License State: Licensed
Active entitlement: ASAv-UNIVERSAL-V10, enforce mode: Authorized
Firewall throughput limited to 1 Gbps

Licensed features for this platform:
Maximum VLANs                     : 50
Inside Hosts                      : Unlimited
Failover                          : Active/Standby
Encryption-DES                    : Enabled
Encryption-3DES-AES               : Enabled
Security Contexts                 : 0
Carrier                           : Enabled
AnyConnect Premium Peers          : 250
AnyConnect Essentials             : Disabled
Other VPN Peers                   : 250
Total VPN Peers                   : 250
AnyConnect for Mobile             : Enabled
AnyConnect for Cisco VPN Phone    : Enabled
Advanced Endpoint Assessment      : Enabled
Shared License                    : Disabled
Total TLS Proxy Sessions          : 500
Botnet Traffic Filter             : Enabled
Cluster                           : Enabled
```

Cada Imagen PLR viene con un UUID en su archivo YML. Sin el, la imagen no funciona, debe estar en la misma carpeta donde descargaste la imagen, copialo y tengo en mente

> Crea el archivo `asav-plr.yml`
```
---
type: qemu
config_script: config_asav.py
description: Cisco ASAv PLR Licensed
name: ASAv-PLR
cpulimit: 1
icon: ASA.png
cpu: 1
ram: 2048
ethernet: 8
eth_name: ["Mgmt0/0"]
eth_format: "Gi0/{0}"
qemu_nic: virtio-net-pci
console: telnet
qemu_arch: x86_64
qemu_version: 11.0.2
uuid: 91f99252-4bf8-406d-afc2-183ccee337c1
qemu_options: -machine type=q35,accel=kvm -serial mon:stdio -nographic -no-user-config -cpu host -nodefaults -display none -rtc base=utc -uuid 91f99252-4bf8-406d-afc2-183ccee337c1
...
```

> Copia `asav-plr.yml` para Template de Intel
```
rsync -Phvr asav-plr.yml root@{ip-server}:/opt/unetlab/html/templates/intel/
```

> Copia `asav-plr.yml` para template de AMD
```
rsync -Phvr asav-plr.yml root@{ip-server}:/opt/unetlab/html/templates/amd/
```

> Crear la carpeta para `ASAv-PLR`
```
mkdir /opt/unetlab/addons/qemu/asav-plr-{version}
```

> Envia la imagen al servidor
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/asav-plr{version}/
```

> Arregla permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

> [!WARNING] Funcionamiento de Licencia PLR
> El template deberia cargar el UUID directamente al ASAv-PLR y verificar su licencia durante el inicio. En caso de fallar, modificar el valor UUID de EVE-NG con el UUID del template

### Cisco vIOS

Esta imagen entra en el listado de las que [[#No recomiendo]]

> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - Cisco vIOS](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-cisco-vios-from-virl/)
> - [Cisco CML-Free Docs](https://developer.cisco.com/docs/modeling-labs/cml-free/#installing-cml-free)
> - [Cisco Meraki - CML Free Register](https://mkto.cisco.com/cml-opt-in.html)
> - [Cisco Software](https://software.cisco.com/download/home/)
> 	- [CML Free](https://software.cisco.com/download/home/286193282/type/286326381/release/CML-Free)

| Carpeta            | Disco   | User | Pass | Boot  |
| ------------------ | ------- | ---- | ---- | ----- |
| `vios-{version}`   | virtioa | N/A  | N/A  | 2 min |
| `viosl2-{version}` | virtioa | N/A  | N/A  | 2 min |

Recomiendo utilizar [[#Cisco IOS XE]], ya que su comportamiento es el mismo

Para acceder a estas imagenes debes extraerlas desde CML-Free, accedes a `virl-base-images` y luego buscas las carpetas:
- Router L3: `iosv-159-3-m12`
- Switch L2/L3: `iosvl2-20200929`

> Crea carpeta para el Router L3
```
mkdir /opt/unetlab/addons/qemu/vios-{version}
```

> Crea Carpeta para el Switch L2
```
mkdir /opt/unetlab/addons/qemu/viosl2-{version}
```

> Renombra la imagen del Router L3
```
mv vios-adventerprisek9-m.spa.{version}.qcow2 virtioa.qcow2
```

> Renombra la imagen del Switch L2/L3
```
mv vios_l2-adventerprisek9-m.ssa.high_iron_{version}.qcow2 virtioa.qcow2
```

> Envia la imagen del Router L3
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/vios-{version}
```

> Envia la imagen del Switch L2/L3
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/viosl2-{version}
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### CSR1000vng

> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add CSR1000vng](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-cisco-csrv1000-16-x-denali-everest-fuji/)
> - [Cisco CSR1000v Data Sheet](https://www.cisco.com/c/en/us/products/collateral/routers/cloud-services-router-1000v-series/data_sheet-c78-733443.html)
> - [Cisco Software Download - CSR1000v](https://software.cisco.com/download/home/284364978/type)

| Carpeta                | Disco   | User    | Pass    | Boot   |
| ---------------------- | ------- | ------- | ------- | ------ |
| `csr1000vng-{version}` | virtioa | `admin` | `admin` | 10 min |

Cisco **C**loud **S**ervices **R**outer 1000v es un router virtual basado en Cisco IOS XE incorporando funciones de automatizacion mediante APIs, NETCONF, RESTCONF, entre otras.

Su estado es EOL y se usa para laboratorios viejos para aprender automatizacion

> Crea la carpeta para CSR1000vng
```
mkdir /opt/unetlab/addons/qemu/csr1000vng-{version}
```

> Renombra el archivo
```
mv csr1000vng-universalk9.{version}-serial virtioa.qcow2
```

> Mueve la imagen para CSR1000vng
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/csr1000vng-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

> [!TIP] Sobre inicio
> El primer inicio posiblemente reinstale sus sistema, al iniciar, puede mostrar un largo rato `%BOOT-5-OPMODE_LOG: R0/0: binos: System booted in AUTONOMOUS mode` y luego se iniciara

### Cisco Viptela SD-WAN

> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - Cisco SDWAN Viptela image set](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-cisco-viptela-images-set/)
> - [Network Academy Blog - Cisco SD-WAN on EVE-NG](https://www.networkacademy.io/ccie-enterprise/sdwan/cisco-sd-wan-on-eve-ng)
> - [Youtube - Michael O'Briens CCIE Journal - How to create Smart Account and License file for Cisco SD-WAN](https://youtu.be/Caze1TZldCM?si=tqOw6fs_mmWNqM1Y)

| Carpeta             | Disco   | User    | Pass    | Boot |
| ------------------- | ------- | ------- | ------- | ---- |
| `vtmgmt-{version}`  | virtioa | `admin` | `admin` | IDK  |
| `vtsmart-{version}` | virtioa | `admin` | `admin` | IDK  |
| `vtbond-{version}`  | virtioa | `admin` | `admin` | IDK  |
| `vtedge-{version}`  | virtioa | `admin` | `admin` | IDK  |
#### vManage

> [!WARNING] Sobre Requisitos
> Solo este nodo (vtmgmt) es extremadamente pesado, necesita 100GB de espacio extra y 32GB de ram para correr correctamente

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/vtmgmt-{version}
```

> Mueve el Qcow2
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/vtmgmt-{version}/
```

> Ve a esa carpeta
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

#### vSmart

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/vtsmart-{version}
```

> Mueve el Qcow2
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/vtsmart-{version}/
```

> Aregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

#### vBond

> [!INFO] Qcow2
> vBond y vEdge utilizan la misma imagen: `viptela-edge-{version}-genericx86-64.qcow2`

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/vtbond-{version}
```

> Mueve el Qcow2
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/vtbond-{version}/
```

> Aregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

#### vEdge

> [!INFO] Qcow2
> vBond y vEdge utilizan la misma imagen: `viptela-edge-{version}-genericx86-64.qcow2`

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/vtedge-{version}
```

> Mueve el Qcow2
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/vtedge-{version}/
```

> Aregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

## Extreme Networks

| Carpeta                 | Disco | User    | Pass  | Boot  |
| ----------------------- | ----- | ------- | ----- | ----- |
| `extremevoss-{version}` | hda   | `rwa`   | `rwa` | 6 min |
| `extremexos-{version}`  | hda   | `admin` | N/A   | 2 min |

### ExtremeVOSS
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - Extreme VOSS](https://www.eve-ng.net/index.php/documentation/howtos/extreme-voss/)
> - [Github - extremenetworks/Virtual_VOSS](https://github.com/extremenetworks/Virtual_VOSS)
> - [Extreme Networks Docs](https://supportdocs.extremenetworks.com/support/documentation/)
> 	- [VOSS](https://supportdocs.extremenetworks.com/support/documentation/vsp-operating-system-software-voss-document-collections/) (Legacy)
> 	- [Fabric Engine](https://supportdocs.extremenetworks.com/support/documentation/fabric-engine-document-collections/)

VOSS significa VSP Operating System Software, desde la version 9.0.0 que cambio el nombre a Fabric Engine (FE)

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/extremevoss-{version}
```

> Cambia el nombre del archivo
```
mv FEGNS3.{version}.qcow2 hda.qcow2
```

> Mueve el archivo
```
rsync -Phvr hda.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/extremevoss-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

> [!TIP] Sobre iniciar
> Se queda 3 minutos en "Loading RootFS" y luego otros 3 minutos para el login

### ExtremeXOS

> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - Extreme EXOS](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-extreme-exos/)
> - [Github - extremenetworks/Virtual_EXOS](https://github.com/extremenetworks/Virtual_EXOS)
> - [Extreme Networks Docs - ExtremeXOS](https://supportdocs.extremenetworks.com/support/documentation/extremexos-33-6-1/) (Legacy)
> - [Extreme Networks Docs - Switch Engine 33.6.1](https://supportdocs.extremenetworks.com/support/documentation/switch-engine-33-6-1/)

Desde la version 31.6.x, EXOS ahora pasa a ser Switch Engine

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/extremexos-{version}
```

> Renombra el archivo
```
mv EXOS-VM_{version}.qcow2 hda.qcow2
```

> Envia el archivo al servidor
```
rsync -Phvr hda.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/extremexos-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

> [!TIP] Sobre Inicio
> Se autoselecciona Serial como inicio en el disco primario, se demora 2 minutos en iniciar

## F5 BIG-IP

Es una de las imagenes que [[#No recomiendo]]

| Carpeta           | Disco   | CLI User | CLI Pass  | WEB User | WEB Pass          | Boot  |
| ----------------- | ------- | -------- | --------- | -------- | ----------------- | ----- |
| `bigip-{version}` | virtioa | `root`   | `default` | `admin`  | Nueva Pass de CLI | 3 min |

> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - F5 BigIP](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-f5-bigip/)
> - [F5 - Registro Cuenta](https://account.f5.com/myf5/signin/register)
> 	- [Descarga Imagenes](https://my.f5.com/manage/s/downloads)
> - [F5 Docs](https://docs.cloud.f5.com/docs-v2)
> - [F5 Article - K7752: Licensing the BIG-IP system](https://my.f5.com/manage/s/article/K7752)

Registra e inicia sesion en una cuenta, luego ve al menu de Descarga y selecciona lo siguiente, siempre revisa versiones mas actuales
- Group: BIG-IP
- Product Line: BIG-IP v21.X
- Product Version: 21.1.0-LTS
- Product Container: 21.1.0_Virtual-Edition

Yo descargue: `BIGIP-21.1.0-0.0.38.ALL.qcow2`

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/bigip-{version}
```

> Descomprime el archivo
```
7z x BIGIP-{version}.ALL.qcow2.zip
```

> Renombra el archivo
```
mv BIGIP-{version}.ALL.qcow2 virtioa.qcow2
```

> Mueve la imagen
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/bigip-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

> [!WARNING] Tipo Consola
> Durante la instalacion (primer inicio) debes configurar como VNC

## Freebsd

> [!IMPORTANT] Documentacion Recomendada
> - [FreeBSD](https://www.freebsd.org/)
> 	- [Releases](https://www.freebsd.org/releases/)
> 	- [Newbies](https://www.freebsd.org/projects/newbies/)
> 	- [Download FreeBSD](https://www.freebsd.org/where/) | [Mirrors](https://docs.freebsd.org/en/books/handbook/mirrors/)

| Carpeta             | Disco   | User   | Pass          | Boot  |
| ------------------- | ------- | ------ | ------------- | ----- |
| `freebsd-{version}` | virtioa | `root` | (Configurada) | 1 min |

EVE-NG no tiene documentacion oficial, pero tiene un template, debes descargar el instalador DVD1 para una instalacion offline, en mi caso `FreeBSD-15.1-RELEASE-amd64-dvd1.iso`

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/freebsd-{version}
```

> Renombra el archivo
```
mv FreeBSD-{version}-RELEASE-amd64-dvd1.iso cdrom.iso
```

> Envia la imagen al servidor
```
rsync -Phvr cdrom.iso root@{ip-server}:/opt/unetlab/addons/qemu/freebsd-{version}/
```

> Ve a la carpeta de Freebsd
```
cd /opt/unetlab/addons/qemu/freebsd-{version}
```

> Crea un disco de 10GB
```
/opt/qemu/bin/qemu-img create -f qcow2 virtioa.qcow2 10G
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Sigue estas instrucciones de instalacion
- Crea un nodo de FreeBSD, conecta la interfaz `vtnet0` a `Cloud0` para que tenga salida a internet e inicia el nodo, se autoselecciona la instalacion y cargara el sistema
- En la bienvenida selecciona "Install"
- Continua con el keymap por defecto dando Enter
- Deja el hostname en blanco
- Selecciona "Packages (Tech Preview)"
- Selecciona "Network"
- Selecciona "vtnet0" y deberia buscar una IP disponible
- Selecciona "Auto(UFS)" ya que es mas ligero
- Selecciona "Entire Disk" para utilizar todo el disco
- Selecciona "MBR DOS Partition"
- Selecciona "Finish" y luego "Commit" para crear las particiones e inicializarlas
- Elige con "espacio" para marcar con "X" los set de paquetes que necesitas, en mi caso, ninguno, asi que apreta "Enter"
- Descargara, extraera e instalara los paquetes, se demora un par de minutos.
- Te pedira una contraseña, debes escribir tu contraseña (`root`), Presionar "Tab" y luego escribir la contraseña otra vez (`root`) y dar "Enter"
- Selecciona tu region/Pais
- Configura la fecha, si se conecta a internet, la extraera automaticamente, y dale en "Skip"
- Configura la hora, si se conecta a internet, la extraera automaticametne, y dale en "Skip"
- Selecciona los servicios que quieres que se inicien, agrega con espacio a "ntpd_sync_on_start" y luego presiona enter
- Elige opciones de Hardening, como es un laboratorio, no eligire ninguna, y dare Enter
- Revisara si necesita instalar firmware extra (No deberia encontrar)
- Te pregunta si quieres añadir un usuario al sistema, como es un lab, le doy en "No"
- Le das en "Finish" para terminar con la instalacion y te preguntara si quieres hacer mas modificaciones le das en "No"
- La instalacion esta completa!. Ahora le das en "Shutdown" para apagar la maquina

Ahora deberas hacer [[#Commit al Qcow2]] y elimina el disco `cdrom.iso`

> Nunca olvides arreglar los permisos para eve-ng
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

## Fortinet

Esta es una imagen que [[#No recomiendo]]

> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - Fortinet](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-fortinet-images/)
> - [Fortinet Docs](https://docs.fortinet.com/)
> - [Fortinet Support - Download VM](https://support.fortinet.com/support/#/downloads/vm)
> - [Fortinet Video](https://video.fortinet.com/)
> - [Fortinet Community](https://community.fortinet.com/)
> 	- [How to run a real-time Wireshark inside FortiGate](https://community.fortinet.com/t5/FortiGate/Technical-Tip-How-to-run-a-real-time-Wireshark-capture-on/ta-p/213805)
> - [Reddit - Tricks and tips for new and old players](https://old.reddit.com/r/fortinet/comments/lnxv0h/fgtfazfmg_tricks_and_tips_for_new_and_old_players/)
> - [End Of Life - FortiOS](https://endoflife.date/fortios)

Para acceder a algunos VMs, basta con create una cuenta en Fortinet Support, y servira para servicios como Support, FortiCare, FortiCloud, etc. Las imagenes que estan disponibles son:
- FortiADC (FAD)
- FortiAnalyzer (FAZ)
- FortiGate (FGT)
- FortiManager (FMG)
- FortiWeb (FWB)
- Other (Necesita un contrato activo con Fortinet)

Fortigate te entrega con tu cuenta una licencia trial que esta muy muy limitada, y solo puede estar activa en un dispositivo a la vez

### FGT

Esta es una imagen que [[#No recomiendo]]

| Carpeta                  | Disco   | CLI User | CLI Pass | WEB User | WEB Pass          | Boot  |
| ------------------------ | ------- | -------- | -------- | -------- | ----------------- | ----- |
| `fortinet-FGT-{version}` | virtioa | `admin`  | N/A      | `admin`  | Nueva Pass de CLI | 1 min |

> [!TIP] Documentacion Recomendada
> - [Fortinet Support](https://support.fortinet.com/welcome/#/)
> 	- [Crear Cuenta](https://support.fortinet.com/cred/#/sign-up)
> 	- [Download VMs](https://support.fortinet.com/support/#/downloads/vm)
> - [Fortinet Docs](https://docs.fortinet.com/)
> 	- [FortiGate Product](https://docs.fortinet.com/product/FortiGate)
> 	- [ForiGate 8.0.0 - Permanent Trial Mode for FGT VM](https://docs.fortinet.com/document/fortigate/8.0.0/administration-guide/441460/permanent-trial-mode-for-fortigate-vm)
> - [Youtube - Elias Miranda - Lab Fortigate EVE-NG](https://youtu.be/Sa9AGPaImls?si=0sQWuXglxhbfRNwC)

Ve a Fortinet Download VM, elige el producto es FortiGate, y la plataforma es KVM. Yo elegi: `New deployment of FortiGate for KVM FGT_VM64_KVM-v8.0.0.F-build0167-FORTINET.out.kvm.zip (120.91 MB)`

Antes de la version `7.2.0` el licenciamiento era mas laxo, algunas personas las utilizan, yo encontre: `fortinet-FGT-v7.0.3build0237`

> Descomprimimos el ZIP
```
7z x FGT_VM64_KVM-{version}-FORTINET.out.kvm.zip
```

> Renombramos el archivo
```
mv fortios.qcow2 virtioa.qcow2
```

> Crear carpeta para FGT
```
mkdir /opt/unetlab/addons/qemu/fortinet-FGT-{version}
```

> Mueve la imagen para FGT
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/fortinet-FGT-{version}/
```

> Arregla permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Crea un nodo de FGT, y conectalo a Cloud0 e inicia el nodo, se demora en iniciar 2 minutos

Desde la CLI configura una nueva contraseña (Esta tambien te servira para acceder a la web) y revisa que ip te dio en el port1
```
show system interface ?
```

Ingresa a la ip de port1 y licencia tu VM, con una licencia Trial asociada a tu cuenta

Te saldra un mensaje en la consola y se reiniciara
```
show system interface Requesting FortiCare Trial license, proxy:(null)
```

Supongo que cuando la uses en otra vm, la primera quedara nula cuando la intente verificar, por lo que no es posible quitarla...

Lee la documentacion para el resto, me parecio muy desagradable Fortinet como empresa... Si alguien quiere hacer un PR para agregar mas info, bienvenido sea

## Hillstone

> [!IMPORTANT] Documentacion Recomendada
> - [Passport Hillstone - Registrar Cuenta](https://passport.hillstonenet.com/Account/Register)
> - [Hillstone Images - Login](https://images.hillstonenet.com/index/user/login.html)
> - [Docs Tecnicos](https://docs.hillstonenet.com/web/) | [Chino (Mas completos)](https://docs.hillstonenet.com.cn/web/)

| Carpeta                      | Disco | CLI User    | CLI Pass    | WEB User    | WEB Pass          | Boot  |
| ---------------------------- | ----- | ----------- | ----------- | ----------- | ----------------- | ----- |
| `hillstone-sg6000-{version}` | hda   | `hillstone` | `hillstone` | `hillstone` | Nueva Pass de CLI | 5 min |
| `hillstone-vADC-{version}`   | hda   | `hillstone` | `hillstone` | `hillstone` | Nueva Pass de CLI | 7 min |
| `hillstone-vIPS-{version}`   | hda   | `hillstone` | `hillstone` | `hillstone` | Nueva Pass de CLI | 7 min |

Debes crearte una cuenta y verificarla desde el correo, y luego iniciar sesion en el portar de imagenes, alli ya puedes descargar las ultimas versiones de cada imagen

EVE-NG solo tiene consideracion por la imagen de "FW" (CloudEdge), pero puedes crear versiones de la imagen, con distintas carpetas basadas en el nombre de hillstone al principio

### CloudEdge

> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - Hillstone](https://www.eve-ng.net/index.php/documentation/howtos/hillstone-firewall/)
> - [Hillstone - CloudEdge Firewall Showcase](https://www.hillstonenet.com/products/cloud-protection/cloud-security-cloudedge/)
> - [Hillstone Images - CloudEdge NGFW](https://images.hillstonenet.com/index/index/content?cid=59)
> - [Hillstone Docs (CN) - NGFW A/B Series](https://docs.hillstonenet.com.cn/web/doc-list/30) | [En (A Series)](https://docs.hillstonenet.com/web/doc-list/13) | [En (E Series)](https://docs.hillstonenet.com/web/doc-list/14)

La ultima version que encontre fue
- `SG6000-CloudEdge-5.5R12P2.44-v6.qcow2 - Tamaño: 268,2 MB - Última actualización: 30/06/2026 17:53:24`

> Crear carpeta para CloudEdge
```
mkdir /opt/unetlab/addons/qemu/hillstone-sg6000-CloudEdge-{version}
```

> Renombra el archivo
```
mv SG6000-CloudEdge-{version}.qcow2 hda.qcow2
```

> Mueve el archivo
```
rsync -Phvr hda.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/hillstone-sg6000-CloudEdge-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Crea un nodo, luego conectalo a Cloud0 y enciendelo

> [!TIP] Sobre inicio
> Inicia automaticamente en el sistema, y se queda pegado unos 4 minutos en "Loading System Software", cargara el sistema y te dara una bienvenida al Login

> [!NOTE]- Aclaracion Licencia
> Tiene 3 tipos de licenciamiento
> - Permanente
> 	- ZTNA
> - Trial User Automatico disponible por 30 dias (Deberias crear un nuevo nodo y funcionaria)
> 	- QoS
> 	- IPSec VPN
> 	- SSL VPN
> 	- APP Signature
> 	- URL DB
> 	- IP Reputation
> 	- Botnet Prevention
> 	- IPS
> 	- Antivirus
> 	- Platform
> - No disponible (Se compran por separado)
> 	- DomesticDB
> 	- Bandwidth Control
> 	- ZTNA Upgrade
> 	- SR-IOV Throughput
> 	- IoT Monitor & Control
> 	- Virtual CPU
> 	- Threat Intelligente

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

> Renombra el archivo
```
mv SG6000-vADC-{version}-v6.qcow2 hda.qcow2
```

> Mueve el archivo
```
rsync -Phvr hda.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/hillstone-sg6000-vADC-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Crea un nodo, entras las opciones, configura 2vCPU y 4096MB de ram, en caso contrario se quedara reiniciando, luego conectalo a Cloud0 y enciendelo

> [!TIP] Sobre inicio
> Inicia automaticamente en el sistema, y se queda pegado unos 6 minutos en "Loading System Software", salen diferentes mensajes sobre licencias, puedes omitirlos y te dara la bienvenida al Login

> [!NOTE]- Aclaracion Licencia
> Este solo tiene licencias TRIAL disponibles por 30 dias, que son
> - GSLB
> - VSYS
> - QoS
> - vCPU
> - APP Signature
> - Platform

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

> Renombra el archivo
```
mv SG6000-vIPS-{version}-v6.qcow2 hda.qcow2
```

> Mueve el archivo
```
rsync -Phvr hda.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/hillstone-sg6000-vIPS-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Crea un nodo, en las opciones, configura 2vCPU y 4096MB de ram, en caso contrario se querada reiniciando, luego conectalo a Cloud0 y enciendelo

> [!TIP] Sobre inicio
> Inicia automaticamente en el sistema, y se queda pegado unos 6 minutos en "Loading System Software", empezara a cargar el sistema y luego te dara la bienvenida al login

> [!NOTE]- Aclaracion Licencia
> Este solo tiene licencias TRIAL disponibles por 30 dias, que son
> - VSYS
> - QoS
> - APP Signature
> - URL DB
> - IP Reputation
> - Botnet Prevention
> - IPS
> - AntiVirus
> - Platform

## Huawei
> [!IMPORTANT] Documentacion Recomendada
> - [Networking Hints Blog - Huawei NE40 and CE12800 on EVE-NG](https://networking-hints.blogspot.com/2021/01/huawei-ne40-12800-on-eve-ng.html)
> - [Youtube - Deploy Huawei NE40E and CE12800 on EVE-NG](https://youtu.be/8XgdSGLODD4?si=TOyf1mVb7pr5LhUj)
> - [KevinJin - Huawei in Eve-NG](https://www.kevinjin.com/posts/eve-ng/eve-ng/)
> - [Youtube - Matheus Leal - Como importar Imagenes Huawei AR1K, NE40, CE12800 (Br)](https://youtu.be/fRoNEfALo90?si=PnDSDl8Yh01qXFSd)
> 	- [Google Drive - EVE-NG](https://drive.google.com/drive/folders/1nlDACO-gKSIpSRcOuOcsncfOOcup7Q3E)
> - [Huawei Docs - DCN Design Guide](https://support.huawei.com/enterprise/en/doc/EDOC1100023542/426cffd9/about-this-document?idPath=24030814%7C21782165%7C21782236%7C252837173)
> - [Huawei Support - Partner Account](https://support.huawei.com/enterprise/toSiteHelp/homepage_account#PartnerUpgrade)

La forma de acceder a las imagenes de Huawei es comprando HW, registrando el producto o siendo un Huawei Partner.

| Carpeta                   | Disco | User    | Pass        | Boot   |
| ------------------------- | ----- | ------- | ----------- | ------ |
| `huaweiar1k-{version}`    | hda   | `super` | `super`     | 10 min |
| `huaweine40e-{version}`   | hda   | N/A     | N/A         | 5 min  |
| `huaweice12800-{version}` | hda   | N/A     | N/A         | 5 min  |
| `huaweiusg6kv-{version}`  | hda   | `admin` | `Admin@123` | 4 min  |

### AR1000v
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - Huawei AR1000v](https://www.eve-ng.net/index.php/documentation/howtos/huawei-ar1000v/)
> - [Huawei CLI Docs (ES)](https://support.huawei.cn/enterprise/es/routers/ar1000v-pid-21768212) | [EN](https://support.huawei.cn/enterprise/en/routers/ar1000v-pid-21768212/)

Yo encontre: `huaweiar1k-5.170 - 509.75M` (`V300R019C00SPC300`)

NetEngine AR1000v es un router NFV (Network Functions Virtualization) de borde empresarial, equivalente a un AR fisico usando su propia plataforma VRP (Versatile Routing Platform), permitiendo hacer VPN, routing, SD-WAN para la nube, entre otros.

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/huaweiar1k-{version}
```

> Envia la imagen al servidor
```
rsync -Phvr hda.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/huaweiar1k-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Crea un nodo, en las opciones, cambia de VNC a Telnet, luego conectalo a Cloud0 y enciendelo

> [!TIP] Sobre inicio
> Inicia Linux, y carga el kernel durante un 1 minuto y luego inicia el sistema, en lo cual se demora unos 8 minutos

### NE40E
> [!IMPORTANT] Documentacion Recomendada
> - [Huawei Forums - NE40e image](https://forum.huawei.com/enterprise/intl/en/thread/ne40e-image-for-ensp-v100r003c00spc100/667245683289243648?blogId=667245683289243648)
> - [Huawei Docs - NE40E](https://support.huawei.com/enterprise/en/routers/ne40e-pid-15837?category=learn-about-products&subcategory=product-description)
> 	- [Features](https://support.huawei.com/enterprise/en/doc/EDOC1100278546/29078124/using-the-packet-format-query-tool)
> 	- [Configuration Guide](https://support.huawei.com/enterprise/en/doc/EDOC1100278545/f3e2de1e/configuration)
> 	- [Troubleshooting](https://support.huawei.cn/enterprise/en/doc/EDOC1000177634/abe6702f/about-this-document?idPath=24030814%7C9856750%7C22715517%7C9858933%7C15837)
> - [Huawei - NetEngine 40E Showcase](https://e.huawei.com/en/products/routers/ne40e)

Yo encontre: `Huawei NE40e - 524.00M` (`V800R011C00SPC607B607`)

NetEngine 40E Universal Service Router es un router SDN (Software Define Network) enfocado en MPLS y nube

Su template no esta en EVE-NG, no confundir con NE40, son distintos

Un router con aires mas de ISP, para MPLS, BGP y ese tipo de cosas

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/huaweine40e-{version}
```

> Mueve el archivo
```
rsync -Phvr hda.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/huaweine40e-{version}/
```

> Crea el archivo `huaweine40e.yml`
```
---
type: qemu
description: Huawei NE40E
name: NE40E
cpulimit: 1
icon: Router.png
cpu: 2
ram: 2048
ethernet: 12
eth_name:
- eth0
- eth1
eth_format: "1/0/{0}"
console: telnet
qemu_arch: x86_64
qemu_version: 11.0.2
qemu_options: -machine type=pc,accel=kvm -cpu host -serial mon:stdio -nographic -nodefaults -rtc base=utc 
...
```

> Envia el template `huaweine40e.yml` a Intel
```
rsync -Phvr huaweine40e.yml root@{ip-server}:/opt/unetlab/html/templates/intel/
```

> Envia el template `huaweine40e.yml` a AMD
```
rsync -Phvr huaweine40e.yml root@{ip-server}:/opt/unetlab/html/templates/amd/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Crea un nodo y enciendelo

> [!TIP] Sobre inicio
> Inicia Linux, carga el sistema en 1 minutos, luego carga el resto del sistema en 4 minutos

### CE12800
> [!IMPORTANT] Documentacion Recomendada
> - [Huawei Forums - Run CE12800 in EVE-NG](https://forum.huawei.com/enterprise/intl/en/thread/run-ce12800-ne40e-in-eve-ng/667237045992570881?blogId=667237045992570881)
> - [Huawei Docs - CE12800](https://support.huawei.com/enterprise/en/switches/cloudengine-12800-16800-pid-252837173)
> 	- [Product Overview](https://support.huawei.com/enterprise/en/doc/EDOC1100068139/5ff55479/product-overview?idPath=24030814%7C21782165%7C21782236%7C252837173)
> 	- [Configuration Guide - Basic Config](https://support.huawei.com/enterprise/en/doc/EDOC1100518792/426cffd9/about-this-document?idPath=24030814%7C21782165%7C21782236%7C252837173)
> 	- [Configuration Examples](https://support.huawei.com/enterprise/en/doc/EDOC1000039339/426cffd9/about-this-document?idPath=24030814%7C21782165%7C21782236%7C252837173)
> 	- [Troubleshooting](https://support.huawei.com/enterprise/en/doc/EDOC1000060766/426cffd9/about-this-document?idPath=24030814%7C21782165%7C21782236%7C252837173)

Yo encontre: `Huawei CE12800 - 694.63M` (`V200R005C10SPC607B607`)

Cloud Engine 12800 es un Switch empresarial enfocado a Data Center y VXLAN

No esta el template en EVE-NG, asi que debes importarlo desde un archivo yml

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/huaweice12800-{version}
```

> Envia el icono de `ce.png`
```
rsync -Phvr ce.png root@{ip-server}:/opt/unetlab/html/images/icons/
``` 

> Crea el archivo `huaweice12800.yml` con el siguiente contenido
```
# Copyright (c) 2016, Andrea Dainese
# Copyright (c) 2018, Alain Degreffe
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are met:
#     * Redistributions of source code must retain the above copyright
#       notice, this list of conditions and the following disclaimer.
#     * Redistributions in binary form must reproduce the above copyright
#       notice, this list of conditions and the following disclaimer in the
#       documentation and/or other materials provided with the distribution.
#     * Neither the name of the UNetLab Ltd nor  the name of EVE-NG Ltd nor the
#       names of its contributors may be used to endorse or promote products
#       derived from this software without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
# ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
# WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
# DISCLAIMED. IN NO EVENT SHALL <COPYRIGHT HOLDER> BE LIABLE FOR ANY
# DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
# (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
# LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND
# ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
# (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
# SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
---
type: qemu
description: Huawei CloudEngine 12800
name: CE12800-CE
cpulimit: 1
icon: ce.png
cpu: 2
ram: 2048
ethernet: 12
eth_name:
- MEth0/0/0
- NULL0
eth_format: GE1/0/{0}
console: telnet
shutdown: 1
qemu_arch: x86_64
qemu_version: 2.12.0
qemu_nic: virtio-net-pci
qemu_options:  -machine type=q35,accel=kvm -serial mon:stdio -nographic -nodefaults -rtc base=utc -cpu host 
...
```

> Envia el template `huaweice12800.yml` a Intel
```
rsync -Phvr huaweice12800.yml root@{ip-server}:/opt/unetlab/html/templates/intel/
```

> Envia el template `huaweice12800.yml` a AMD
```
rsync -Phvr huaweice12800.yml root@{ip-server}:/opt/unetlab/html/templates/amd/
```

> Mueve el archivo
```
rsync -Phvr hda.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/huaweice12800-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Crea un nodo y enciendelo

> [!TIP] Sobre inicio
> Inicia Linux, carga el sistema en 1 minutos, luego carga el resto del sistema en 4 minutos

### USG6000v
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - Huawei USG6000v](https://www.eve-ng.net/index.php/documentation/howtos/huawei-usg6000v/)

Yo encontre: `huaweiusg6kv-5.1.7-2018 - 728.52M` (`V500R005C00SPC100`)

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

Crea un nodo, en las opciones, cambia de VNC a Telnet, luego conectalo a Cloud0 y enciendelo

> [!TIP] Sobre inicio
> Carga el kernel de Linux e inicializa el sistema, se demora 3 minutos

No logre iniciar sesion debido al `@` que utiliza, Skill Issue

## Linux
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - Create own Linux Host Image](https://www.eve-ng.net/index.php/documentation/howtos/howto-create-own-linux-host-image/)
> - [Youtube - The Network Berg - EVE-NG Importing a Linux host](https://youtu.be/ZLvdJa3MXTU?si=ud_AM3k1wUfK0UuC)
> - [EVE-NG Docs - Mega - Download Linux Images](https://mega.nz/folder/30p3TKob#42_S__9wwPVO0zHIfC4xow)

| Carpeta                         | Disco   | User   | Pass   | Boot  |
| ------------------------------- | ------- | ------ | ------ | ----- |
| `linux-archlinux-{version}`     | virtioa | `arch` | `arch` | 1 min |
| `linux-alpine-{version}`        | virtioa | `root` | N/A    | 1 min |
| `linux-kali-{version}`          | virtioa | `kali` | `kali` | 2 min |
| `linux-rocky-{version}`         | virtioa | N/A    | N/A    | 1 min |
| `linux-debian-{version}`        | virtioa | N/A    | N/A    | 1 min |
| `linux-ubuntu-server-{version}` | virtioa | N/A    | N/A    | 1 min |
| `linux-ipfire-{version}`        | virtioa | N/A    | N/A    | 1 min |
| `linux-issabel-{version}`       | virtioa | N/A    | N/A    | 2 min |

### Arch Linux

> [!IMPORTANT] Documentacion Recomendada
> - [Gitlab - archlinux/arch-boxes](https://gitlab.archlinux.org/archlinux/arch-boxes)
> 	- [Fastly Mirro - Latest Image](https://fastly.mirror.pkgbuild.com/images/latest/)
> 	- [Geo Mirror - Latest Image](https://geo.mirror.pkgbuild.com/images/latest/)

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/linux-archlinux-{version}
```

> Descarga la ultima imagen Base
```
wget https://fastly.mirror.pkgbuild.com/images/latest/Arch-Linux-x86_64-basic.qcow2 -O /opt/unetlab/addons/qemu/linux-archlinux-{version}/virtioa.qcow2
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Crea un nodo, conectalo a Cloud0, y enciendelo

### Alpine Linux

> [!TIP] Lecturas Recomendadas
> - [Sitio Oficial](https://www.alpinelinux.org/)
> 	- [Descarga](https://www.alpinelinux.org/downloads/)
> - [Alpine Wiki - Setup Alpine](https://wiki.alpinelinux.org/wiki/Alpine_configuration_management_scripts#setup-alpine)

En el sitio de descarga, ve a la categoria "Virtual" y descarga la version "x86_64", en mi caso: `alpine-virt-{version}-x86_64.iso`

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/linux-alpine-{version}
```

> Renombra el ISO
```
mv alpine-virt-{version}-x86_64.iso cdrom.iso
```

> Envia el archivo
```
rsync -Phvr cdrom.iso root@{ip-server}:/opt/unetlab/addons/qemu/linux-alpine-{version}/
```

> Ve a la carpeta
```
cd /opt/unetlab/addons/qemu/linux-alpine-{version}
```

> Crea un disco de 10GB
```
/opt/qemu/bin/qemu-img create -f qcow2 virtioa.qcow2 10G
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Crea el nodo, conectalo a Cloud0 y enciendelo

Ejecuta el comando `setup-alpine` y sigue los siguientes pasos
- Configura tu keymap: `es`
- Configura tu layout: `es`
- Configura tu Hostname: `Enter` (localhost por defecto)
- Configura tu acceso a internet para eth0 presionando `Enter`
- Activa DHCP para eth0 presionando `Enter`
- Presiona `Enter` para auto-configurar IPv6
- Presiona `n` para no modificar mas redes
- Configura tu contraseña, en mi caso: `alpine` y luego verificala
- Configura tu timezone
- Presiona `Enter` para no configurar ningun proxy
- Presiona `f` para buscar el mirror mas rapido
- Escribe `no` para no crear ningun usuario extra
- Presiona `Enter` para seleccionar OpenSSH
- Escribe `yes` para permitir el acceso de root con contraseña
- Presiona `Enter` para no configurar ninguna llave SSH
- Escribe `vda` para utilizar el disco en la instalacion
- Escribe `sys` para utilizar ese disco como un disco normal de instalacion
- Presiona `y` para eliminar el contenido del disco y utilizarlo
- Termino la instalacion, escribe `poweroff` para apagar la maquina

> Ve a la carpeta de Linux Alpine
```
cd /opt/unetlab/addons/qemu/linux-alpine-{version}
```

> Elimina el disco iso
```
rm cdrom.iso
```

Ahora debes hacer [[#Commit al Qcow2]]

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### Kali Linux
> [!IMPORTANT] Documentacion Recomendada
> [Kali Linux - Download VM](https://www.kali.org/get-kali/#kali-virtual-machines)

Debes descargar la version de Qemu, yo recomiendo siempre tomar la Weekly, ya que se actualiza cada semana

Busca un [Mirror](https://cdimage.kali.org/README?mirrorlist), yo por ejemplo utilizo [elmirror](https://elmirror.cl/kali-images/kali-weekly/)

Yo utilizo: `kali-linux-2026-W28-qemu-amd64.7z`

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/linux-kali-{version}
```

> Descarga la imagen directamente en el servidor (debes copiar el archivo desde el mirror)
```
wget kali-linux-{version}-qemu-amd64.7z
```

> Descomprime la imagen
```
7z x kali-linux-{version}-qemu-amd64.7z
```

> Elimina el 7z
```
rm kali-linux-{version}-qemu-amd64.7z
```

> Renombra el archivo
```
mv kali-linux-{version}-qemu-amd64.qcow2 virtioa.qcow2
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Crea un nodo, conectalo a Cloud0 y enciendelo

### Rocky Linux
> [!IMPORTANT] Documentacion Recomendada
> - [Pagina Oficial](https://rockylinux.org/)
> 	- [Descarga](https://rockylinux.org/download)

Tienes 3 ramas para elegir
- 8.10
- 9.8
- 10.2

Descarga el DVD ISO desde la pagina oficial

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/linux-rockylinux-{version}
```

> Renombra el ISO
```
mv Rocky-{version}-x86_64-dvd.iso cdrom.iso
```

> Envia el archivo
```
rsync -Phvr cdrom.iso root@{ip-server}:/opt/unetlab/addons/qemu/linux-rockylinux-{version}/
```

> Ve a la carpeta
```
cd /opt/unetlab/addons/qemu/linux-rockylinux-{version}
```

> Crea un disco de 10GB
```
/opt/qemu/bin/qemu-img create -f qcow2 virtioa.qcow2 10G
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Crea un nodo, conectalo a "Cloud0" y sigue los siguientes pasos

Al arrancar con la ISO de Rocky Linux 8.10, aparece el menu de instalacion grafico. Detallo los pasos a seguir:
1. Idioma y Localizacion
	- Idioma: `Español`
	- Localizacion: `Español (Chile)`
2. Configuracion del sistema
	- Selecciona "Destino de la Instalacion"
		- En "Configuracion del Almacenamiento", selecciona la opcion "Personalizada" y luego presiona "Hecho"
		- Seleccion "Listo" 2 veces (Ignoramos el mensaje de advertencia sobre la falta de Swap", no se necesita)
	- Selecciona "KDUMP"
		- Desactiva la opcion "Habiitar KDUMP" y le das en "Hecho"
	- Selecciona "Red y Nombre del Equipo"
		- Enciendes la interfaz de red y le das en "Hecho"
	- Selecciona "Contraseña de Root"
		- Configuras `eve` y `eve` y le das 2 veces en "Hecho"
	- Selecciona "Creacion de usuario"
		- Configura `eve` en todos los espacios y marca en "Hacer de este usuario un administrador" y le das 2 veces en "Hecho"
	- Selecciona "Seleccion del Software" y elije "Instalacion Minima" y le das en "Hecho"
3. Ahora apretas en "Empezar Instalacion" y mientras carga, continua
	- Apretas 2 veces en Listo y esperas que la instalacion termine (Se demora unos 6 minutos en un SSD)
4. Cuando termine apreta "Reiniciar", ya que el instalador no lo hace automaticamente

Luego apagas la maquina y haces [[#Commit al Qcow2]]

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### Debian

> [!IMPORTANT] Documentacion Recomendada
> - [Debian Official Site](https://www.debian.org/)
> 	- [Debian Release Stable](https://www.debian.org/releases/stable/debian-installer/)
> 	- [Download Mirror](https://www.debian.org/CD/http-ftp/#mirrors)

Debes descargar el ISO DVD para AMD64

Yo utilize: `13.5.0`

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/linux-debian-{version}
```

> Renombra el ISO
```
mv debian-{version}-amd64-DVD-1.iso cdrom.iso
```

> Envia el archivo
```
rsync -Phvr cdrom.iso root@{ip-server}:/opt/unetlab/addons/qemu/linux-debian-{version}/
```

> Ve a la carpeta
```
cd /opt/unetlab/addons/qemu/linux-debian-{version}
```

> Crea un disco de 10GB
```
/opt/qemu/bin/qemu-img create -f qcow2 virtioa.qcow2 10G
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Crea el nodo, haz la instalacion

Luego apagas la maquina y haces [[#Commit al Qcow2]]

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### Ubuntu Server
> [!IMPORTANT] Documentacion Recomendada
> - [Pagina Oficial](https://ubuntu.com/)
> 	- [Descarga Ubuntu Server](https://ubuntu.com/download/server)

Descarga Ubuntu Server 26.04 LTS

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/linux-ubuntu-server-{version}
```

> Renombra el ISO
```
mv ubuntu-{version}-live-server-amd64.iso cdrom.iso
```

> Envia el archivo
```
rsync -Phvr cdrom.iso root@{ip-server}:/opt/unetlab/addons/qemu/linux-ubuntu-server-{version}/
```

> Ve a la carpeta
```
cd /opt/unetlab/addons/qemu/linux-ubuntu-server-{version}
```

> Crea un disco de 10GB
```
/opt/qemu/bin/qemu-img create -f qcow2 virtioa.qcow2 10G
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Crea un nodo y haz la instalacion y apagas la maquina

Recuerda que debes hacer [[#Commit al Qcow2]]

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### IPFire
> [!TIP] Lecturas Recomendadas
> - [Official Page](https://www.ipfire.org/)
> - [IPFire Docs](https://www.ipfire.org/docs)

Yo utilize: `ipfire-2.29-core202-x86_64.iso`

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/linux-ipfire-{version}
```

> Renombra el ISO
```
mv ipfire-2.29-core202-x86_64.iso cdrom.iso
```

> Envia el archivo
```
rsync -Phvr cdrom.iso root@{ip-server}:/opt/unetlab/addons/qemu/linux-ipfire-{version}/
```

> Ve a la carpeta
```
cd /opt/unetlab/addons/qemu/linux-ipfire-{version}
```

> Crea un disco de 10GB
```
/opt/qemu/bin/qemu-img create -f qcow2 virtioa.qcow2 10G
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Crea un nodo y conectalo a Cloud0 e inicialo

Aparece Grub, selecciona la opcion de instalarlo para evitar la espera de 60 segundos, luego sigue estos pasos
1. Selecciona el idioma Español y comienza la instalacion
2. Confirma el disco y borra todos sus datos
3. Selecciona el sistema ext4 para particionar los discos
4. Te pedira Reiniciar el nodo, debes presionar Reiniciar y luego apagas el nodo

Recuerda que debes hacer [[#Commit al Qcow2]]

### FreeIPA

Interesante...
- https://www.freeipa.org/
- https://codeberg.org/freeipa/freeipa
- https://www.freeipa.org/page/Documentation
- https://hub.docker.com/r/freeipa/freeipa-server/



### Issabel
> [!TIP] Lecturas Recomendadas
> - [Official Page](https://www.issabel.org/)
> - [SourceForge - issabelofficial/IssabelPBX Files](https://sourceforge.net/projects/issabelpbx/files/)

Issabel 5 es un PBX basado en Asterisk con un WebUI encima, puedes descargarlo desde SourceForge

> Renombra el ISO
```
mv issabel5-USB-DVD-x86_64-20240430.iso cdrom.iso
```

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/linux-issabel-{version}
```

> Copia el iso
```
rsync -Phvr cdrom.iso root@{ip-server}:/opt/unetlab/addons/qemu/linux-issabel-{version}/
```

> Crea un disco de 10GB
```
/opt/qemu/bin/qemu-img create -f qcow2 virtioa.qcow2 10G
```

Debes de crear un nodo "Linux" y seleccionar "Issabel", ademas creas un nodo de red "Cloud0" y lo conectas al nodo para que pueda conectarse a internet

Inicia el Nodo y sigue las instrucciones a continuacion
1. Apreta "Test this media and install" y espera un buen rato hasta que aparesca el instalador grafico
2. Luego selecciona el idioma y le das en siguiente
3. Seleccionas "Teclado" y le das en "Hecho"
4. Seleccionas "Contraseña de Root" y creas la contraseña "eve"
5. Creas el usuario "eve" con contraseña "eve"
6. Seleccionas "Internet" y ya puedes continuar con la instalacion
7. Una vez que finalice y este listo, debes apagar la maquina

Ahora deberas hacer [[#Commit al Qcow2]] y Elimina el disco

> Nunca olvides arreglar los permisos para eve-ng
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### Uso de Contenedores

EVE-NG Community no permite crear nodos Docker Nativos, pero eso no significa que Docker (o incluso Kubernetes) no funcionen. La solucion que veo es tratar al contenedor como lo que es: Software que corre sobre Linux, y Linux si es un nodo normal de EVE-NG.

La idea es construir una imagen base reutilizable:
1. Crea un nodo con la distribucion Linux de tu eleccion y conectalo temporalmente a `Cloud0` (o la red management bridge que desees) para que tenga salida directa a internet.
2. Realiza la instalacion normal de la distro
3. Instala los paquetes necesarios, como Docker, containerd, kubeadm o lo que desees
4. Una vez lista, apaga el nodo y haz commit de la imagen, ahora esa es tu plantilla base

Una vez que tengas tu nodo clonado y listo para la topologia, si necesitas imagenes adicionales de contenedores, desconectalo de la red del lab, conectalo temporalmente de vuelta a `Cloud0`, haz pull de lo que necesites, apaga el nodo y reconectalo a la topologia. De esta forma el lab no necesita conectividad permanente a internet y puedes explorar el comportamiento de los contenedores sin tener que tener una via a internet directa

## MS Windows

| Carpeta                      | Disco   | User | Pass | Boot  |
| ---------------------------- | ------- | ---- | ---- | ----- |
| `win-{version}`              | virtioa | N/A  | N/A  | 3 min |
| `winserver-{version}` > 2016 | virtioa | N/A  | N/A  | 5 min |
| `winserver-{version}` < 2012 | hda     | N/A  | N/A  | 5 min |

### Win Host (XP, 7, 10, 11)
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - MS Windows Host](https://www.eve-ng.net/index.php/documentation/howtos/howto-create-own-windows-host-on-the-eve/)
> - [Youtube - EVE-NG - How to Add Windows Host](https://youtu.be/Q96f0QeCpVg?si=oZu87NaEDKrDNvda)
> - [Massgrave - Download Windows](https://massgrave.dev/genuine-installation-media)

Los discos Qcow2 para un Host de windows son de 40GB para <= Win 7 y de 60GB para Windows 10 y 11

Recomiendo descargar las isos desde Massgrave, son las mas limpias y windows no te entregara una iso actualizada de XP o Win 7 por ejemplo.

> Yo modifique el template para que me funcionara correctamente  QEMU options
```
-smp cpus=8,sockets=1,cores=1,threads=8
```

Yo utilize la ISO de Win10: `es-es_windows_10_consumer_editions_version_22h2_updated_oct_2025_x64_dvd_38efd00d.iso` 

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/win-{version}
```

> Renombra el archivo
```
mv es-es_windows_10_consumer_editions_{version}_x64_dvd.iso cdrom.iso
```

> Copia el Archivo
```
rsync -Phvr cdrom.iso root@{ip-server}:/opt/unetlab/addons/qemu/win-{version}/
```

> Ve a la carpeta
```
cd /opt/unetlab/addons/qemu/win-{version}
```

> Crea un disco de 60GB
```
/opt/qemu/bin/qemu-img create -f qcow2 virtioa.qcow2 60G
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Crea un nodo, configuralo con 4vCPU y 8196M de ram, conectalo a Cloud0 e inicia el nodo y sigue las instrucciones
- Elige tus preferencias de idioma y teclado y dale en "Siguiente"
- Presiona en "No tengo una clave del producto"
- Eliges "Windows 10 Pro" y eliges la instalacion avanzada

Ahora no te reconoce el disco virtioa, y es importante que hagas los siguientes pasos correctamente
- Ve a "Cargar Controlador" y dale en "Aceptar" al mensaje
- Ahora dale en "Examinar"
- Ve a "Unidad de disquete `A:`"
- Abre `Storage` > `2003R2` > `amd64` y dale click en "Aceptar"
- Elige el controlador "`Red Hat VirtIO SCSI controller, packaged by Canonical, Ltd. blablabla`" y dale en "Siguiente"

Ahora te reconoce el disco, seleccionalo y dale en "Siguiente" y empezara con la instalacion. Debes seleccionar la region y distribucion del teclado y empezara a configurarse

Luego configuras una cuenta para uso personal y le das en "Siguiente", luego le das en "Cuenta sin conexion", luego en "Experiencia Limitada" y crea tu cuenta local, yo le pongo de usuario `eve` y de contraseña `eve` y a las preguntas de seguridad `eve`. 

Dile "Ahora no" a Microsoft Edge y luego desactiva cada una de las opciones de privacidad y el das en "Aceptar". Luego dale en "Omitir" para personalizar la experiencia

Apaga la maquina y elimina el disco

> Ve a la carpeta de Windows
```
cd /opt/unetlab/addons/qemu/win-{version}
```

> Elimina el iso
```
rm cdrom.iso
```

Enciende el nodo otra vez y haz un par de configuraciones

1. Activar RDP
	- Vas a Configuraciones > Sistema > Escritorio Remoto y Activas "Escritorio Remoto" y presionas "Confirmar"
	- Desactiva el Firewall (O configura tu interfaz como Privada para que confie en su entorno)
2. Activa Windows (Porfavor)
	- Abre Powershell en modo Administrador y pide porfavor y listo :P
3. Instala drivers de Virtio
	- Descarga [WinCDEmu](https://wincdemu.sysprogs.org/)
	- Ve a [Fedora Community](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/latest-virtio/) y descarga "Virtio Win GT x64" y "Virtio Win Guest Tools"
4. Instala Firefox

Finaliza la instalacion y apaga el VM cuando este listo

Ahora deberas hacer [[#Commit al Qcow2]]

> Nunca olvides arreglar los permisos para eve-ng
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

El rendimiento es bastante malo, me imagino porque no tiene aceleracion 3D, pero bueno, nada que hacerle

### Win Server (2008-2025)
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - MS Windows Server](https://www.eve-ng.net/index.php/documentation/howtos/howto-create-own-windows-server-on-the-eve/)
> - [Endoflife - Windows Server](https://endoflife.date/windows-server)
> - [Massgrave - Download Windows Server](https://massgrave.dev/windows-server-links)

Los discos Qcow2 para Windows server son de minimo 60GB de espacio

La version minima que recomiendo es Windows Server 2022, para atras dependes de soporte de seguridad extendido, aunque siguen exactamente el mismo metodo

Como imagen ISO, utilize: `es-es_windows_server_2022_updated_june_2026_x64_dvd_dda28eeb.iso`

> Renombra la iso
```
mv es-es_windows_server_2022_{version}_x64_dvd_dda28eeb.iso cdrom.iso
```

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/winserver-{version}
```

> Copia el Archivo
```
rsync -Phvr cdrom.iso root@{ip-server}:/opt/unetlab/addons/qemu/winserver-{version}/
```

> Ve a la carpeta
```
cd /opt/unetlab/addons/qemu/winserver-{version}
```

> Crea un disco de 60GB
```
/opt/qemu/bin/qemu-img create -f qcow2 virtioa.qcow2 80G
```

Crea un nodo, configuralo con 4vCPU y 8196M de ram, conectalo a Cloud0 e inicia el nodo y sigue las instrucciones
- Elige tus preferencias de idioma y teclado y dale en "Siguiente", y luego en "Instalar ahora"
- Presiona en "No tengo clave del producto"
- Eliges "Windows 2022 Datacenter (experiencia de escritorio)" y presionas "Siguiente"
- Lees y Aceptas los terminos de licencia
- Selecciona "Instalacion Avanzada"

No reconocera el disco virtual, debes cargar el driver
- Ve a "Cargar Controlador" y dale en "Examinar" al mensaje
- Ve a "Unidad de disquete `A:`"
- Abre `Storage` > `2003R2` > `amd64` y dale click en "Aceptar"
- Elige el controlador "`Red Hat VirtIO SCSI controller, packaged by Canonical, Ltd. blablabla`" y dale en "Siguiente"

Elige el disco y dale a siguiente, la instalacion se demora una hora y se reinicia automaticamente

Al reiniciar, parece que no carga la ISO y continua con la instalacion, Configurando la contraseña de "`Administrator`", en mi caso sera: `Alumno.2026` y confirma

- Ve a configuraciones > Sistema > Escritorio Remoto y habilitalo
- Ve a Firewall de Windows y apagalo (Para hacer funcionar RDP)

Apaga el nodo

> Ve a la carpeta de instalacion de Windows Server
```
cd /opt/unetlab/addons/qemu/winserver-{version}
```

> Elimina el disco iso
```
rm cdrom.iso
```

Ve a la configuracion del nodo y cambia la configuracion de VNC a RDP para un poquitin de mejor rendimiento

Te conectas con
- User: `ADMINISTRADOR`
- Pass: `Alumno.2026`

Respecto a las licencias, te toca hacer un poco de Magia Negra

Busca Actualizaciones y actualiza el nodo. Reinicia cuando sea necesario hasta que este listo para ser utilizado

Apaga el nodo y deberas hacer [[#Commit al Qcow2]]

> Nunca olvides arreglar los permisos para eve-ng
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

## Mikrotik RouterOS
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - Microtik Cloud Router](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-mikrotik-cloud-router/)
> - [Microtik - CHR Download](https://mikrotik.com/download/chr)
> - [Microtik Manual](https://manual.mikrotik.com/docs/introduction)

| Carpeta              | Disco | CLI User | CLI Pass | WEBFig User | WebFig Pass       | Boot  |
| -------------------- | ----- | -------- | -------- | ----------- | ----------------- | ----- |
| `mikrotik-{version}` | hda   | `admin`  | N/A      | `admin`     | Nueva Pass de CLI | 1 min |

Yo descargue: `v7.23.2`

Vas al centro de descargas de CHR y eliges el Canal "Stable" y desde "Install Images" descarga el "`RAW disk`"

> Creas la carpeta
```
mkdir /opt/unetlab/addons/qemu/mikrotik-{version}
```

> Convierte el disco (Creo que un simple mv igual sirve)
```
qemu-img convert -f raw -O qcow2 chr-{version}.img hda.qcow2
```

> Enviar el archivo
```
rsync -Phvr hda.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/mikrotik-{version}/
```

> Arreglar Permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Crea un nodo, conectalo a Cloud0 y enciendelo

> [!TIP] Sobre inicio
> Inicia el sistema desde el disco, se demora 1 minuto

## OcNOS

> [!TIP] Lecturas Recomendadas
> - [VM Demo Gratuitas con Registro](https://www.ipinfusion.com/free-software-demos/ocnos-eve/) - PSST: Puedes poner info falsa, no verifica nada
> - [IPinfusion SP Docs 7.x](https://documentation.ipinfusion.com/ocnos-sp-release-notes-7.0/Content/Home.htm)
> - [Youtube - Zero to Hero Course](https://www.youtube.com/playlist?list=PLMeBQ51gYDADN31R_Wga3VnOTvePIGR_4)

| Carpeta           | Disco   | User    | Pass    | Boot  |
| ----------------- | ------- | ------- | ------- | ----- |
| `ocnos-{version}` | virtioa | `ocnos` | `ocnos` | 1 min |

OcNOS VM, creada por IP Infusion, se creo para validar configuraciones y probar L2, L3 y MPLS limitado sin costos asociados y tiene una licencia trial de 365 dias.

La historia de OcNOS empieza con GNU Zebra en los años 90s, el cual fue uno de los primeros proyectos open source en implementar protocolos de enrutamiento basados en Linux, de los cuales, salieron dos caudales
- FOSS: Quagga -> FRRouting
- Corporativo: ZebOS -> OcNOS

Ademas ZebOS fue licenciado por distintos fabricantes, asi que es influyente y resulta familiar su uso

No esta soportado por EVE-NG, por lo que debes agregarlo a mano

Yo usare: `OcNOS-SP-PLUS-x86-7.0.0-262-GA`

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/ocnos-{version}
```

> Cambia de tamaño la imagen porque es gigante
```
convert ocnos.png -resize 64x43 -strip ocnos.png
```

> Envia el icono de `ocnos.png`
```
rsync -Phvr ocnos.png root@{ip-server}:/opt/unetlab/html/images/icons/
``` 

> Crea el archivo `ocnos.yml` con el siguiente contenido
```
################################################################################
#
#
# If you know how to make this script beter, please drop me an email:
# piotr.kedra@ipinfusion.com
#
#
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
icon: ocnos.png
cpu: 2
ram: 4096
ethernet: 6
eth_name:
- eth0
eth_format: eth{1}
console: vnc
shutdown: 1
qemu_arch: x86_64
qemu_version: 11.0.2
qemu_nic: virtio-net-pci
qemu_options: -machine type=pc,accel=kvm -vga std -serial mon:stdio -usbdevice tablet -boot order=cd
...
```

> Envia el template `ocnos.yml` a Intel
```
rsync -Phvr ocnos.yml root@{ip-server}:/opt/unetlab/html/templates/intel/
```

> Envia el template `ocnos.yml` a AMD
```
rsync -Phvr ocnos.yml root@{ip-server}:/opt/unetlab/html/templates/amd/
```

> Descomprime la imagen
```
7z x OcNOS-SP-PLUS-x86-{version}-GA.qcow2.xz
```

> Renombra el archivo
```
mv OcNOS-SP-PLUS-x86-{version}-GA.qcow2 virtioa.qcow2
```

> Mueve el archivo
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/ocnos-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Crea un nodo, conectalo a Cloud0 y enciendelo

> [!TIP] Sobre inicio
> Iniciara Linux, y en 1 minuto esta listo

## OPNsense
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - OPNsense](https://www.eve-ng.net/index.php/documentation/howtos/opnsense-firewall/)
> - [Pagina Oficial](https://opnsense.org/)
> 	- [Full Mirror Listing](https://opnsense.org/download/#full-mirror-listing)

| Carpeta              | Disco   | CLI/WEB User | CLI/WEB Pass | Boot  |
| -------------------- | ------- | ------------ | ------------ | ----- |
| `opnsense-{version}` | virtioa | `root`       | `opnsense`   | 3 min |

OPNsense nacio en 2015 como un fork de pfSense para ofrecer un desarrollo mas abierto, transparente y comunitario, ademas de adoptar tecnologias y versiones reciente de FreeBSD con mayor rapidez.

Debes descargar la imagen correspondiente de algun mirror
- Arquitectura: amd64
- Tipo de Imagen: dvd

En mi caso descargue: `OPNsense-26.7-dvd-amd64.iso.bz2`

> Descomprime el archivo
```
7z x OPNsense-{version}-dvd-amd64.iso.bz2
```

> Renombra el archivo
```
mv OPNsense-{version}-dvd-amd64.iso cdrom.iso
```

> Crea la carpeta en el servidor
```
mkdir /opt/unetlab/addons/qemu/opnsense-{version}
```

> Copia el ISO a la carpeta
```
rsync -Phvr cdrom.iso root@{ip-server}:/opt/unetlab/addons/qemu/opnsense-{version}/
```

> Ve a la carpeta de OPNsense
```
cd /opt/unetlab/addons/qemu/opnsense-{version}
```

> Crea un disco de 15GB
```
/opt/qemu/bin/qemu-img create -f qcow2 virtioa.qcow2 15G
```

Crea un nodo, ve a las opciones y configura 2vCPU y 4096MB de ram.

Debes conectar VNET1 a Cloud0. VNET1 es la interfaz WAN por defecto

Inicias la imagen, se selecciona automaticamente la instalacion hasta que aparesca el login, y debes seguir los siguientes pasos de instalacion
1. Inicia con el usuario "`installer`" y la contraseña "`opnsense`"
2. Continua con el keymap por defecto presionando Enter
3. Selecciona "Install (UFS)"
4. Selecciona el disco "vtbd0" y dale a "YES" para formatearlo, este proceso se demora 1 minuto
5. Ahora empezara la instalacion, la cual se demora 8 minutos
6. Selecciona "Complete Install" y luego "Reboot Now", apaga la maquina

> Elimina el disco
```
rm -drf /opt/unetlab/addons/qemu/opnsense-26.1/cdrom.iso
```

Inicia otra vez el nodo temporal para las ultimas modificaciones
1. Accede a la WebUI con la IP desde WAN, cuando aparesca el Wizard, presiona "Abort"
2. Ve a "System" > "Firmware" > "Status" y presiona "Check for updates", te saldra un changelog, lo cierras y vas al fondo de la pestaña y presionas "Update" y luego "OK"
3. Se demora aprox 20 minutos y reiniciara automaticamente el nodo
4. Cuando encienda, accede otra vez a la WebUI con la IP desde WAN
5. Ve a "System" > "Firmware" > "Plugins"
6. Instala el paquete `os-frr` apretando el signo "`+`", esta listo cuando aparesca "`***DONE***`"
7. Ahora que ya terminaste con la configuracion basica de OPNsense, puedes apagar el nodo

Ahora debes realizar el [[#Commit al Qcow2]]

> Nunca olvides arreglar los permisos para eve-ng
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

## Palo Alto
> [!IMPORTANT] Documentacion Recomendada
> - [Eve-NG Docs - Palo Alto](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-palo-alto/)
> - [Endoflife - PAN-OS](https://endoflife.date/panos)

| Carpeta              | Disco   | User    | Pass    | Boot  |
| -------------------- | ------- | ------- | ------- | ----- |
| `paloalto-{version}` | virtioa | `admin` | `admin` | 5 min |

Yo usare: `11.2.5`

Si eres mas exotico esta la version [Sysin - PAN-OS 12.1.7 KVM](https://sysin.org/blog/pan-os-12/) for 5USD en Alipay...

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/paloalto-{version}
```

> Renombra el archivo
```
mv PA-VM-KVM-{version}.qcow2 virtioa.qcow2
```

> Envia el Qcow2 al servidor
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/paloalto-{version}/
```

> Modifica el template `/opt/unetlab/html/templates/intel/paloalto.yml`. solo te muestro las lineas que debes modificar para las releases 11.x y superiores
```
qemu_version: 5.2.0
```

> Haz lo mismo en `/opt/unetlab/html/templates/amd/paloalto.yml`
```
qemu_version: 5.2.0
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Crea un nodo, conectalo a Cloud0 y enciendelo

## pfSense

Prefiere [[#OPNsense]]

> [!NOTE] Rant sobre NetGate
> - [Netgate Blog - Release PFsense CE 2.8.0](https://www.netgate.com/blog/netgate-releases-pfsense-community-edition-version-2.8.0)
> - [Netgate Forums - PFsense 2.8.0 full iso img](https://forum.netgate.com/topic/197601/pfsense-2-8-0-full-iso-img)
> 
> A partir de pfSense CE 2.8.0, obtener la Community Edition, requiere crear una cuenta, asociar un metodo de pago, proporcionar informacion personal y aceptar un EULA antes de descargar un instalador ONLINE. En versiones anteriores (< 2.7.2) la descarga era directa y offline.
> Personalmente, considero que este cambio hace que OPNsense sea una alternativa mucho mas comoda para la mayoria de usuarios

> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - pfSense](https://www.eve-ng.net/index.php/3380-2/)
> - [PFsense Direct Download Directory for 2.7.2](https://atxfiles.netgate.com/mirror/downloads/)

| Carpeta             | Disco   | User    | Pass      | Boot  |
| ------------------- | ------- | ------- | --------- | ----- |
| `pfsense-{version}` | virtioa | `admin` | `pfsense` | 3 min |

Una vez mas, recomiendo utilizar la version 2.7.2, descargando la ISO `pfSense-CE-2.7.2-RELEASE-amd64.iso.gz` desde el directorio

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/pfsense-{version}
```

> Copia el Archivo (Cambia "`netgate-installer-amd64.iso`" a "`cdrom.iso`")
```
rsync -Phvr cdrom.iso root@{ip-server}:/opt/unetlab/addons/qemu/pfsense-{version}/
```

> Ve a la carpeta
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

Ahora deberas hacer [[#Commit al Qcow2]] y Elimina el disco

> Nunca olvides arreglar los permisos para eve-ng
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

## SONiC

> [!TIP] Documentacion Recomendada
> - [Sonic Foundation](https://sonicfoundation.dev/)
> - [Github - sonic-net/SONiC](https://github.com/sonic-net/SONiC)
> 	- [User Manual](https://github.com/sonic-net/SONiC/blob/master/doc/user-manual/SONiC-User-Manual.md)
> 	- [Wiki](https://github.com/sonic-net/SONiC/wiki)
> - [Github - sonic-net/sonic-buildimage](https://github.com/sonic-net/sonic-buildimage)
> - [Blog - Networkz - vSONIC on EVE-NG](https://networkzblogger.wordpress.com/2021/07/31/vsonic-virtual-switch-on-eve-ng/)
> - [Sonic - Latest Images](https://sonic-net.github.io/SONiC/sonic_latest_images.html) | [Alternative Unnoficial Automatic Index](https://sonic.software/)

| Carpeta           | Disco   | User    | Pass           | Boot  |
| ----------------- | ------- | ------- | -------------- | ----- |
| `sonic-{version}` | virtioa | `admin` | `YourPaSsWoRd` | 1 min |

SONiC (**S**oftware for **O**pen **N**etworking *i*n the **C**loud) es un sistema operativo de red de codigo abierto, desarrollado originalmente Microsoft para Azure y actualmente mantenido por la Linux Foundation. Tiene distintos appliance para chips ASIC, para CPU general aka. x86 se utiliza **VS** (Virtual Switch), por lo que debes descargar desde la imagen "`sonic-vs.img.gz`".

Las compilaciones publicas se generan desde la rama "Master" por lo que no tiene releases, recomiendo utilizar la fecha de compilacion como version de carpeta, ejemplo: `sonic-20260707`

Yo utilize la rama `Master` el dia: `12-Jul-2026`

> Descomprime el archivo
```
gunzip sonic-vs.img.gz
```

> Renombra la imagen, no la conviertas
```
mv sonic-vs.img virtioa.qcow2
```

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/sonicsw-{version}
```

> Envia la imagen
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/sonicsw-{version}/
```

> Modifica el final `/opt/unetlab/html/templates/intel/sonicsw.yml`
```
qemu_arch: x86_64
qemu_version: 11.0.2
qemu_nic: virtio-net-pci
qemu_options: -machine type=q35,accel=kvm -vga std -device usb-ehci -device usb-tablet
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Crea un nodo, y modifica los siguientes parametros
- QEMU Version: `11.0.2`
- QEMU Arch: x86_64
- QEMU custom options: `-machine type=q35,accel=kvm -vga std -device usb-ehci -device usb-tablet`

> [!TIP] Sobre inicio
> Inicia la imagen desde grub, y dice que el sistema no encontro los archivos para iniciar y se cuelga

## VyOS
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - VyOS](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-vyos-vyatta/)
> - [VyOS - Official Site](https://vyos.net/)
> - [VyOS Docs](https://docs.vyos.io/en/rolling/)
> 	- [QuickStart](https://docs.vyos.io/en/rolling/quick-start.html)
> 	- [Configuration Examples](https://docs.vyos.io/en/rolling/configexamples/index.html)
> - [Nathan Paul Blog - Build Eve-NG with VyOS](https://npaul.uk/2021/01/build-the-best-free-network-learning-environment-with-eve-ng/)

| Carpeta          | Disco   | User   | Pass   | Boot  |
| ---------------- | ------- | ------ | ------ | ----- |
| `vyos-{version}` | virtioa | `vyos` | `vyos` | 2 min |

Debes descargar una ISO desde VyOS rolling release (nighly-build)

Yo utilize: `vyos-2026.06.30-0048-rolling-generic-amd64.iso`

> Crear Carpeta
```
mkdir /opt/unetlab/addons/qemu/vyos-{version}
```

> Renombra el iso
```
mv vyos-{version}-rolling-generic-amd64.iso cdrom.iso
```

> Mover las imagenes a esa carpeta
```
rsync -Phvr cdrom.iso root@{ip-server}:/opt/unetlab/addons/qemu/vyos-{version}/
```

> Ve a la carpeta
```
cd /opt/unetlab/addons/qemu/vyos-{version}/
```

> Crear disco Qcow2
```
/opt/qemu/bin/qemu-img create -f qcow2 virtioa.qcow2 10G
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Agrega un nodo, conectalo a Cloud0 e inicialo

Automaticamente seleccionara la instalacion via KVM console, para su correcta instalacion sigue los siguientes pasos
- Inicia sesion en la CLI con el usuario `vyos` y la contraseña `vyos`
- Inicia el proceso de instalacion con `install image`
- Presiona `y` para continuar
- Presiona `Enter` para utilizar el nombre por defecto de la imagen
- Configura una contraseña, yo utilizare `vyos`
- Confirma la contraseña
- Presiona `Enter` para utilizar la consola por defecto (`S: Serial`)
- Presiona `Enter` para utilizar el disco que configuraste
- Presiona `y` para continuar con la instalacion en el disco
- Presiona `y` para utilizar todo el contenido del disco
- Presiona `Enter` para utilizar el archivo de configuracion de boot por defecto (`Default: 1`)
- Una vez que termine la instalacion, apaga el nodo con `poweroff`
- Confirma el apagado con `y`

> Ve a la carpeta de VyOS
```
cd /opt/unetlab/addons/qemu/vyos-{version}/
```

> Elimina el disco iso
```
rm cdrom.iso
```

Ahora deberas hacer [[#Commit al Qcow2]]

> Nunca olvides arreglar los permisos para eve-ng
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

## Commit al Qcow2

Antes de empezar a hacer commit, SIEMPRE LA MAQUINA DEBE ESTAR APAGADA.

El commit es un paso critico, si no lo haces, todo lo que hagas en esa imagen desaparecera, por lo que EVE-NG trabaja con una imagen temporal como difurcacion de la imagen temporal, el commit configura esa imagen temporal como la nueva base a utilizar

Necesitas 3 valores

- UUID: Es un identificador unico del laboratorio, este valor lo encuentras en la barra lateral izquierda, en "Lab Details"
	- Ejemplo: `ID: 85bd7141-e2a7-43e6-8307-bfa7b301a12b`
- POD ID: Identificador de tu usuario en EVE-NG. Lo encuentras en la pestaña de usuarios, aunque si solo usas un usuario, este sera el `0` por defecto
	- Ejemplo: `0`
- NODE ID: Identificador del nodo dentro del lab. Hack click derecho sobre un nodo en el lab y busca el nombre que sale entre parentesis junto al nombre
	- Ejemplo: `OPNsense (1)`

Con esos tres valores, armas la ruta temporal donde EVE-NG copio la imagen para levantar el nodo, ruta ejemplo: `/opt/unetlab/tmp/{POD-ID}/{LAB-UUID}/{NODE-ID}`
```
cd /opt/unetlab/tmp/0/85bd7141-e2a7-43e6-8307-bfa7b301a12b/1/
```

Dentro de esa carpeta vas a encontrar el disco de la imagen. Siempre la extension sera `.qcow2`, pero el nombre puede ser `virtioa`, `hda`, `sataa`, etc.

> Haces commit a la imagen
```
/opt/qemu/bin/qemu-img commit virtioa.qcow2
```

> En caso de que falle con un error `Co-routine re-entered recursively` o `CORE DUMMPED`, prueba con utilizar la version de qemu mas moderna para hacer el commit
```
/usr/bin/qemu-img commit virtioa.qcow2
```

> Vuelve a la carpeta que estas configurando dentro de qemu
```
cd /opt/unetlab/addons/qemu/{carpeta-imagen}
```

Debes de eliminar el archivo iso, o cuando inicies otra vez el nodo, empezara la instalacion, si quieres guardar el iso, bastan con cambiar el nombre a cualquiera que no sea cdrom.iso, de esa forma, EVE-NG lo ignorara

> Eliminas el archivo cdrom.iso
```
rm -f cdrom.iso
```

> Arreglas los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Y de esa forma ya esta disponible la imagen para su uso, si quieres que utilize aun menos espacio, la siguiente seccion comprime los discos

## Comprimir imagenes

Si funciona la version comprimida, puedes borrar el original
1. Ir a la carpeta
```
cd /opt/unetlab/addons/qemu/{carpeta-imagen}
```

2. Comprime `virt-sparsify`
```
virt-sparsify --compress virtioa.qcow2 cvirtioa.qcow2
```

3. Reescribe el archivo
```
mv cvirtioa.qcow2 virtioa.qcow2
```

# Configuraciones

## Interfaces

> [!TIP] Lecturas recomendadas
> - [PeteNetLive - EVE-NG Connecting to the internet](https://www.petenetlive.com/KB/Article/0001432)

En EVE-NG, se pueden conectar interconectar nodos dentro del lab o hacia el exterior sin necesidad de hardware real. Y estos son

- Bridge: Se comporta como un Switch no gestionado. Todos los dispositivos conectados se comparten, no tiene salida a Internet por si mismo, pero puede ser extendido si se conecta a otros elementos como `Cloud0`
- Cloud0 (Management): Es la interfaz de gestion de EVE-NG. Esta preconfigurada para permitir la salida hacia la red externa a travez del host.

> Revisa el estado de los bridges
```
brctl show
```

Si ejecutas EVE-NG como una maquina virtual (QEMU/KVM, VMware) puedes agregar interfaces de red virtuales (vNIC) adicionales. Mi recomendacion es mantener Cloud0 como una interfaz de administracion, conectada en modo Bridge, y agregar una o mas vNIC configuradas en modo NAT para utilizarlas en los laboratorios.

Estas interfaces apareceran dentro de EVE-NG como `eth0`, `eth1`, etc. Y estan asociadas automaticamente a `pnet0`, `pnet1`, etc. Y esas se ven dentro de EVE-NG como `cloud0`, `cloud1`, etc. respecticamente. De esta forma obtienes redes independientes de la interfaz de administracion, sin modificar el servidor.

## Consolas Nativas

> [!TIP] Lecturas Recomendadas
> - [EVE-NG - Download Client Side](https://www.eve-ng.net/index.php/download/): Debes bajar hasta las herramientas de tu OS
> - [Youtube - EVE-NG - EVE Install Telnet VNC Wireshark Local Management](https://youtu.be/Ea4U93991dw?si=ZJexX-GdwKTjVSS3)
> - [Putty Features SSH Handler .reg config (Ru)](https://putty.org.ru/features/ssh-handler): En caso de necesitar un .reg base para modificar

Para Windows, debes descargar la el pack oficial desde EVE-NG, este verifica las instalaciones y ademas instala los wrappers, registros y configuraciones extras

En caso de fallar por ejemplo Putty al iniciar, deberas modificar un archivo .reg y apuntar las rutas correctamente

## Actualizar Templates

> [!TIP] Lecturas Recomendadas
> - [EVE-NG Docs - Update Template](https://www.eve-ng.net/index.php/documentation/howtos/template-icons-and-config-scripts-update-from-git/)
> - [Gitlab - eve-ng-dev](https://gitlab.com/eve-ng-dev)

> Ve a la carpeta de templates
```
cd /opt/unetlab/html/
```

> Elimina los viejos templates
```
rm -drf templates
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

## Labs

Ahora es momento de utilizar tu version de EVE-NG, si no tienes ideas, puedes leer [[800 - Extras/Write-Ups/EVE-NG/EVE-NG - Labs|EVE-NG - Labs]]

# Extra
## Porque no PNETLab?
> [!TIP] Fuente
> - [EVE-NG Forums - SCAMMERS PNETLAB](https://eve-ng.net/forum/viewtopic.php?t=16925)

PNETLab es un fork de EVE-NG, el cual extendio las funciones de EVE-NG Pro sin contar con una licencia oficial, lo que desencadeno polemicas por posible uso de codigo cerrado.

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

## OpenWRT

Oficialmente EVE-NG no soporta funciones Wireless, pero igual hay que recopilar informacion en caso de que si sea soportado

> [!IMPORTANT] Documentacion Recomendada
> - [Github Gist - rodrigojusto/Eve-NG - OpenWRT x86](https://gist.github.com/rodrigojusto/684308f6d65ac86a3c845912cee86789)
> - [OpenWRT Download 25.12.5](https://downloads.openwrt.org/releases/25.12.5/targets/x86/64/)
> - [OpenWRT Docs - Run in QEMU x86-64](https://openwrt.org/docs/guide-user/virtualization/qemu#openwrt_in_qemu_x86-64)

> [!NOTE] Nombre Imagen
> - Carpeta OpenWRT: `openwrt-{version}`
> 	- Disco QEMU: `hda`

## Huawei WAF5K

> [!NOTE] Nombre Imagen
> - Carpeta Huawei USG6000v: `huaweiwaf5k-{version}`
> 	- Disco QEMU: `hda`
> - Login
> 	- User: `admin`
> 	- Pass: `Admin@123`

**W**eb **A**pplication **F**irewall, complemento del USG6000v

Yo encontre: `huaweiwaf5k-VV200R001C00 - 754.18M`

No esta en EVE-NG por lo que hay que agregar

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/huaweiwaf5k-{version}
```

> Crea el archivo `huaweiwaf5k.yml` con el siguiente contenido
```
# Copyright (c) 2016, Andrea Dainese
# Copyright (c) 2018, Alain Degreffe
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are met:
#     * Redistributions of source code must retain the above copyright
#       notice, this list of conditions and the following disclaimer.
#     * Redistributions in binary form must reproduce the above copyright
#       notice, this list of conditions and the following disclaimer in the
#       documentation and/or other materials provided with the distribution.
#     * Neither the name of the UNetLab Ltd nor  the name of EVE-NG Ltd nor the
#       names of its contributors may be used to endorse or promote products
#       derived from this software without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
# ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
# WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
# DISCLAIMED. IN NO EVENT SHALL <COPYRIGHT HOLDER> BE LIABLE FOR ANY
# DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
# (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
# LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND
# ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
# (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
# SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
---
type: qemu
description: Huawei WAF5000
name: HWAF5000
cpulimit: 1
icon: Firewall.png
cpu: 2
ram: 2048
ethernet: 2
console: vnc
qemu_arch: x86_64
qemu_version: 4.1.0
qemu_nic: virtio-net-pci
qemu_options: -machine type=pc,accel=kvm -vga std -usbdevice tablet -boot order=dc
...
```

> Envia el template `huaweiwaf5k.yml` a Intel
```
rsync -Phvr huaweiwaf5k.yml root@{ip-server}:/opt/unetlab/html/templates/intel/
```

> Envia el template `huaweiwaf5k.yml` a AMD
```
rsync -Phvr huaweiwaf5k.yml root@{ip-server}:/opt/unetlab/html/templates/amd/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Crea un nodo y enciendelo. No logre hacer funcionar esta imagen...

> [!TIP] Sobre inicio
> Inicia Linux, selecciona automaticamente CentOS 7, e inicia en 1 minuto

## Juniper
En general no me funciono ninguna imagen, en teoria deberian de funcionar


> [!TIP] Documentacion Recomendada
> - [Juniper - Create Account](https://userregistration.juniper.net/): Es mañoso
> - [Juniper Learning Portal - Open Learning](https://learningportal.juniper.net/juniper/user_activity_info.aspx?id=JUNIPER-OPEN-LEARNING)

Hay unos reemplazos que aclaran el panorama, los viejos son considerados EOL
- vMX -> vJunos Router o Evolved
- vQFX -> vJunos EX Switch
- vSRX -> vSRX 3.0

En este write up, no hare ni la instalacion de Apstra AOS ni SDWAN 128T

Tuve que crearme una cuenta con Chromium, porque no cargaba reCaptcha, elegi "Guest User Access" para el tipo de cuenta

### vJunos Router
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - vJunos-Router](https://www.eve-ng.net/index.php/documentation/howtos/vjunos-router/)
> - [Juniper Support - Download vJunos-Router](https://support.juniper.net/support/downloads/?p=vjunos-router): Debes seleccionar el OS: "vJunos-Router"
> - [Juniper Docs](https://www.juniper.net/documentation/)
> 	- [vJunos-Router Docs](https://www.juniper.net/documentation/product/us/en/vjunos-router/)
> 	- [vJunos-Router HW requirements](https://www.juniper.net/documentation/us/en/software/vjunos-router/vjunos-router-kvm/topics/vjunos-router-kvm-hw-requirements.html)

> [!NOTE] Nombre Imagen
> - Carpeta vJunos-Router: `vjunosrouter-{version}`
> 	- Disco QEMU: `virtioa`
> - Login
> 	- User: `root`
> 	- Pass: N/A (Presiona Enter)

Este es un Router Clasico de proposito general para laboratorios donde trabajes con BGP, OSPF, MPLS basico, con un comportamiento basado en vMX

Descarga la ultima version disponible, en mi caso `26.2R1`

> En el servidor crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/vjunosrouter-{version}
```

> Renombra la imagen
```
mv vJunos-router-{version} virtioa.qcow2
```

> Envia la imagen Qcow2 descargada
```
rsync -Phvr virtioa.qcow2.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/vjunosrouter-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Crea un nodo, conectalo a Cloud0 y enciendelo

> [!TIP] Sobre inicio
> En total se demora unos 15 minutos en iniciar el VM. Inicia el sistema en 1 minutos, luego verifica los componentes del sistema durante unos 6 minutos, se queda pegado en `random: HMAC-DRBG: instantiated with 1024 primary SW events...` durante unos 2 minutos y luego continua para quedarse otra vez pegado aunque no me inicia ningun Login...
> 
> Estoy en un VM sobre KVM, y dice explicitamente que no es compatible, asi que personalmente no me funciona esta imagen

https://community.juniper.net/discussion/anyone-here-success-play-around-with-vjunos-router

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

Yo usare: `vJunosEvolved-26.2R1.7-EVO.qcow2`

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/vjunosevo-{version}
```

> Renombra el archivo
```
mv vJunosEvolved-{version}-EVO.qcow2 virtioa.qcow2
```

> Envia el Qcow2 al servidor
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/vjunosevo-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

Crea un nodo, y conecta la interfaz `re0:mgmt-0` a Cloud0. Luego crea una red bridge y conecta `pfe1`, `rpio2`, `rpio3`, `pfe4` a esta. Enciende el nodo y reza

> [!TIP] Sobre inicio
> Estoy en un VM sobre KVM, y dice explicitamente que no es compatible, asi que personalmente no me funciona esta imagen

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

Yo usare: `vJunos-switch-26.2R1.7.qcow2`

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/vjunosswitch-{version}
```

> Renombra el archivo
```
mv vJunos-switch-{version}.qcow2 virtioa.qcow2
```

> Envia el Qcow2 al servidor
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/vjunosswitch-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

> NOTA: Este switch se apaga desde CLI antes de apagarlo desde la WEBUI
```
request system power-off
```

Crea un nodo, Enciendelo y reza

> [!TIP] Sobre inicio
> En total se demora unos 15 minutos en iniciar el VM. Inicia el sistema en 1 minutos, luego verifica los componentes del sistema durante unos 6 minutos, se queda pegado en `random: HMAC-DRBG: instantiated with 1024 primary SW events...` durante unos 2 minutos y luego continua para quedarse otra vez pegado aunque no me inicia ningun Login...
> 
> Estoy en un VM sobre KVM, y dice explicitamente que no es compatible, asi que personalmente no me funciona esta imagen


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

Necesitas tener un contrato activo para poder descargar esta imagen...

vSRX 3.0 es la version virtualizada de los Firewall SRX de Juniper, por lo que tiene la misma CLI y Junos OS que el HW fisico. Trabaja con zonas de seguridad (trust, untrust, dmz) y politicas entre zonas. SIn licencia puedes usar todo lo Standard, que es Stateful Firewall, NAT, VPN, IPsec/SSL y routing. las funciones avanzadas como IPS, antivirus y filtrado web necesitan una licencia para utilizarse, de igual forma, un laboratorio no necesita ser tan fancy.

Yo utilize: ``

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
