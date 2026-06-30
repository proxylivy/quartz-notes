# Info
Version 6.2
## Imagen Topologia
![1000](https://slink.proxylivy.work/image/f7636938-25d3-4a52-9b0f-fa45f37c3edb.png)
## Contexto
Usted ha creado su empresa “CCNP ENARSI”, en donde tienen una gran red del tipo corporativa, mediante dos protocolos de enrutamiento dinámico.
Recientemente se ha creado una sucursal llamada “PERITA”, la cual debe tener acceso a Internet pasando a través de los backbone de EIGRP y OSPF, y por la sucursal “PERON”.
Lamentablemente, dicha situación no ocurre, por lo cual su labor será la detección, documentación y resolución de todos los problemas que se encuentran presentes en la red.
## Requerimientos
### A. Ticket 1: PPP
- La nueva sucursal “Perita” se encuentra conectada hacia el backbone de EIGRP mediante un enlace serial, el cual por ahora su encapsulación es Protocolo Punto a Punto. Por algún motivo se estableció un sistema de autenticación y el enlace no levanta. Se ha solicitado revisar el problema.
- Documentar todos los errores y soluciones efectuadas.
### B. Ticket 2: EIGRP
- Revisar y corregir todos los inconvenientes detectados en la red.
- Por petición explicita del cliente, deben utilizar todos los parámetros de EIGRP para el cálculo de métrica correspondiente.
- Documentar todos los errores y soluciones efectuadas.
### C. Ticket 3: OSPF
- Revisar y corregir todos los inconvenientes detectados en la red.
- El cliente solicita que los temporizadores entre R12 y R10; R13 y R11 tengan un valor de 8 para el hello y el tiempo que corresponda para el dead(32).
- Documente todos los errores y soluciones efectuadas.
### D. Ticket 4: Redistribuciones y Control de Rutas
- Por parte de la empresa, se ha mencionado que la redistribución entre protocolo IGP se realiza mediante mapas de rutas.
- Se ha solicitado que ejecute balanceo de carga en EIGRP para la red 0.0.0.0/0
- La red 10.100.2.0/24 no debe ser vista por R4 ni R3.
- Lograr filtrar la red 172.32.11.0/24 sin aplicar ACL, Distribute-Lists, Prefix-Lists o Route-Maps.
- Documente todos los errores y soluciones efectuadas.
### E. Ticket 5: Conectividad
- Comprobar que equipo final pueda recibir IP mediante DHCP.
- Equipo de Sucursal “PERITA” debe tener conectividad completa hacia Internet, según el siguiente output:
```
PC#ping www.caso1.cl
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.10.11.X, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 8/9/11 ms
```

# Configuracion
## Equipo: R1
- Enfoque Utilizado: Divide y Venceras
- Problema Detectado (Descripcion Breve): DHCP: Excluidas todas las ip de la red "10.10.11.0/24"

| Comando para encontrar el problema | Comando Aplicado                                    |
| ---------------------------------- | --------------------------------------------------- |
| show run \| section dhcp           | no ip dhcp excluded-address 10.10.11.0 10.10.11.255 |

---

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): PPP: Contraseña equivocada

| Comando para encontrar el problema | Comando Aplicado                                                |
| ---------------------------------- | --------------------------------------------------------------- |
| show run \| section username       | no username R2 password 0 Caso1<br>username R2 password 0 caso1 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Redistribucion entre sistemas EIGRP

| Comando para encontrar el problema | Comando Aplicado                                                          |
| ---------------------------------- | ------------------------------------------------------------------------- |
| show ip route                      | redistribute static<br>redistribute eigrp 200 metric 1544 2000 255 1 1500 |





---
## Equipo: R2

- Enfoque Utilizado: Comparacion
- Problema Detectado (Descripcion Breve): Tipo de autenticacion Erronea

| Comando para encontrar el problema      | Comando Aplicado                                                                        |
| --------------------------------------- | --------------------------------------------------------------------------------------- |
| show ppp all<br>show run \| section ppp | R2(config)#int s1/0<br>R2(config-if)#ppp authe<br>R2(config-if)#ppp authentication chap |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): EIGRP Todas las network estan incorrectas

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | R2(config)#router eigrp 200<br>R2(config-router)#no network 10.10.23.1 0.0.0.0<br>R2(config-router)#no network 10.10.24.1 0.0.0.0<br>R2(config-router)#no network 10.100.2.1 0.0.0.0<br>R2(config-router)#no network 10.100.4.1 0.0.0.0<br>R2(config-router)#network 10.10.23.2 0.0.0.0<br>R2(config-router)#network 10.10.24.2 0.0.0.0<br>R2(config-router)#network 10.100.2.2 0.0.0.0<br>R2(config-router)#network 10.100.4.2 0.0.0.0<br> |

---

- Enfoque Utilizado: Seguimiento de Ruta
- Problema Encontrado (Descripcion Breve): EIGRP: e0/3 pasiva

| Comando para encontrar el problema | Comando Aplicado                                                               |
| ---------------------------------- | ------------------------------------------------------------------------------ |
| show ip protocols                  | R2(config)#router eigrp 200<br>R2(config-router)#no passive-interface e0/3<br> |

---
- Enfoque Utilizado: Seguimiento de Ruta
- Problema Detectado (Descripcion Breve): IP L2 y L22 Incorrecta 

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                        |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip int brief                  | R2(config)#int l2<br>R2(config-if)#no ip address 10.100.2.1 255.255.255.0<br>R2(config-if)#ip address 10.100.2.2 255.255.255.0<br>R2(config-if)#exit<br>R2(config)#int l22<br>R2(config-if)#no ip address 10.100.4.1 255.255.255.0<br>R2(config-if)#ip address 10.100.4.2 255.255.255.0 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Falta ruta estatica a R1

| Comando para encontrar el problema | Comando Aplicado                               |
| ---------------------------------- | ---------------------------------------------- |
| show ip route                      | R2(config)#ip route 0.0.0.0 0.0.0.0 10.10.12.1 |

---
- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): EIGRP: Falta Redistribuir rutas estaticas

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                   |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip route                      | R2(config)#router eigrp 200<br>R2(config-router)#redistribute static<br>router eigrp 200<br>redistribute eigrp 100 metric 1544 2000 255 1 1500<br> |

---
- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): R3 y R4 pueden ver la ruta 10.100.2.0/24

| Comando para encontrar el problema                       | Comando Aplicado                                                                                                                                                                                                             |
| -------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| R2#show ip route<br>R3#show ip route<br>R4#show ip route | R2(config)#access-list 99 deny 10.100.2.0 0.0.0.255<br>R2(config)#access-list 99 permit any<br>R2(config)#router eigrp 200<br>R2(config-router)#distribute-list 99 out e0/0<br>R2(config-router)#distribute-list 99 out e0/3 |

---

## Equipo: R3

- Enfoque Utilizado: Divide y Venceras
- Problema Detectado (Descripcion Breve): EIGRP: Parametros K4 Desabilitado

| Comando para encontrar el problema                  | Comando Aplicado                                                            |
| --------------------------------------------------- | --------------------------------------------------------------------------- |
| show ip protocols<br>show ip protocols \| include K | R3(config)#router eigrp 200<br>R3(config-router)#metric weights 0 1 1 1 1 1 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): EIGRP: Temporizador hello mal configurado

| Comando para encontrar el problema | Comando Aplicado                                                      |
| ---------------------------------- | --------------------------------------------------------------------- |
| show ip eigrp timers               | R3(config)#int e0/1<br>R3(config-if)#no ip hello-interval eigrp 200 1 |

---
## Equipo: R4

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): EIGRP: AS 1 mal configurado

| Comando para encontrar el problema | Comando Aplicado             |
| ---------------------------------- | ---------------------------- |
| show ip protocols                  | R4(config)#no router eigrp 1 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Access-List: Configuracion Avandonada

| Comando para encontrar el problema | Comando Aplicado                             |
| ---------------------------------- | -------------------------------------------- |
| show access-lists                  | R4(config)#no ip access-list standard FILTRO |

---
## Equipo: R5

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): EIGRP: AS 1 mal configurado

| Comando para encontrar el problema | Comando Aplicado             |
| ---------------------------------- | ---------------------------- |
| show ip protocols                  | R5(config)#no router eigrp 1 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Redistribucion: Rutas EIGRP -> OSPF esta incorrecta

| Comando para encontrar el problema                       | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                       |
| -------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show route-map<br>show access-lists<br>show ip route<br> | R5(config)#route-map REDISTRIBUCION-B permit 10<br>R5(config-route-map)#no match ip address EIGRP<br>R5(config-route-map)#match ip address OSPF<br>R5(config-route-map)#no set metric 1544 2000 255 1 1500<br>R5(config-route-map)#set metric 10000 100 255 1 1500<br>R5(config-route-map)#exit<br>R5(config)#router eigrp 200<br>R5(config-router)#redistribute ospf 1 route-map REDISTRIBUCION-B<br> |

---
- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): OSPF: Network 172.16.56.0 incorrecta

| Comando para encontrar el problema | Comando Aplicado                                                                                                                            |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | R5(config)#router ospf 1<br>R5(config-router)#no network 172.16.56.0 0.0.0.0 area 0<br>R5(config-router)#network 172.16.56.5 0.0.0.0 area 0 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): EIGRP: No existe balanceo de carga

| Comando para encontrar el problema            | Comando Aplicado                                                                                 |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| show ip route eigrp<br>show ip eigrp topology | R5(config)#router eigrp 200<br>R5(config-router)#maximum-paths 4<br>R5(config-router)#variance 2 |

---
## Equipo: R6

- Enfoque Utilizado: Divide y Venceras
- Problema Detectado (Descripcion Breve): OSPF: Interfaz E0/0 Pasiva

| Comando para encontrar el problema | Comando Aplicado                                                        |
| ---------------------------------- | ----------------------------------------------------------------------- |
| show ip protocols                  | R6(config)#router ospf 1<br>R6(config-router)#no passive-interface e0/0 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): OSPF: Timers Incorrectos 

| Comando para encontrar el problema              | Comando Aplicado                                                  |
| ----------------------------------------------- | ----------------------------------------------------------------- |
| R6#show ip ospf interface e0/0 \| include Timer | R6(config)#int e0/0<br>R6(config-if)#no ip ospf hello-interval 11 |

---
## Equipo: R7

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): OSPF: Interfaz e0/0 Pasiva

| Comando para encontrar el problema | Comando Aplicado                                                        |
| ---------------------------------- | ----------------------------------------------------------------------- |
| show ip protocols                  | R7(config)#router ospf 1<br>R7(config-router)#no passive-interface e0/0 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): OSPF: Temporizadores Incorrectos

| Comando para encontrar el problema              | Comando Aplicado                                                        |
| ----------------------------------------------- | ----------------------------------------------------------------------- |
| R7#show ip ospf interface e0/0 \| include Timer | R7(config-router)#int e0/0<br>R7(config-if)#no ip ospf hello-interval 9 |

---
## Equipo: R8 

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): OSPF: Tiempos Hello y Dead, de la interfaz e0/3 incorrectos

| Comando para encontrar el problema              | Comando Aplicado                                                                                              |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| R8#show ip ospf interface e0/3 \| include Timer | R8(config)#int e0/3<br>R8(config-if)#no ip ospf dead-interval 39<br>R8(config-if)#no ip ospf hello-interval 9 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): No se que es max-metric router-lsa, asi que a la chingada

| Comando para encontrar el problema | Comando Aplicado                                                       |
| ---------------------------------- | ---------------------------------------------------------------------- |
| show run                           | R8(config)#router ospf 1<br>R8(config-router)#no max-metric router-lsa |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): OSPF: Network 172.16.67.7 incorrecta

| Comando para encontrar el problema | Comando Aplicado                                                                                                                            |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | R8(config)#router ospf 1<br>R8(config-router)#no network 172.16.67.7 0.0.0.0 area 0<br>R8(config-router)#network 172.16.67.8 0.0.0.0 area 0 |

---
## Equipo: R9

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): OSPF: Tiempos Hello y Dead mal configurados

| Comando para encontrar el problema      | Comando Aplicado                                                           |
| --------------------------------------- | -------------------------------------------------------------------------- |
| show ip ospf interface e0/3<br>show run | int e0/3<br>no ip ospf dead-interval 38<br>no ip ospf hello-interval 9<br> |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): OSPF: Red 172.16.100.9 tiene incorrecta el area

| Comando para encontrar el problema | Comando Aplicado                                                                                                                              |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | R9(config)#router ospf 1<br>R9(config-router)#no network 172.16.100.9 0.0.0.0 area 0<br>R9(config-router)#network 172.16.100.9 0.0.0.0 area 1 |

---
## Equipo: R10
- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): OSPF: Temporizadores les falta ajuste

| Comando para encontrar el problema               | Comando Aplicado                                                                                           |
| ------------------------------------------------ | ---------------------------------------------------------------------------------------------------------- |
| R10#show ip ospf interface e0/0 \| include Timer | R10(config)#int e0/0<br>R10(config-if)#ip ospf hello-interval 8<br>R10(config-if)#ip ospf dead-interval 32 |

---
## Equipo: R11

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): OSPF: Router-ID Incorrecto

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                            |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | R11(config)#router ospf 1<br>R11(config-router)#no router-id 10.10.10.10<br>R11(config-router)#router-id 11.11.11.11<br>R11(config-router)#do clear ip ospf process<br>Reset ALL OSPF processes? \[no]: yes |

---
- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Falta Filtrar Ruta 172.32.11.0/24

| Comando para encontrar el problema | Comando Aplicado                                                                                     |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------- |
| show ip route ospf                 | R11(config)#router ospf 1<br>R11(config-router)#area 1 range 172.32.11.0 255.255.255.0 not-advertise |

---
- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Falta configurar Timers OSPF

| Comando para encontrar el problema                   | Comando Aplicado                                                                                               |
| ---------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| R11#show ip ospf interface e0/3 \| include Timer<br> | R11(config)#int e0/3<br>R11(config-if)#ip ospf hello-interval 8<br>R11(config-if)#ip ospf dead-interval 32<br> |

---
## Equipo: R12

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): OSPF: Area Stub Activado

| Comando para encontrar el problema            | Comando Aplicado                                               |
| --------------------------------------------- | -------------------------------------------------------------- |
| show ip protocols<br>show run \| section ospf | R12(config)#router ospf 1<br>R12(config-router)#no area 1 stub |

---
- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Falta configurar Timers OSPF

| Comando para encontrar el problema                   | Comando Aplicado                                                                                               |
| ---------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| R12#show ip ospf interface e0/0 \| include Timer<br> | R12(config)#int e0/0<br>R12(config-if)#ip ospf hello-interval 8<br>R12(config-if)#ip ospf dead-interval 32<br> |

---
## Equipo: R13

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): OSPF: Area Stub Activado

| Comando para encontrar el problema            | Comando Aplicado                                               |
| --------------------------------------------- | -------------------------------------------------------------- |
| show ip protocols<br>show run \| section ospf | R13(config)#router ospf 1<br>R13(config-router)#no area 1 stub |

---
- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Falta configurar Timers OSPF

| Comando para encontrar el problema                   | Comando Aplicado                                                                                               |
| ---------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| R13#show ip ospf interface e0/3 \| include Timer<br> | R13(config)#int e0/3<br>R13(config-if)#ip ospf hello-interval 8<br>R13(config-if)#ip ospf dead-interval 32<br> |

---
## Equipo: ISP

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): Rutas estaticas Equivocadas

| Comando para encontrar el problema                   | Comando Aplicado                                                                                                                                                                                                 |
| ---------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip route static<br>show run \| section ip route | ISP(config)#no ip route 0.0.0.0 0.0.0.0 200.0.0.4<br>ISP(config)#no ip route 0.0.0.0 0.0.0.0 201.0.0.4 10<br>ISP(config)#ip route 0.0.0.0 0.0.0.0 200.0.0.1<br>ISP(config)#ip route 0.0.0.0 0.0.0.0 201.0.0.1 10 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Interfaz e0/0 Apagada

| Comando para encontrar el problema | Comando Aplicado                               |
| ---------------------------------- | ---------------------------------------------- |
| show ip int brief                  | ISP(config)#int e0/0<br>ISP(config-if)#no shut |

---