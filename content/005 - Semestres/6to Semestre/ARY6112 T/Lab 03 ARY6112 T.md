# Info
## Imagen Topologia
![1000](https://slink.proxylivy.work/image/c2014129-8167-447d-b019-0a5a02a645e4.png)
## Sobre
Resolución de Problemas de EIGRP y OSPF
## Requerimientos
### Ticket 1: OSPF
- Se ha solicitado dejar en funcionamiento la porción de OSPF. Se ha indicado que los router-id utilizan el formato "X.X.X.X" donde la "X" reemplaza al número del router.
- Se ha indicado que los temporizadores a utilizar son los por defecto.
- Se ha solicitado sumarizar las loopback del área 1 para que sean sumarizadas en el resto de la red.

### Ticket 2: EIGRP
- Se ha solicitado dejar en funcionamiento la porción de EIGRP con AS solicitado.
- Se ha solicitado que las loopback de R4 sean sumarizadas.

### Ticket 3: Conectividad
- R2 debe proporcionar direccionamiento dinámico a los PC de R1 y R3.
- Deberá redistribuir ruta que conecta al ISP, además entre los protocolos IGP.
- Comprobar conectividad completa en la topología.

# Configuracion
## Ticket 1: OSPF
### Equipo R1
- Enfoque Utilizado: Divides y Vencerás
- Problema Detectado (Descripcion Breve):  Interfaz e0/1 IP Incorrecta

| Comando para encontrar el problema    | Comando Aplicado                                                                                        |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| show ip int brief<br>show ip int e0/1 | Int e0/1<br><br>no ip address 192.168.111.1 255.255.255.0<br><br>ip address 192.168.110.1 255.255.255.0 |

- Enfoque Utilizado: Punto de Diferencias
- Problema Encontrado (Descripcion Breve): DHCP esta configurado en R2 y no lo alcanza

| Comando para encontrar el problema | Comando Aplicado                                                        |
| ---------------------------------- | ----------------------------------------------------------------------- |
| show running-config                | R1(config)#int e0/1<br><br>R1(config-if)#ip helper-address 192.168.12.2 |

- Enfoque Utilizado: Seguimiento de Ruta
- Problema Encontrado (Descripcion Breve): OSPF: Int e0/2 Pasiva

| Comando para encontrar el problema | Comando Aplicado                                                            |
| ---------------------------------- | --------------------------------------------------------------------------- |
| show ip protocols                  | R1(config)#router ospf 1<br><br>R1(config-router)#no passive-interface e0/2 |

- Enfoque Utilizado: Seguimiento de Ruta
- Problema Detectado (Descripcion Breve): OSPF: Area Stub Configurada 

| Comando para encontrar el problema | Comando Aplicado                                                        |
| ---------------------------------- | ----------------------------------------------------------------------- |
| show ip protocols                  | R1(config-router)#router ospf 1<br><br>R1(config-router)#no area 1 stub |

- Enfoque Utilizado: Seguimiento de Ruta
- Problema Encontrado (Descripcion Breve): OSPF: Network 192.168.12.0 incorrecta

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                      |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | R1(config)#router ospf 1<br><br>R1(config-router)#no network 192.168.12.0 0.0.0.0 area 1<br><br>R1(config-router)#network 192.168.12.1 0.0.0.0 area 1 |

- Enfoque Utilizado: Enfoque de Punto de Diferencias
- Problema Encontrado (Descripcion Breve): OSPF: int e0/2 tiene parametros de tiempo incorrectos

| Comando para encontrar el problema | Comando Aplicado                                                                                                 |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| show ip ospf int e0/2              | R1(config)#int e0/2<br><br>R1(config-if)#no ip ospf hello-interval<br><br>R1(config-if)#no ip ospf dead-interval |
### Equipo: R2

- Enfoque Utilizado: Punto de Diferencias
- Problema Encontrado (Descripcion Breve): DHCP Excluey todas las ip del rango 192.168.110.0/24

| Comando para encontrar el problema | Comando Aplicado                                                         |
| ---------------------------------- | ------------------------------------------------------------------------ |
| show running-config                | R2(config)#no ip dhcp excluded-address 192.168.110.1 192.168.110.254<br> |


- Enfoque Utilizado: Seguimiento de Ruta
- Problema Detectado (Descripcion Breve): OSPF: network "192.168.23.2" tiene el area incorrecta

| Comando para encontrar el problema | Comando Aplicado                                                                                                                              |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | R2(config)#router ospf 1<br>R2(config-router)#no network 192.168.23.2 0.0.0.0 area 1<br>R2(config-router)#network 192.168.23.2 0.0.0.0 area 0 |

- Enfoque Utilizado: Acercamiento del Problema
- Problema Encontrado (Descripcion Breve): Int e0/1 tiene configurado parametros tiempo OSPF

| Comando para encontrar el problema | Comando Aplicado                                                 |
| ---------------------------------- | ---------------------------------------------------------------- |
| show run                           | R2(config)#int e0/1<br>R2(config-if)#no ip ospf hello-interval 5 |


- Enfoque Utilizado: Acercamiento del Problema
- Problema Encontrado (Descripcion Breve): OSPF: Redes no estan sumarizadas a la otra Area
- Adjunto: Tabla Sumarizacion
## Tabla Sumarizada IPv4
IP-network 1: `192.168.10.1`
IP-network 2: `192.168.11.1`
IP-network 3: `192.168.20.1`

| 1er <br>Octeto | 2do <br>Octeto | 128 | 64  | 32  | Corte | 16  | 8   | 4   | 2   | 1   | 4to <br>Octeto |
| -------------- | -------------- | --- | --- | --- | ----- | --- | --- | --- | --- | --- | -------------- |
| 192            | 168            |     |     |     | /     |     | x   |     | x   |     |                |
| 192            | 168            |     |     |     | /     |     | x   |     | x   | x   |                |
| 192            | 168            |     |     |     | /     | x   |     | x   |     |     |                |

IP Sumarizada: `192.168.0.0/19`

| Comando para encontrar el problema | Comando Aplicado                                        |
| ---------------------------------- | ------------------------------------------------------- |
| show ip protocols                  | router ospf 1<br>area 1 range 192.168.0.0 255.255.224.0 |

### Equipo: R3

- Enfoque Utilizado: Punto de Diferencias
- Problema Detectado (Descripcion Breve): OSPF: Router-ID en conflicto con R2

| Comando para encontrar el problema | Comando Aplicado                                                                                              |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | R3(config)#router ospf 1<br>R3(config-router)#no router-id 2.2.2.2<br>R3(config-router)#router-id 3.3.3.3<br> |

- Enfoque Utilizado: Seguimiento de Ruta
- Problema Encontrado (Descripcion Breve): OSPF: Interfaz e0/3 pasiva

| Comando para encontrar el problema | Comando Aplicado                                                            |
| ---------------------------------- | --------------------------------------------------------------------------- |
| show ip protocols                  | R3(config)#router ospf 1<br>R3(config-router)#no passive-interface e0/3<br> |

---

- Enfoque Utilizado: Punto de Diferencias
- Problema Detectado (Descripcion Breve): DHCP ip helper no configurado a R2

| Comando para encontrar el problema | Comando Aplicado                                                    |
| ---------------------------------- | ------------------------------------------------------------------- |
| show run                           | R3(config)#int e0/1<br>R3(config-if)#ip helper-address 192.168.23.2 |

---
- Enfoque Utilizado: Seguimiento de Problemas
- Problema Detectado (Descripcion Breve): OSPF: No esta distribuyendo la ruta estatica desde EIGRP

| Comando para encontrar el problema | Comando Aplicado                                                                |
| ---------------------------------- | ------------------------------------------------------------------------------- |
| show ip route                      | R3(config)#router ospf 1<br>R3(config-router)#default-information originate<br> |

---
---
## Ticket 2: EIGRP
### Equipo: R3

- Enfoque Utilizado: Seguimiento de Ruta
- Problema Detectado (Descripcion Breve): EIGRP: Int e0/0 esta pasiva 

| Comando para encontrar el problema                                                           | Comando Aplicado                                                         |
| -------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| show running-config \| section router eigrp<br>show ip protocols<br>show ip eigrp interfaces | R3(config)#router eigrp 1<br>R3(config-router)#no passive-interface e0/0 |

- Enfoque Utilizado: Seguimiento de Ruta
- Problema Encontrado (Descripcion Breve): EIGRP: network 192.168.34.0 mal configurada

| Comando para encontrar el problema | Comando Aplicado                                                                                                                 |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | R3(config)#router eigrp 1<br>R3(config-router)#no network 192.168.34.0 0.0.0.0<br>R3(config-router)#network 192.168.34.3 0.0.0.0 |
### Equipo: R4

- Enfoque Utilizado: Seguimiento de Ruta
- Problema Detectado (Descripcion Breve): int e0/0 esta pasiva

| Comando para encontrar el problema                                                           | Comando Aplicado                                                         |
| -------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| show running-config \| section router eigrp<br>show ip protocols<br>show ip eigrp interfaces | R4(config)#router eigrp 1<br>R4(config-router)#no passive-interface e0/0 |

---

- Enfoque Utilizado: Punto de Diferencia
- Problema Encontrado (Descripcion Breve): EIGRP: Network Loopack `172.16.44.1` no configurada

| Comando para encontrar el problema     | Comando Aplicado                                                           |
| -------------------------------------- | -------------------------------------------------------------------------- |
| show ip protocols<br>show ip int brief | R4(config)#router eigrp 1<br>R4(config-router)#network 172.16.44.1 0.0.0.0 |

---

- Enfoque Utilizado: Divide y Venceras
- Problema Encontrado (Descripcion Breve): IP int e0/0 mal configurada

| Comando para encontrar el problema | Comando Aplicado                                                       |
| ---------------------------------- | ---------------------------------------------------------------------- |
| show ip int brief                  | R4(config)#int e0/0<br>R4(config-if)#ip add 192.168.34.4 255.255.255.0 |

---

- Enfoque Utilizado: Seguimiento de Ruta
- Problema Encontrado (Descripcion Breve): Falta Ruta Estatica a ISP

| Comando para encontrar el problema | Comando Aplicado                              |
| ---------------------------------- | --------------------------------------------- |
| show ip route                      | R4(config)#ip route 0.0.0.0 0.0.0.0 200.0.0.2 |

---
- Enfoque Utilizado: Seguimiento de Ruta
- Problema Encontrado (Descripcion Breve): EIGRP: Falta redistribucion de rutas estaticas

| Comando para encontrar el problema | Comando Aplicado                                                   |
| ---------------------------------- | ------------------------------------------------------------------ |
| show ip eigrp topology             | R4(config)#router eigrp 1<br>R4(config-router)#redistribute static |

---

- Enfoque Utilizado: Seguimiento de Ruta
- Problema Encontrado (Descripcion Breve): EIGRP: Redistribucion de Loopback
- Vease: Tabla de Sumarizacion
## Tabla Sumarizada IPv4
IP-network 1: `172.16.44.1`
IP-network 2: `172.16.54.1`

| 1er <br>Octeto | 2do <br>Octeto | 128 | 64  | 32  | Corte | 16  | 8   | 4   | 2   | 1   | 4to <br>Octeto |
| -------------- | -------------- | --- | --- | --- | ----- | --- | --- | --- | --- | --- | -------------- |
| 172            | 16             |     |     | x   | /     |     | x   | x   |     |     |                |
| 172            | 16             |     |     | x   | /     | x   |     | x   | x   |     |                |

IP Sumarizada: `172.16.32.0/19`

| Comando para encontrar el problema      | Comando Aplicado                                                                                           |
| --------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| show ip eigrp topology<br>show ip route | R3(config)#interface Ethernet0/0<br>R3(config-if)#ip summary-address eigrp 1 172.16.32.0 255.255.224.0<br> |

---




### Equipo: ISP

- Enfoque Utilizado: Seguimiento de Ruta
- Problema Detectado (Descripcion Breve): Falta Ruta Estatica a R4 (En caso de fallos??)

| Comando para encontrar el problema | Comando Aplicado                                  |
| ---------------------------------- | ------------------------------------------------- |
| show ip route                      | R4(config)#ip route 0.0.0.0 0.0.0.0 200.0.0.1<br> |

---
## Ticket 3: Conectividad
### Equipo: R3

- Enfoque Utilizado: Seguimiento de Ruta
- Problema Detectado (Descripcion Breve): Redistribucion: Rutas OSPF -> EIGRP

| Comando para encontrar el problema | Comando Aplicado                                                  |
| ---------------------------------- | ----------------------------------------------------------------- |
| show ip route                      | router eigrp 1<br>redistribute ospf 1 metric 10000 100 255 1 1500 |

---
- Enfoque Utilizado: Seguimiento de Ruta
- Problema Encontrado (Descripcion Breve): Redistribucion: Rutas EIGRP -> OSPF

| Comando para encontrar el problema | Comando Aplicado                              |
| ---------------------------------- | --------------------------------------------- |
| show ip route                      | router ospf 1<br>redistribute eigrp 1 subnets |

---

## Probar conectividad completa con tclsh
Desde PC-R1
```
tclsh
foreach address {
192.168.110.1
192.168.10.1
192.168.11.1
192.168.20.1
192.168.12.1
192.168.12.2
192.168.23.2
192.168.23.3
192.168.130.3
192.168.34.3
192.168.34.4
172.16.44.1
172.16.54.1
200.0.0.1
200.0.0.2
8.8.8.8
} { ping $address repeat 5 size 1500 }
```