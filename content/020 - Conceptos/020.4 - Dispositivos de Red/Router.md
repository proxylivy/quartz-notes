# Info
Un dispositivo ubicado dentro de la capa 3 del [[020 - Conceptos/020.3 - Fundamentos/Modelo OSI#Capa 3|Modelo OSI]], Dispositivo principal de la capa anterior es un [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]] 
Cada interfaz puede conectar un tipo de red (Gigabit, FastEthernet, HWIC, Seriales, DSL o cable)

# Troubleshooting
> Enfoques: Seguimiento de Ruta - Divide y Venceras - Comparacion

1. Direccionamiento [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv4|IPv4]] - [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv6|IPv6]] y [[020 - Conceptos/020.1 - Administracion/Enrutamiento Inter-Vlan|Enrutamiento Inter-Vlan]]
- Problemas: IP / Mascara / Gateway Incorrecto, Conflicto de IPs
	- Configuracion IP y Sub-interfaces VLAN
		- `sh ip int br`: IPv4 asignado a una interfaz
		- `sh ipv6 int br`: IPv6 asignado a una interfaz
		- `sh run int [int S/S/P]`: Ver configuracion de la interfaz
	- Conflictos IP
		- `show arp`: Ver tabla ARP IPv4, mac virtual vrrp
		- `show ipv6 neighbor`
	- Ver Rutas
		- `sh ip route`: Rutas hacia el gateway VLAN
		- `sh ipv6 route`
		- `sh ip cef`: Forwarding de paquetes IPv4
		- `sh ipv6 cef`: Forwarding de Paquetes IPv6
	- Conectividad Basica
		- `ping [ip-address]`
		- `traceroute [ip-address] numeric`: Ver los saltos hasta el destino

2. [[020 - Conceptos/020.2 - Seguridad/FHRP|FHRP]] ([[010 - Protocolos/010.2 - Switching/HSRP|HSRP]] - [[010 - Protocolos/010.1 - Routing/VRRP|VRRP]])
- Problemas: Virtual-IP / Prioridades / Preempt Incorrectos
	- Valores en interfaz
		- `sh run int [int S/S/P.V]`
	- Estado HSRP
		- `sh standby`
		- `sh standby brief`
	- Estado VRRP
		- `sh vrrp`
		- `sh vrrp brief`

3. [[010 - Protocolos/010.3 - Comunicaciones/DHCP|DHCP]]
- Problemas: IP-Network / Mascara / Gateway Incorrecto, IP Excluidas, No IP-Helper
	- Pools DHCP
		- `sh ip dhcp pool`
		- `sh ip dhcp pool [pool-name]`
	- IP Asignadas
		- `sh ip dhcp binding`
	- Pool IP Excluidas
		- `sh run | inc excluded-address`
	- Configuracion IP-Helper
		- `sh run int [int S/S/P.V]`: Asegurate ver `ip helper-address` en la interfaz correcta
	- DHCP Snooping
		- `show ip dhcp snooping`
	- ACL bloqueando DHCP
		- `show access-lists`: Ver que no este bloqueando el funcionamiento a DHCP

4. Protocolos [[020 - Conceptos/020.3 - Fundamentos/IGP|IGP]]
	1. [[010 - Protocolos/010.1 - Routing/OSPF/OSPF|OSPF]] ([[010 - Protocolos/010.1 - Routing/OSPF/OSPFv2|OSPFv2]] - [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv3|OSPFv3]])
	- Problemas: R-ID / Pasivas / Network / Neighbor / Area / Tipo Area / Temporizadores (int) / Autenticacion (int) Incorrectas
		- Ver Detalles configuracion (R-ID, A-ID, Pasivas, Estado, etc.)
			- `sh ip protocol`
			- `sh run | s router ospf`
			- `show ip ospf`
		- Vecinos OSPF
			- `sh ip ospf neighbor`
			- `sh ipv6 ospf neighbor`
		- Estado Interfaces OSPF y su autenticacion
			- `sh ip ospf int brief`
			- `sh ipv6 ospf int brief`
			- `sh ip ospf int`
			- `sh ipv6 ospf int`
		- Temporizadores
			- `sh ip ospf int [int S/S/P]`
			- `sh ip ospf int [int S/S/P] | s Timer`
		- ACL Bloqueando red Broadcast (224.0.0.5)
			- `sh access-lists`
		- Verificar MTU Correcta (compara)
			- `show int [int S/S/P]`
			- `show ip ospf int [int S/S/P]`

	2. [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP|EIGRP]] ([[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP Clasico|EIGRP Clasico]] - [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP Nombrado|EIGRP Nombrado]])
	- Problemas: Valores K / Pasivas / Network / AS / Temporizadores (int) / Autenticacion (int)
		- Configuracion EIGRP
			- `sh ip protocols`
			- `sh ip protocols | inc K`
			- `sh ip eigrp interfaces`
			- `sh ip eigrp interfaces detail`
			- `sh run | s router eigrp`
		- Autenticacion
			- `sh key chain`
			- `sh ip eigrp interfaces detail [int S/S/P]`
		- ACL Bloqueando Red Broadcast (224.0.0.10)
			- `sh access-lists`: Lista las ACL
			- `sh ip int [int S/S/P]`: Ver si esta aplicada
		- Temporizadores: Pueden variar, pero debe respetar la formula "$\text{Dead Time} < 3 \times \text{Hello Time}$"
			- `sh ip eigrp interfaces detail`
			- `sh ip eigrp interfaces detail | inc Hello`

5. Tecnicas Filtrado Paquetes ([[020 - Conceptos/020.1 - Administracion/ACL|ACL]] / [[020 - Conceptos/020.1 - Administracion/Prefix-List|Prefix-List]])
- Problemas: Reglas  Trafico mal configurado (Permit / Deny) / Wildcard incorrecta
	- ACL
		- `sh access-lists`: Ver Conficiones "Permit / Deny", "Wildcard" usados correctamente, revisar conteo de paquetes filtrados
		- `sh access-lists [acl-number or acl-name]`
	- Aplicacion de ACL en interfaz
		- `sh ip int [int S/S/P]`: Indica si hay ACL "IN / OUT" aplicada en la interfaz
	- Prefix-list
		- `sh ip prefix-list`

6. Tecinas Manipulacion de rutas ([[020 - Conceptos/020.1 - Administracion/Route-Map|Route-Map]] / [[020 - Conceptos/020.1 - Administracion/PBR|PBR]])
- Problemas: Rutas / Aplicion Incorrectas
	- Route-Map
		- `sh route-map`: Lista todas las listas
		- `sh route-map [map-name]`: Detalles en especifico, verificar valores "match" y "set"
	- Aplicacion de Route-Map
		- `sh ip protocols`
		- `sh run | s router`: Revisa donde se aplico el route-map
	- PBR
		- `sh ip policy`: PBR configurados globalmente
		- `sh run int [int S/S/P]`: PBR aplicado por interfaz

7. [[010 - Protocolos/010.1 - Routing/Tunel GRE|Tunel GRE]]
- Problemas: Tunel Apagado, No hay conectividad en el tunel
	- Estado del tunel
		- `sh int tunnel [tunnel-number]`:
	- Ver rutas al destino
		- `sh ip route`
	- Ver detalles configuracion del tunel
		- `sh tun int tunnel [tunnel-number]`

8. [[010 - Protocolos/010.1 - Routing/Redistribucion|Redistribucion]]
- Problemas: Nombre / Metricas / Filtrado / Route-Map / Rutas incorrectas
	- Ver detalles de configuracion
		- `sh run | inc redistribute`
	- Ver rutas redistribuidas
		- `sh ip route`
		- `sh ip route [protocol]`

9. [[010 - Protocolos/010.1 - Routing/NAT|NAT]]
- Problemas: Filtrado / Parametros / Pool / Overload Incorrecto
   - Ver traducciones
		- `sh ip nat translations`
	- Ver estadisticas
		- `sh ip nat statistics`
	- Ver detalles configuracion
		- `sh run | s ip nat`
	- Ver interfaces Inside/Outside
		- `sh run int [int S/S/P]`

10. [[010 - Protocolos/010.1 - Routing/BGP/BGP|BGP]] ([[010 - Protocolos/010.1 - Routing/BGP/BGP-3|BGP-3]] - [[010 - Protocolos/010.1 - Routing/BGP/BGP-4|BGP-4]])
- Problemas: AS / Neighbor / Network / AF / IP NHS (iBGP) Incorrecto
- Recuerda configurar Activate para los vecinos IPv6
	- Ver estado vecinos
		- `sh ip bgp`: Ver tabla BGP Interna
		- `sh ip bgp summary`: Ver tabla BGP Interna Resumida
		- `show bgp ipv6 unicast summary`
	- Verificar detalles de vecinos
		- `sh ip bgp neighbors`
	- Ver detalles configuracion
		- `sh run | s router bgp`: Verifica si el vecino tiene AS bien configurado

11. Configurar Mejoras Solicitadas Comunes
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