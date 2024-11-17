# Info
## Imagen Topologia
![1000](https://slink.proxylivy.work/image/60bedc4d-abc7-414d-b830-c250b271f351.png)
## Sobre
Resolviendo Problemas Especificos de EIGRP y OSPF
## Requerimientos
**Ticket 1: EIGRP IPv4**
- Se ha solicitado resolver todos los inconvenientes presentados en el protocolo de enrutamiento.
- Todas las interfaces correspondientes deben estar pasivas.
- Deben estar todas las loopbacks sumarizadas.
- Deberá permitir que la red 192.168.22.0/24 pueda ser balanceada la carga.
- Documentar los problemas detectados y su solución respectiva.

**Ticket 2: EIGRP IPv6**
- Se ha solicitado resolver todos los inconvenientes presentados en el protocolo de enrutamiento.
- Todas las interfaces correspondiente deben estar pasivas.
- Deben estar todas las loopbacks sumarizadas.
- Deberá permitir que la red 2021:ACAD:ACAD:22::/64 puede ser balanceada la carga.
- Documentar los problemas detectados y su solución respectiva.

**Ticket 3: OSPF IPv4**
- Resolver todos los inconvenientes presentados en el protocolo de enrutamiento.
- Debe reducir a la mitad los timers.
- Se solicita que las loopback de área correspondiente sean sumarizados.
- Documentar los problemas detectados y su solución respectiva.

**Ticket 4: OSPF IPv6**
- Resolver todos los inconvenientes presentados en el protocolo de enrutamiento.
- Se solicita que las loopback de área correspondiente sean sumarizados.
- Documentar los problemas detectados y su solución respectiva. 

**Ticket 5: OSPF IPv6**
- Permitir salida hacia router ISP en IPv4/IPv6.
- Las redistribuciones en ambos protocolos de enrutamiento deben ser de manera directa y parámetros por defecto. 
- Usted al resolver todos los tickets debe tener el resultado como los siguientes outputs.
```
PC#ping 8.8.8.8
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echo's to 8.8.8.8, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 7/8/9 ms
PC# 
PC#ping 2021:ACAD:ACAD:8::8
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echo's to 2021:ACAD:ACAD:8::8, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 7/8/9 ms
PC#
```


# Configuracion
## Equipo: PC-R2

- Enfoque: Divide y Venceras
- Problema Detectado: IP route incorrecto

|                                    |                                                                                                                |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| Comando para encontrar el Problema | Comando Aplicado                                                                                               |
| show ip route<br>show run          | PC-R2(config)#no ip route 0.0.0.0 0.0.0.0 192.168.130.3<br>PC-R2(config)#ip route 0.0.0.0 0.0.0.0 192.168.22.2 |

- Enfoque: Divide y Venceras
- Problema Detectado: Interfaz e0/3 apagada

|                                    |                                                    |
| ---------------------------------- | -------------------------------------------------- |
| Comando para encontrar el Problema | Comando Aplicado                                   |
| show ip int brief                  | PC-R2(config)#int e0/3<br>PC-R2(config-if)#no shut |

- Enfoque: Divide y Venceras
- Problema Detectado: Interfaz e0/3 no configurada DHCP

|                                    |                                                            |
| ---------------------------------- | ---------------------------------------------------------- |
| Comando para encontrar el Problema | Comando Aplicado                                           |
| show ip int brief                  | PC-R2(config)#int e0/3<br>PC-R2(config-if)#ip address dhcp |

- Enfoque: Divide y Venceras
- Problema Detectado: Interfaz e0/1 mal configurada (Encendida y esperando DHCP)

|                                    |                                                                                           |
| ---------------------------------- | ----------------------------------------------------------------------------------------- |
| Comando para encontrar el Problema | Comando Aplicado                                                                          |
| show ip int brief                  | PC-R2(config)#int e0/1<br>PC-R2(config-if)#no ip address dhcp<br>PC-R2(config-if)#no shut |

---
- Enfoque Utilizado: Seguimiento de Ruta
- Problema Encontrado (Descripcion Breve): No tiene activado el enrutamiento IPv6 

| Comando para encontrar el problema | Comando Aplicado                   |
| ---------------------------------- | ---------------------------------- |
| show run                           | PC-R2(config)#ipv6 unicast-routing |

---
- Enfoque Utilizado: Seguimiento de Ruta
- Problema Encontrado (Descripcion Breve): No tiene IPv6 Configurada

| Comando para encontrar el problema | Comando Aplicado                                                                                             |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| show ipv6 int brief                | PC-R2(config)#int e0/3<br>PC-R2(config-if)#ipv6 add<br>PC-R2(config-if)#ipv6 address 2021:ACAD:ACAD:22::A/64 |

---
- Enfoque Utilizado: Seguimiento de Ruta
- Problema Encontrado (Descripcion Breve): Le falta ruta estatica IPv6

| Comando para encontrar el problema | Comando Aplicado                                   |
| ---------------------------------- | -------------------------------------------------- |
| show ipv6 route                    | PC-R2(config)#ipv6 route ::/0 2021:ACAD:ACAD:22::2 |

---


## Equipo: R2

- Enfoque: Seguimiento de Ruta
- Problema Detectado: DHCP: Necesita ip-helper

|                                                       |                                                                     |
| ----------------------------------------------------- | ------------------------------------------------------------------- |
| Comando para encontrar el Problema                    | Comando Aplicado                                                    |
| R2#show ip int brief  <br>R3#show run \| section dhcp | R2(config)#int e0/3<br>R2(config-if)#ip helper-address 192.168.23.3 |

- Enfoque:
- Problema Detectado: EIGRP: network `192.168.12.0` incorrecta

|                                    |                                                                                                                                  |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| Comando para encontrar el Problema | Comando Aplicado                                                                                                                 |
| show ip protocols                  | R2(config)#router eigrp 2<br>R2(config-router)#no network 192.168.12.0 0.0.0.0<br>R2(config-router)#network 192.168.12.2 0.0.0.0 |


- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): EIGRP IPv4: Interfaz e0/0 pasiva

| Comando para encontrar el problema | Comando Aplicado                                                         |
| ---------------------------------- | ------------------------------------------------------------------------ |
| show ip protocols                  | R2(config)#router eigrp 2<br>R2(config-router)#no passive-interface e0/0 |



- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): EIGRP IPv6: Interfaz e0/0 Pasiva 

| Comando para encontrar el problema | Comando Aplicado                                                              |
| ---------------------------------- | ----------------------------------------------------------------------------- |
| show ipv6 protocols                | R2(config)#ipv6 router eigrp 1<br>R2(config-router)#no passive-interface e0/0 |


---

- Enfoque Utilizado: Seguimiento de ruta
- Problema Encontrado (Descripcion Breve): Interfaz e0/0 IPv4 Incorrecta

| Comando para encontrar el problema   | Comando Aplicado                                                                                                                     |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| show ip int brief<br>show ip int 0/0 | R2(config)#int e0/0<br>R2(config-if)#no ip address 192.168.22.2 255.255.255.0<br>R2(config-if)#ip address 192.168.12.2 255.255.255.0 |

---

- Enfoque Utilizado: Seguimiento de ruta
- Problema Detectado (Descripcion Breve): Interfaz e0/0 IPv6 Incorrecta 

| Comando para encontrar el problema        | Comando Aplicado                                                                                                                   |
| ----------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| show ipv6 int brief<br>show ipv6 int e0/0 | R2(config)#int e0/0<br>R2(config-if)#no ipv6 address 2021:ACAD:ACAD:22::2/64<br>R2(config-if)#ipv6 address 2021:ACAD:ACAD:12::2/64 |

---

- Enfoque Utilizado: Comparacion
- Problema Encontrado (Descripcion Breve): Interfaz e0/3 IPv6 no configurado

| Comando para encontrar el problema        | Comando Aplicado                                                          |
| ----------------------------------------- | ------------------------------------------------------------------------- |
| show ipv6 int brief<br>show ipv6 int e0/0 | R2(config)#int e0/3<br>R2(config-if)#ipv6 address 2021:ACAD:ACAD:22::2/64 |

---

- Enfoque Utilizado: Comparacion
- Problema Encontrado (Descripcion Breve): EIGRP: Interfaz e0/2 numero AS Incorrecto

| Comando para encontrar el problema | Comando Aplicado                                                                   |
| ---------------------------------- | ---------------------------------------------------------------------------------- |
| show ipv6 protocols<br>show run    | R2(config)#int e0/2<br>R2(config-if)#no ipv6 eigrp 2<br>R2(config-if)#ipv6 eigrp 1 |

---



- Enfoque Utilizado: Comparacion
- Problema Encontrado (Descripcion Breve): EIGRP: Permitir Balanceo de carga IPv4 

| Comando para encontrar el problema | Comando Aplicado                                                                               |
| ---------------------------------- | ---------------------------------------------------------------------------------------------- |
| show ip route eigrp<br>show run    | R2(config)#router eigrp 2<br>R2(config-router)#maximum-paths 4<br>R2(config-router)#variance 2 |


---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): EIGRP: Permitir Balanceo de carga IPv6

| Comando para encontrar el problema    | Comando Aplicado                                            |
| ------------------------------------- | ----------------------------------------------------------- |
| show ipv6 route eigrp<br>show run<br> | R2(config)#ipv6 router eigrp 1<br>R2(config-rtr)#variance 2 |

---

- Enfoque Utilizado: Seguimiento de ruta
- Problema Encontrado (Descripcion Breve): EIGRP Interfaz E0/3 hacia PC no esta pasiva IPv4 e IPv6

| Comando para encontrar el problema                                                                 | Comando Aplicado                                                                                                                                                               |
| -------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show ip protocols<br>show ipv6 protocols<br>show ip eigrp interfaces<br>show ipv6 eigrp interfaces | R2(config)#router eigrp 2<br>R2(config-router)#passive-interface e0/3<br>R2(config-router)#exit<br>R2(config)#ipv6 router eigrp 1<br>R2(config-rtr)#passive-interface e0/3<br> |

---
## Equipo: R3

- Enfoque: Seguimiento de Ruta
- Problema Detectado: DHCP Mal configurada el default router

|                                    |                                                                                                                            |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| Comando para encontrar el Problema | Comando Aplicado                                                                                                           |
| R3#show run \| section dhcp        | R3(config)#ip dhcp pool PC <br>R3(dhcp-config)#no default-router 22.22.22.2<br>R3(dhcp-config)#default-router 192.168.22.2 |

---
- Enfoque Utilizado: Divide y Venceras
- Problema Detectado (Descripcion Breve): EIGRP IPv6: Router-ID Incorrecto

| Comando para encontrar el problema | Comando Aplicado                                                                                                          |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| show ipv6 protocols                | R3(config)#ipv6 router eigrp 1<br>R3(config-rtr)#no eigrp router-id 2.2.2.2<br>R3(config-rtr)#eigrp router-id 3.3.3.3<br> |

---

- Enfoque Utilizado: Divide y Venceras
- Problema Encontrado (Descripcion Breve): Redistribucion IPv4: Faltan Metricas en route-map para redistribuir rutas OSPF -> EIGRP

| Comando para encontrar el problema                 | Comando Aplicado                                                                                       |
| -------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| show ip access-lists<br>show route-map<br>show run | R3(config)#route-map REDISTRIBUCION2 permit 10<br>R3(config-route-map)#set metric 10000 100 255 1 1500 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Redistribucion IPv4: Metricas de sobra en Route-map para redistribuir EIGRP -> OSPF

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                   |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show route-map<br>show run         | R3(config)#no route-map REDISTRIBUCION1 permit 10<br>R3(config-router)#no redistribute eigrp 2 subnets route-map REDISTRIBUCION1<br>R3(config-router)#redistribute eigrp 2 subnets |

---

- Enfoque Utilizado: Seguimiento de Ruta
- Problema Encontrado (Descripcion Breve): No hay Redistribucion IPv6: Rutas OSPF -> EIGRP

| Comando para encontrar el problema | Comando Aplicado                                                                                                   |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| show ipv6 route<br>show run        | R3(config)#ipv6 router eigrp 1<br>R3(config-rtr)#redistribute ospf 1 metric 10000 100 255 1 1500 include-connected |

---
- Enfoque Utilizado: Seguimiento de Ruta
- Problema Encontrado (Descripcion Breve): No hay Redistribucion IPv6: Rutas EIGRP -> OSPF

| Comando para encontrar el problema | Comando Aplicado                                                                       |
| ---------------------------------- | -------------------------------------------------------------------------------------- |
| show ipv6 route<br>show run        | R3(config)#ipv6 router ospf 1<br>R3(config-rtr)#redistribute eigrp 1 include-connected |

---
- Enfoque Utilizado: Seguimiento de Ruta
- Problema Encontrado (Descripcion Breve): Falta Redistribuir rutas estaticas en EIGRP

| Comando para encontrar el problema | Comando Aplicado                                                     |
| ---------------------------------- | -------------------------------------------------------------------- |
| show ipv6 route                    | R3(config)#ipv6 router eigrp 1<br>R3(config-rtr)#redistribute static |

---

## Equipo: R1

- Enfoque Utilizado: Divide y Venceras
- Problema Detectado (Descripcion Breve): IPv6 Loopback 1 Incorrecta

| Comando para encontrar el problema      | Comando Aplicado                                                                                                                           |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| show ipv6 int brief<br>show ipv6 int l1 | R1(config)#int l1<br>R1(config-if)#no ipv6 address 2021:ACAD:ACAD:1:11::1/128<br>R1(config-if)#ipv6 address 2021:ACAD:ACAD:11:1::1/128<br> |

---

- Enfoque Utilizado: Divide y Venceras
- Problema Encontrado (Descripcion Breve): IPv6 Loopback 11 Incorrecta

| Comando para encontrar el problema       | Comando Aplicado                                                                                                                            |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| show ipv6 int brief<br>show ipv6 int l11 | R1(config)#int l11<br>R1(config-if)#no ipv6 address 2021:ACAD:ACAD:1:21::1/128<br>R1(config-if)#ipv6 address 2021:ACAD:ACAD:21:1::1/128<br> |

### Sumarizacion Loopback
#### Tabla Sumarizada IPv4

| IP-Network      | 128 | 64  | 32  | Corte | 16  | 8   | 4   | 2   | 1   |
| --------------- | --- | --- | --- | ----- | --- | --- | --- | --- | --- |
| 192.168.11.1/32 |     |     |     | /     |     | x   |     | x   | x   |
| 192.168.21.1/32 |     |     |     | /     | x   |     | x   |     | x   |

IP Sumarizada: `192.168.0.0/19`

---
#### Tabla Sumarizada IPv6
Sextetos (16 bits)
16 : 32 : 48 : 64 : 80
0-9

| A   | 10  |
| --- | --- |
| B   | 11  |
| C   | 12  |
| D   | 13  |
| E   | 14  |
| F   | 15  |

| Network IPv6               | Diferencia | 8   | 4   | 2   | 1   | \|  | 8   | 4   | 2   | 1   | \|  | 8   | 4   | Corte | 2   | 1   | \|  | 8   | 4   | 2   | 1   | \|  |
| -------------------------- | ---------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | ----- | --- | --- | --- | --- | --- | --- | --- | --- |
| 2021:ACAD:ACAD:11:1::1/128 | 0011       |     |     |     |     | \|  |     |     |     |     | \|  |     |     | /     |     | x   | \|  |     |     |     | x   | \|  |
| 2021:ACAD:ACAD:21:1::1/128 | 0021       |     |     |     |     | \|  |     |     |     |     | \|  |     |     | /     | x   |     | \|  |     |     |     | x   | \|  |


La red Sumarizada es: `2021:ACAD:ACAD::/58`

---

- Enfoque Utilizado: Comparacion
- Problema Encontrado (Descripcion Breve): EIGRP: Rutas Loopback IPv4 Sumarizadas

| Comando para encontrar el problema | Comando Aplicado                                                                                            |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| show ip route<br>show ip protocols | R1(config)#int range e0/0-1<br>R1(config-if-range)#ip summary-address eigrp 2 192.168.0.0 255.255.224.0<br> |

---
- Enfoque Utilizado: Comparacion
- Problema Encontrado (Descripcion Breve): EIGRP: Rutas Loopback IPv6 Sumarizadas

| Comando para encontrar el problema     | Comando Aplicado                                                                                    |
| -------------------------------------- | --------------------------------------------------------------------------------------------------- |
| show ipv6 route<br>show ipv6 protocols | R1(config)#int range e0/0-1<br>R1(config-if-range)#ipv6 summary-address eigrp 1 2021:ACAD:ACAD::/58 |

---


- Enfoque Utilizado: Seguimiento de Ruta
- Problema Detectado (Descripcion Breve): EIGRP IPv4: Interfaz e0/0 y e0/1 pasivas

| Comando para encontrar el problema | Comando Aplicado                                                                                                        |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | R1(config)#router eigrp 2<br>R1(config-router)#no passive-interface e0/0<br>R1(config-router)#no passive-interface e0/1 |

---

- Enfoque Utilizado: Comparacion
- Problema Encontrado (Descripcion Breve): EIGRP IPv6: Interfaz e0/1 Pasiva

| Comando para encontrar el problema | Comando Aplicado                                                           |
| ---------------------------------- | -------------------------------------------------------------------------- |
| show ipv6 protocols                | R1(config)#ipv6 router eigrp 1<br>R1(config-rtr)#no passive-interface e0/1 |

---
## Equipo: R4

- Enfoque Utilizado: Divide y Venceras
- Problema Detectado (Descripcion Breve): Ruta Estica incorrecta

| Comando para encontrar el problema | Comando Aplicado                                                                                    |
| ---------------------------------- | --------------------------------------------------------------------------------------------------- |
| show ip route                      | R4(config)#no ip route 0.0.0.0 0.0.0.0 209.50.0.3<br>R4(config)#ip route 0.0.0.0 0.0.0.0 209.50.0.2 |

---

- Enfoque Utilizado: Divide y Venceras
- Problema Encontrado (Descripcion Breve): OSPFv2 no redistribute la ruta estatica

| Comando para encontrar el problema | Comando Aplicado                                                                |
| ---------------------------------- | ------------------------------------------------------------------------------- |
| show run                           | R4(config)#router ospf 2<br>R4(config-router)#default-information originate<br> |

---
- Enfoque Utilizado: Seguimiento de Ruta
- Problema Detectado (Descripcion Breve): OSPFv2: network 192.168.45.4 tiene area incorrecta

| Comando para encontrar el problema | Comando Aplicado                                                                                                                              |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | R4(config)#router ospf 2<br>R4(config-router)#no network 192.168.45.4 0.0.0.0 area 0<br>R4(config-router)#network 192.168.45.4 0.0.0.0 area 3 |

---

- Enfoque Utilizado: Seguimiento de ruta
- Problema Encontrado (Descripcion Breve): No puede llegar a L0 de ISP

| Comando para encontrar el problema                                            | Comando Aplicado                                                       |
| ----------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| show ipv6 route<br>traceroute 2021:ACAD:ACAD:8::8<br>ping 2021:ACAD:ACAD:8::8 | R4(config)#ipv6 route 2021:ACAD:ACAD:8::8/128 2021:ACAD:ACAD:209::2 15 |

---

---
### Tabla Sumarizacion
#### Tabla Sumarizada IPv4

| IP-Network   | 128 | 64  | 32  | 16  | Corte | 8   | 4   | 2   | 1   |
| ------------ | --- | --- | --- | --- | ----- | --- | --- | --- | --- |
| 192.168.50.5 |     |     | x   | x   | /     |     |     | x   |     |
| 192.168.55.5 |     |     | x   | x   | /     |     | x   | x   | x   |

IP Sumarizada: `192.168.48.0/20`

---
#### Tabla Sumarizada IPv6
Sextetos (16 bits)
16 : 32 : 48 : 64 : 80
0-9

| A   | 10  |
| --- | --- |
| B   | 11  |
| C   | 12  |
| D   | 13  |
| E   | 14  |
| F   | 15  |

| Network IPv6               | Diferencia | 8   | 4   | 2   | 1   | \|  | 8   | 4   | 2   | 1   | \|  | 8   | 4   | 2   | 1   | \|  | 8   | Corte | 4   | 2   | 1   | \|  |
| -------------------------- | ---------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | ----- | --- | --- | --- | --- |
| 2021:ACAD:ACAD:50:1::5/128 | 0050       |     |     |     |     | \|  |     |     |     |     | \|  |     | x   |     | x   | \|  |     | /     |     |     |     | \|  |
| 2021:ACAD:ACAD:55:1::5/128 | 0055       |     |     |     |     | \|  |     |     |     |     | \|  |     | x   |     | x   | \|  |     | /     | x   |     | x   | \|  |


La red Sumarizada es: `2021:ACAD:ACAD:50::/61`

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): 192.168.48.0/20

| Comando para encontrar el problema | Comando Aplicado                                                                      |
| ---------------------------------- | ------------------------------------------------------------------------------------- |
| show ip route<br>show run          | R4(config)#router ospf 2<br>R4(config-router)#area 3 range 192.168.48.0 255.255.240.0 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): 2021:ACAD:ACAD:50::/61

| Comando para encontrar el problema | Comando Aplicado                                                                    |
| ---------------------------------- | ----------------------------------------------------------------------------------- |
| show ipv6 route<br>show run        | R4(config)#ipv6 router ospf 1<br>R4(config-rtr)#area 3 range 2021:ACAD:ACAD:50::/61 |

---

## Equipo: R5

- Enfoque Utilizado: Seguimiento de Ruta
- Problema Encontrado (Descripcion Breve): Enrutamiento IPv6 no habilitado

| Comando para encontrar el problema | Comando Aplicado                |
| ---------------------------------- | ------------------------------- |
| show ipv6 protocols<br>show run    | R5(config)#ipv6 unicast-routing |

---

- Enfoque Utilizado: Seguimiento de Ruta
- Problema Encontrado (Descripcion Breve): OSPFv3 no configurado

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                      |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ipv6 ospf                     | R5(config)#ipv6 router ospf 1<br>R5(config-rtr)#router-id 5.5.5.5<br>R5(config-rtr)#passive-interface l5<br>R5(config-rtr)#passive-interface l55<br>R5(config-rtr)#exit<br>R5(config)#int range e0/1,l5,l55<br>R5(config-if-range)#ipv6 ospf 1 area 3 |

---

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): IPv6 L5 y L55 IP Incorrecta

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                        |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ipv6 int brief                | R5(config)#int l5<br>R5(config-if)#no ipv6 address<br>R5(config-if)#ipv6 address 2021:ACAD:ACAD:50:1::5/128<br>R5(config-if)#exit<br>R5(config)#int l55<br>R5(config-if)#no ipv6 address<br>R5(config-if)#ipv6 address 2021:ACAD:ACAD:55:1::5/128<br>R5(config-if)#exit |

---
## Equipo: ISP

- Enfoque Utilizado: Comparacion
- Problema Detectado (Descripcion Breve): IPv6 Incorrecta L0

| Comando para encontrar el problema      | Comando Aplicado                                                                                                                    |
| --------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| show ipv6 int brief<br>show ipv6 int l0 | ISP(config)#int l0<br>ISP(config-if)#no ipv6 address 2020:ACAD:ACAD:8::8/128<br>ISP(config-if)#ipv6 address 2021:ACAD:ACAD:8::8/128 |
