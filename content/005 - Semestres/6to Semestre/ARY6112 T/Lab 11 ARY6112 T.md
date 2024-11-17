# Info
## Imagen Topologia
![1000](https://slink.proxylivy.work/image/373d6179-22f0-4e5b-a87e-3010d54ecc44.png)
## Sobre
Resolviendo Problemas de Alta Disponibilidad en una Red
## Requerimientos
### Ticket 1: Capa 2 y Alta Disponibilidad
- Se pide revisar el funcionamiento de los [[010 - Protocolos/010.2 - Switching/Etherchannel|Etherchannel]] que se encuentran configurados en la topología. Si existe anomalía realizar la corrección.
- Se pide revisar la versión de [[010 - Protocolos/010.2 - Switching/spanning-tree/802.1D (STP)|802.1D (STP)]], la cual debe ser consistente en todos los switches. Si hay algún equipo con alguna configuración diferente, deberá realizar la corrección respectiva.
- Las interfaces que conecten a equipos finales deben tener habilitado algún mecanismo de seguridad, que permita aprender solo la primera MAC que se registre en dicho puerto. | [[010 - Protocolos/010.2 - Switching/Seguridad de Puerto|Seguridad de Puerto]]
- Las interfaces que conecten a equipos finales deben tener mecanismos de estabilización que permita que el puerto pase al estado de FWD, además de no enviar ni recibir BPDU. | [[010 - Protocolos/010.2 - Switching/spanning-tree/Mecanismos de estabilizacion|Mecanismos de estabilizacion]]
- Documentar todos los errores detectados.

### Ticket 2: Capa 3 y Alta Disponibilidad
- Revisar el funcionamiento de Port-Channel L3, el cual debe quedar operativo.
- Se ha solicitado revisar el funcionamiento de alta disponibilidad implementado. La configuración final de este protocolo debe lograr que el tráfico de la VLAN10 prefiera router NORTE, y el tráfico de VLAN20 y VLAN30 prefiera el router SUR.
- También debe lograr monitorizar los enlaces ascendentes en caso de caida.
- Se ha solicitado revisar el túnel 6 over 4, el cual debe tener conectividad IPv6 para lograr alcanzabilidad entre las loopback que operan en IPv6.
- Documentar todos los errores detectados. 

### Ticket 3: Enrutamiento IGP-EGP
- Se ha solicitado revisar y poner nuevamente en funcionamiento los protocolos IGP de la topología.
- Los temporizadores deben estar al doble de la configuración por defecto.
- Se ha solicitado revisar los mecanismos de redistribución que se tiene implementado.
- Se ha solicitado comprobar el funcionamiento de protocolo EGP, garantizando que las relaciones de vecindad sean mediante IP de siguiente salto, y que las loopback sean participen dentro del protocolo. 
- Verificar que la redistribución entre IGP y EGP sea mediante mapas de rutas.
- Documentar todos los errores detectados.

### Ticket 4: Conectividad de Equipos Finales
- Equipos finales deben recibir IPv4 de manera dinámica por parte del servidor DHCP que se encuentra en CENTRAL.
- Equipos finales no pueden solicitar más de dos IP, en caso de exceder, interfaz debe deshabilitarse.
- Documentar todos los errores detectados.
# Configuracion
## Equipo: SW1

- Enfoque Utilizado: Comparacion de configuracion
- Problema Detectado (Descripcion Breve): Port-Security tiene mal configurada la MAC del PC VLAN20

| Comando para encontrar el problema             | Comando Aplicado                                                                                                                                                                                                                                                  |
| ---------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show int status<br>show port-security int e0/3 | SW1(config)#int e0/3<br>SW1(config-if)#no switchport port-security mac-address 0000.aaaa.bbbb<br>SW1(config-if)#switchport port-security maximum 1<br>SW1(config-if)#switchport port-security mac-address sticky<br>SW1(config-if)#shut<br>SW1(config-if)#no shut |

---

- Enfoque Utilizado: Divide y Venceras
- Problema Encontrado (Descripcion Breve): No esta enviando ninguna VLAN por ningun enlace troncal de port-channel 1

| Comando para encontrar el problema       | Comando Aplicado                                                             |
| ---------------------------------------- | ---------------------------------------------------------------------------- |
| show vlan brief<br>show interfaces trunk | SW1(config)#int po1<br>SW1(config-if)#switchport trunk allowed vlan 10,20,30 |

---
- Enfoque Utilizado: Comparacion de Configuracion
- Problema Encontrado (Descripcion Breve): Port-channel 5 le falta ser troncal y admitir las vlan asignadas

| Comando para encontrar el problema                   | Comando Aplicado                                                                                                                                                                  |
| ---------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show vlan brief<br>show interfaces trunk<br>show run | SW1(config)#int po5<br>SW1(config-if)# switchport trunk encapsulation dot1q<br>SW1(config-if)# switchport mode trunk<br>SW1(config-if)#switchport trunk allowed vlan 10,20,30<br> |

---
- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): DHCP: IP snooping desactivado sin limite

| Comando para encontrar el problema | Comando Aplicado                                                                                        |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------- |
| show run                           | SW1(config)#int e0/3<br>SW1(config-if)#ip dhcp snoo<br>SW1(config-if)#ip dhcp snooping limit rate 2<br> |

---


---
## Equipo: SW2

- Enfoque Utilizado: Comparacion de configuraciones
- Problema Detectado (Descripcion Breve): Etherchannel: LACP Pasivo, deberia ser activo

| Comando para encontrar el problema                                                                             | Comando Aplicado                                                                                                                                                      |
| -------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show etherchannel summary<br>show etherchannel port-channel<br>show etherchannel 5 detail<br>show ip int brief | SW2(config)#int range e1/0-1<br>SW2(config-if-range)#no channel-group 5 mode passive<br>SW2(config-if-range)#channel-group 5 mode active<br>SW2(config-if-range)#exit |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Etherchannel: Faltan VLANS Troncales

| Comando para encontrar el problema       | Comando Aplicado                                                                      |
| ---------------------------------------- | ------------------------------------------------------------------------------------- |
| show interfaces trunk<br>show vlan brief | SW2(config-if)#int po5<br>SW2(config-if-range)#switchport trunk allowed vlan 10,20,30 |

---

- Enfoque Utilizado: Comparacion de configuraciones
- Problema Encontrado (Descripcion Breve): Spanning-tree desconfigurado con MST, deberia ser STP

| Comando para encontrar el problema | Comando Aplicado                                                                      |
| ---------------------------------- | ------------------------------------------------------------------------------------- |
| show spanning-tree summary         | SW2(config)#no spanning-tree mst configuration<br>SW2(config)#spanning-tree mode pvst |

---

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Faltan vlans troncales hacia Router Norte via e0/0

| Comando para encontrar el problema | Comando Aplicado                                                              |
| ---------------------------------- | ----------------------------------------------------------------------------- |
| show interfaces trunk<br>show run  | SW2(config)#int e0/0<br>SW2(config-if)#switchport trunk allowed vlan 10,20,30 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Interfaz de acceso e0/3 con port-security deshabilitado y sin mac-sticky

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show port-security int e0/3        | SW2(config)#int e0/3<br>SW2(config-if)#switchport port<br>SW2(config-if)#switchport port-security<br>SW2(config-if)#switchport port-security mac-address sticky |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): DHCP Snooping mal configurado, interfaces e0/0 y po5 sin confiar

| Comando para encontrar el problema | Comando Aplicado                                                                                                              |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| show run                           | SW2(config)#int e0/0<br>SW2(config-if)#ip dhcp snooping trust<br>SW2(config)#int po5<br>SW2(config-if)#ip dhcp snooping trust |

---
- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): DHCP: IP snooping desactivado sin limite y trust desconfigurado

| Comando para encontrar el problema | Comando Aplicado                                                                                                 |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| show run                           | SW2(config)#int e0/3<br>SW2(config-if)#no ip dhcp snooping trust<br>SW2(config-if)#ip dhcp snooping limit rate 2 |


## Equipo: SW3

- Enfoque Utilizado: Comprobacion de configuraciones
- Problema Detectado (Descripcion Breve): Etherchannel existe como L2, deberia ser PAgP L3

| Comando para encontrar el problema                                                               | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                        |
| ------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SW3#show etherchannel 3 detail<br>SW2#show etherchannel summary<br>SW3#show etherchannel summary | SW3(config)#no int po3<br>SW3(config)#int range e0/1-2<br>SW3(config-if-range)#no channel-group 3 mode active<br>SW3(config-if-range)#no switchport<br>SW3(config-if-range)#no ip address<br>SW3(config-if-range)#no shut<br>SW3(config-if-range)#channel-group 3 mode auto<br>SW3(config-if-range)#exit<br>SW3(config)#int po3<br>SW3(config-if)#no switchport<br>SW3(config-if)#ip address 172.16.100.2 255.255.255.0 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Interfaz Port-Channel 1 no esta configurado como como troncal y no envia las vlan correspondientes

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                            |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show int trunk<br>show run<br>     | SW3(config)#int po1<br>SW3(config-if)#switchport trunk encapsulation dot1q<br>SW3(config-if)#switchport mode trunk<br>SW3(config-if)#switchport trunk allowed vlan 10,20,30 |

---
- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Interfaz de acceso e0/3 esta configurada con la VLAN 20, deberia ser VLAN 30

| Comando para encontrar el problema | Comando Aplicado                                                                                                |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| show int status                    | SW3(config)#int e0/3<br>SW3(config-if)#no switchport access vlan 20<br>SW3(config-if)#switchport access vlan 30 |

---


- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Interfaz de acceso e0/3 con port--security deshabilitado y permite 3 macs, solo debe permitir 1

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                             |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show port-security int e0/3        | SW3(config)#int e0/3<br>SW3(config-if)#switchport port-security<br>SW3(config-if)#no switchport port-security maximum 3<br>SW3(config-if)#switchport port-security maximum 1 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Interfaz troncal e0/0 desconfigurada

| Comando para encontrar el problema    | Comando Aplicado                                                                                                                                                                 |
| ------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show int status<br>show int trunk<br> | SW3(config)#int e0/0<br>SW3(config-if)#switchport trunk encapsulation dot1q<br>SW3(config-if)#switchport mode trunk<br>SW3(config-if)#switchport trunk allowed vlan 10,20,30<br> |

---
- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): DHCP: IP snooping desactivado sin limite

| Comando para encontrar el problema | Comando Aplicado                                                     |
| ---------------------------------- | -------------------------------------------------------------------- |
| show run                           | SW3(config)#int e0/3<br>SW3(config-if)#ip dhcp snooping limit rate 2 |

---
## Equipo: R-Norte

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Tunnel GRE: Prefijo IPv6 incorrecto, configurado /64, deberia ser /112; Interfaz Source incorrecta, configurado e0/2, deberia ser e0/1; IPv4 destino incorrecta, configurado 23.23.23.2, deberia ser 192.168.23.2; Esta apagada; Le falta keepalive

| Comando para encontrar el problema                                               | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                               |
| -------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show ipv6 int brief<br>show int tunnel 0<br>show run \| section interface Tunnel | NORTE(config)#int tunnel0<br>NORTE(config-if)#no ipv6 address<br>NORTE(config-if)#ipv6 address 2021:ACAD:ACAD:100::1/112<br>NORTE(config-if)#no tunnel source e0/2<br>NORTE(config-if)#tunnel source e0/1<br>NORTE(config-if)#no tunnel destination 23.23.23.2<br>NORTE(config-if)#tunnel destination 192.168.23.2<br>NORTE(config-if)#no shut<br>NORTE(config-if)#keepalive 5 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): IP e0/1 incorrecta, configurada 31.31.31.1, deberia ser 192.168.31.1

| Comando para encontrar el problema    | Comando Aplicado                                                                                                                            |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip int e0/1<br>show ip int brief | NORTE(config)#int e0/1<br>NORTE(config-if)#no ip address 31.31.31.1 255.255.255.0<br>NORTE(config-if)#ip address 192.168.31.1 255.255.255.0 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): VRRP Prioridad VLAN 20,30 incorrecta, configurado 110, deberia ser 90

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                   |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show vrrp brief                    | NORTE(config)#int e0/0.20<br>NORTE(config-subif)#vrrp 20 address-family ipv4<br>NORTE(config-if-vrrp)#no priority 110<br>NORTE(config-if-vrrp)#priority 90<br>NORTE(config-if-vrrp)#exit<br>NORTE(config-subif)#exit<br>NORTE(config)#int e0/0.30<br>NORTE(config-subif)#vrrp 30 address-family ipv4<br>NORTE(config-if-vrrp)#no priority 110<br>NORTE(config-if-vrrp)#priority 90 |

---

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): VRRP: Falta crear Track VLAN 10 y cambiar el valor decrement, configurado en 20, deberia ser 21

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                  |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show track<br>show vrrp            | NORTE(config)#track 1 int e0/1 line-protocol<br>NORTE(config-track)#delay down 5 up 10<br>NORTE(config-track)#exit<br>NORTE(config)#int e0/0.10<br>NORTE(config-subif)#vrrp 10 address-family ipv4<br>NORTE(config-if-vrrp)#no track 1 decrement 20<br>NORTE(config-if-vrrp)#track 1 decrement 21 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): VRRP: quitar track e0/0.20 y e0/0.30

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                                         |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show track<br>show vrrp            | NORTE(config)#int e0/0.20<br>NORTE(config-subif)#vrrp 20 address-family ipv4<br>NORTE(config-if-vrrp)#no track 1 decrement 20<br>NORTE(config-if-vrrp)#exit<br>NORTE(config-subif)#exit<br>NORTE(config)#int e0/0.30<br>NORTE(config-subif)#vrrp 30 address-family ipv4<br>NORTE(config-if-vrrp)#no track 1 decrement 20 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): EIGRPv6: No existe configuracion

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                          |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ipv6 protocols                | NORTE(config)#ipv6 router eigrp 1<br>NORTE(config-rtr)#eigrp router-id 1.1.1.1<br>NORTE(config-rtr)#exit<br>NORTE(config)#int loopback 1<br>NORTE(config-if)#ipv6 eigrp 1<br>NORTE(config-if)#exit<br>NORTE(config)#int tunnel0<br>NORTE(config-if)#ipv6 eigrp 1<br>NORTE(config-if)#exit |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): DHCP: Falta IP-Helper para las inter-vlans

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                              |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show run                           | NORTE(config)#int e0/0.10<br>NORTE(config-subif)#ip helper-address 192.168.31.3<br>NORTE(config-subif)#exit<br>NORTE(config)#int e0/0.20<br>NORTE(config-subif)#ip helper-address 192.168.31.3<br>NORTE(config-subif)#exit<br>NORTE(config)#int e0/0.30<br>NORTE(config-subif)#ip helper-address 192.168.31.3 |

---


## Equipo: R-CENTRAL

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): DHCP: VLAN 30 tiene todas las ip excluidas

| Comando para encontrar el problema | Comando Aplicado                                                                                                                           |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| show run \| s dhcp                 | CENTRAL(config)#no ip dhcp excluded-address 172.16.30.1 172.16.30.254<br>CENTRAL(config)#ip dhcp excluded-address 172.16.30.1 172.16.30.10 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): OSPF: Network 23.23.23.3 no existe, deberia ser 192.168.23.3

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                           |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | CENTRAL(config)#router ospf 1<br>CENTRAL(config-router)#no network 23.23.23.3 0.0.0.0 area 0<br>CENTRAL(config-router)#network 192.168.23.3 0.0.0.0 area 0 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): OSPF: Network 31.31.31.31 no existe, deberia ser 192.168.31.3

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                           |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | CENTRAL(config)#router ospf 1<br>CENTRAL(config-router)#no network 31.31.31.3 0.0.0.0 area 0<br>CENTRAL(config-router)#network 192.168.31.3 0.0.0.0 area 0 |

---
- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): EIGRP: Timers Hold incorrecto, configurado 40, deberia ser 30

| Comando para encontrar el problema                                               | Comando Aplicado                                                                                                                                                                                                                                                            |
| -------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip eigrp interface detail \| s Hello-interval<br>show run \| s router eigrp | CENTRAL(config)#router eigrp TSHOOT<br>CENTRAL(config-router)#address-family ipv4 unicast autonomous-system 1<br>CENTRAL(config-router-af)#af-interface e0/3<br>CENTRAL(config-router-af-interface)#no hold-time 40<br>CENTRAL(config-router-af-interface)#hold-time 30<br> |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Redistribucion: Faltan rutas EIGRP -> OSPFv2

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                           |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | CENTRAL(config)#ip prefix-list EIGRP permit 192.168.43.0/24<br>CENTRAL(config)#route-map OSPF permit<br>CENTRAL(config-route-map)#no match ip address OSPF<br>CENTRAL(config-route-map)#match ip address prefix-list EIGRP |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Redistribucion: Faltan rutas OSPFv2 -> EIGRP

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | CENTRAL(config)#ip prefix-list OSPF permit 172.16.10.0/24<br>CENTRAL(config)#ip prefix-list OSPF permit 172.16.20.0/24<br>CENTRAL(config)#ip prefix-list OSPF permit 172.16.30.0/24<br>CENTRAL(config)#ip prefix-list OSPF permit 192.168.31.0/24<br>CENTRAL(config)#ip prefix-list OSPF permit 192.168.23.0/24<br>CENTRAL(config)#ip prefix-list OSPF permit 1.1.1.1/32<br>CENTRAL(config)#ip prefix-list OSPF permit 2.2.2.2/32<br>CENTRAL(config)#route-map EIGRP permit<br>CENTRAL(config-route-map)#match ip address prefix-list OSPF<br>CENTRAL(config-route-map)#exit |

---


## Equipo: R-SUR

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): OSPF: Router-ID Incorrecto, configurado 1.1.1.1, deberia ser 2.2.2.2

| Comando para encontrar el problema | Comando Aplicado                                                                                             |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| show ip protocols                  | SUR(config)#router ospf 1<br>SUR(config-router)#no router-id 1.1.1.1<br>SUR(config-router)#router-id 2.2.2.2 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Tunnel Gre: IPv6 Prefijo incorrecto

| Comando para encontrar el problema                                         | Comando Aplicado                                                                                                                                                                                                                                                                             |
| -------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ipv6 int brief<br>show int tunnel 0<br>show run \| s interface Tunnel | SUR(config)#int tunnel0<br>SUR(config-if)#no ipv6 address<br>SUR(config-if)#ipv6 address 2021:ACAD:ACAD:100::2/112<br>SUR(config-if)#no tunnel source e0/1<br>SUR(config-if)#tunnel source e0/2<br>SUR(config-if)#tunnel mode ipv6ip<br>SUR(config-if)#no shut<br>SUR(config-if)#keepalive 5 |

---
- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Interfaz e0/0 Apagada

| Comando para encontrar el problema | Comando Aplicado                               |
| ---------------------------------- | ---------------------------------------------- |
| show ip int brief                  | SUR(config)#int e0/0<br>SUR(config-if)#no shut |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): No existe Enrutamiento Inter-VLAN

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip int brief                  | SUR(config)#int e0/0.10<br>SUR(config-subif)#encapsulation dot1q 10<br>SUR(config-subif)#ip address 172.16.10.2 255.255.255.0<br>SUR(config-subif)#exit<br>SUR(config)#int e0/0.20<br>SUR(config-subif)#encapsulation dot1q 20<br>SUR(config-subif)#ip address 172.16.20.2 255.255.255.0<br>SUR(config-subif)#exit<br>SUR(config)#int e0/0.30<br>SUR(config-subif)#encapsulation dot1q 30<br>SUR(config-subif)#ip address 172.16.30.2 255.255.255.0 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): No existe configuracion VRRP :(

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show vrrp                          | SUR(config)#int e0/0.10<br>SUR(config-subif)#vrrp 10 address-family ipv4<br>SUR(config-if-vrrp)#priority 90<br>SUR(config-if-vrrp)#address 172.16.10.254 primary<br>SUR(config-if-vrrp)#exit-vrrp<br>SUR(config-subif)#exit<br>SUR(config-if)#int e0/0.20<br>SUR(config-subif)#vrrp 20 address-family ipv4<br>SUR(config-if-vrrp)#priority 110<br>SUR(config-if-vrrp)#track 1 decrement 21<br>SUR(config-if-vrrp)#address 172.16.20.254 primary<br>SUR(config-if-vrrp)#exit-vrrp<br>SUR(config-subif)#exit<br>SUR(config)#int e0/0.30<br>SUR(config-subif)#vrrp 30 address-family ipv4<br>SUR(config-if-vrrp)#priority 110<br>SUR(config-if-vrrp)#track 1 decrement 21<br>SUR(config-if-vrrp)#address 172.16.30.254 primary<br>SUR(config-if-vrrp)#exit-vrrp<br> |

---

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): VRRP: Crear Track

| Comando para encontrar el problema | Comando Aplicado                                                                   |
| ---------------------------------- | ---------------------------------------------------------------------------------- |
| show track                         | SUR(config)#track 1 int e0/2 line-protocol<br>SUR(config-track)#delay down 5 up 10 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): EIGRPv6

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                         |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show ipv6 protocols                | SUR(config)#ipv6 router eigrp 1<br>SUR(config-rtr)#eigrp router-id 2.2.2.2<br>SUR(config-rtr)#exit<br>SUR(config)#int loopback 2<br>SUR(config-if)#ipv6 eigrp 1<br>SUR(config-if)#exit<br>SUR(config)#int tunnel 0<br>SUR(config-if)#ipv6 eigrp 1<br>SUR(config-if)#exit |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): DHCP: Falta IP-Helper para las inter-vlans

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                              |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show run                           | SUR(config)#int e0/0.10<br>SUR(config-subif)#ip helper-address 192.168.23.3<br>SUR(config-subif)#exit<br>SUR(config)#int e0/0.20<br>SUR(config-subif)#ip helper-address 192.168.23.3<br>SUR(config-subif)#exit<br>SUR(config)#int e0/0.30<br>SUR(config-subif)#ip helper-address 192.168.23.3 |

---
## Equipo: R-CORE

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): EIGRP: AS Mal configurado, configurado AS 10, deberia ser AS 1

| Comando para encontrar el problema           | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                       |
| -------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show ip protocols<br>show ip eigrp neighbors | CORE(config)#no router eigrp TSHOOT<br>CORE(config)#router eigrp TSHOOT<br>CORE(config-router)#address-family ipv4 unicast autonomous-system 1<br>CORE(config-router-af)#topology base<br>CORE(config-router-af-topology)#redistribute static<br>CORE(config-router-af-topology)#exit-af-topology<br>CORE(config-router-af)#network 192.168.43.4 0.0.0.0<br>CORE(config-router-af)#exit-address-family |

---
- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): EIGRP: Configurado Timers por defecto, deberian ser: Hello -> 10, dead -> 30

| Comando para encontrar el problema                 | Comando Aplicado                                                                                                                                                                                                                                           |
| -------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip eigrp interface detail \| s Hello-interval | CORE(config)#router eigrp TSHOOT<br>CORE(config-router)#address-family ipv4 unicast autonomous-system 1<br>CORE(config-router-af)#af-interface e0/3<br>CORE(config-router-af-interface)#hello-interval 10<br>CORE(config-router-af-interface)#hold-time 30 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): BGP: AS Local incorrecto, configurado 100, deberia ser 200; Agregar vecino vieja configuracion

| Comando para encontrar el problema | Comando Aplicado                                                                                                      |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | CORE(config)#no router bgp 100<br>CORE(config)#router bgp 200<br>CORE(config-router)#neighbor 210.0.0.2 remote-as 100 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Redistribucion: Rutas EGP -> IGP

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip route                      | CORE(config)#no ip prefix-list RUTAS-IGP seq 5 deny 0.0.0.0/0 le 32<br>CORE(config)#ip prefix-list RUTAS-IGP permit 192.168.43.0/24<br>CORE(config)#ip prefix-list RUTAS-IGP permit 192.168.31.0/24<br>CORE(config)#ip prefix-list RUTAS-IGP permit 192.168.23.0/24<br>CORE(config)#ip prefix-list RUTAS-IGP permit 2.2.2.2/32<br>CORE(config)#ip prefix-list RUTAS-IGP permit 1.1.1.1/32<br>CORE(config)#ip prefix-list RUTAS-IGP permit 172.16.10.0/24<br>CORE(config)#ip prefix-list RUTAS-IGP permit 172.16.20.0/24<br>CORE(config)#ip prefix-list RUTAS-IGP permit 172.16.30.0/24<br>CORE(config)#route-map BGP permit<br>CORE(config-route-map)#no set metric 50<br>CORE(config-route-map)#set metric 50<br>CORE(config-route-map)#exit<br>CORE(config)#router bgp 200<br>CORE(config-router)#redistribute eigrp 1 route-map BGP |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Redistribucion: Rutas IGP -> EGP

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                      |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip route                      | CORE(config)#router eigrp TSHOOT<br>CORE(config-router)#address-family ipv4 unicast autonomous-system 1<br>CORE(config-router-af)#topology base<br>CORE(config-router-af-topology)#redistribute bgp 200 route-map IGP |

---
## Equipo: R-ISP

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Interfaz e0/0 y e0/1 Apagada

| Comando para encontrar el problema | Comando Aplicado                                       |
| ---------------------------------- | ------------------------------------------------------ |
| show ip int brief                  | ISP(config)#int range e0/0-1<br>ISP(config-if)#no shut |

---

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Interfaz e0/1 le falta IP

| Comando para encontrar el problema | Comando Aplicado                                                                |
| ---------------------------------- | ------------------------------------------------------------------------------- |
| show ip int brief                  | ISP(config)#int e0/1<br>ISP(config-if)#ip address 220.0.0.1 255.255.255.252<br> |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): BGP: Network incorrecta, configurada 222.222.222.222, deberia ser 8.8.8.8

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                            |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show run \| s router bgp           | ISP(config)#router bgp 100<br>ISP(config-router)#no network 222.222.222.222 mask 255.255.255.255<br>ISP(config-router)#network 8.8.8.8 mask 255.255.255.255 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): BGP: Falta Vecino BGP 300

| Comando para encontrar el problema | Comando Aplicado                                                                  |
| ---------------------------------- | --------------------------------------------------------------------------------- |
| show ip protocols                  | ISP(config)#router bgp 100<br>ISP(config-router)#neighbor 220.0.0.2 remote-as 300 |

---
## Equipo: R-EDGE

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): BGP: Vecino incorrecto AS

| Comando para encontrar el problema | Comando Aplicado                                                                                                                               |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| show run \| s router bgp           | EDGE(config)#router bgp 300<br>EDGE(config-router)#no neighbor 220.0.0.1 remote-as 300<br>EDGE(config-router)#neighbor 220.0.0.1 remote-as 100 |

---