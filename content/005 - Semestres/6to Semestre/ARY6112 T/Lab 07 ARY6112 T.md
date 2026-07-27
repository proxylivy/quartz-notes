# Info
## Imagen Topologia
![](https://slink.proxylivy.work/image/f06b5a0c-da08-4baa-80a2-646ecd3828b4.png)
## Sobre
Resolviendo Problemas de BGP en una Red
## Requerimientos

Ticket 1: EIGRP
- Revisar funcionamiento de protocolo de enrutamiento.
- Se solicita de ser necesario, dejar interfaces pasivas según corresponda.
- Es importante que propagar rutas sumarizadas según sea necesario.
- Documente todos los inconvenientes detectados

Ticket 2: OSPF
- Revisar funcionamiento de protocolo de enrutamiento.
- Se solicita de ser necesario, dejar interfaces pasivas según corresponda.
- Área más aislada del protocolo de enrutamiento solo debe ver rutas 0.0.0.0/0
- Documente todos los inconvenientes detectados.

Ticket 3: BGP
- Revisar funcionamiento de protocolo de enrutamiento.
- Por parte de cliente ha mencionado que las relaciones de vecindad se deben realizar entre routers, y las loopback deben participar dentro del protocolo.
- Documente todos los inconvenientes detectados.

Ticket 4: Redistribuciones y Conectividad
- El cliente ha solicitado que la conectividad IGP-EGP sea mediante mapas de rutas.
- El cliente ha solicitado que loopback 66 no sea propaga a las demás áreas sin utilizar distribute -lists, prefix-lists ni route-maps.
- A nivel de EIGRP se ha solicitado que exista balanceo de carga.
- Comprobar conectividad completa.

### Equipo: R1

- Enfoque Utilizado: Abajo hacia arriba
- Problema Detectado (Descripcion Breve): Interfaces E0/0 y E0/1 estan apagadas

| Comando para encontrar el problema | Comando Aplicado                                           |
| ---------------------------------- | ---------------------------------------------------------- |
| show ip int brief                  | R1(config)#int range e0/0-1<br>R1(config-if-range)#no shut |

---

- Enfoque Utilizado: Abajo hacia Arriba
- Problema Encontrado (Descripcion Breve): EIGRP: Interfaces por defecto como pasivas

| Comando para encontrar el problema             | Comando Aplicado                                                                                                                                                                                            |
| ---------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show run \| section eigrp<br>show ip protocols | R1(config)#router eigrp TSHOOT<br>R1(config-router)#address-family ipv4 unicast autonomous-system 1<br>R1(config-router-af)#af-interface default<br>R1(config-router-af-interface)#no passive-interface<br> |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): EIGRP: Network 192.168.12.0 y 192.168.13.0 Incorrectas

| Comando para encontrar el problema             | Comando Aplicado                                                                                                                                                                                                                                                                                                              |
| ---------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols<br>show run \| section eigrp | R1(config)#router eigrp TSHOOT<br>R1(config-router)#address-family ipv4 unicast autonomous-system 1<br>R1(config-router-af)#no network 192.168.12.0 0.0.0.0<br>R1(config-router-af)#no network 192.168.13.0 0.0.0.0<br>R1(config-router-af)#network 192.168.12.1 0.0.0.0<br>R1(config-router-af)#network 192.168.13.1 0.0.0.0 |

---

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): BGP: Vecino 192.168.13.3 AS incorrecto

| Comando para encontrar el problema | Comando Aplicado                                                                                                                               |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip bgp summary                | R1(config)#router bgp 100<br>R1(config-router)#no neighbor 192.168.13.3 remote-as 200<br>R1(config-router)#neighbor 192.168.13.3 remote-as 100 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): BGP: Vecinos iBGP le falta loopback

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                      |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show run                           | R1(config)#router bgp 100<br>R1(config-router)#neighbor 192.168.12.2 update-source loopback 1<br>R1(config-router)#neighbor 192.168.13.3 update-source loopback 1<br> |

---
### Equipo: R2

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Redistribucion: Rutas EIGRP en BGP

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip route                      | R2(config)#ip prefix-list EIGRP permit 192.168.12.0/24<br>R2(config)#ip prefix-list EIGRP permit 192.168.13.0/24<br>R2(config)#ip prefix-list EIGRP permit 192.168.23.0/24<br>R2(config)#ip prefix-list EIGRP permit 11.11.11.1/32<br>R2(config)#ip prefix-list EIGRP permit 11.11.21.1/32<br>R2(config)#ip prefix-list EIGRP permit 33.33.33.3/32<br>R2(config)#ip prefix-list EIGRP permit 33.33.34.3/32<br>R2(config)#route-map BGP permit<br>R2(config-route-map)#match ip address prefix-list EIGRP<br>R2(config-route-map)#set metric 10000 100 255 1 1500<br>R2(config-route-map)#router bgp 100<br>R2(config-router)#redistribute eigrp 1 route-map BGP<br> |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Redistribucion: Ruta BGP en EIGRP

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip route                      | R2(config)#ip prefix-list BGP permit 209.50.0.0/30<br>R2(config)#route-map EIGRP permit<br>R2(config-route-map)#match ip address prefix<br>R2(config-route-map)#match ip address prefix-list BGP<br>R2(config-route-map)#set metric 10000 100 255 1 1500<br>R2(config-route-map)#router eigrp TSHOOT<br>R2(config-router)#address-family ipv4 unicast autonomous-system 1<br>R2(config-router-af)#topology base<br>R2(config-router-af-topology)#redistribute bgp 100 route-map EIGRP |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Optimizacion: Permitir balanceo de carga mediante variance

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                      |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | R2(config)#router eigrp TSHOOT<br>R2(config-router)#address-family ipv4 unicast autonomous-system 1<br>R2(config-router-af)#topology base<br>R2(config-router-af-topology)#variance 2 |

---
### Equipo: R3

- Enfoque Utilizado: Comparacion de configuraciones
- Problema Detectado (Descripcion Breve): EIGRP: Network 192.168.13.0 incorrecta

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                 |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | R3(config)#router eigrp TSHOOT<br>R3(config-router)#address-family ipv4 unicast autonomous-system 1<br>R3(config-router-af)#no network 192.168.13.0 0.0.0.0<br>R3(config-router-af)#network 192.168.13.3 0.0.0.0 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): BGP: Vecino 192.168.23.2 AS incorrecto

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                   |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip bgp summary                | R3(config)#router bgp 100<br>R3(config-router)#no neighbor 192.168.23.2 remote-as 200<br>R3(config-router)#neighbor 192.168.23.2 remote-as 100<br> |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): iBGP: Loopback no participan de BGP

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                  |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
|                                    | R3(config)#router bgp 100<br>R3(config-router)#neighbor 192.168.23.2 update-source l3<br>R3(config-router)#neighbor 192.168.13.1 update-source l3 |


---
### Equipo: ISP

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Falta BGP

| Comando para encontrar el problema | Comando Aplicado                                                                       |
| ---------------------------------- | -------------------------------------------------------------------------------------- |
| show run                           | ISP(config)#router bgp 200<br>ISP(config-router)#neighbor 209.50.0.2 remote-as 100<br> |

---
### Equipo: R4

- Problema Encontrado (Descripcion Breve): BGP: Falta vecino BGP con ISP

| Comando para encontrar el problema | Comando Aplicado                                                                 |
| ---------------------------------- | -------------------------------------------------------------------------------- |
| show ip protocols                  | R4(config)#router bgp 300<br>R4(config-router)#neighbor 219.50.0.1 remote-as 200 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Redistribucion: Faltan rutas OSPF a BGP

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip route                      | R4(config)#ip prefix-list OSPF permit 192.168.45.0/24<br>R4(config)#ip prefix-list OSPF permit 192.168.56.0/24<br>R4(config)#ip prefix-list OSPF permit 192.168.60.6/32<br>R4(config)#ip prefix-list OSPF permit 192.168.66.6/32<br>R4(config)#route-map BGP permit<br>R4(config-route-map)#match ip address prefix-list OSPF<br>R4(config-route-map)#set metric 20<br>R4(config)#router bgp 300<br>R4(config-router)#redistribute ospf 1 route-map BGP |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Redistribucion Faltan Rutas BGP a OSPF

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip route                      | R4(config)#ip prefix-list BGP permit 219.50.0.0/30<br>R4(config)#ip prefix-list BGP permit 209.50.0.0/30<br>R4(config)#route-map OSPF permit<br>R4(config-route-map)#match ip address prefix-list BGP<br>R4(config-route-map)#set metric 20<br>R4(config-route-map)#set metric-ty<br>R4(config-route-map)#set metric-type type-1<br>R4(config-route-map)#router ospf 1<br>R4(config-router)#redistribute bgp 300 subnet<br>R4(config-router)#redistribute bgp 300 subnets route-map OSPF<br> |

---
### Equipo: R5

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): OSPF: Tiempos dead interfaz e0/1 y saludos e0/2 incorrectos

| Comando para encontrar el problema                                                                    | Comando Aplicado                                                                                                                                                                                                                           |
| ----------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| R5#show ip ospf int e0/1 \| include Timer<br>R5#show ip ospf int e0/2 \| include Timer<br>R5#show run | R5(config)#int e0/1<br>R5(config-if)#no ip ospf dead-interval<br>R5(config-if)#no ip ospf hello-interval<br>R5(config-if)#exit<br>R5(config)#int e0/2<br>R5(config-if)#no ip ospf dead-interval<br>R5(config-if)#no ip ospf hello-interval |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): OSPF: Network 192.168.45.5 area incorrecta

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                 |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| show ip protocls                   | R5(config-if)#router ospf 1<br>R5(config-router)#no network 192.168.45.5 0.0.0.0 area 1<br>R5(config-router)#network 192.168.45.5 0.0.0.0 area 0 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): BGP: No esta configurado

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                       |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | R5(config)#router bgp 300<br>R5(config-router)#bgp router-id 5.5.5.5<br>R5(config-router)#neighbor 192.168.56.6 remote-as 300<br>R5(config-router)#neighbor 192.168.45.4 remote-as 300 |

---
- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Interfaz e0/1 incorrecta, deberia ser S1/0

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                           |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip int brief                  | R5(config)#int e0/1<br>R5(config-if)#no ip address 192.168.45.5 255.255.255.0<br>R5(config-if)#exit<br>R5(config)#int s1/0<br>R5(config-if)#ip address 192.168.45.5 255.255.255.0<br>R5(config-if)#no shut |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Interfaz e0/2 incorrecta, deberia ser s1/1

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                           |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip int brief                  | R5(config)#int e0/2<br>R5(config-if)#no ip address 192.168.56.5 255.255.255.0<br>R5(config-if)#exit<br>R5(config)#int s1/1<br>R5(config-if)#ip address 192.168.56.5 255.255.255.0<br>R5(config-if)#no shut |

---
### Equipo: R6

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Interfaz e0/2 incorrecta, deberia ser S1/1

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                       |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show ip int brief                  | R6(config)#int e0/2<br>R6(config-if)#no ip address 192.168.56.6 255.255.255.0<br>R6(config-if)#shut<br>R6(config-if)#exit<br>R6(config)#int s1/1<br>R6(config-if)#ip address 192.168.56.6 255.255.255.0<br>R6(config-if)#no shut<br>R6(config-if)#exit |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): BGP: Vecino usa Loopback 6

| Comando para encontrar el problema | Comando Aplicado                                                                                  |
| ---------------------------------- | ------------------------------------------------------------------------------------------------- |
| show run                           | R6(config)#router bgp 300<br>R6(config-router)#neighbor 192.168.56.5 update-source loopback 6<br> |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Optimizacion: L66 no se muestra a los demas vecinos

| Comando para encontrar el problema | Comando Aplicado                                                                                      |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------- |
| show run                           | R6(config)#router ospf 1<br>R6(config-router)#area 1 range 192.168.66.6 255.255.255.255 not-advertise |

---
