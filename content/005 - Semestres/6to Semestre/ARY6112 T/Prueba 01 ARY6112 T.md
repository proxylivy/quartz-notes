# Info
## Imagen Topologia
![700](https://slink.proxylivy.work/image/3c0b30d7-f82b-4b55-9e28-1ea2991c5997.jpg)
## Contexto
Usted trabaja en la empresa “**PATRIOTAS CHILE**” donde su función es detectar y resolver problemas de red. Un cliente lo ha contactado para requerir sus servicios, ya que los PC y la red tienen problemas de conexión y por la festividad de fiestas Patrias necesitan a la brevedad que estén resueltos, de manera que, al retorno a la actividad laboral, los colaboradores de la empresa trabajen sin problemas.
Su labor será la detección, documentación y resolución de problemas, que han sido agrupados en tickets.
## Requerimientos
### A. Ticket 1: PPP
- En la red del cliente existen dos enlaces que se encuentra en configurados con PPP con CHAP en R1-R3 y PAP en R5-R6. Lamentablemente ninguno de los dos enlaces levanta. La password debe ser en minúscula.
- Detecte, documente y resuelva los inconvenientes existentes.
### B. Ticket 2: EIGRP
- La red de cliente tiene una porción configurada con EIGRP en formato de 32 bits. Lamentablemente algunos de los encargados TI, mencionan que los equipos no tienen relaciones de vecindad entre sí y además de conectividad muy limitada.
- Por política de la empresa solo las interfaces a equipos finales deben estar pasivas.
- Detecte, documente y resuelva los inconvenientes existentes.
### C. Ticket 3: OSPF
- La red del cliente se encuentra configurada con una red de OSPF Multiarea, donde existe un área backbone que conecta con un ISP. Además, posee dos áreas no centrales, las cuales permiten la conectividad con equipos remotos. Lamentablemente ninguna área tiene convergencia, lo que involucra que no tenemos comunicación con los hosts.
- Por política de la empresa solo las interfaces a equipos finales deben estar pasivas.
- Detecte, documente y resuelva los inconvenientes existentes.
 ### D. Ticket 4: Redistribuciones
- El cliente menciona que todo el tráfico hacia Internet sale por defecto por la Ethernet 0/0, y en caso de caída utiliza la Ethernet 0/1.
- El cliente menciona que, por flexibilidad de la red, como existen dos protocolos de enrutamiento, el intercambio de información entre protocolos de enrutamiento es mediante mapas de rutas.
- Detecte, documente y resuelva los inconvenientes existentes.
### E. Ticket 5: Mejoras en la red
- Después de que la red de la empresa se encuentra en funcionamiento en gran parte, el cliente le ha solicitado que el PC de R3 pueda utilizar todos los caminos disponibles por los enlaces que utiliza EIGRP.
- El PC de R2 ha solicitado que todo el flujo de información utiliza de manera incondicional la Ethernet 0/1 (PBR)
- Se ha solicitado que los timers de OSPF entre R1-RB y RB-R5 queden reducidos en un 50%.
- Documente las mejoras solicitadas

### F. Ticket 6: Conectividad
- Los PC de R2 y R3, deben recibir direccionamiento IP por DHCP a partir del router R1.
- Comprobar conectividad completa entre los PC y salida hacia Internet.
- Detecte, documente y resuelva los inconvenientes existentes.
### G. Capturas de pantalla de conectividad
- Inserte una captura de pantalla donde se pueda visualizar la fecha y hora de su máquina con la prueba de conectividad por PING desde el PC de R3 hacia 8.8.8.8 y 8.8.4.4
- Inserte una captura de pantalla donde se pueda visualizar la fecha y hora de su máquina con la prueba de conectividad por TRACERT desde el PC2 de R2 hacia 8.8.8.8
- Inserte una captura de pantalla donde se pueda visualizar la fecha y hora de su máquina donde se debe mostrar el balanceo de carga realizado en IPV4.
# Configuracion
### Equipo: R7

- Enfoque Utilizado: Divide y Venceras
- Problema Detectado (Descripcion Breve): Interfaz e0/2 apagada

| Comando para encontrar el problema | Comando Aplicado                             |
| ---------------------------------- | -------------------------------------------- |
| show ip int brief                  | R7(config)#int e0/2<br>R7(config-if)#no shut |

---
## Equipo: R6

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): OSPF: Interfaz e0/1 Pasiva

| Comando para encontrar el problema | Comando Aplicado                                                        |
| ---------------------------------- | ----------------------------------------------------------------------- |
| show ip protocols                  | R6(config)#router ospf 1<br>R6(config-router)#no passive-interface e0/1 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): PPP: Username Enviado desde R6 Incorrecto

| Comando para encontrar el problema           | Comando Aplicado                                                                                                                                 |
| -------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| show ppp all<br>show run \| section username | R6(config)#int s1/1<br>R6(config-if)#no ppp pap sent-username R5 password 0 prueba1<br>R6(config-if)#ppp pap sent-username R6 password 0 prueba1 |

---
## Equipo: R5

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): OSPF: Network 192.168.56.5 area incorrecta

| Comando para encontrar el problema | Comando Aplicado                                                                                                                               |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | R5(config)#router ospf 1<br>R5(config-router)#no network 192.168.56.5 0.0.0.0 area 11<br>R5(config-router)#network 192.168.56.5 0.0.0.0 area 1 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): OSPF: Falta Virtual-link hacia R6

| Comando para encontrar el problema | Comando Aplicado                                                          |
| ---------------------------------- | ------------------------------------------------------------------------- |
| show ip ospf virtual-links         | R5(config)#router ospf 1<br>R5(config-router)#area 1 virtual-link 6.6.6.6 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): OSPF: Max metric no permite adyacencia Virtual-Links

| Comando para encontrar el problema | Comando Aplicado                                                                                                                      |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| show run                           | R5(config)#router ospf 1<br>R5(config-router)#no max-metric router-lsa on-startup 86400<br>R5(config-router)#no max-metric router-lsa |

---
- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Mejoras en la Red: OSPF Timers deben quedar al 50% (RB-R5)

| Comando para encontrar el problema                                | Comando Aplicado                                                                                            |
| ----------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| show ip protocols<br>show ip ospf interface s1/0 \| include Timer | R5(config)#int s1/0<br>R5(config-if)#ip ospf hello-interval 5<br>R5(config-if)#ip ospf dead-interval 20<br> |

---
## Equipo: RB

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): OSPF: Falta network 192.168.54.0/24

| Comando para encontrar el problema                              | Comando Aplicado                                                                  |
| --------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| show ip protocols<br>show ip ospf neighbor<br>show ip int brief | RB(config)#router ospf 1<br>RB(config-router)#network 192.168.54.4 0.0.0.0 area 0 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): REDISTRIBUCION: Debe tener mejor prioridad e0/0, y usar e0/1 como respaldo

| Comando para encontrar el problema    | Comando Aplicado                                                                                                                  |
| ------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| show ip route<br>show ip route static | RB(config)#no ip route 0.0.0.0 0.0.0.0 Ethernet0/1 219.50.0.1 10<br>RB(config)#ip route 0.0.0.0 0.0.0.0 Ethernet0/1 219.50.0.1 15 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Mejoras en la Red: OSPF Timers deben quedar al 50% (R1-RB)

| Comando para encontrar el problema                                | Comando Aplicado                                                                                           |
| ----------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| show ip protocols<br>show ip ospf interface s1/2 \| include Timer | RB(config-if)#int s1/2<br>RB(config-if)#ip ospf hello-interval 5<br>RB(config-if)#ip ospf dead-interval 20 |

---
- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Mejoras en la Red: OSPF Timers deben quedar al 50% (RB-R5)

| Comando para encontrar el problema                                | Comando Aplicado                                                                                        |
| ----------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| show ip protocols<br>show ip ospf interface s1/0 \| include Timer | RB(config)#int s1/0<br>RB(config-if)#ip ospf hello-interval 5<br>RB(config-if)#ip ospf dead-interval 20 |

---
- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Le falta redistribuir las rutas a OSFP

| Comando para encontrar el problema | Comando Aplicado                                                            |
| ---------------------------------- | --------------------------------------------------------------------------- |
| show ip route                      | RB(config)#router ospf 1<br>RB(config-router)#default-information originate |




---
## Equipo: R1

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): OSPF: Protocolo Apagado

| Comando para encontrar el problema                                     | Comando Aplicado         |
| ---------------------------------------------------------------------- | ------------------------ |
| show ip protocols<br>show ip ospf neighbor<br>show run \| section ospf | router ospf 1<br>no shut |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): EIGRP: Parametros EIGRP no estan por defecto

| Comando para encontrar el problema                  | Comando Aplicado                                                          |
| --------------------------------------------------- | ------------------------------------------------------------------------- |
| show ip protocols<br>show ip protocols \| include K | R1(config)#router eigrp 1<br>R1(config-router)#metric weights 0 1 0 1 0 0 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): EIGRP: Interfaz S1/1 pasiva

| Comando para encontrar el problema | Comando Aplicado                                                         |
| ---------------------------------- | ------------------------------------------------------------------------ |
| show ip protocols                  | R1(config)#router eigrp 1<br>R1(config-router)#no passive-interface s1/1 |

---

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): EIGRP: Network 172.16.12.0 Incorrecta

| Comando para encontrar el problema | Comando Aplicado                                                           |
| ---------------------------------- | -------------------------------------------------------------------------- |
| show ip protocols                  | R1(config)#router eigrp 1<br>R1(config-router)#network 172.16.12.1 0.0.0.0 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Mejoras en la Red: OSPF Timers deben quedar al 50% (R1-RB)

| Comando para encontrar el problema                                | Comando Aplicado                                                                                        |
| ----------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| show ip protocols<br>show ip ospf interface s1/2 \| include Timer | R1(config)#int s1/2<br>R1(config-if)#ip ospf hello-interval 5<br>R1(config-if)#ip ospf dead-interval 20 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Redistribucion yo creo
	- EIGRP
		- redistribute ospf 1 route-map REDISTRIBUCION-1
			- REDISTRIBUCION-1
				- route-map REDISTRIBUCION-1 permit 10
					match ip address OSPF
					set metric 1544 2000 255 1 1500

	- OSPF
	- En Route-map le falta el match address
		- redistribute eigrp 1 subnets route-map REDISTRIBUCION-2
			- REDISTRIBUCION-2
				- math address EIGRP
				- route-map REDISTRIBUCION-2 permit 10
				  set metric 10
				  set metric-type type-1




| Comando para encontrar el problema | Comando Aplicado                                                                                                                                    |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
|                                    | R1(config)#route-map REDISTRIBUCION-1 permit 10<br>R1(config-route-map)#no match ip address EIGRP<br>R1(config-route-map)#match ip address OSPF<br> |

---
- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Redistribucion OSPF

| Comando para encontrar el problema | Comando Aplicado                                                                               |
| ---------------------------------- | ---------------------------------------------------------------------------------------------- |
|                                    | R1(config)#route-map REDISTRIBUCION-2 permit 10<br>R1(config-route-map)#match ip address EIGRP |

---

## Equipo: R2

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): EIGRP: Metricas no son por defecto

| Comando para encontrar el problema                  | Comando Aplicado                                                          |
| --------------------------------------------------- | ------------------------------------------------------------------------- |
| show ip protocols<br>show ip protocols \| include K | R2(config)#router eigrp 1<br>R2(config-router)#metric weights 0 1 0 1 0 0 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): EIGRP: Access-Lists Bloquea todo el trafico de EIGRP y esta aplicado a las interfaces que participan de EIGRP

| Comando para encontrar el problema                         | Comando Aplicado                                                                                                                                                                                       |
| ---------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show access-lists<br>show ip protocols \| section filtered | R2(config)#no access-list 1 deny   172.16.0.0 0.0.255.255<br>R2(config)#router eigrp 1<br>R2(config-router)#no distribute-list 1 in Ethernet0/1<br>R2(config-router)#no distribute-list 1 in Serial1/0 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): EIGRP: Router-id en conflicto con R1

| Comando para encontrar el problema | Comando Aplicado                                                                                                       |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | R2(config)#router eigrp 1<br>R2(config-router)#no eigrp router-id 1.1.1.1<br>R2(config-router)#eigrp router-id 2.2.2.2 |

---
- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): EIGRP: Falta network 172.16.22.2

| Comando para encontrar el problema | Comando Aplicado                                                           |
| ---------------------------------- | -------------------------------------------------------------------------- |
| show ip protocols                  | R2(config)#router eigrp 1<br>R2(config-router)#network 172.16.22.2 0.0.0.0 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): EIGRP: Network 172.16.33.3 incorrecta

| Comando para encontrar el problema | Comando Aplicado                                                                                                               |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| show ip protocols                  | R2(config)#router eigrp 1<br>R2(config-router)#no network 172.16.33.3 0.0.0.0<br>R2(config-router)#network 172.16.12.2 0.0.0.0 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Mejoras en la red: No esta configuraba PBR preferir el camino e0/0

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                       |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show run                           | R2(config)#access-list 80 permit 172.16.22.0 0.0.0.255<br>R2(config)#route-map PBR permit 20<br>R2(config-route-map)#match ip address 80<br>R2(config-route-map)#set ip next-hop 172.16.12.1<br>R2(config-route-map)#int e0/0<br>R2(config-if)#ip policy route-map PBR |

---

## Equipo: R3

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): PAP: Base de datos usuarios y contraseña incorrecta
	- username R3 password 0 Prueba1

| Comando para encontrar el problema                                      | Comando Aplicado                                                                          |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| show ppp all<br>show run \| section pap<br>show run \| section username | R3(config)#no username R3 password 0 Prueba1<br>R3(config)#username R1 password 0 prueba1 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): EIGRP: Metricas no son las por defecto

| Comando para encontrar el problema | Comando Aplicado                                                         |
| ---------------------------------- | ------------------------------------------------------------------------ |
| show ip protocols                  | R3(config)#router eigrp 1<br>R3(config-router)#metric weight 0 1 0 1 0 0 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): EIGRP: Protocolo Apagado

| Comando para encontrar el problema                                            | Comando Aplicado                                       |
| ----------------------------------------------------------------------------- | ------------------------------------------------------ |
| show ip protocols<br>show ip eigrp neighbors<br>show run \| section eigrp<br> | R3(config)#router eigrp 1<br>R3(config-router)#no shut |

---
- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): DHCP: IP helper incorrecto

| Comando para encontrar el problema | Comando Aplicado                                                                                                     |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| show run \| section ip helper      | R3(config)#int e0/0<br>R3(config-if)#no ip helper-address 172.16.13.0<br>R3(config-if)#ip helper-address 172.16.13.1 |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): Mejoras de red EIGRP: Permite Carga Desigual

| Comando para encontrar el problema      | Comando Aplicado                                                                               |
| --------------------------------------- | ---------------------------------------------------------------------------------------------- |
| show ip route<br>show ip eigrp topology | R3(config)#router eigrp 1<br>R3(config-router)#maximum-paths 4<br>R3(config-router)#variance 2 |

---
