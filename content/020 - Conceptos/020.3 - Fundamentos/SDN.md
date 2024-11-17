# Info
**S**oftware **D**efined **N**etwork o Redes Definidas por Software, Permiten aplicar modularidad a la red, separando el "Plano de Control" de "Plano de Datos", tambien puede unificar diferentes "Planos de Control" en uno solo y manejar varios "Planos de datos" via API, otra aproximacion a SDN, es, Permitir administrar recursos fisicos en multiples logicos.

El enfasis de desarrollo se centra en:
- Innovacion de y para administradores de redes
- Programabilidad para el plano de control
- Observabilidad y Control en el area de Redes

## Terminologia
- Control Plane (Plano de Control): Responsable de procesar protocolos de enrutamiento, gestion y otros tipos de trafico a travez de decisiones
- Data Plane o Forwarding Plane (Plano de datos): Se refiere a la informacion recibida o enviada, ademas de procesarlo a un puerto, Ejemplos:
	- [[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]]
	- [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]]
	- [[020 - Conceptos/020.4 - Dispositivos de Red/Firewall|Firewall]]

## Ejemplos
- OpenFlow: Define un estandar para la comunicacion entre dispositivos de red y el controlador SDN | [Especificaciones](https://opennetworking.org/sdn-resources/openflow-switch-specification/)
- Uso de Virtualizacion: (La virtualizacion no es SDN per se, pero se relacionan estrechamente)
	- Virtual Machines (libvirt/qemu o LXC)
		- Paravirtualizacion: Es la parte que extrae la capa fisica y la confierte en Logica
			- SR-IOV (Single Root - Input Output Virtualization)
			- PCI-Passthrough
	- Containers (Docker, Kubernetes)
	- NFV (*N*etworking *F*unctions *V*irtualization): Permite separar el hardware del software, abstraerlos y usarlos en una infraestructura compartida (Es diferente a SDN pero se relacionan estrechamente)
		- vSwitch: Emulacion de Puertos virtuales(Switch), Licenciado por VMWare.
		- [Open vSwitch](https://www.openvswitch.org/): Compatible con [DPDK](https://github.com/DPDK/dpdk): 
		- pNIC: Interfaz Virtual atada a una real
		- veth: Interfaces Ethernet Virtuales
		- Cisco vWLC, MANO, NFVIS, DNA Center
- SD-WAN
	- Google Wan B4 (2010~): Control Centralizado utilizando OpenFlow para optimizar el uso de cables en entornos de borde, definiendo clases y demanda, hace balanceo de carga automaticamente, incrementando la eficiencia 2 a 3 veces en comparacion a practicas [[999 - Archivado/WAN]] estandar. Para enrutar usa [[010 - Protocolos/010.1 - Routing/BGP/BGP|BGP]] e [[010 - Protocolos/010.1 - Routing/IS-IS|IS-IS]].
	- Juniper Apstra: Permite 1 petabyte por segundo :O | [Docs](https://www.juniper.net/documentation/product/us/en/apstra/)
- [OpenDaylight](https://www.opendaylight.org/): Plataforma SDN Open Source para controlar y gestionar redes
- [OpenNetworking](https://opennetworking.org/): Poyectos a cargo
	- [P4](https://p4.org/) (*P*rogramming *P*rotocol-indepent *P*acket *P*rocessors): Lenguaje DSL para programar procesadores de red independientes al protocolo
	- [Aether Project](https://aetherproject.org/): Open Source 5G, SD-Core, O-RAN and SD-RAN running on kubernetes edge cloud.
	- [ONOS](https://opennetworking.org/onos/): Distributed Network, deribado de [Floodlight](https://github.com/floodlight/floodlight) 0.9.0, usa [TitanDB](https://github.com/thinkaurelius/titan) o [Apache Casandra](https://cassandra.apache.org/_/index.html) como base de datos, [Apache Zookeper](https://zookeeper.apache.org/) como coordinador, ademas de poseer REST API
- [Mininet Lab](http://mininet.org/): Plataforma de simulacion de rede SDN con enfoque IaC(Infraestructure as code)

Lenguajes
- NETCONF: Protocolo para configurar una red
- RESTCONF: REST + Config files 
- [Netmiko](https://github.com/ktbyers/netmiko): Multi-vendor ssh to CLI
- REST API: Estilo de Arquitectura en red API + HTTP (xml, json)
- YANG (**Y**et **A**nother **N**ext **G**eneration): Lenguaje de modelado de datos
- YAML: 

# Extras
## Papel de Openflow
OpenFlow empezo la fiebre del SDN pero su modelo era ineficiente debido a que el vendedor tenia que implementarlo en su sistema, pero marco un primer paso. El objetivo final es crear una arquitectura de software que te permita contruir redes sin preocuparte por el hardware.

No todas las tecnologias de SDN son equitativas, pueden variar mucho de un punto a otro

## Fuentes
- Fuente Principal: The road to SDN by Nick Feamster | [O'reilly PDF](https://oreil.ly/o-pq0)
- B4 Info
	- Google B4 SDN WAN Explain by ztex, Tony, Liu | [Medium Blog](https://ztex.medium.com/b4-googles-software-defined-wan-c1bd66cd38f3)
	- B4: Experience with a Globally-Deployed Software Defined WAN by Google, Inc | [ACM doi](https://dl.acm.org/doi/10.1145/2534169.2486019)