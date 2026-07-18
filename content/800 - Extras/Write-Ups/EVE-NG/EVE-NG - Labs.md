# Info
En este punto, deberias tener tu instancia de [[800 - Extras/Write-Ups/EVE-NG/EVE-NG - Install|EVE-NG - Install]] completa y funcionando

Creo que podrias separar esto en 2 grandes ideas
- Practicar Certificaciones
- Laboratorios Aprender

# Prepara Certificacion

Enfocados a certificaciones, con explicaciones de cada uno :P

## Cisco

- [Reddit - r/ccie - CCIE RSv5 Further Reading](https://www.reddit.com/r/ccie/comments/6bwc2a/ccie_rsv5_ocg_further_reading_links/)

### CCNA

El clasico, el basico de todo ingeniero, STP, y cosas asi

### CCNP

Aqui ya eres mas pro

### CCIE

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
- Redundancia de Primer Salto (HSRP/VRRP/GLBP)
	- Configurar 2 o 3 routers Cisco en una VLAN
	- Probar la tolerancia de fallos y la conmutacion entre gateway virtuales
- Multicast Routing (PIM-SM, PIM-DM)
	- Tener varios routers que soporten multicast (Cisco, VyOS, etc..)
	- Usar un servidor de streaming (por ejemplo, VLC en Linux) para probar flujos multicast
- QoS
	- Simular congestion en una red con varios nodos
	- Configurar colas, politicas de marcado (DSCP), shaping y policy

## Extreme Networks
- [Extreme Networks](https://www.extremenetworks.com/support/training)


## Fortinet
- [Fortinet Training](https://training.fortinet.com/) | [Alternativo](https://www.fortinet.com/training-certification)



# Laboratorios

> [!TIP] Lecturas recomendadas
> - [Github](https://github.com/)
> 	- [hegdepavankumar](https://github.com/hegdepavankumar)
> 		- [eve-ng-labs](https://github.com/hegdepavankumar/eve-ng-labs)
> 		- [cisco-asa-firewall-training](https://github.com/hegdepavankumar/cisco-asa-firewall-training)
> 		- [Fortigate-Firewall-Complete-Guide](https://github.com/hegdepavankumar/Fortigate-Firewall-Complete-Guide) | [Web Version](https://hegdepavankumar.github.io/Fortigate-Firewall-Complete-Guide/)
> 	- [CiscoDevNet/cml-community](https://github.com/CiscoDevNet/cml-community/tree/master/lab-topologies/ccna-prep)
> - [Eve-NG - Labs Library](https://www.eve-ng.net/index.php/lab-library/)
> - [NetworkTut - TSHOOT 300-135 TT Eve-NG](https://www.networktut.com/practice-tshoot-tickets-with-packet-tracer)
> - [Cisco Learning Network - Cisco CCIE Practice](https://learningnetwork.cisco.com/s/article/ccie-enterprise-infrastructure-practice-labs)

> [!WARNING] Sobre Compatibilidad
> Los laboratorios de EVE-NG PRO no son usualmente compatibles con EVE-NG Community

Estos laboratorios, son para aprender cosas x, sin necesariamente estudiar para una certificacion, me entretienen personalmente

## Linux

**Balanceadores**
- NGINX / HAProxy en Linux
	- Lab simple de balanceo de carga HTTP/HTTPS
	- Comparar rendimiento y configuracion con F5 u otros load balancers

**LDAP con FreeIPA**


**HA en K8s**

Un cluster de Kubernetes (K8s) en alta disponibilidad requiere minimo tres nodos para el control plane, de forma que si uno cae, el cluster sigue operando sin intervencion manual. EVE-NG simula tener esos 3 dispositivos interconectados

Asi que creas 3 VMs Linux como nodos del cluster mas un nodo controlador, todos conectados entre si dentro de la topologia.

Algo interesante es que al hacerlo en EVE-NG sobre QEMU es que puedes usar interfaces "`virtio-net`" y si tu NIC lo permite, utilizar offloading real, por lo que podrias explorar las funcionalidades de Cilium con XDP y eBPF.

Puedes agregar Keepalived + HAproxy para ver como es que funcionan

**PKI**

eJBCA-CE:
- https://hub.docker.com/r/keyfactor/ejbca-ce/
- https://www.ejbca.org/download/
- https://github.com/Keyfactor/ejbca-ce



## Multi-Vendor

### Arquitectura

- Spine-Leaf con VXLAN EVPN
	- Usando (Cisco NX-OSv, Arista vEOS o Cumulus Linux)
	- Configurar VXLAN para extender L2 sobre una red IP underlay
	- Explorar BGP EVPN como control plane


### Redes

**BGP**

Puedes eligir multiples Vendors, ejemplo
- Cisco IOS XE
- VyOS

Y conecta los 3 y haz que hablen BGP :P

### Seguridad

**Comparacion de Firewalls**

Configura OPNsense en modo HA (CARP) + IDS/IPS (Snort/Suricata) y compara con vIPS

**VPNs**

- VPN Site-to-Site IPsec
	- Conectar dos redes simuladas a travez de un tunel IPsec
	- Usar distintos firewall y routers
