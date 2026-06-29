# Info
**O**perative **S**ystem (Sistema Operativo) es el software base que actua como intermediario entre los usuarios y el hardware, gestionando recursos como CPU, memoria, almacenamiento y dispositivos. En el contexto de redes, tambien incluye la capacidad de comunicacion entre sistemas.

El OS no provee servicios por si solo, sino que es la plataforma sobre la cual se ejecutan las herramientas y servicios que permiten compartir recursos, gestionar usuarios y operar la red. La interoperabilidad es un concepto clave en este contexto, ya que garantiza que distintos sistemas y tecnologias puedan integrarse y funcionar en conjunto

En el contexto de redes se distinguen dos categorias. Los sistemas de proposito general con funciones de red como una distribucion de [[020 - Conceptos/020.6 - Sistemas/Linux|Linux]], [[020 - Conceptos/020.6 - Sistemas/BSD|BSD]] o [[020 - Conceptos/020.6 - Sistemas/Windows|Windows]] (Incluyendo Windows Server), y los sistemas especializados para dispositivos de red, diseñados para operar un [[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]], [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]] L2, [[020 - Conceptos/020.4 - Dispositivos de Red/Switch L3|Switch L3]] y/o [[020 - Conceptos/020.4 - Dispositivos de Red/Firewall|Firewall]], optimizados para el manejo de trafico y protocolos. Su base suele estar basado en Linux o BSD

Ejemplos OS especializados
- [VyOS](https://vyos.net/)
- [OPNsense](https://opnsense.org/)
- [IPFire](https://www.ipfire.org/)
- [OpenWRT](https://openwrt.org/)
- [DD-WRT](https://dd-wrt.com/)
- [Cisco IOS XE](https://www.cisco.com/site/us/en/products/networking/cloud-networking/ios-xe/index.html) | [Cisco IOS](https://www.cisco.com/c/en/us/products/ios-nx-os-software/ios-software-releases-listing.html)
- [Junos OS](https://www.juniper.net/us/en/products/network-operating-system/junos-os.html)
- [RouterOS](https://mikrotik.com/software)

Al encender un equipo, la CPU comienza ejecutando instrucciones almacenadas en la BIOS/UEFI. Este firmware realiza verificaciones del hardware (POST (Power-On Self Test)), detecta componentes como memoria ram, discos y dispositivos conectados y determina si el sistema puede iniciar correctamente.

Luego el firmware busca un dispositivo de arranque valido, cuando lo encuentra, ejecuta el programa llamado "bootloader", encargado de cargar el OS como tal en la memoria ram

Ejemplos de Bootloaders
- Linux: [GNU GRUB](https://www.gnu.org/software/grub/), [Limine](https://github.com/Limine-Bootloader/Limine), [Unified Kernel Image (UKI)](https://uapi-group.org/specifications/specs/unified_kernel_image/), [rEFInd](https://www.rodsbooks.com/refind/), etc.
- Mac: [IBOOT](https://theapplewiki.com/wiki/IBoot_(Bootloader))
- Win: [Bootmgr](https://en.wikipedia.org/wiki/Windows_Boot_Manager)

Una vez cargado el kernel del OS, este reconoce y monta el Sistema de Archivos (FS o File System), que define la formma en la cual los datos se organizan y almacenan dentro de un disco. Cada OS tiene el suyo

Tipos
- Linux: [EXT4](https://docs.kernel.org/admin-guide/ext4.html), [XFS](https://www.kernel.org/doc/html/latest/filesystems/xfs/index.html), [BTRFS](https://btrfs.readthedocs.io/en/latest/), [ZFS](https://github.com/openzfs/zfs), [BcacheFS](https://bcachefs.org/), etc.
- Mac: APFS
- Win: NTFS

