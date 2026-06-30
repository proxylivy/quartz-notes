# Info
Okey, ahora que ya tienes tu [[800 - Extras/Write-Ups/EVE-NG/EVE-NG|EVE-NG]] funcionando, debes utilizarlo

Algunas ideas que te dejo

## Aprender
- [Reddit - r/ccie - CCIE RSv5 Further Reading](https://www.reddit.com/r/ccie/comments/6bwc2a/ccie_rsv5_ocg_further_reading_links/)

## Laboratorios

> [!TIP] Lecturas recomendadas
> - [Github - hegdepavankumar/eve-ng-labs](https://github.com/hegdepavankumar/eve-ng-labs)
> - [Eve-NG Lab](https://www.eve-ng.net/index.php/lab-library/)
> - [NetworkTut - TSHOOT 300-135 TT Eve-NG](https://www.networktut.com/practice-tshoot-tickets-with-packet-tracer)
> - [Cisco CCIE Practice](https://learningnetwork.cisco.com/s/article/ccie-enterprise-infrastructure-practice-labs)

> [!WARNING] Sobre Compatibilidad
> Los laboratorios de EVE-NG PRO no son usualmente compatibles con EVE-NG Community

**Laboratorios de ejemplo**

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