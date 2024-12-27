# Info
## Imagen Topologia
![1000](https://slink.proxylivy.work/image/908c85f5-d08c-44bf-8118-4f09cc01e479.png)
## Instrucciones Generales
Contenidos para repasar ET
1. Configuración y verificación de errores de [[010 - Protocolos/010.2 - Switching/Etherchannel|Etherchannel]] capa2 y capa 3
2. Configuración y verificación de errores de [[010 - Protocolos/010.2 - Switching/spanning-tree/802.1D (STP)|802.1D (STP)]] y [[010 - Protocolos/010.2 - Switching/spanning-tree/802.1S (MSTP)|802.1S (MSTP)]]
3. Configuración y verificación de errores asociados a [[020 - Conceptos/020.1 - Administracion/PACL y VACL|PACL y VACL]]
4. Configuración y verificación de errores de [[010 - Protocolos/010.2 - Switching/HSRP|HSRP]] con track e IPSLA
5. Configuración y verificación de errores de [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP|EIGRP]] con [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP Numerado|EIGRP Numerado]] o clásico
6. Configuración y verificación de errores de [[010 - Protocolos/010.1 - Routing/OSPF/OSPF|OSPF]] con [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv2|OSPFv2]]
7. Configuración y verificación de errores de [[010 - Protocolos/010.1 - Routing/Redistribucion|Redistribucion]] de protocolos
8. Configuración y verificación de errores de [[010 - Protocolos/010.1 - Routing/BGP/BGP|BGP]] con [[010 - Protocolos/010.1 - Routing/BGP/BGP-3|BGP-3]]
9. Configuración y verificación de errores de [[010 - Protocolos/010.1 - Routing/MPLS|MPLS]] (generación de etiquetas, RD, VPNv4, VRF)
10. Configuración y verificación de errores de [[999 - Archivado/DMVPN|DMVPN]] (NHRP, enrutamiento, IPSEC)
11. Configuración y verificación de errores de [[010 - Protocolos/010.3 - Comunicaciones/DHCP|DHCP]]
12. Configuración y verificación de errores de enrutamiento estático | [[010 - Protocolos/010.1 - Routing/Ruta Estatica|Ruta Estatica]]
13. Comprobación de conectividad (ping, traceroute)

---

Lea atentamente cada uno de los ítems indicados a continuación:
- La evaluación se realizará de forma individual y/o parejas.
- El tiempo para desarrollar la evaluación es de 200 min.
- Se utilizará la herramienta de virtualización IOUWeb para desarrollar la evaluación.
- Debe entregar evidencias de todo los realizado según lo solicitado.
- Resolver caso práctico mediante requerimientos, aplicando metodología de resolución de problemas.
- Documentar en documento adjunto Tickets - ARY6112.
- El archivo debe ser guardado con el nombre: ET_ARY6112_NOMBRE, y debe ser enviado mediante AVA Ultra en el link indicado por el docente.
## Recursos
Recursos necesarios:
- [VM IOU WEB](https://duoccl0-my.sharepoint.com/:f:/g/personal/ga_zunigam_duocuc_cl/ElTeBCifMqJBhacbv5sdSAYBOMJP9e8P5JejXMTSIQjPAw?e=jPCisp)
- [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
## Contexto
La empresa “ENARSI_ET” dedicada al rubro financiero, actualmente presentes con 3 sucursales. Desde hace un tiempo la organización comenzó a actualizar su infraestructura y servicios TI. Para esta actualización se contrató a una empresa externa, la cual no realizó un buen trabajo dejando sin conectividad a la empresa y con muchos problemas en su funcionamiento, tanto a nivel interno como externo incluyendo tecnología del proveedor de servicios.

Empresa “ENARSI_ET” lo contrató para solucionar todos los problemas en la red de las sucursales y a nivel de proveedor de servicios, para este trabajo se le solicita documentar todos los problemas detectados según los tickets entregados y proponer la mejor solución de acuerdo a los requerimientos solicitados por la empresa.

En cada problema detectado, debe completar con la información solicitada para cada ticket.
## Requerimientos
### Ticket 1 – Tecnologías de capa 2.
- Usuarios de sucursal central reportan problemas de conectividad, personal de TI realizó pruebas para determinar el problema, y de acuerdo a los resultados, el problema radica en las capas de distribución y acceso.
- Además, también se informó que se han presentado los que parecen ser loops de capa 2 y tormentas de broadcast en el mismo segmento de red, al solucionar este inconveniente, tomar en cuenta balanceo de carga en congruencia con alta disponibilidad de capa 3 según topología.
- Adicionalmente se solicita que a nivel de acceso se configure algún tipo de mecanismo de seguridad tanto para el acceso de los equipos finales como para evitar ataques a protocolos de capa 2.
- Se solicita diagnosticar, resolver y documentar el o los problemas detectados.

### Ticket 2 – Alta disponibilidad de capa 3.
- Personal de TI de sucursal central reporta problemas con el protocolo FHRP, no se logra definir un router activo para las VLAN de la red, provocando problemas con el gateway de los equipos finales de la red.
- Se solicita la implementación de algún sistema de monitoreo a los enlaces ascendentes de tal manera que MLS1 y MLS2 detecten la caída de uno de estos para que permita la asistencia del dispositivo en estado standby.
- Se solicita diagnosticar, corregir y documentar cualquier problema que no permita definir un router activo y un standby para cada VLAN en el protocolo FHRP de la red.
- Se solicita mantener la congruencia con protocolos de redundancia de capa 2.

### Ticket 3 – PROTOCOLOS EIGRP Y OSPF
- Se solicita que se establezca el porcentaje de uso de ancho de banda para protocolo EIGRP en 15% para todas sus interfaces.
- Verificar redistribución de protocolo EIGRP y OSPF en sucursal central, debe estar configurado a través de mapa de rutas. Las rutas redistribuidas hacia red OSPF deben tener una métrica semilla de 0 desde MLS1 y en MLS2 10, deben quedar como tipo E1
- Se ha solicitado que a nivel de OSPF se verifique los timers de los intervalos hello y dead, deben estar configurados a la mitad de su valor predeterminado.
- Se solicita verificar la autenticación de enlaces OSPF.
- Se ha solicitado detectar, resolver y documentar todos los problemas de la topología.

### Ticket 4 – PROTOCOLO BGP
- Los equipos de TI de todas las sucursales reportaron la pérdida de conectividad entre ellas, el jefe de operaciones solicita verificar la conexión hacia el Provider Edge de cada sucursal para determinar si el problema radica a nivel local y no escalar el problema al proveedor de servicios.
- Para una eficiencia en la red, el equipo de TI solicita que las interfaces loopback de Sucursal Norte se vean sumarizadas en toda la red.
- Se ha solicitado detectar, resolver y documentar todos los problemas de la topología.

### Ticket 5 – MPLS
- Tras haber solucionado los inconvenientes del ticket anterior, los equipos de TI, han reportado que aún no logran tener conectividad hacia las sucursales, por lo cual solicitan escalar el problema a los especialistas del backbone MPLS.
- Se reportaron cambios en algunas configuraciones del backbone MPLS, lo que provocó pérdidas de conexión entre las sucursales del cliente, además se reportaron cambios en los rangos de etiqueta utilizados en la red. Verificar según la siguiente tabla:

| Equipo | Rango     |
| ------ | --------- |
| PE1    | 100 - 199 |
| PE3    | 200 - 299 |
| PE4    | 300 - 399 |
| P5     | 500 - 599 |
| P6     | 600 - 699 |

### Ticket 6 – DMVPN
- Los usuarios de sucursales remotas reportaron que no tienen conectividad hacia sucursal sur, se le solicita verificar las configuraciones de router CE44 y los spoke RMT1 y RMT2.
- Además, personal de ciberseguridad de la empresa, realizó una auditoría de la red y señaló en su informe que la VPN entre las sucursales remotas y la sucursal sur no tienen ningún tipo de seguridad, por lo cual se requiere añadir IPsec para que todo el tráfico viaje de forma cifrada. Utilizar parámetros a elección.

## \*\*Requerimiento adicional
1. Para comprobar conectividad tomar un recorte de una traza de ruta desde Sucursal Central hacia Sucursal Norte y Sucursal Sur.
2. Evidenciar el cifrado entre Sucursales Remotas y Sucursal Sur.

# Configuracion
> [!NOTE]- Nota
> Estas configuraciones no estan completas debido a que nos quedamos sin tiempo
> Este examen no pide anotar los comandos "show" usados para identificar el problema

## Equipo: SW1
- Problema Detectado (Descripcion Breve): El switch1 está configurado para operar con el protocolo PVST en lugar de MST.

| Comando Aplicado                                                             |
| ---------------------------------------------------------------------------- |
| SW1(config)#no spanning-tree mode pvst<br>SW1(config)#spanning-tree mode mst |

---
- Problema Encontrado (Descripcion Breve): Interfaz ethernet1/1 y ethernet 1/3 no se declaro modo ni protocolo de negociacion de puertos (lacp)

| Comando Aplicado                                                                   |
| ---------------------------------------------------------------------------------- |
| SW1(config)#int range ethernet 1/0,3<br>SW1(config-if)#channel-group 2 mode active |

---
- Problema Encontrado (Descripcion Breve): Se activo port security en la interfaz ethernet0/0 y ethernet 0/1

| Comando Aplicado                                                                |
| ------------------------------------------------------------------------------- |
| SW1(config)#int range ethernet 0/0-1<br>SW1(config-if)#switchport port-security |

---
## Equipo: SW2
- Problema Encontrado (Descripcion Breve): No se encuentra configurada la interfaz e0/1 como acceso

| Comando Aplicado                                              |
| ------------------------------------------------------------- |
| SW2(config)#int e0/1<br>SW2(config-if)#switchport mode access |

---
- Problema Encontrado (Descripcion Breve): No se encuentra configurado la interfaz e0/0 como troncal

| Comando Aplicado                                                                                                    |
| ------------------------------------------------------------------------------------------------------------------- |
| SW2(config)#int e0/0<br>SW2(config-if)#switchport trunk encapsulation dot1q<br>SW2(config-if)#switchport mode trunk |

---
## Equipo: MLS1
- Problema Encontrado (Descripcion Breve): Interfaz port-channel 1 tiene encapsulacion incorrecta

| Comando Aplicado                                                                                                                                                                                                                            |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| MLS1(config)#interface port-channel 1<br>MLS1(config-if)#no switchport mode trunk<br>MLS1(config-if)#no switchport trunk encapsulation isl<br>MLS1(config-if)#switchport trunk encapsulation dot1q<br>MLS1(config-if)#switchport mode trunk |

---
- Problema Encontrado (Descripcion Breve): Las interfaces e0/3 y e1/2 tienen las interfaces en modo pasivo, lo que no permite generar adyacencia debido a que ambos extremos esperar la conexion

| Comando Aplicado                                                                                                                  |
| --------------------------------------------------------------------------------------------------------------------------------- |
| MLS1(config)#int range e0/3,1/2<br>MLS1(config-if)#no channel-group 1 mode passive<br>MLS1(config-if)#channel-group 1 mode active |

---
- Problema Encontrado (Descripcion Breve): IP de Standby Incorrecta para HSRP en la VLAN20, esta configurada 172.16.20.254 y deberia ser 172.16.20.1

| Comando Aplicado                                                                                                              |
| ----------------------------------------------------------------------------------------------------------------------------- |
| MLS1(config)#interface vlan 20<br>MLS1(config-if)#no standby 20 ip 172.16.20.254<br>MLS1(config-if)#standby 20 ip 172.16.20.1 |

---
- Problema Encontrado (Descripcion Breve): IP de Standby Incorrecta para HSRP en la instancia HSRP de VLAN10, esta configurada y deberia ser 172.16.20.1


| Comando Aplicado                                                                                                        |
| ----------------------------------------------------------------------------------------------------------------------- |
| MLS1(config)#interface vlan 10<br>MLS1(config-if)#no standby 11 priority 150<br>MLS1(config-if)#standby 10 priority 150 |

---
- Problema Encontrado (Descripcion Breve): Los tiempos de envío de paquetes no están configurados en la interfaces VLAN 10

| Comando Aplicado                                                                      |
| ------------------------------------------------------------------------------------- |
| MLS1(config)#interface vlan 10<br>MLS1(config-if)#standby 10 timers msec 200 msec 800 |

---
- Problema Encontrado (Descripcion Breve): Implementación de sistema de monitoreo a los enlaces ascendentes (ethernet 1/0 de mls1)

| Comando Aplicado                                                                                                                                                                                                                        |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| MLS1(config)#track 10 interface ethernet 1/0 line-protocol<br>MLS1(config-track)#delay down 5 up 5<br>MLS1(config-track)#ex<br>MLS1(config)#interface vlan 10<br>MLS1(config-if)#vrrp 10 track 10 decrement 71<br>MLS1(config-if)#no sh |

---
- Problema Encontrado (Descripcion Breve): EIGRP Nombrado tiene todas las interfaces pasivas 

| Comando Aplicado                                                                                                                                                                                                                                                        |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| MLS1(config)#router eigrp ENARSI<br>MLS1(config-router)#address-family ipv4 unicast autonomous-system 100<br>MLS1(config-router-af)#af-interface default<br>MLS1(config-router-af-interface)#no passive-interface<br>MLS1(config-router-af-interface)#exit-af-interface |

---
## Equipo: MLS2
- Problema Encontrado (Descripcion Breve): Las interfaces Ethernet1/1 y Ethernet 1/3 tiene configurado el modo _passive_ en el port-channel 2, al igual que las interfaces del otro extremo, lo que causa falta de conectividad debido a que ambos lados están esperando que el otro inicie la negociación.

| Comando Aplicado                                                                                                                   |
| ---------------------------------------------------------------------------------------------------------------------------------- |
| MLS2(config)#int range e1/1,e1/3<br>MLS2(config-if)#no channel-group 2 mode passive<br>MLS2(config-if)#channel-group 2 mode active |

---
- Problema Encontrado (Descripcion Breve): La interfaz VLAN 10 actualmente tiene la prioridad más alta, pero debería configurarse con una prioridad menor, ya que en este switch debe actuar como standby.

| Comando Aplicado                                                                                                       |
| ---------------------------------------------------------------------------------------------------------------------- |
| MLS2(config)#interface vlan 10<br>MLS2(config-if)#no standby 10 priority 150<br>MLS2(config-if)#standby 10 priority 80 |

---
- Problema Encontrado (Descripcion Breve): Implementación de sistema de monitoreo a los enlaces ascendentes(ethernet 1/0 de mls2)

| Comando Aplicado                                                                                                                                                                                               |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| MLS2(config)#track 20 interface ethernet 1/0 line-protocol<br>MLS2(config-track)#delay down 5 up 5<br>MLS2(config)#interface vlan 20<br>MLS2(config-if)#vrrp 20 track 20 decrement 71<br>MLS2(config-if)#no sh |

---
- Problema Encontrado (Descripcion Breve): EIGRP Nombrado: Falta agregar el port-channel 2 como interfaz activa (es decir, no pasiva)

| Comando Aplicado                                                                                                                                                                                                                                                               |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| MLS2(config)#router eigrp ENARSI<br>MLS2(config-router)#address-family ipv4 unicast autonomous-system 100<br>MLS2(config-router-af)#af-interface port-channel 2<br>MLS2(config-router-af-interface)#no passive-interface<br>MLS2(config-router-af-interface)#exit-af-interface |

---
## Equipo: CE11
- Problema Encontrado (Descripcion Breve): OSPF: Tiempos en la interfaz e0/0 no estan configurados con los valores correctos, deberian ser 20 para dead-interval y 5 para hello-interval

| Comando Aplicado                                                                                              |
| ------------------------------------------------------------------------------------------------------------- |
| CE11(config)#int e0/0<br>CE11(config-if)#ip ospf dead-interval 20<br>CE11(config-if)#ip ospf hello-interval 5 |

---
- Problema Encontrado (Descripcion Breve): OSPF: Autenticacion no esta configurada

| Comando Aplicado                                                                 |
| -------------------------------------------------------------------------------- |
| CE11(config)#int e0/1<br>CE11(config-if)#ip ospf authentication key-chain LLAVE1 |

---
## Equipo PE1
- Problema Encontrado (Descripcion Breve): BGP falta actualizar la ruta al vecino 123.3.3.3 a travez de la Loopback 0

| Comando Aplicado                                                                     |
| ------------------------------------------------------------------------------------ |
| PE1(config)#router bgp 123<br>PE1(config-router)#neighbor 123.3.3.3 update-source l0 |

---
- Problema Encontrado (Descripcion Breve): Falta configuracion de rango de etiquetas para LDP en el rango 100 - 199

| Comando Aplicado                     |
| ------------------------------------ |
| PE1(config)#mpls label range 100 199 |

---
- Problema Encontrado (Descripcion Breve): Route Distinguisher esta mal configurado con el valor 100:1, deberia ser 10001:1

| Comando Aplicado                                                                                                                                                                                                                              |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| PE1(config)#ip vrf SUCURSAL_CENTRAL<br>PE1(config-vrf)#no rd 100:1<br>PE1(config-vrf)#rd 10001:1<br>PE1(config-vrf)#route-target export 10001:1<br>PE1(config-vrf)#route-target import 14501:1<br>PE1(config-vrf)#route-target import 15401:1 |

---
## Equipo: P5
- Problema Encontrado (Descripcion Breve): No tiene OSPF Configurado

| Comando Aplicado                                                                                                                                                                                                                                                                                                                                                      |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| P5(config)#mpls ldp router-id l0<br>P5(config)#router ospf 1<br>P5(config-router)#mpls ldp autoconfig<br>P5(config-router)#router-id 123.5.5.5<br>P5(config-router)#network 123.5.5.5 0.0.0.0 area 0<br>P5(config-router)#network 10.1.1.10 0.0.0.0 area 0<br>P5(config-router)#network 10.1.1.2 0.0.0.0 area 0<br>P5(config-router)#network 10.1.1.13 0.0.0.0 area 0 |

---
- Problema Encontrado (Descripcion Breve): MPLS: Falta configuracion de rango de etiquetas para LDP en el rango 500 - 599

| Comando Aplicado                    |
| ----------------------------------- |
| P5(config)#mpls label range 500 599 |

---
## Equipo: PE3
- Problema Encontrado (Descripcion Breve): BGP RID Incorrecto, configurado 100.3.3.3 deberia ser 123.3.3.3

| Comando Aplicado                                                                                                          |
| ------------------------------------------------------------------------------------------------------------------------- |
| PE3(config)#router bgp 123<br>PE3(config-router)#no bgp router-id 100.3.3.3<br>PE3(config-router)#bgp router-id 123.3.3.3 |

---
- Problema Encontrado (Descripcion Breve): MPLS: VRF route-target incorrecto, rd 14501:1 como export no existe

| Comando Aplicado                                                                 |
| -------------------------------------------------------------------------------- |
| PE3(config)#ip vrf SUCURSAL_NORTE<br>PE3(config-vrf)#route-target export 14501:1 |

---
- Problema Encontrado (Descripcion Breve): MPLS: No tiene MPLS LDP Configurado para OSPF

| Comando Aplicado                                                                                         |
| -------------------------------------------------------------------------------------------------------- |
| PE3(config)#mpls ldp router-id l0<br>PE3(config)#router ospf 1<br>PE3(config-router)#mpls ldp autoconfig |

---
- Problema Encontrado (Descripcion Breve): Falta configuracion de rango de etiquetas para LDP en el rango 200 - 299

| Comando Aplicado                     |
| ------------------------------------ |
| PE3(config)#mpls label range 200 299 |

---
## Equipo: CE33
- Problema Encontrado (Descripcion Breve): BGP: Vecino BGP con AS Incorrecto, esta configurado 1234 y deberia ser 123

| Comando Aplicado                                                                                                                                      |
| ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| CE33(config)#router bgp 14501<br>CE33(config-router)#no neighbor 190.90.9.10 remote-as 1234<br>CE33(config-router)#neighbor 190.90.9.10 remote-as 123 |

---
- Problema Encontrado (Descripcion Breve): BGP: Loopback no se encuentran sumarizadas

| Comando Aplicado                                                                                             |
| ------------------------------------------------------------------------------------------------------------ |
| CE33(config)#router bgp 14501<br>CE33(config-router)#aggregate-address 120.0.32.0 255.255.248.0 summary-only |

---
## Equipo: P6
- Problema Encontrado (Descripcion Breve): MPLS: Falta configuracion de rango de etiquetas para LDP en el rango 600 - 699

| Comando Aplicado                    |
| ----------------------------------- |
| P6(config)#mpls label range 600 699 |

---
## Equipo: PE4
- Problema Encontrado (Descripcion Breve): MPLS: Falta configuracion de rango de etiquetas para LDP en el rango 300 - 399

| Comando Aplicado                     |
| ------------------------------------ |
| PE4(config)#mpls label range 300 399 |

---
## Equipo: CE44
- Problema Encontrado (Descripcion Breve): EIGRP: AS Incorrecto, esta configurado el valor 200, deberia ser valor 100

| Comando Aplicado                                                                                                                                                                                                                                                                 |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CE44(config)#no router eigrp 200<br>CE44(config)#router eigrp 100<br>CE44(config-router)# network 154.44.44.44 0.0.0.0<br>CE44(config-router)# network 200.10.0.0<br>CE44(config-router)# redistribute connected<br>CE44(config-router)# redistribute bgp 15401 metric 1 1 1 1 1 |

---
- Problema Encontrado (Descripcion Breve): Conectividad Basica: Interfaz s1/0 se encuentra apagada, se debe encender

| Comando Aplicado                                 |
| ------------------------------------------------ |
| CE44(config)#int s1/0<br>CE44(config-if)#no shut |

---
- Problema Encontrado (Descripcion Breve): DMVPN: Interfaz Source e0/0 incorrecta, deberia ser e0/1 , ademas de configurar no split horizon

| Comando Aplicado                                                                                                                                           |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CE44(config)#int tunnel0  <br>CE44(config-if)#no tunnel source e0/0<br>CE44(config-if)#tunnel source e0/1<br>CE44(config-if)#no ip split-horizon eigrp 100 |

---
- Problema Encontrado (Descripcion Breve): DMVPN: Falta configurar IPSEC para poder asegurar el tunnel

| Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| CE44(config-if)#crypto isakmp policy 10<br>CE44(config-isakmp)#encryption 3des<br>CE44(config-isakmp)#authentication pre-share<br>CE44(config-isakmp)#group 5<br>CE44(config-isakmp)#lifetime 3600<br>CE44(config-isakmp)#exit<br>CE44(config)#crypto isakmp key ET address 0.0.0.0<br>CE44(config)#crypto ipsec transform-set PC ah-sha-hmac esp-3des<br>CE44(cfg-crypto-trans)#mode transport<br>CE44(cfg-crypto-trans)#exit<br>CE44(config)#crypto ipsec profile DMVPN<br>CE44(ipsec-profile)#set transform-set PC<br>CE44(ipsec-profile)#int tunnel 0<br>CE44(config-if)#tunnel protection ipsec profile DMVPN |

---
## Equipo: PC-SUR
- Problema Encontrado (Descripcion Breve): Interfaz DHCP Incorrecta, configurada e0/1, deberia ser e0/0, y esta se encuentra apagada, se debe encender

| Comando Aplicado                                                                                                                                                                    |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| PC-SUR(config)#int e0/1<br>PC-SUR(config-if)#no ip address<br>PC-SUR(config-if)#exit<br>PC-SUR(config)#int e0/0<br>PC-SUR(config-if)#ip address dhcp  <br>PC-SUR(config-if)#no shut |

---
- Problema Encontrado (Descripcion Breve): Ruta Estatica configurada con IP de salida incorrecta, configurada e0/1, deberia ser e0/0

| Comando Aplicado                                                                                              |
| ------------------------------------------------------------------------------------------------------------- |
| PC-SUR(config)#no ip route 0.0.0.0 0.0.0.0 Ethernet0/1<br>PC-SUR(config)#ip route 0.0.0.0 0.0.0.0 Ethernet0/0 |

---
## Equipo: RMT1
- Problema Encontrado (Descripcion Breve): NHRP Map Multicast tiene ip mal configurada

| Comando Aplicado                                                                                                                   |
| ---------------------------------------------------------------------------------------------------------------------------------- |
| RMT1(config)#int tun0<br>RMT1(config-if)#no ip nhrp map multicast 190.90.9.13<br>RMT1(config-if)#ip nhrp map multicast 188.55.30.1 |

---
- Problema Encontrado (Descripcion Breve): Falta configurar IPSEC para poder asegurar el tunnel

| Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RMT1(config)#crypto isakmp policy 10<br>RMT1(config-isakmp)#encryption 3des<br>RMT1(config-isakmp)#authentication pre-share<br>RMT1(config-isakmp)#group 5<br>RMT1(config-isakmp)#lifetime 3600<br>RMT1(config-isakmp)#exit<br>RMT1(config)#crypto isakmp key ET address 0.0.0.0<br>RMT1(config)#crypto ipsec transform-set PC ah-sha-hmac esp-3des<br>RMT1(cfg-crypto-trans)#mode transport<br>RMT1(cfg-crypto-trans)#exit<br>RMT1(config)#crypto ipsec profile DMVPN<br>RMT1(ipsec-profile)#set transform-set PC<br>RMT1(ipsec-profile)#int tunnel 0<br>RMT1(config-if)#tunnel protection ipsec profile DMVPN |

---
## Equipo: RMT2
- Problema Encontrado (Descripcion Breve): Interfaz e0/2 apagada, deberia estar encendida para que el tunnel pueda ser enrutado hacia internet

| Comando Aplicado                                 |
| ------------------------------------------------ |
| RMT2(config)#int e0/2<br>RMT2(config-if)#no shut |

---
- Problema Encontrado (Descripcion Breve): Network ID incorrecto, esta configurado con el valor 10, deberia ser 1

| Comando Aplicado                                                                                          |
| --------------------------------------------------------------------------------------------------------- |
| RMT2(config)#int Tun0<br>RMT2(config-if)#no ip nhrp network-id 10<br>RMT2(config-if)#ip nhrp network-id 1 |

---
- Problema Encontrado (Descripcion Breve): Falta configurar IPSEC para poder asegurar el tunnel

| Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RMT2(config)#crypto isakmp policy 10<br>RMT2(config-isakmp)#encryption 3des<br>RMT2(config-isakmp)#authentication pre-share<br>RMT2(config-isakmp)#group 5<br>RMT2(config-isakmp)#lifetime 3600<br>RMT2(config-isakmp)#exit<br>RMT2(config)#crypto isakmp key ET address 0.0.0.0<br>RMT2(config)#crypto ipsec transform-set PC ah-sha-hmac esp-3des<br>RMT2(cfg-crypto-trans)#mode transport<br>RMT2(cfg-crypto-trans)#exit<br>RMT2(config)#crypto ipsec profile DMVPN<br>RMT2(ipsec-profile)#set transform-set PC<br>RMT2(ipsec-profile)#int tunnel 0<br>RMT2(config-if)#tunnel protection ipsec profile DMVPN |

---
## Prueba de Conectividad
Solo logre 1/2 pruebas, que fue el funcionamiento de DMVPN entre Spokes

![1000](https://slink.proxylivy.work/image/d277ba26-b0cf-476e-944c-db5a56624810.png)