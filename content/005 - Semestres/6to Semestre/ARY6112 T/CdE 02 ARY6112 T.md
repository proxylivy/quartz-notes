# Info
## Imagen Topologia
![1000](https://slink.proxylivy.work/image/9478a1b7-a84a-44f3-ab0a-2d5fbe18439f.png)
## Contexto
Usted ha creado su empresa PYME “RROJASC LTDA”, la cual se encarga de prestar servicios de resolución de problemas a clientes de pequeño y mediano tamaño. Debido a la fama que ha alcanzado hasta el momento, ha firmado una actividad de trabajo con una empresa que tiene una red empresarial, la cual, al tener más usuarios, la cantidad de configuraciones y protocolos involucrados, requerirá de su experiencia para lograr que la red vuelva a quedar operativa en el menor tiempo posible. Esta empresa tiene dos sucursales remotas, las cuales por ahora está mediante dos proveedores de servicios diferentes, uno mediante BGP, y otro mediante solución de Service Provider que ofrece este proveedor. Debe existir la conectividad entre las sucursales remotas y hacia la casa matriz.
## Requerimientos
### A. Ticket 1: VLAN, Seguridad de puerto y EtherChannel
- Revisar [[010 - Protocolos/010.2 - Switching/VLAN|VLAN]]
- Se solicita revisar el funcionamiento de los [[010 - Protocolos/010.2 - Switching/Etherchannel|Etherchannel]] que se encuentran configurados en la topología. Si existen anomalías, realizar las correcciones necesarias.
- Las interfaces que conecten a equipos finales deben tener habilitado algún mecanismo de seguridad ([[010 - Protocolos/010.2 - Switching/Seguridad de Puerto|Seguridad de Puerto]]), que permita solo aprender la primera MAC que se registre en dicho puerto y en caso de violación, la interface se debe apagar.
- Documentar los errores detectados.
### B. Ticket 2: STP y mecanismos de estabilización de capa 2
- El cliente ha solicitado verificar que su versión de [[010 - Protocolos/010.2 - Switching/spanning-tree/802.1D (STP)|802.1D (STP)]] se encuentre correctamente configurada y operativa en todos los switches de la topología.
- Es importante para el cliente conservar la configuración de la versión de [[010 - Protocolos/010.2 - Switching/spanning-tree/802.1D (STP)|802.1D (STP)]], de ser así, replicarla en los demás equipos de capa 2 (debe ser [[010 - Protocolos/010.2 - Switching/spanning-tree/802.1S (MSTP)|802.1S (MSTP)]])
- Todas las interfaces que conectan a equipos finales deben tener mecanismos de estabilización que permitan que el puerto esté en el estado de FWD, además de no enviar ni recibir [[010 - Protocolos/010.2 - Switching/spanning-tree/BPDU|BPDU]].
- Debe configurar en las interfaces que conectan a equipos finales mecanismo que permita proteger a la interface en caso de conexión de un switch.
- Documentar los errores detectados.
### C. Ticket 3: Alta disponibilidad en capa 3
- El cliente ha solicitado verificar el funcionamiento del protocolo de alta disponibilidad implementado estándar de la industria. | [[010 - Protocolos/010.1 - Routing/VRRP|VRRP]]
- Por petición de cliente, se ha solicitado revisar que la VLAN10 prefiera el router NORTE; VLAN20 y VLAN30 utilice prefiera el router SUR.
- También se ha solicitado monitorizar las interfaces en caso de caídas, mediante IP SLA.
- Documentar los errores detectados.
### D. Ticket 4: Enrutamiento IGP
- El cliente ha solicitado que revise y que garantice el funcionamiento de sus protocolos de enrutamiento [[020 - Conceptos/020.3 - Fundamentos/IGP|IGP]].
- El cliente necesita comprobar que los temporizadores estén configurados como él solicita, es decir, tanto los tiempos para [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv2#Temporizadores|OSPFv2]] deben ser los tiempos de [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP#Modificar Temporizadores|EIGRP]] y los tiempos de EIGRP los tiempos de OSPF (OSPF hello en 5; EIGRP en 10)
- Comprobar [[010 - Protocolos/010.1 - Routing/Redistribucion|Redistribucion]] según mecanismos que tiene implementado.
- Documentar los errores encontrados.
### E. Ticket 5: Túnel GRE 6over4
- El cliente ha solicitado revisar su [[010 - Protocolos/010.1 - Routing/Tunel GRE|Tunel GRE]] 6over4, el cual debe tener conectividad IPv6 además de poder participar dentro de EIGRPv6.
- Documentar los errores encontrados.
### F. Ticket 6: Enrutamiento EGP
- El cliente ha solicitado comprobar el funcionamiento de EGP en IPv4.
- Todas las Loopback deben participar como prefijos para [[010 - Protocolos/010.1 - Routing/BGP/BGP|BGP]].
- La relación de vecindad entre router eBGP debe ser por next-hop.
- Generar la redistribución entre IGP y EGP mediante route-map, en equipo correspondiente.
- Documentar los errores encontrados.
### G. Ticket 7: Conectividad Casa Matriz y Sucursales Remotas
- Equipos deben recibir IPv4 de manera dinámica por parte del servidor [[010 - Protocolos/010.3 - Comunicaciones/DHCP|DHCP]] que se encuentra en CENTRAL, permitir que los equipos finales reciban IPv4.
- Documentar los errores encontrados
- Adjunte captura de pantalla desde PC HOST Prueba hacia el servidor al final de la documentación de los tickets

# Configuracion
## Equipo: SW1

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Esta configurado PVST, deberia ser MST

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                          |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show spa sum                       | SW1(config)#spanning-tree mode mst<br>SW1(config)#spanning-tree mst configuration<br>SW1(config-mst)# name CASO3<br>SW1(config-mst)# revision 1<br>SW1(config-mst)# instance 1 vlan 10<br>SW1(config-mst)# instance 2 vlan 20, 30<br><br> |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Interfaz Po1 no deja pasar las vlans necesarias

| Comando para encontrar el problema | Comando Aplicado                                                                                                            |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| show int trunk                     | SW1(config)#interface Port-channel1<br>SW1(config-if)#switchport trunk allowed vlan 10,20,30<br>SW1(config-if)#exit<br><br> |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Interfaz Po5 no esta configurada como troncal ni deja pasar las vlans

| Comando para encontrar el problema | Comando Aplicado                                                                                                         |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| show int trunk                     | SW1(config)#int po5<br>SW1(config-if)#switchport mode trunk<br>SW1(config-if)#switchport trunk allowed vlan 10,20,30<br> |

---
- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Interfaz e0/3 tiene el puerto en err-disable debido a una mac mal configurada

| Comando para encontrar el problema    | Comando Aplicado                                                                                                                                                                                                                                                      |
| ------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show int status<br>show port int e0/3 | SW1(config)#int e0/3<br>SW1(config-if)#no switchport port-security mac-address 0000.aaaa.bbbb<br>SW1(config-if)#switchport port-security mac-address sticky<br>SW1(config-if)#switchport port-security maximum 1<br>SW1(config-if)#shut<br>SW1(config-if)#no shut<br> |

---
## Equipo: SW2

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Interfaz Po3 y po5 no esta configurada como troncal ni deja pasar las vlans

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                          |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show int trunk                     | SW2(config)#int range po3,po5<br>SW2(config-if-range)# switchport trunk encapsulation dot1q<br>SW2(config-if-range)# switchport mode trunk<br>SW2(config-if-range)#switchport trunk allowed vlan 10,20,30 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Configurado Etherchannel modo pasivo, deberia ser activo

| Comando para encontrar el problema | Comando Aplicado                                                                                                                         |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| show etherchannel summary          | SW2(config)#int range e1/0-1<br>SW2(config-if-range)#no channel-group 5 mode passive<br>SW2(config-if-range)#channel-group 5 mode active |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Interfaz e0/0 no deja pasar las vlans necesarias

| Comando para encontrar el problema | Comando Aplicado                                                                           |
| ---------------------------------- | ------------------------------------------------------------------------------------------ |
| show int trunk                     | SW2(config)#interface Ethernet0/0<br>SW2(config-if)#switchport trunk allowed vlan 10,20,30 |

---
## Equipo: SW3

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Interfaz Port channel 3 desconfigurada

| Comando para encontrar el problema | Comando Aplicado                                                                                         |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------- |
| show etherchannel summary          | SW3(config)#no int po3<br>SW3(config)#int range e0/1-2<br>SW3(config-if-range)#channel-group 3 mode auto |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve):  Interfaces e0/1-2 Apagadas, deberian estar encendidas

| Comando para encontrar el problema | Comando Aplicado                                                                              |
| ---------------------------------- | --------------------------------------------------------------------------------------------- |
| show ip int brief                  | SW3(config)#int range e0/1-2<br>SW3(config-if-range)#no shut<br>SW3(config-if-range)#exit<br> |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Interfaz Po5 no esta configurada como troncal ni deja pasar las vlans

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                        |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sh int trunk                       | SW3(config)#int range po1,po3<br>SW3(config-if-range)#switchport trunk encapsulation dot1q<br>SW3(config-if-range)#switchport mode trunk<br>SW3(config-if-range)#switchport trunk allowed vlan 10,20,30 |

---

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Spanning tree configurado en PVST, deberia ser MSTP

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                      |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show spa sum                       | SW3(config)#spanning-tree mode mst<br>SW3(config)#spanning-tree mst configuration<br>SW3(config-mst)# name CASO3<br>SW3(config-mst)# revision 1<br>SW3(config-mst)# instance 1 vlan 10<br>SW3(config-mst)# instance 2 vlan 20, 30<br> |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Interfaz e0/0 no esta configurada como troncal ni deja pasar las vlans

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                 |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show int trunk                     | SW3(config)#int e0/0<br>SW3(config-if)#switchport trunk encapsulation dot1q<br>SW3(config-if)#switchport mode trunk<br>SW3(config-if)#switchport trunk allowed vlan 10,20,30<br> |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Interfaz e0/3 de acceso tiene mal configurada su VLAN y la seguridad de puerto

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                             |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show int status                    | SW3(config)#int e0/3<br>SW3(config-if)#no switchport access vlan 20<br>SW3(config-if)#switchport access vlan 30<br>SW3(config-if)#no switchport port-security maximum 3<br>SW3(config-if)#switchport port-security maximum 1 |

---
- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Interfaz e0/3 de acceso no tiene configuracion para recordar la mac ni limites

| Comando para encontrar el problema | Comando Aplicado                                                                                                                        |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| show port int e0/3                 | SW2(config)#int e0/3<br>SW2(config-if)#switchport port-security mac-address sticky<br>SW2(config-if)#switchport port-security maximum 1 |

---
## Equipo: R-NORTE

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Int s1/0, mal configurada la IP

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip int brief                  | NORTE(config)#int s1/0<br>NORTE(config-if)#no ip address 31.31.31.1 255.255.255.0<br>NORTE(config-if)#ip address 192.168.31.1 255.255.255.0<br> |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): VLAN 20 y 30 tienen preferencia 110, deberia ser 90 y desactivar track

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show vrrp brief                    | NORTE(config)#int e0/0.20<br>NORTE(config-subif)#vrrp 20 address-family ipv4<br>NORTE(config-if-vrrp)#no priority 110<br>NORTE(config-if-vrrp)#priority 90<br>NORTE(config-if-vrrp)#no track 1 decrement 20<br>NORTE(config-if-vrrp)#exit<br>NORTE(config-subif)#int e0/0.30<br>NORTE(config-subif)#vrrp 30 address-family ipv4<br>NORTE(config-if-vrrp)#no priority 110<br>NORTE(config-if-vrrp)#priority 90<br>NORTE(config-if-vrrp)#no track 1 decrement 20<br> |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Tunnel 0 tiene mal configurada la ip destino y le falta keepalive

| Comando para encontrar el problema       | Comando Aplicado                                                                                                                                                   |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show int tunnel0<br>show run \| s Tunnel | NORTE(config)#int tunnel0<br>NORTE(config-if)#no tunnel destination 23.23.23.2<br>NORTE(config-if)#tunnel destination 192.168.23.2<br>NORTE(config-if)#keepalive 5 |

---

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Implementar EIGRPv6 para el tunnel

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                        |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ipv6 protocols                | NORTE(config)#ipv6 router eigrp 1<br>NORTE(config-rtr)#exit<br>NORTE(config)#int tunnel 0<br>NORTE(config-if)#ipv6 eigrp 1<br>NORTE(config-if)#exit<br>NORTE(config)#int loop<br>NORTE(config)#int loopback 1<br>NORTE(config-if)#ipv6 eigrp 1<br>NORTE(config-if)#exit |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Crear Track para VRRP

| Comando para encontrar el problema | Comando Aplicado                                                                       |
| ---------------------------------- | -------------------------------------------------------------------------------------- |
| show track                         | NORTE(config)#track 1 int s1/0 line-protocol<br>NORTE(config-track)#delay down 5 up 10 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Configuracion IP-Helper no habilitada hacia Central

| Comando para encontrar el problema                             | Comando Aplicado                                                                                                                                                                                                                                                                                                  |
| -------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sh run int e0/0.10<br>sh run int e0/0.20<br>sh run int e0/0.30 | NORTE(config)#int e0/0.10<br>NORTE(config-subif)#ip helper-address 192.168.31.3<br>NORTE(config-subif)#exit<br>NORTE(config)#int e0/0.20<br>NORTE(config-subif)#ip helper-address 192.168.31.3<br>NORTE(config-subif)#exit<br>NORTE(config)#int e0/0.30<br>NORTE(config-subif)#ip helper-address 192.168.31.3<br> |

---

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Le falta modificar tiempos de saludo OSPF a EIGRP

| Comando para encontrar el problema     | Comando Aplicado                                                                                                 |
| -------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| show ip ospf int s1/0 \| include Timer | NORTE(config)#int s1/0<br>NORTE(config-if)#ip ospf hello-interval 5<br>NORTE(config-if)#ip ospf dead-interval 20 |

---
## Equipo: R-SUR

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Activar y configurar vrrp

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show vrrp brief<br>show run        | SUR(config)#fhrp version vrrp v3<br>SUR(config)#int e0/0<br>SUR(config-if)#no shut<br>SUR(config-if)#exit<br>SUR(config)#int e0/0.10<br>SUR(config-subif)#encapsulation dot1q 10<br>SUR(config-subif)#ip address 172.16.10.2 255.255.255.0<br>SUR(config-subif)#vrrp 10 address-family ipv4<br>SUR(config-if-vrrp)#priority 90<br>SUR(config-if-vrrp)#address 172.16.10.254 primary<br>SUR(config-if-vrrp)#exit<br>SUR(config-subif)#exit<br>SUR(config)#int e0/0.20<br>SUR(config-subif)#encapsulation dot1q 20<br>SUR(config-subif)#ip address 172.16.20.2 255.255.255.0<br>SUR(config-subif)#vrrp 20 address-family ipv4<br>SUR(config-if-vrrp)#priority 110<br>SUR(config-if-vrrp)#track 1 decrement 21<br>SUR(config-if-vrrp)#address 172.16.20.254 primary<br>SUR(config-if-vrrp)#exit<br>SUR(config-subif)#exit<br>SUR(config)#int e0/0.30<br>SUR(config-subif)#encapsulation dot1q 30<br>SUR(config-subif)#ip address 172.16.30.2 255.255.255.0<br>SUR(config-subif)#vrrp 30 address-family ipv4<br>SUR(config-if-vrrp)#priority 110<br>SUR(config-if-vrrp)#track 1 decrement 21<br>SUR(config-if-vrrp)#address 172.16.30.254 primary<br>SUR(config-if-vrrp)#exit |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve):  Interfaz s1/1 mal encapsulamiento, configurado PPP, deberia ser HDLC

| Comando para encontrar el problema | Comando Aplicado                                                |
| ---------------------------------- | --------------------------------------------------------------- |
| show ppp all                       | SUR(config)#int s1/1<br>SUR(config-if)#no encapsulation ppp<br> |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Tunnel 0 tiene mal configurado el prefijo ipv6, le falta el modo GRE IPV6IP y Keepalive

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show run \| s Tunnel               | SUR(config)#int tunnel 0<br>SUR(config-if)#no ipv6 address 2021:ACAD:ACAD:100::2/64<br>SUR(config-if)#ipv6 address 2021:ACAD:ACAD:100::2/112<br>SUR(config-if)#tunnel mode ipv6ip<br>SUR(config-if)#keepalive 5 |

---

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Implementar EIGRPv6 para el tunnel

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                        |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ipv6 protocols                | SUR(config)#ipv6 router eigrp 1<br>SUR(config-rtr)#exit<br>SUR(config)#int range tunnel 0, loopback 2<br>SUR(config-if-range)#ipv6 eigrp 1<br>SUR(config-if-range)#exit |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Faltan IP Helper-Address hacia Central

| Comando para encontrar el problema                                   | Comando Aplicado                                                                                                                                                                                                                                                                                                                                          |
| -------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show run int e0/0.10<br>show run int e0/0.20<br>show run int e0/0.30 | SUR(config)#int e0/0.10<br>SUR(config-subif)#ip helper-address 192.168.23.3<br>SUR(config-subif)#exit<br>SUR(config)#int e0/0.20<br>SUR(config-subif)#ip help<br>SUR(config-subif)#ip helper-address 192.168.23.3<br>SUR(config-subif)#exit<br>SUR(config)#int e0/0.30<br>SUR(config-subif)#ip helper<br>SUR(config-subif)#ip helper-address 192.168.23.3 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Le faltan modificar los tiempos de saludo OSPF a EIGRP

| Comando para encontrar el problema     | Comando Aplicado                                                                                               |
| -------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| show ip ospf int s1/1 \| include Timer | SUR(config)#int s1/1<br>SUR(config-if)#ip ospf hello-interval 5<br>SUR(config-if)#ip ospf dead-interval 20<br> |

---
## Equipo: R-CENTRAL

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Segmento VLAN30, todas las ips bloqueadas

| Comando para encontrar el problema | Comando Aplicado                                                                                                                               |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| show run \| inc excluded           | CENTRAL(config)#no ip dhcp excluded-address 172.16.30.1 172.16.30.254<br>CENTRAL(config)#ip dhcp excluded-address 172.16.30.1 172.16.30.10<br> |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): OSPF: Redes Incorrectas

| Comando para encontrar el problema             | Comando Aplicado                                                                                                                                                                                                                                                                            |
| ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols<br>show run \| s router ospf | CENTRAL(config)#router ospf 1<br>CENTRAL(config-router)#no network 23.23.23.3 0.0.0.0 area 0<br>CENTRAL(config-router)#no network 31.31.31.3 0.0.0.0 area 0<br>CENTRAL(config-router)#network 192.168.23.3 0.0.0.0 area 0<br>CENTRAL(config-router)#network 192.168.31.3 0.0.0.0 area 0<br> |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Modificar temporizadores de puertas s1/0 y s1/0 en OSPF hacia los tiempos de saludo EIGRP

| Comando para encontrar el problema                                               | Comando Aplicado                                                                                                                                                                                                                                                            |
| -------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip ospf int s1/0 \| include Timer<br>show ip ospf int s1/1 \| include Timer | CENTRAL(config)#int s1/0<br>CENTRAL(config-if)#ip ospf hello-interval 5<br>CENTRAL(config-if)#ip ospf dead-interval 20<br>CENTRAL(config-if)#exit<br>CENTRAL(config)#int s1/1<br>CENTRAL(config-if)#ip ospf hello-interval 5<br>CENTRAL(config-if)#ip ospf dead-interval 20 |

---

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Modificar Interfaz s1/2 en EIGRP hacia los tiempos de saludo (y hold) OSPF

| Comando para encontrar el problema                 | Comando Aplicado                                                                                                                                                                                                                                                             |
| -------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip eigrp interface detail \| s Hello-interval | CENTRAL(config)#router eigrp TSHOOT<br>CENTRAL(config-router)#address-family ipv4 unicast autonomous-system 1<br>CENTRAL(config-router-af)#af-interface Serial1/2<br>CENTRAL(config-router-af-interface)#no hold-time 40<br>CENTRAL(config-router-af-interface)#hold-time 30 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Arreglar Redistribucion EIGRP -> OSPF

| Comando para encontrar el problema                     | Comando Aplicado                                                                                                                                                                                                                                             |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show ip route<br>show route-map<br>show ip prefix-list | CENTRAL(config)#ip prefix-list EIGRP permit 192.168.43.0/24<br>CENTRAL(config)#route-map OSPF permit<br>CENTRAL(config-route-map)#no match ip address OSPF<br>CENTRAL(config-route-map)#match ip address prefix-list EIGRP<br>CENTRAL(config-route-map)#exit |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Arreglar Redistribucion OSPF -> EIGRP

| Comando para encontrar el problema                     | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip route<br>show route-map<br>show ip prefix-list | CENTRAL(config)#ip prefix-list OSPF permit 192.168.31.0/24<br>CENTRAL(config)#ip prefix-list OSPF permit 192.168.23.0/24<br>CENTRAL(config)#ip prefix-list OSPF permit 1.1.1.1/32<br>CENTRAL(config)#ip prefix-list OSPF permit 2.2.2.2/32<br>CENTRAL(config)#ip prefix-list OSPF permit 172.16.10.0/24<br>CENTRAL(config)#ip prefix-list OSPF permit 172.16.20.0/24<br>CENTRAL(config)#ip prefix-list OSPF permit 172.16.30.0/24<br>CENTRAL(config)#route-map EIGRP<br>CENTRAL(config-route-map)#match ip address prefix-list OSPF<br>CENTRAL(config-route-map)#exit |

---

---
## Equipo: R-CORE

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Arreglar flujo interfaces PAT

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                           |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show run                           | CORE(config)#int s1/0<br>CORE(config-if)#no ip nat inside<br>CORE(config-if)#ip nat outside<br>CORE(config-if)#exit<br>CORE(config)#int s1/2<br>CORE(config-if)#no ip nat outside<br>CORE(config-if)#ip nat inside<br><br> |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Cambiar AS EIGRP Nombrado 10 → 1, agregar Network preconfigurada

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | CORE(config)#no router eigrp TSHOOT<br>CORE(config)#router eigrp TSHOOT<br>CORE(config-router)#address-family ipv4 unicast autonomous-system 1<br>CORE(config-router-af)#  !<br>CORE(config-router-af)#  topology base<br>CORE(config-router-af-topology)#   redistribute static<br>CORE(config-router-af-topology)#  exit-af-topology<br>CORE(config-router-af)#  network 192.168.43.4 0.0.0.0<br>CORE(config-router-af)# exit-address-family<br> |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Arreglar ACL PAT

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                          |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show access-list                   | CORE(config)#ip access-list standard PAT<br>CORE(config-std-nacl)#no permit 172.16.10.0 0.0.0.3<br>CORE(config-std-nacl)#permit 172.16.10.0 0.0.0.255<br> |

---
- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Cambiar temporizador puerta EIGRP a los saludos OSPF

| Comando para encontrar el problema                 | Comando Aplicado                                                                                                                                                                                                                                           |
| -------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip eigrp interface detail \| s Hello-interval | CORE(config)#router eigrp TSHOOT<br>CORE(config-router)#address-family ipv4 unicast autonomous-system 1<br>CORE(config-router-af)#af-interface s1/2<br>CORE(config-router-af-interface)#hello-interval 10<br>CORE(config-router-af-interface)#hold-time 30 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Arreglar Redistribucion IGP -> EGP

| Comando para encontrar el problema                     | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip route<br>show route-map<br>show ip prefix-list | CORE(config)#ip prefix-list IGP permit 192.168.43.0/24<br>CORE(config)#ip prefix-list IGP permit 192.168.31.0/24<br>CORE(config)#ip prefix-list IGP permit 192.168.23.0/24<br>CORE(config)#ip prefix-list IGP permit 1.1.1.1/32<br>CORE(config)#ip prefix-list IGP permit 2.2.2.2/32<br>CORE(config)#ip prefix-list IGP permit 172.16.10.0/24<br>CORE(config)#ip prefix-list IGP permit 172.16.20.0/24<br>CORE(config)#ip prefix-list IGP permit 172.16.30.0/24<br>CORE(config)#route-map BGP permit<br>CORE(config-route-map)#match ip address prefix-list IGP<br>CORE(config-route-map)#exit<br>CORE(config)#router bgp 200<br>CORE(config-router)#redistribute eigrp 1 route-map BGP |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Arreglar Redistribucion EGP -> IGP

| Comando para encontrar el problema                     | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip route<br>show route-map<br>show ip prefix-list | CORE(config)#ip prefix-list EGP permit 210.0.0.0/30<br>CORE(config)#ip prefix-list EGP permit 220.0.0.0/30<br>CORE(config)#ip prefix-list EGP permit 8.8.8.8/32<br>CORE(config)#ip prefix-list EGP permit 4.4.4.4/32<br>CORE(config)#route-map IGP permit<br>CORE(config-route-map)#match ip address prefix-list EGP<br>CORE(config-route-map)#set metric 1544 2000 1 255 1500<br>CORE(config-route-map)#router eigrp TSHOOT<br>CORE(config-router)#address-family ipv4 unicast autonomous-system 1<br>CORE(config-router-af)#topology base<br>CORE(config-router-af-topology)#redistribute bgp 200 route-map IGP |

---
## Equipo: ISP

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): interfaz s1/1 con ip incorrecta y apagada

| Comando para encontrar el problema | Comando Aplicado                                                                                      |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------- |
| show ip int brief                  | ISP(config)#int s1/1<br>ISP(config-if)#ip address 220.0.0.1 255.255.255.252<br>ISP(config-if)#no shut |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Network Incorrecta

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                            |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip bgp                        | ISP(config)#router bgp 100<br>ISP(config-router)#no network 222.222.222.222 mask 255.255.255.255<br>ISP(config-router)#network 8.8.8.8 mask 255.255.255.255 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Vecino sin configurar

| Comando para encontrar el problema | Comando Aplicado                                                                      |
| ---------------------------------- | ------------------------------------------------------------------------------------- |
|                                    | ISP(config)#router bgp 100<br>ISP(config-router)#neighbor 220.0.0.2 remote-as 300<br> |

---
## Equipo: R-EDGE

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Encapsulacion incorrecta, configurada PPP, deberia ser HDLC

| Comando para encontrar el problema | Comando Aplicado                                              |
| ---------------------------------- | ------------------------------------------------------------- |
| sh ppp all                         | Edge(config)#int s1/1<br>Edge(config-if)#no encapsulation ppp |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Network incorrecta

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                       |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show ip bgp                        | Edge(config)#router bgp 300<br>Edge(config-router)#no network 6.6.6.6 mask 255.255.255.255<br>Edge(config-router)#network 4.4.4.4 mask 255.255.255.255 |

---
