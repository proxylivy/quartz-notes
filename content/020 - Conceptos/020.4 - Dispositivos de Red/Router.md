# Info
Un dispositivo ubicado dentro de la capa 3 del [[020 - Conceptos/020.3 - Fundamentos/Modelo OSI#Capa 3|Modelo OSI]], Dispositivo principal de la capa anterior es un [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]] 
Cada interfaz puede conectar un tipo de red (Gigabit, FastEthernet, HWIC, Seriales, DSL o cable)

# Visualizar
`show ip cef`: Verifica Forwarding de paquetes (Experimental)

# Troubleshooting
> Enfoques: Seguimiento de Ruta - Divide y Venceras - Comparacion
1. [[020 - Conceptos/020.1 - Administracion/Enrutamiento Inter-Vlan|Enrutamiento Inter-Vlan]]
- Problemas: Dispositivos Finales no llegan al Gateway VLAN para comunicarse
	- Revisar Interfaces VLAN
		- `show ip int brief`: Ver interfaces y sub-interfaces creadas
	- Verificar Rutas
		- `show ip route`: Rutas hacia el gateway VLAN
2. Direccionamiento [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv4|IPv4]] - [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv6|IPv6]]
- Problemas: IP incorrecta, Mascara de red incorrecta, conflicto IP
	- Configuracion IP
		- `sh ip int br`: IPv4 asignado a una interfaz
		- `sh ipv6 int br`: IPv6 asignado a una interfaz
		- `sh run int [int S/S/P]`: Ver configuracion de la interfaz
	- Comprobar conflictos IP
		- `show arp`: Ver tabla ARP IPv4, mac virtual vrrp
		- `show ipv6 neighbor`
	- Verificar Conectividad Basica
		- `ping [ip-address]`
		- `traceroute [ip-address] numeric`: Ver los saltos hasta el destino
3. [[020 - Conceptos/020.2 - Seguridad/FHRP|FHRP]] ([[010 - Protocolos/010.2 - Switching/HSRP|HSRP]] - [[010 - Protocolos/010.1 - Routing/VRRP|VRRP]])
- Problemas: Routers no se conocen, valores erroneos (Virtual-IP, Prioridades, Preempt)
	- Estado HSRP
		- `show standby`
		- `show standby br`
	- Estado VRRP
		- `show vrrp`
		- `show vrrp br`
	- Configuracion Valores VRRP por cada interfaz
		- `sh run int [int S/S/P.V]`
4. [[010 - Protocolos/010.3 - Comunicaciones/DHCP|DHCP]]
- Problemas: DHCP IP Excluidas en exceso, Informacion incorrecta (GW, Mascara), Falta IP-Helper
	- Existencia de Pools DHCP
		- `sh ip dhcp pool`
		- `sh ip dhcp pool [pool-name]`
	- Asignaciones actuales de pools DHCP
		- `sh ip dhcp binding`
	- Direcciones excluidas del pool
		- `sh run | inc excluded-address`
	- IP-Helper Configurado
		- `sh run int [int S/S/P.V]`: Asegurate ver `ip helper-address` en la interfaz correcta
	- ACL bloqueando DHCP
		- `show access-lists`: Ver que no este bloqueando el funcionamiento a DHCP
	- DHCP Snooping
		- `show ip dhcp snooping`
5. Protocolos [[020 - Conceptos/020.3 - Fundamentos/IGP|IGP]]
	1. [[010 - Protocolos/010.1 - Routing/OSPF/OSPF|OSPF]] ([[010 - Protocolos/010.1 - Routing/OSPF/OSPFv2|OSPFv2]] - [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv3|OSPFv3]])
	- Problemas: Paramtros incorrectos (R-ID, Pasivas, Network, Neighbor, Area, tipo de area, Temporizadores/int, Autenticacion/int)
		- Ver Detalles configuracion (R-ID, A-ID, Pasivas, Estado, etc.)
			- `sh ip protocol`
			- `sh run | s router ospf`
			- `show ip ospf`
		- Verificar Subredes y estado interfaz
			- `show ip int brief`
		- Verificar Vecinos OSPF
			- `sh ip ospf neighbor`
			- `sh ipv6 ospf neighbor`
		- Estado Interfaces OSPF y su autenticacion
			- `sh ip ospf int brief`
			- `sh ip ospf int`
			- `sh ipv6 ospf int`
		- Temporizadores
			- `sh ip ospf int [int S/S/P]
		- ACL Bloqueando red Broadcast (224.0.0.5)
			- `sh access-lists`
		- Verificar MTU Correcta (compara)
			- `show int [int S/S/P]`
			- `show ip ospf int [int S/S/P]`
	1. [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP|EIGRP]] ([[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP Clasico|EIGRP Clasico]] - [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP Nombrado|EIGRP Nombrado]])
	- Problemas: Parametros Incorrectos (Valores K, Pasivas, Network, AS, Temporizadores/int, Autenticacion/int)
		- Ver Detalles Configuracion (Pasivas, AS, Parametros K, Estado interfaces, redes no anunciadas, network)
			- `sh ip protocols`
			- `sh ip protocols | inc K`
			- `sh ip eigrp interfaces`
			- `sh ip eigrp interfaces detail`
			- `sh run | s router eigrp`
		- Verificar Subredes y estado interfaz
			- `show ip int brief`
		- Autenticacion
			- `show key chain`
			- `sh ip eigrp interfaces detail [int S/S/P]`
		- ACL Bloqueando Red Broadcast (224.0.0.10)
			- `sh access-lists`: Lista las ACL
			- `sh ip int [int S/S/P]`: Ver si esta aplicada
		- Temporizadores: Pueden variar, pero debe respetar la formula "$\text{Dead Time} < 3 \times \text{Hello Time}$"
			- `show ip eigrp interfaces`
			- `show ip eigrp interfaces detail | include Hello-interval`
6. Tecnicas Filtrado Paquetes
- Componentes: [[020 - Conceptos/020.1 - Administracion/ACL|ACL]] / [[020 - Conceptos/020.1 - Administracion/Prefix-List|Prefix-List]]
- Problemas: Trafico mal configurado (Permit / Deny), Wildcard incorrecta
	- ACL
		- `sh access-lists`: Ver Conficiones "Permit / Deny", "Wildcard" usados correctamente, revisar conteo de paquetes filtrados
		- `sh access-lists [acl-number or acl-name]`
	- Aplicacion de ACL en interfaz
		- `sh ip int [int S/S/P]`: Indica si hay ACL "IN / OUT" aplicada en la interfaz
	- Prefix-list
		- `sh ip prefix-list`
7. Tecinas Manipulacion de rutas
- Componenentes: [[010 - Protocolos/010.1 - Routing/Route-Map|Route-Map]] / [[999 - Archivado/PBR|PBR]]
- Problemas: Trafico mal aplicado, no funciona como se desea
	- Route-Map
		- `sh route-map`: Lista todas las listas
		- `sh route-map [map-name]`: Detalles en especifico, verificar valores "match" y "set"
	- Aplicacion de Route-Map
		- `sh ip protocols`
		- `sh run | s router`: Revisa donde se aplico el route-map
	- PBR
		- `sh ip policy`: PBR configurados globalmente
		- `sh run int [int S/S/P]`: PBR aplicado por interfaz
8. [[010 - Protocolos/010.1 - Routing/Tunel GRE|Tunel GRE]]
- Problemas: Tunel Apagado, No hay conectividad en el tunel
	- Estado del tunel
		- `sh int tunnel [tunnel-number]`:
	- Ver rutas al destino
		- `sh ip route`
	- Ver detalles configuracion del tunel
		- `sh tun int tunnel [tunnel-number]`
9. [[010 - Protocolos/010.1 - Routing/Redistribucion|Redistribucion]]
- Problemas: Nombres incorrectos, Metricas incorrectas (Metric Type o Infinita), Filtrado Incorrecto (ACL / Prefix-list), Aplicacion manipulacion de rutas incorrecta, rutas no se comparten entre protocolos
	- Ver detalles de configuracion
		- `sh run | inc redistribute`
	- Ver rutas redistribuidas
		- `sh ip route`
		- `sh ip route [protocol]`
10. [[010 - Protocolos/010.1 - Routing/NAT|NAT]]
- Problemas: Filtrado Incorrecto (ACL / Prefix-list), Parametros incorrectos ("INSIDE / OUTSIDE", Pool IP Publicas, Overload)
	- Ver traducciones
		- `sh ip nat translations`
	- Ver estadisticas
		- `sh ip nat statistics`
	- Ver detalles configuracion
		- `sh run | s ip nat`
	- Ver interfaces Inside/Outside
		- `sh ip nat interfaces`
11. [[010 - Protocolos/010.1 - Routing/BGP/BGP|BGP]] ([[010 - Protocolos/010.1 - Routing/BGP/BGP-3|BGP-3]] - [[010 - Protocolos/010.1 - Routing/BGP/BGP-4|BGP-4]])
- Problemas: Parametros Incorrectos (AS, Neighbor, Network, AF, IP Next Hop Self (iBGP))
- Recuerda configurar Activate para los vecinos IPv6
	- Ver estado vecinos
		- `sh ip bgp`: Ver tabla BGP Interna
		- `sh ip bgp summary`: Ver tabla BGP Interna Resumida
		- `show bgp ipv6 unicast summary`
	- Verificar detalles de vecinos
		- `sh ip bgp neighbors`
	- Ver detalles configuracion
		- `sh run | s router bgp`: Verifica si el vecino tiene AS bien configurado
12. Configurar Mejoras Solicitadas Comunes
- Temporizadores Protocolos "Hello / Dead" por interfaz
	- OSPF:
		- `ip ospf hello-interval [sec]`
		- `ip ospf dead-interval [sec]`
	- EIGRP:
		- `ip hello-interval eigrp [AS-Number] [sec]`
		- `ip hold-time eigrp [AS-Number] [sec]`
- [[020 - Conceptos/020.3 - Fundamentos/Sumarizacion de Redes|Sumarizacion de Redes]] por interfaz de reenvio
	- OSPF
		- ABR
			- `area [area-id] range [ip-network] [dec-mask]`
		- ABR No-advertise (Filtra LSA tipo 3)
			- `area [area-id] range [ip-network] [dec-mask] not-advertise`
		- ASBR
			- `summary-address [ip-network] [dec-mask]`
	- EIGRP
		- `ip summary-address eigrp [AS-Number] [ip-network] [dec-mask]`
- Area STUB
	- OSPF: Toda el area
		- `area [area-id] stub`
	- EIGRP: Router Final
		- `eigrp stub`



# Extra
Ver PPP
```
show ppp all
show run | section ppp
```