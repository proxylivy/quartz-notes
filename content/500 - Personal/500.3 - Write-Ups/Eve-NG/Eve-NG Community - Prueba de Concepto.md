# Info

Version 0.3

## Un poco de Historia
En el mundo de la virtualizacion de rede, EVE-NG es una solucion que unifica diversas tecnologias y proyectos en una plataforma robusta y flexible. Su desarrollo se basa en varios proyectos previos como Cisco WebIOL, IOU-WEB y UnetLab. 

Hay que remontarse al 2011, Cisco trabaja en una herramienta interna llamada "WebIOU on Linux (x64)" a.k.a WebIOL, desarrollada en Perl para sus propios ingenieros, utilizada en el programa Cisco 360. Al no estar accesible al publico y contar con licencia cerrada, surgio la necesidad de una alternativa.

En respuesta, el 23 de enero de 2012, [Andrea Dainese](https://www.adainese.it/), conocido en Github como [dainok](https://github.com/dainok), lanzo IOU-WEB, un emulador escrito en PHP que aprovechaba las imagenes IOL (IOS on Linux). Esta solucion destaco por ser portable, estable y en muchos casos, mas agil que Dynamips. Durante sus primeros años IOU-WEB acumulo centenares de usuarios, pero con el tiempo Andrea decidio pausar su mantenimiento y su ultima version (1.2.2-24) data del 28 de mayo de 2015 y se encuentra su repositorio en Github [dainok/iou-web](https://github.com/dainok/iou-web)

Durante ese tiempo existio un sitio web llamado "Evil Routers" a.k.a "routereflector" que recopilaba informacion del funcionamiento, algunas paginas se pueden recuperar con ayuda de Wayback Machine son: La [pagina de inicio](https://web.archive.org/web/20190617180122/http://evilrouters.net/), o el [FAQ de Cisco IOU](https://web.archive.org/web/20180207132822/http://evilrouters.net/2011/01/18/cisco-iou-faq/).

> [!NOTE] Nota sobre IOU WEB
> He trabajado un buen tiempo en maquinas virtuales con [[020 - Conceptos/020.4 - Dispositivos de Red/IOU WEB|IOU WEB]], solucionando un par de problemas importantes, puedes ver mi esfuerzo en distintos Write UPS
> - [[500 - Personal/500.3 - Write-Ups/IOU-WEB/IOU WEB - 32 Bits CentOS 6 (Original Upgrade)|IOU WEB - 32 Bits CentOS 6 (Original Upgrade)]]: La implementacion que me entregaron, una maquina de 32 bits con CentOS 6, es la que mejor funciona
> - [[500 - Personal/500.3 - Write-Ups/IOU-WEB/IOU WEB - 64 Bits CentOS 7 (Icarus to Nirvana)|IOU WEB - 64 Bits CentOS 7 (Icarus to Nirvana)]]: Intente mejorar el funcionamiento para aceptar maquinas de 64 bits como los routers XE
> - [[500 - Personal/500.3 - Write-Ups/IOU-WEB/IOU WEB - Config Win 10-11|IOU WEB - Config Win 10-11]]: Set de configuraciones para Windows
> - [[500 - Personal/500.3 - Write-Ups/IOU-WEB/IOU WEB - Intentos Fallidos|IOU WEB - Intentos Fallidos]]: Coleccion de ideas que no llegaron a buen puerto...
> - [Youtube - Nicolas Contador - IOU-WEB](https://youtu.be/Wo_W6hcBXy0?si=GzVKPTpF-42eVTHx)

Dainok, necesitaba no solo ejecutar imagenes IOL, sino tambien emuladores basados en Dynamips y QEMU/KVM, asi que en octubre de 2014, inicia el desarrollo de "UnetLab", el cual su fuente esta disponible en Github [dainok/unetlab](https://github.com/dainok/unetlab), junto a otros colaboradores como:
- Uldis Dzerkals
- Alain Degreffe
- Michael Yandulov
- Ruslan Foutorianski
- Dragoş Vasiloi

Este proyecto amplio el soporte a multiples vendors. A inicios de 2015, Dainok anuncio que se alejaba del proyecto por falta de tiempo, dejando la evolucion a manos de la comunidad. Aunque hubieron luego planes para crear UnetLab v2, no llego a buen puerto y la rama quedo discontinuada en algun punto de 2016.

> [!TIP] Lecturas recomendadas
> - [Dainok - UnetLab: The Story](https://www.adainese.it/blog/2019/01/01/unified-networking-lab-unetlab-the-story/)
> - [Dainok - Speculating About UnetLabv3](https://www.adainese.it/blog/2022/04/20/speculating-about-unetlabv3/)
> - [Dainok - Unetlabv3 Development Series](https://www.adainese.it/blog/2024/07/22/unetlab-v3-development-series/)
> - [Conproly - Networking lab Setup with UnetLab](https://www.conproly.com/blogs/20161129-networking-lab-setup-with-unetlab/)
> - [Packet Pusher Priority Queue Podcast - PQ Show 61: The UnetLab Project via Internet Archive Wayback Machine](https://web.archive.org/web/20151029024028/http://packetpushers.net/podcast/podcasts/pq-show-61-unetlab-project/)

En 2016, se crea "EVE-NG LTD", con la cual fue trabajando internamente, El 5 de enero de 2017, Uldis Dzerkalis y un equipo presentaron al publico [EVE-NG](https://www.eve-ng.net/), el cual es un fork del trabajo hecho en UnetLab que continua en desarrollo.

## Otros Emuladores
> [!TIP] Lectura Recomendada
> - [Habr MaxxxZ Blog - Imitacion de Cisco, Identica al original (Ru)](https://habr.com/ru/articles/494504/)
> - [Nil Uniza SK Blog - Network Simulation Software List](https://nil.uniza.sk/en/network-simulation-virtualization-software-list/)

Durante mas de dos decadas, han existido intentos para emular o virtualizar entornos de red complejos sin necesidad de hardware fisico. Aunque muchos proyectos han quedado obsoletos o abandonados, otros todavia persisten hasta el dia de hoy (10-Junio-2025)

Aunque si tienes dinero de sobre y luz para gastar, podrias hasta comprar el equipamiento de red que necesites, es el mas entretenido pero mas costoso de realizar y no es muy dinamico para intercambiar dispositivos, pero es una opcion

### Emuladores Clasicos
Estos son los primeros que se crearon y los que van evolucionando por una linea muy marcada por el uso de imagenes IOS tradicionales, extension de funcionalidades y laboratorios estilo CCNA/CCNP.

**Dynamips**
> [!TIP] Lectura Recomendada
> - [Github GNS3/dynamips](https://github.com/GNS3/dynamips)
> - [IPflow Blog via Internet Archive Wayback Machine](https://web.archive.org/web/20120427212815/http://www.ipflow.utc.fr/blog/?cat=1)
> - [Dynagen Blog via Internet Archive Wayback Machine](https://web.archive.org/web/20151127223804/https://www.dynagen.org/)
> - [IPflow Blog - Cisco 7200 Simulator via Internet Archive Wayback Machine](https://web.archive.org/web/20140314073202/www.ipflow.utc.fr/index.php/Cisco_7200_Simulator)
> - [Dynagen Docs - Tutorial via Internet Archive Wayback Machine](https://web.archive.org/web/20120212144911/http://dynagen.org/tutorial.htm)
> - [Dynagui Sourceforge via Internet Archive Wayback Machine](https://web.archive.org/web/20140714192058/http://dynagui.sourceforge.net/)

Es el tio abuelo de la emulacion, nacio en Agosto de 2005 por Fabien Devaux, Christophe Fillot y MtvE para emular routers cisco 1700, 2600, 3600, 3700 y 7200. Fue pionero y todavia sobrevive como parte del backend de GNS3, aunque no se recomienda para entornos modernos

En 2006, Greg Anuzelli creo Dynagen, un front-end escrito en Python para facilitar el uso

Dynagui esta basado en la libreria Dyna-gen, creado por "Yannick Le Teigner" para aplicar distintos parches

Luego el "30 de Julio de 2013", con el lanzamiento de la version 0.2.9 de Dinamips se creo un fork a cargo de GNS3 disponible en Github, continua siendo usado, pero no es recomendado realmente

**Cisco Packet Tracer**
> [!TIP] Lectura Recomendada
> - [Netacad Learning Resources - Download Packet Tracer](https://www.netacad.com/es/resources/lab-downloads)
> - [Packettracer Networks Blog - New Features Packet Tracer 9.0.0 Beta](https://www.packettracernetwork.com/features/packet-tracer-9-new-features.html)

Packet Tracer es la herramienta de simulacion oficial desarrollada por Cisco para entornos educativos, la cual es gratuita pero necesita una cuenta registrada en el programa Netacad (Network Academy). Su objetivo no es emular el hardware real, sino ofrecer una plataforma para crear topologias ilustrativas con configuraciones y comportamientos simplificados.

Desde la version 8 en adelante, Cisco ha ido integrando soporte para dispositivos IoT, programacion con Python y microcontroladores como parte de las iniciativas STEM.


**GNS3**
> [!TIP] Lectura Recomendada
> - [GNS3 Docs - Getting Started](https://docs.gns3.com/docs/)
> - [GNS3 - Marketplace](https://gns3.com/marketplace/featured)
> - [GNS3 - Teachable Academy](https://gns3.teachable.com/)
> - [Github GNS3/ubridge](https://github.com/GNS3/ubridge)
> - VPCS
> 	- [Github GNS3/vpcs](https://github.com/GNS3/vpcs)
> 	- [Freecode VPCS Wiki via Internet Archive Wayback Machine](https://web.archive.org/web/20120622133308/http://www.freecode.com.cn/doku.php?id=wiki:vpcs)
> - [Rednectar Blog - A Little GNS3 History](https://rednectar.net/gns3-workbench/a-little-gns3-history/)
> - [Tolgabagci Blog - What is GNS3?](https://www.tolgabagci.com/en/what-is-gns3/)

**G**raphical **N**etwork **S**imulator 3, nacio en 2007 como una interfaz grafica para facilitar el uso de Dynamips, que era bastante famoso en esa epoca. Su creador, Jeremy Grossman, buscaba una forma mas accesible y visual de diseñar laboratorios sin tener que escribir manualmente cada conexion. Desde entonces, ha evolucionado sin parar, hoy soporta multiples tecnologias como QEMU/KVM, Docker, IOU, VMs, e imagenes Multivendor.

Ademas cuenta con una comunidad activa basado en el Open Source, un marketplace de dispositivos preconfigurados y una academia con cursos (Gratis y Pagados) para quienes esten empezando o quieren profundizar en automatizacion.

Como extra, mantienen forks de programas viejos integrados a su stack, algunos ejemplos son Dynamips (Descrito mas arriba), VPCS y ubridge (antes iouyap).

### Emuladores Comerciales

**CML-P**
> [!TIP] Lectura Recomendada
> - [Cisco Devnet Docs - Introduction to Cisco Modeling Labs v2.8](https://developer.cisco.com/docs/modeling-labs/2-8/)
> - [Cisco Devnet Docs - CML 2.8 Release Notes Changelog](https://developer.cisco.com/docs/modeling-labs/2-8/cml-release-notes/#summary-of-changes)
> - [Cisco DevNet Docs - CML-Free 2.8](https://developer.cisco.com/docs/modeling-labs/2-8/cml-free/)
> 	- [Cisco ID Login](https://id.cisco.com/)
> 	- [Cisco MKTO - Register to CML-Free](https://mkto.cisco.com/cml-free.html)
> 	- [Cisco - Software Download](https://software.cisco.com/download/home/)
> 	- [Cisco Devnet Docs - CML Installation Guide](https://developer.cisco.com/docs/modeling-labs/cml-installation-guide/)
> - [Cisco Learning Network Store - CML Personal License](https://learningnetworkstore.cisco.com/cisco-modeling-labs-personal/cisco-modeling-labs-personal/CML-PERSONAL.html)
> - [Packet Thrower Blog - Using VIRL](https://the-packet-thrower.com/using-virl/)
> - [David Bombal Blog - Cisco CML-P Series (Part 1)](https://ccnax.com/cisco-cml-p-virl-2-download-install-and-configure-part-1/)

**C**isco **M**odeling **L**ab - **P**ersonal, es una plataforma oficial de Cisco con un costo de $200USD/año, este nacio en 2014 bajo el nombre de "VIRL" (**V**irtual **I**nternet **R**outing **L**ab), luego fue rebautizado como "CML" para uso corporativo, y en 2020, el trabajo en VIRL² paso a llamarse CML-P o CML².

Permite simular topologias con imagenes reales de IOSv, IOS-XRv, NX-OS, entre otros mas directamente de la fuente de Cisco

Cisco lanzo una propuesta llamad CML-Free en su version 2.8.0 del 21 de noviembre de 2024 de CML², a la cual debes registrarte en Cisco MKTO.

Entre los limites de esta version esta
- Solo permite 5 nodos simultaneamente en un laboratorio (Los conectores externos y Switches tontos no cuentan como nodos)
- Usa un mayor nivel de telemetria anonima
- Permite descargar imagenes de Cisco directamente pero solo del tier FREE

**Boson Cisco Netsim**
> [!TIP] Lectura Recomendada
> - [Boson - Cisco Network Simulator](https://www.boson.com/netsim-cisco-network-simulator)

Un simulador cerrado de pago que permite emular laboratorios de Cisco enfocado en sus certificaciones CCNA, CCNP y CCIE. No permite utilizar imagenes externas y es sponsor de varios canales de redes, lo dejo aqui para que lo reconoscan, yo que ustedes, no me dejaria engañar

### Basado en contenedores
**ContainerLab**
> [!TIP] Lectura Recomendada
> - https://containerlab.dev/

**Kathara**
> [!TIP] Lectura Recomendada
> - https://www.kathara.org/

### Enfoque Academico o Investigacion

**Cloonix**
> [!TIP] Lectura Recomendada
> - http://clownix.net/
> - https://github.com/clownix/cloonix

**CORE**
> [!TIP] Lectura Recomendada
> - https://github.com/coreemu/core

Common Research Emulator

**Imunes**
> [!TIP] Lectura Recomendada
> - https://www.imunes.net/


**Mini-Net**
> [!TIP] Lectura Recomendada
> - http://mininet.org/

**Mini-Net Wifi**
> [!TIP] Lectura Recomendada
> - https://mininet-wifi.github.io/

**NS-3**
> [!TIP] Lectura Recomendada
> - https://www.nsnam.org/

**Shadow**
> [!TIP] Lectura Recomendada
> - https://shadow.github.io/
> - https://shadow.github.io/docs/guide/

**Netkit JH**
> [!TIP] Lectura Recomendada
> - https://netkit-jh.github.io/

Fork activo y mantenido de Netkit-ng y Netkit, se enfoca en laboratorios ligeros de red en entornos Linux. Se basa en User Mode Linux (UML) y tiene una fisolofia muy academica: terminal, scripting y ligereza. Ideal para contextos universitarios o cursos introductorios

## Finalmente EVE-NG
**Licenciamiento de Eve-NG**
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

Nos centraremos en la version "`Community`" para nuestras pruebas de conceptos
> [!WARNING] Notas sobre licencia y Docker
> La edicion Community **no soporta Docker**. Para utilizar utilizar contenedores integrados (Paquete `eve-ng-dind`), se necesita tener la version PRO o Learning Center
> Mas al respecto: [Youtube - EVE-NG - EVE Pro embedded Docker Setup and Usage](https://www.eve-ng.net/index.php/documentation/howtos-video/eve-embedded-dockers-setup-and-usage/)

> [!WARNING] Workarround sobre Docker
> Una alternativa seria utilizar una maquina `alpine` con docker instalado para que se conecte a las otras maquinas, de esta forma docker no se instala directamente en EVE-NG Community

---
**Soporte de Dispositivos**
> [!TIP] Lecturas recomendadas
> - [EVE-NG Docs - Supported Images](https://www.eve-ng.net/index.php/documentation/supported-images/)
> - [EVE-NG Docs - QEMU Image Namings](https://www.eve-ng.net/index.php/documentation/qemu-image-namings/)
> - [EVE-NG Docs - HowTos](https://www.eve-ng.net/index.php/documentation/howtos/)

Tiene una gran variedad, las cuales varias segun las capacidades de las propias imagenes

| Vendor                | Router<br>L3 | Switch<br>L2 / L3 | Firewall | Load<br>Balancer | SD-WAN      | IDS/IPS |
| --------------------- | ------------ | ----------------- | -------- | ---------------- | ----------- | ------- |
| Cisco                 | ✓            | ✓                 | ✓ ASAv   | ✗                | ✓ CSR1000v  | ✗       |
| Fortinet              | ✓            | ✗                 | ✓ FGT    | ✓ ADC            | ✓ FGT       | ✓ FNDR  |
| Juniper               | ✓ vMX        | ✓ vQFX            | ✓ vSRX   | ✗                | ✓ vSRX      | ✗       |
| MikroTik              | ✓            | ✗                 | ✗        | ✗                | ✗           | ✗       |
| Arista<br>(vEOS)      | ✓            | ✓                 | ✗        | ✗                | ✗           | ✗       |
| Extreme<br>Networks   | ✓ VOSS       | ✓ EXOS            | ✗        | ✗                | ✗           | ✗       |
| Palo Alto<br>(PAN-OS) | ✗            | ✗                 | ✓        | ✗                | ✗           | ✓       |
| F5                    | ✗            | ✗                 | ✗        | ✓ Big-IP         | ✗           | ✗       |
| Hillstone<br>SG6k     | ✓            | ✗                 | ✓ VW     | ✓ vADC           | ✓ CloudEdge | ✗       |
| VyOS                  | ✓            | ✓                 | ✓\*      | ✓\*              | ✗           | ✗       |
| Huawei                | ✓ AR1k       | ✓ CE12800         | ✓ USG6kv | ✗                | ✓ USG6kv    | ✓ WAF5k |
\*: Solo uso basico

**Detalle Imagenes Utilizadas**

Cisco IOL image list:
- Cisco IOL
	- L2/L3 Switch: i86bi_linux_l2-adventerprisek9-ms.SSA.high_iron_20190423.bin (15.2 - 2019-04-23)
	- L2/L3 Switch: i86bi_LinuxL2-AdvEnterpriseK9-M_152_May_2018.bin (15.2 - 2018-05-10)
	- L3 Router and PC: i86bi_LinuxL3-AdvEnterpriseK9-M2_157_3_May_2018.bin (15.7 - 2018-05-10)
	- L3 XE Router 64 bits: x86_64_crb_linux-adventerprisek9-ms.bin (17.12.1)
	- L2/L3 XE Switch 64 bits: x86_64_crb_linux_l2-adventerprisek9-ms.bin (17.12.1)

QEMU image list:
- HPE ArubaCX-10.07
- Cisco
	- ASA-9.1.5
	- ASAv-9.22.1.1-PLR-Licenced
	- c9800cl-17.17.01
	- CSR1000vng-universalk9.17.03.05-serial
	- CSR1000v-universalk9.17.03.08a-serial
- Cisco vIOS Router
	- vios-adventerprisek9-m.SPA.159-3.M6 (Slow)
- Cisco vIOS Switch
	- viosl2-adventerprisek9-m.ssa.high_iron_20200929 (Slow)
- Extreme Networks
	- ExtremeVOSS 9.2.0.0 - [Free](https://github.com/extremenetworks/Virtual_VOSS)
	- ExtremeEXOS 33.1.1.31 - [Free](https://github.com/extremenetworks/Virtual_EXOS)
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
	- Arch Linux (By me ^^) - [Open Source](https://archlinux.org/)
- Microtik RouterOS 7.18.2 - [Free](https://mikrotik.com/download)
- IP Fusion OcNOS 6.6.0
- OpenWRT 24.10.1 - [Open Source](https://openwrt.org/)
- OPNsense 25.1 - [Open Source](https://opnsense.org/)
- Palo Alto 11.2.5
- PfSense-pfs 2.7.2 - [Open Source](https://atxfiles.netgate.com/mirror/downloads/)
- vJunosEVOefi 24.4R1.8
- Vyos 1.5 Rolling Release - [Open Source](https://vyos.net/) - [Changelog](https://github.com/vyos/vyos-nightly-build/releases)
- Virtual PC (VPCS) - [Open Source](https://github.com/GNS3/vpcs)

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

Para los clientes usar la `Consola Nativa` clientless, se debe instalar el paquete el cual esta disponible para [Windows](https://www.eve-ng.net/index.php/download/#DL-WIN), [MacOS](https://www.eve-ng.net/index.php/download/#DL-OSX) y [Linux](https://www.eve-ng.net/index.php/download/#DL-LIN)

> [!NOTE] Extra
> Putty sobre Windows necesita un [.reg (Ru)](https://putty.org.ru/features/ssh-handler) correcto

**Uso Maquina**
Disk Usage: Aproximadamente 25GB

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

# Fase 1: Instalacion de EVE-NG
> [!IMPORTANT] Importante
> - Al instalar [EVE-NG Community](https://www.eve-ng.net/index.php/community/) Usa automaticamente la version de Ubuntu 22.04.4 LTS (Jammy Jellyfish), no se debe actualizar la version o dejara de funcionar
> 	- Disponibilidad hasta Apr 2027 - ESM 2032 | [Ubuntu - Release Page](https://www.releases.ubuntu.com/22.04/)

> [!NOTE] Documentacion Recomendada
> - [Documentacion Eve-NG - First Boot](https://www.eve-ng.net/index.php/documentation/installation/howto-configure-eve-during-first-boot/)
> - [Arny Blog - Instalacion EVE-NG (Ru)](https://arny.ru/linux/ustanovka-eve-ng/)
> - [JD-Networks Blog - Eve NG Install](https://jd-networks.co.uk/blog/2019/05/24/eve-ng-community-edition/)
> - [Dainok - Installing Eve-NG](https://www.adainese.it/blog/2023/09/21/installing-eve-ng/)
> - [EVE-NG Cookbook](https://www.eve-ng.net/index.php/documentation/community-cookbook/)
> 	- Hoja 24: 3.3 BM server install
> 	- Hoja 46: 3.7 Login to the EVE WEB GUI
> 	- Hoja 48: 4.2 EVE-NG Community Upgrade
> 	- Hoja 57: 6 EVE WEB GUI Managent

Requerimientos
- USB de al menos 8GB
- Descargar [Ventoy](https://www.ventoy.net/en/index.html) o [Rufus](https://rufus.ie) para configurar el Pendrive

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

> Paso 3: Prueba de internet y Actualizar Paquetes Servidor
```
ping -c 2 google.cl
apt-get update && apt-get upgrade
apt autoremove
```

> Paso 3.1: Reestablecer servicios
```
# Selecciona todos #
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

> Paso 4.4: Configurar `.config/weston.ini` para la GUI (Opcional)
```
[keyboard]
keymap_layout=latam
```

# Fase 2: Instalar Imagenes
> [!IMPORTANT] Documentacion Recomendada para encontrar imagenes
> - EVE-NG Docs
> 	- [Imagenes Soportadas](https://www.eve-ng.net/index.php/documentation/supported-images/)
> 	- [Tutoriales HowTo](https://www.eve-ng.net/index.php/documentation/howtos/)
> 	- [Nombre de Imagenes QEMU](https://www.eve-ng.net/index.php/documentation/qemu-image-namings/)
> - Las imagenes estan dando vueltas por internet, te recomiendo buscar, te doy unas pistas
> 	- [Github ishare2-org](https://github.com/ishare2-org)
> 		- [Labhub](https://labhub.eu.org/es/), o [Drive Labhub](https://drive.labhub.eu.org/0:/), o [Legacy Labhub](https://legacy.labhub.eu.org/0:/), o [Alist Labhub](https://alist.labhub.eu.org/), o [Beta Labhub (Down?)](https://beta.labhub.eu.org/)
> 	- [Github - hegdepavankumar/Cisco-Images-for-GNS3-and-EVE-NG](https://github.com/hegdepavankumar/Cisco-Images-for-GNS3-and-EVE-NG)

## Cisco IOL
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - Howto add Cisco IOL](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-cisco-iol-ios-on-linux/)
> - [BlackBox Blog (Ru) - Eve-NG Arreglar imagenes IOL](https://it-blackbox.blogspot.com/2018/06/eve-ng-cisco-iouiol.html): Arreglar la licencia
> - [Github - ishare2-org/ishare2-cli](https://github.com/ishare2-org/ishare2-cli) | [Generate new iourc license](https://github.com/ishare2-org/ishare2-cli?tab=readme-ov-file#generate-a-new-iourc-license-for-bin-images)
> - Github CiscoIOUKeygen
> 	- [Github obscur95/CiscoIOUKeygen.py](https://raw.githubusercontent.com/obscur95/gns3-server/refs/heads/master/IOU/CiscoIOUKeygen.py)
> 	- [Github Gist twrandolphchen/CiscoIOUKeygen.py](https://gist.githubusercontent.com/twrandolphchen/ed3588e1128488868c243a432b4bcfb4/raw/de0391a2da1bf1da0e5ad6aee2702c6221ce7dd7/CiscoIOUKeygen.py)

> [!DANGER] Mira la version de las imagenes
> - Evitar usar la version `L3 15.5.2T` debido a que se congela en standby

Cisco no trabaja con qemu directamente, sino que se ejecuta basado en las carpetas
- `/opt/unetlab/addons/iol/bin/`: Imagenes actualizadas con extension "`.bin`", junto al archivo "`iourc`" que actua como licencia
- `/opt/unetlab/addons/iol/lib/`: Librerias para que las imagenes dentro de bin funcionen correctamente, existe "`libcrypto.so.4`", la libreria de OpenSSL compatible

**Tabla IOL Imagen Recomendada**

| Type            | EVE Image Name                                                   | Version                                                                                                                                       | NVRAM | RAM  |
| --------------- | ---------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- | ----- | ---- |
| L2/L3 Switch    | i86bi_linux_l2-adventerprisek9-ms.<br>SSA.high_iron_20190423.bin | Cisco IOS Software, Linux Software <br>(I86BI_LINUXL2-ADVENTERPRISEK9-M),<br>Version 15.2(CML_NIGHTLY_20190423)                               | 1024  | 1024 |
| L2/L3 Switch    | i86bi_LinuxL2-AdvEnterpriseK9-M<br>_152_May_2018.bin             | Cisco IOS Software, Linux Software (I86BI_LINUXL2-<br>ADVENTERPRISEK9-M), Version 15.2(CML_NIG <br>HTLY_20180510)FLO_DSGS7                    | 1024  | 1024 |
| L3 Router       | i86bi_LinuxL3-AdvEnterpriseK9-<br>M2_157_3_May_2018.bin          | Cisco IOS Software, Linux Software (I86BI_LINUX-<br>ADVENTERPRISEK9-M), Version 15.7(3)M2,<br>Compiled Wed 28-Mar-18 11:18 by prod_rel_team   | 1024  | 1024 |
| L3 Router       | L3-ADVENTERPRISEK9<br>-M-15.4-2T.bin                             | Cisco IOS Software, Linux Software (I86BI_LINUX-<br>ADVENTERPRISEK9-M), Version 15.4(2)T4, <br>Compiled Thu 08-Oct-15 21:21 by prod_rel_team  | 1024  | 1024 |
| L3 XE Router    | x86_64_crb_linux-adventerprisek9<br>-ms.bin                      | IOL XE Router Cisco IOS Software [Dublin], Linux <br>Software (X86_64BI_LINUX-ADVENTERPRISEK9-M), <br>Version 17.12.1, RELEASE SOFTWARE (fc5) | 1024  | 1024 |
| L2/L3 XE Switch | x86_64_crb_linux_l2-adventerprisek9<br>-ms.bin                   | IOL XE Switch Cisco IOS Software [Dublin], Linux<br>Software (X86_64BI_LINUX_L2-ADVENTERPRISEK9-M), Version 17.12.1, RELEASE SOFTWARE (fc5)   | 1024  | 1024 |
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
eve-ng = 972f30267ef51616;
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

**Probar las imagenes**
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

**Parchear .bin**
> [!NOTE] Sobre los parches
> La verdad no deberia ser invalido, en caso de algun error, asegurate de seguir cada paso al pie de la letra, algo asi se ve el mensaje de error por licencia

```
IOS On Unix - Cisco Systems confidential, internal use only
IOU License Error: invalid license
License for key 7f0343 required on host "eve-ng".
Obtain a license for this key and host from the following location:
http://wwwin-enged.cisco.com/ios/iou/license/index.html
Place in your iourc file as follows (see also the web page
for further details on iourc file format and location)
```

## Aruba CX Switch
> [!IMPORTANT] Documentacion Recomendada
> - [HPE Support](https://networkingsupport.hpe.com/home): Iniciar sesion con cuenta HPE Certificada
> 	- [Aruba Community - Download Image](https://community.arubanetworks.com/discussion/arubaos-cx-10040001-is-here)
> - [EVE-NG Docs - HowTo add Aruba CX Switch](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-aruba-cx-switch/)
> - [My Ethernet Mind Blog - Adding Aruba AOS-CX to EVE-NG](https://things-on-e.blogspot.com/2019/11/adding-aruba-aos-cx-to-eve-ng.html)

> [!NOTE] Sobre Imagen
> - Carpeta HPE Aruba CX Switch: `arubacx-{version}`
> 	- Disco QEMU: `virtioa`
> - Access
> 	- user: `admin`
> 	- pass: no password

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/arubacx-{version}
```

> Mueve el disco
```
mv virtioa.qcow2 /opt/unetlab/addons/qemu/arubacx-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

## Cisco
**ASA y ASAv**
> [!IMPORTANT] Documentacion Recomendada
> - [Github - hegdepavankumar/cisco-asa-firewall-training](https://github.com/hegdepavankumar/cisco-asa-firewall-training)
> - [EVE-NG Docs - HowTo add Cisco ASAv](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-cisco-asav/)

> [!NOTE] Nombre Imagen
> - Carpeta ASA: `asa-{version}`
> 	- Disco QEMU: `hda`
> - Carpeta ASAv: `asav-{version}`
> 	- Disco QEMU: `virtioa`

> Crear las carpetas para ASA
```
mkdir /opt/unetlab/addons/qemu/asa-{version}
```

> Crear las carpetas para ASAv
```
mkdir /opt/unetlab/addons/qemu/asav-{version}
```

> Envia las imagenes a la carpeta ASA
```
rsync -Phvr hda.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/asa-{version}/
```

> Envia las imagenes a la carpeta ASA
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/asav-{version}/
```

> Arregla permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

---
**c9800cl**
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

---
**CSR1000v and CSR1000vng**
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
mkdir /opt/unetlab/addons/qemu/csr1000v-{version}
```

> Mueve la imagen para CSR1000v
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/csr1000v-{version}/
```

> Mueve la imagen para CSR1000vng
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/csr1000v-{version}/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

**Cisco vIOS (EX-VIRL)**
> [!WARNING] PELIGRO
> Estas imagenes funcionan mas lento, ni idea porque

> [!IMPORTANT] Documentacion Recomendada
> [EVE-NG Docs - HowTo add Cisco ViOS from virl](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-cisco-vios-from-virl/)

> [!NOTE] Sobre Imagen
> - Carpeta L3: `vios-{version}`
> 	- Disco QEMU: `virtioa`
> - Carpeta L2: `viosl2-{version}`
> 	- Disco QEMU: `virtioa`


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

**Cisco Viptela SD-WAN**
(WIP)

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

> [!WARNING] Peso y Requisitos
> El nodo de vtmgmt es extremadamente pesado, necesita 100GB de espacio extra y 32GB de ram para correr correctamente

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
**ExtremeVOSS**
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add Extreme VOSS](https://www.eve-ng.net/index.php/documentation/howtos/extreme-voss/)
> - [Github - extremenetworks/Virtual_VOSS](https://github.com/extremenetworks/Virtual_VOSS)

> [!NOTE] Nombre Imagen
> - Carpeta Extreme VOSS: `extremevoss-{version}`
> 	- Disco QEMU: `hda`

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

**ExtremeEXOS**
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add Extreme EXOS](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-extreme-exos/)
> - [Github - extremenetworks/Virtual_EXOS](https://github.com/extremenetworks/Virtual_EXOS)

> [!NOTE] Nombre Imagen
> - Carpeta Extreme EXOS: `extremexos-{version}`
> 	- Disco QEMU: `hda`

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
> - EVE-NG Docs - [Howto add Fortinet Images](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-fortinet-images/)
> - [Github - hegdepavankumar/Fortigate-Firewall-Complete-Guide](https://github.com/hegdepavankumar/Fortigate-Firewall-Complete-Guide) | [Web Version](https://hegdepavankumar.github.io/Fortigate-Firewall-Complete-Guide/)
> - [Fortinet Community - How to run a real-time Wireshark inside FortiGate](https://community.fortinet.com/t5/FortiGate/Technical-Tip-How-to-run-a-real-time-Wireshark-capture-on/ta-p/213805)
> - [Fortinet Community - Techical Tip: Deciphering abbreviations for Fortinet Products](https://community.fortinet.com/t5/Customer-Service/Technical-Tip-Deciphering-abbreviations-for-Fortinet-products/ta-p/196062)
> - [Reddit - Tricks and tips for new and old players](https://old.reddit.com/r/fortinet/comments/lnxv0h/fgtfazfmg_tricks_and_tips_for_new_and_old_players/)

**FAC, FGT y FNDR**
> [!TIP] Significado Nombres
> - FAC: FortiAuthenticator
> - FGT: Fortigate
> - FNDR: FortiNDR (Network Detection and Response)

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

## Hillstone SG6000
> [!WARNING] Modelos Soportados
> Segun leo, solo existe configuracion para un firewall, se documenta sobre CloudEdge

> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add Hillstone Firewall](https://www.eve-ng.net/index.php/documentation/howtos/hillstone-firewall/)

> [!NOTE] Sesion defecto
> - Login
> 	- User: `hillstone`
> 	- Pass: `hillstone`

**CloudEdge, vADC, VW**
> [!NOTE] Nombre Imagen
> - Carpeta Hillstone CloudEdge: `hillstone-sg6000-{version}`
> 	- Disco QEMU: `virtioa`

> Crea Carpeta
```
mkdir /opt/unetlab/addons/qemu/hillstone-sg6000-{version}
```

> Mover el archivo
```
rsync -Phvr virtioa.qcow2 root@{ip-server}:/opt/unetlab/addons/qemu/hillstone-sg6000-{version}/
```

> Arreglar permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

## Huawei
> [!IMPORTANT] Documentacion Recomendada
> - [Networking Hints Blog - Huawei NE40 and CE12800 on EVE-NG](https://networking-hints.blogspot.com/2021/01/huawei-ne40-12800-on-eve-ng.html)
> - [Youtube - Deploy Huawei NE40E and CE12800 on EVE-NG](https://youtu.be/8XgdSGLODD4?si=TOyf1mVb7pr5LhUj)
> - [KevinJin - Huawei in Eve-NG](https://www.kevinjin.com/posts/eve-ng/eve-ng/)

**AR1000**
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

**USG6000v**
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

**NE40E**
(WIP)
> [!IMPORTANT] Documentacion Recomendada
> - [Huawei Forums - NE40e image](https://forum.huawei.com/enterprise/intl/en/thread/ne40e-image-for-ensp-v100r003c00spc100/667245683289243648?blogId=667245683289243648)

> [!NOTE] Nombre Imagen
> - Carpeta Huawei NE40e: `???-{version}`
> 	- Disco QEMU: `???`

**CE12800**
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

**Alpine**
> [!NOTE] Nombre Imagen
> - Carpeta Alpine: `???-{version}`
> 	- Disco QEMU: `???`
> - Login
> 	- User: `root`
> 	- Pass: `eve`

> Crea la carpeta
```
mkdir /opt/unetlab/addons/qemu/linux-alpine-{version}
```

> Descomprime el archivo
```
tar xvzf linux-alpine-3.18.4.tar.gz
```

> Elimina el archivo comprimido
```
rm -f linux-alpine-3.18.4.tar.gz
```

> Enviar el archivo descargado al servidor
```
rsync -Phvr linux-alpine-{version} root@{ip-server}:/opt/unetlab/addons/qemu/linux-alpine-{version}
```


**Arch Linux**
(WIP)
> [!IMPORTANT] Documentacion Recomendada
> - 

> [!NOTE] Nombre Imagen
> - Carpeta Arch Linux: `asa-{version}`
> 	- Disco QEMU: `hda`


**Freebsd**
(WIP)
> [!IMPORTANT] Documentacion Recomendada
> - 

> [!NOTE] Nombre Imagen
> - Carpeta Freebsd: `asa-{version}`
> 	- Disco QEMU: `hda`


## MS Windows
(WIP)
**MS Windows Host (Win XP, 7, 8.1, 10, 11)**
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add MS Windows Host](https://www.eve-ng.net/index.php/documentation/howtos/howto-create-own-windows-host-on-the-eve/)

> [!NOTE] Nombre Imagen
> - Carpeta MS Windows Host: `???-{version}`
> 	- Disco QEMU: `???`


**MS Windows Server (2008-2025)**
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add MS Windows Server](https://www.eve-ng.net/index.php/documentation/howtos/howto-create-own-windows-server-on-the-eve/)

> [!NOTE] Nombre Imagen
> - Carpeta MS Windows Server: `???-{version}`
> 	- Disco QEMU: `???`


## Mikrotik RouterOS
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add Microtik Cloud Router](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-mikrotik-cloud-router/)
> - [Microtik Download](https://mikrotik.com/download): Ve a Cloud Hosted Router -> Disco "`RAW Disk Image`"

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
(WIP)
> [!IMPORTANT] Documentacion Recomendada
> - [Github Gist - rodrigojusto/Eve-NG - OpenWRT x86](https://gist.github.com/rodrigojusto/684308f6d65ac86a3c845912cee86789)
> - [OpenWRT Download 24.10.1](https://downloads.openwrt.org/releases/24.10.1/targets/x86/64/)
> - [OpenWRT Docs - Run in QEMU x86-64](https://openwrt.org/docs/guide-user/virtualization/qemu#openwrt_in_qemu_x86-64)

> [!NOTE] Nombre Imagen
> - Carpeta OpenWRT: `???-{version}`
> 	- Disco QEMU: `???`

## OPNsense
(WIP)
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add OPNsense](https://www.eve-ng.net/index.php/documentation/howtos/opnsense-firewall/)

> [!NOTE] Nombre Imagen
> - Carpeta OPNsense: `???-{version}`
> 	- Disco QEMU: `???`


## Palo Alto
(WIP)
> [!IMPORTANT] Documentacion Recomendada
> - 

> [!NOTE] Nombre Imagen
> - Carpeta Palo Alto: `???-{version}`
> 	- Disco QEMU: `???`


## PFsense
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

## vJunosEVOefi
(WIP)

Parece que esta instalacion es mas complicada, podria probar con:
- Apstra AOS Server
- Juniper J-Space
- Juniper VRR
- Juniper vSRXng

> [!IMPORTANT] Documentacion Recomendada
> - 

> [!NOTE] Nombre Imagen
> - Carpeta vJunosEVO: `???-{version}`
> 	- Disco QEMU: `???`


## VyOS
> [!IMPORTANT] Documentacion Recomendada
> - [EVE-NG Docs - HowTo add VyOS](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-vyos-vyatta/)
> - [VyOS - Official Site](https://vyos.net/)
> - [VyOS Docs - Ejemplo de Configuracion](https://docs.vyos.io/en/latest/configexamples/index.html)
> - [VyOS Docs - Guia uso Rapido](https://docs.vyos.io/en/latest/quick-start.html)
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

## Comprimir imagenes
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

# Fase 3: Configuraciones

Install Telnet VNC and Wireshark
https://youtu.be/Ea4U93991dw?si=GIRrz9xlBYHS5JdI
- Wireshark
	- Plink
	- Wireshark Wrapper
- UltraVNC
	- UltraVNC Wrapper
- Putty

Modificar los .reg, posiblemente descargar el Windows Integrtion Pack y extraer los archivos

**Actualizar templates, Configs y Icons**
https://gitlab.com/eve-ng-dev

**Importar Configuraciones**
Necesita tener las mismas imagenes de L2 y L3 que los laboratorios que quiere importar

**Exportar Configuraciones**
Se guarda en la nand del dispositivo y de alli se exporta hacia el espacio tmp de la maquina

**Acceso Red Host**
Se crea un `object` de `network` con el tipo `Management(Cloud)` esta nube permite que un dispositivo acceda a internet a travez de la conexion al router fisico como gateway
[aqui hay mas info](https://www.petenetlive.com/KB/Article/0001432)
# Extra
## Gracias
- [Github - hegdepavankumar](https://github.com/hegdepavankumar) | [Buy me a Coffee](https://buymeacoffee.com/hegdepavankumar)
- Andrea Dainese - [Linkedin](https://www.linkedin.com/in/adainese/) | [Github](https://github.com/dainok) | [Blog](https://www.adainese.it/) | [Patreon](https://www.patreon.com/adainese): Por la creacion de IOU-WEB y UnetLab
	- [Andrea Dainese Blog - UnetLab: The Story](https://www.adainese.it/blog/2019/01/01/unified-networking-lab-unetlab-the-story/)
	- [Andrea Dainese Blog - Speculating About UnetLab](https://www.adainese.it/blog/2022/04/20/speculating-about-unetlabv3/)
- GNS3
	- Jeremy Grossman (Co-founder) - [Linkedin](https://www.linkedin.com/in/jeremy-g-8b59463/)
- EVE-NG
	- Uldis Dzerkals (CEO) - [Linkedin](https://www.linkedin.com/in/uldisdzerkals/)
	- Alain Degreffe (CTO) - [Linkedin](https://www.linkedin.com/in/alaindegreffe/)
- Documentos
	- [Scribd WebIOU on Linux](https://es.scribd.com/presentation/277094101/WebIOU-on-Linux): Sera real??
- Foros
	- [EVE-NG Forums](https://www.eve-ng.net/forum/)
- Profesores
	- Dedicado a quienes me mostraron que las clases no terminan al final del bloque
	- A **Nicolás Contador** por sembrar curiosidad técnica cuando recién empezaba a entender IOU.  
	- A **Víctor Araneda**, por hacer del pizarrón una extensión de su memoria y del aula un lugar para pensar más allá.  
	- A **Juan Pablo Calderon** por acompañarme como uno mas en todas las ideas que se me ocurren
	- A todos los profes que en vez de bajarle el volumen a un estudiante curioso, lo invitaron a soñar más alto.


## Aprender
- [Lectura en profundidad CCIE](https://www.reddit.com/r/ccie/comments/6bwc2a/ccie_rsv5_ocg_further_reading_links/)
- David Bombal - EVE NG Installation: [Youtube](https://www.youtube.com/watch?v=FDbgTlr-tnw)
- Se Permite el uso de [clusters](https://www.eve-ng.net/index.php/documentation/eve-ng-cluster/) para usar replicas en diferentes servidores y permitir laboratorios mas grandes

## Porque no PNETLab?
> [!TIP] Fuente
> - [EVE-NG Forums - SCAMMERS PNETLAB](https://eve-ng.net/forum/viewtopic.php?t=16925)

PNETLab es un fork de EVE-NG Community, el cual extendio las funciones de EVE-NG Pro sin contar con una licencia oficial, lo que desencadeno polemicas por posible uso de codigo cerrado.




## Ideas Topologias
- [Github - hegdepavankumar/eve-ng-labs](https://github.com/hegdepavankumar/eve-ng-labs)


**Laboratorios de ejemplo**
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


**Routing y Switching Avanzado**
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

**Laboratorios de Seguridad**
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

**Integracion con Cloud y Laboratorios Hibridos**
- Hybrid Cloud / Multi-Cloud (AWS, Azure, GCP)
	- Simular una conexion desde un router on-premises a distintas nubes a la vez
	- Emular BGP con routers virtuales conectados a "ip cloud" en EVE-NG
- DMVPN Multi-Hub
	- Configurar un lab con hub en diferentes "nubes" y permitir que hablen entre ellos
	- Explorar la escabilidad de DMVPN fase 2 o 3

**Servicios de Acceso Remoto y Gestion**
- OpenVPN "Client-to-Lab"
	- Aislar la topologia en subredes y que se interconecten con redes VPN
- Configurar Bastion Host o Proxy Inverso
	- Montar un servidor Linux por ejemplo, un Ubuntu que sirva de punto de salto
	- Gestionar equipos en la topologia sin exponerlos todos a internet
- Entorno Zero Trust / Microsegmentacion
	- Combinar maquinas virtuales Linux/Windows con un firewall Palo Alto o Cisco ISE
	- Etiquetar trafico y aplicar politicas basadas en identidades de usuarios o dispositivos

**Automatizacion y DevOps**
- Laboratorio con Ansible / Python
	- Crear un inventario dinamico de dispositivos Cisco, Juniper, Linux, etc
	- Automatizar configuraciones con Ansible o Netmiko / NAPALM
- GitOps
	- Almacenar configuraciones en repositorios Git
	- Aplicar cambios con pipelines que ejecuten playbooks de Ansible
- NETCONF / RESTCONF / YANG
	- Usar IOS-XE, JunOS o Nokia SR OS con soporte NETCONF
	- Explorar la gestion de dispositivos via modelos YANG

**Colaboracion VoIP**
- Lab con CUCM
	- Probar llamadas entre softphones (Cisco IP Communicator)
	- Agregar gateways Cisco IOS como CUBE (SIP Trunk con PSTN simulado)
- Issabel / Asterisk / FreePBX
	- Integrar con gateways SIP en routers
	- Configurar IVR, extensiones, llamadas entrantes y salientes

**Balanceadores**
- F5 BIG-IP LTM
	- Simular clientes e instancias web en Linux Alpine
	- Configurar Virtual Server, Pools, Monitores y perfiles de SSL
- NGINX / HAProxy en Linux
	- Lab simple de balanceo de carga HTTP/HTTPS
	- Comparar rendimiento y configuracion con F5 u otros load balancers

**Over IPv6**
- IPv6 Purista
	- Configurar una red totalmente IPv6 (sin dual-stack)
	- Explorar SLAAC, DHCPv6 NAT64 y tecnicas de transicion (6to4, NAT-PT, etc)
- Segmentacion y Routing IPv6
	- Probar OSPFv3
	- Configurar BGP4 para intercambiar rutas IPv6

---
---
---



# Licencia EVE-NG
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

**Virtualizador Tipo 1 (Ignorado por ahora)**
Permite usar otras maquinas

**Instalacion Soportada de VMware ESXi (Ignorado por ahora)**
Valor: $1.000.000 Al año
- [Youtube - VirtualizationHowTo - VMware ESXi do first](https://www.youtube.com/watch?v=-1BMiYZfz38)
- [Youtube - NetworkChuck - VMware ESXi Setup and Install](https://www.youtube.com/watch?v=apC1bOLbzbY&t=822s)
- [Github - hegdepavankumar/VMware Workstation Pro 17 Licence Keys](https://github.com/hegdepavankumar/VMware-Workstation-Pro-17-Licence-Keys)
- [Github - hegdepavankumar/VMware-ESXI-Licese-Keys](https://github.com/hegdepavankumar/VMware-ESXi-License-Keys)

**Alternativa no soportada: Proxmox (Ignorado por ahora)**
Valor: Gratis
- [Guia - Adam From The Future - Running EVE NG under Proxmox](https://adamfromthefuture.wordpress.com/2018/08/30/running-eve-ng-under-proxmox/)
- [Guia - Yzguy - EVE-NG in LXC on Proxmox](https://yzguy.dev/posts/eve-ng-in-lxc-on-proxmox/)
- [Youtube - Gerard O'Brien - EVE-NG on Proxmox](https://www.youtube.com/watch?v=BmuZHjkNCt0)
