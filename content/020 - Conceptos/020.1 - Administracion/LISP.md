Protocolo de Separacion de Ubicacion/ID propietario de Cisco, alternativo a BGP dentro de DFZ(Rutas de Internet)
Las traducciones son DNS(Nombre a IP) → LISP(IP a IP), las mac no se usan
EID(Network ip) -> RLOC(IP) `[Map Reply]`
Problemas que aborda
- Agregacion: Hay muchas tablas BGP
- Ingenieria de Trafico: ???
- Multihoming: ???
- Inestabilidad de Enrutamiento: Requiere menos routers potentes

## Encapsulamiento
Ejemplo [Visual](https://slink.proxylivy.work/image/f4c5ce0d-e2e4-4e07-86c4-b383055fa80b.png)
Realiza encapsulamiento IP-in-IP/UDP
- Outer Ethernet Header
- Outer LISP IP Header
	- Source RLOC IP
	- Destination RLOC IP
- Outer LISP UDP Header
	- UDP Source Port x
	- UDP Destination Port 4341
- LISP Header
	- N
	- L
	- E
	- V
	- I
	- Flags
	- Nonce/Map Version
	- Instance ID
	- LSBs
- Original IP Header
	- Source EID IP
	- Destination EID IP
- Data

## Tipos
- ETR: Se configuran con RLOC para pasar un network EID para registrarse con MS/MR
- ITR: Debe Traducir con DNS el dominio a la IP del router ITR y funciona como ETR
- PETR: Un router de borde que comunica sitios LISP -> No LISP, no usa EID, Por lo que revisa su base de datos y da una respuesta directa
- PITR: Un router de borde que comunica sitios No LISP -> LISP, usa EID, Resuelve el EID y encapsula y reenvia a RLOC la peticion al destino a travez del MS/MR para comunicarse directamente con el destino, Si no esta en EID local, no se envia
- MS: Aprende las rutas y las mantiene en una base de datos
- MR: Resuelve las Map Request


Los router ITR tienen funcionando un protocolo IGP(Como OSPF), Por lo que el Equipo final para alcanzar el ITR usa un dns para traducir la IP/Hostname, luego la peticion sobre cual es el segmento que quiere llegar, se envia a MS/MR, este envia la peticion al ETR envia el trafico a ETR y luego se comunica ETR con ITR

# Extras
## NO SE SI ESTO VA ACA REALMENTE
Vlan, tiene un rango normal y un rango extendido(Soluciones Corporativas)

## VXLAN
Red Superpuesta entre capa 2 y capa 3, usa tuneles MAC-in-IP
- UDP
- Default Puerto 4789
- Linux Puerto 8472}
Se origino a partir de una especificacion de LISP de capa 2 para soportar segmentacion, transfirio algunos campos de la estructura de LISP pero otros no
Diferencias entre Esctructuras [Visual](https://slink.proxylivy.work/image/73b23038-7edc-4f92-9034-dd51d057a383.png)
### Estructura
Ejemplo [Visual](https://slink.proxylivy.work/image/9f3b7d60-537d-4981-af97-691bbc191c00.png)
- Outer Ethernet Frame
- Outer VXLAN IP Header
	- Source VXLAN IP
	- Destination VXLAN IP
- Outer VXLAN UDP Header
	- UDP Source Port
	- UDP Destination Port 4789/8472
- VXLAN Header
	- VXLAN RRRR 1RRR
	- Reservado
	- VXLAN VNI
	- Reservado
- Original Ethernet Header
- Original IP Header
- Data

## VNI
Las ID de [[010 - Protocolos/010.2 - Switching/VLAN|VLAN]] usan 16 bits(4000 VLANs), VNI usa 24 bits (16 Millones VLANs)
Red Superpuesta que permite la segmentacion entre capa 2 y capa 3
## Estructura
Ejemplo [Visual](https://slink.proxylivy.work/image/d9fd6ec2-4f77-4145-9f27-e209ac7b9496.png)
- Outer Mac Header (14B) (4B opc)
	- Dst Mac Address (48b)
	- Src Mac Address (48b)
	- VLAN Type 0x8100 (16b)
	- VLAN ID Tag (16b)
	- Ether Type 0x0800 (16b)
- Outer Ip Header (20B)
	- IP Header Misc Data (72b)
	- Protocol 0x11 (8b)
	- Header Checksum (16b)
	- Outer Src IP (32b)
	- Outer Dst IP (32b)
- UDP Header (8B)
	- UDP Src Port (16b)
	- VXLAN Port (16b)
	- UDP Length (16b)
	- Checksum 0x0000 (16b)
- VXLAN Header (8b)
	- VXLAN RRRR 1RRR (8b)
	- Revervado 1 (24b)
	- VNID (24b)
	- Revervado 2 (8b)
- Original L2 Frame
- FCS

## VTEP
Para facilitar el descrubrimiento de VNI a travez de capa 3, usa tuneles virtuales , creando y terminando tuneles VXLAN.
Mapean Paquetes en la red Superpuesta Capa 2 y 3 a VNI
Usa 2 Interfaces
- LAN Locales: entre host y host locales
- IP: identifica el VTEP de red VXLAN, se usa para encapsular y desencapsular trafico VXLAN

## SD-Access
Acceso definido por Software, implementa VXLAN sobre plano de control LISP