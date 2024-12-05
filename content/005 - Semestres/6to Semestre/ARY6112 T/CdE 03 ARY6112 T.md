# Info
## Imagen Topologia
![1000](https://slink.proxylivy.work/image/c465592e-6871-425b-b80c-b59c345452e8.png)
## Contexto
Usted trabaja en la empresa “CASO 3 LTDA.”, la cual se encarga de prestar servicios de artículos de peras y plantaciones de perales en el país. El éxito en los últimos años ha sido tan grande, que la empresa ha tenido un crecimiento explosivo. Por tal motivo, usted como especialista de redes, deberá revisar el funcionamiento de esta red para que pueda quedar en un óptimo funcionamiento y eficiencia.
## Requerimientos
### A. Ticket 1: PPP Multilink
- La empresa ha solicitado revisar el funcionamiento de los PPP - [[010 - Protocolos/010.1 - Routing/Multilink|Multilink]] que se encuentran conectados entre dos sucursales y el backbone [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP|EIGRP]]. **No se solicita autenticación.**
- Realice las configuraciones pertinentes para solucionar los problemas.
- Debe documentar todos los errores encontrados en la hoja de documentación, señalando de forma correcta lo solicitado.

### B. Ticket 2: Capa 2
- La empresa tiene varios [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]] repartidos en varias sucursales. Es importante poder revisar el funcionamiento, realizar correcciones y realizar mejoras. Entre las cuales, se incluye que debe existir [[010 - Protocolos/010.2 - Switching/Seguridad de Puerto|Seguridad de Puerto]] para un máximo de 3 host. En el caso que se viola la seguridad, la interface se debe apagar.
- Realice las configuraciones pertinentes para solucionar los problemas.
- Debe documentar todos los errores encontrados en la hoja de documentación, señalando de forma correcta lo solicitado.

### C. Ticket 3: Enrutamiento IGP
- La empresa le ha solicitado revisar el funcionamiento de los protocolos IGP. Según informaciones requeridas, los temporizadores son por defecto.
- Las redistribuciones deben ser mediante mapas de rutas. | [[020 - Conceptos/020.1 - Administracion/Route-Map|Route-Map]]
- Se ha solicitado, que el backbone EIGRP sea configurado con autenticación mediante SHA-256. | [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP#Autenticacion|EIGRP]]
- Realice las configuraciones pertinentes para solucionar los problemas.
- Debe documentar todos los errores encontrados en la hoja de documentación, señalando de forma correcta lo solicitado.

### D. Ticket 4: Enrutamiento EGP
- Se ha solicitado revisar el funcionamiento de protocolo [[020 - Conceptos/020.3 - Fundamentos/EGP|EGP]] en la red. Toda sesión será mediante IP de siguiente salto. Los hosts deben participar como prefijos.
- Se ha solicitado que ISP, pueda utilizar a R13 (como PATH preferido) para esto asegurar influencia mediante route-maps. | [[020 - Conceptos/020.1 - Administracion/Route-Map|Route-Map]]
- Realice las configuraciones pertinentes para solucionar los problemas.
- Debe documentar todo

### E. Ticket 5: MPLS
- La empresa tiene implementado un backbone [[010 - Protocolos/010.1 - Routing/MPLS|MPLS]]. Se ha solicitado verificar la generación de etiquetas dentro de esta ubicación.
- Compruebe la generación de etiquetas, donde se solicita, cambiar el rango para un seguimiento más específico.
- Realice las configuraciones pertinentes para solucionar los problemas.
- Debe documentar todos los errores encontrados en la hoja de documentación, señalando de forma correcta lo solicitado.

### F. Ticket 6: Enrutamiento EGP (VPN MPLS)
- La empresa desea implementar una [[010 - Protocolos/010.1 - Routing/MPLS#Implementar MP-BGP y Montar VPNv4|VPNv4]] entre sucursales Pera Menor 1 y Pera Menor 2. Por tal motivo, es importante realizar las configuraciones pertinentes, para que ambas sucursales puedan comunicarse mediante esta forma, a través del backbone [[010 - Protocolos/010.1 - Routing/MPLS|MPLS]].
- Realice las configuraciones pertinentes para solucionar los problemas.
- Debe documentar todos los errores encontrados en la hoja de documentación, señalando de forma correcta lo solicitado.

### G. Ticket 7: DMVPN
- Implementar solución de [[999 - Archivado/DMVPN|DMVPN]] de Fase 2, entre Sucursales Perita 1 y Perita 2, hacia "Sucursal Pera Mayor". Es importante que las conexiones se encuentren cifradas.
- Compruebe el funcionamiento de túnel y que exista conectividad entre las sucursales mediante el túnel.
- Debe documentar todos los errores encontrados en la hoja de documentación, señalando de forma correcta lo solicitado.

### H. Ticket 8: Conectividad Equipos Finales
- Permitir que los equipos finales de las sucursales, puedan llegar al ISP, mediante IP pública (debe enviar captura de pantalla)
- Los equipos deben poder llegar a `www.caso3.cl` (`8.8.8.8`) (debe enviar captura de pantalla)
- Debe documentar todos los errores encontrados en la hoja de documentación, señalando de forma correcta lo solicitado.

# Configuracion
## Equipo: CASO3

- Problema Detectado (Descripcion Breve): Le falta configuracion iBGP con ISP

| Comando para encontrar el problema       | Comando Aplicado                                                                        |
| ---------------------------------------- | --------------------------------------------------------------------------------------- |
| show ip bgp summary<br>show ip protocols | CASO3(config)#router bgp 10000<br>CASO3(config-router)#neighbor 8.8.8.1 remote-as 10000 |

---
## Equipo: ISP

- Problema Detectado (Descripcion Breve): Falta Vecindad BGP con CASO3 y compartir rutas

| Comando para encontrar el problema       | Comando Aplicado                                                                                                                             |
| ---------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip bgp summary<br>show ip protocols | ISP(config)#router bgp 10000<br>ISP(config-router)#neighbor 8.8.8.8 remote-as 10000<br>ISP(config-router)#network 8.8.8.0 mask 255.255.255.0 |

---
- Problema Encontrado (Descripcion Breve): Falta IPv6 hacia R13 en interfaz e0/0

| Comando para encontrar el problema | Comando Aplicado                                                            |
| ---------------------------------- | --------------------------------------------------------------------------- |
| show ipv6 int brief                | ISP(config)#int e0/0<br>ISP(config-if)#ipv6 address 2019:ACAD:ACAD:3::2/112 |

---
## Equipo: R12

- Problema Detectado (Descripcion Breve): Interfaces NAT Incorrectas

| Comando para encontrar el problema | Comando Aplicado                                                                                             |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| sh run int e0/0<br>sh run int e0/1 | R12(config)#int range e0/0-1<br>R12(config-if-range)#no ip nat outside<br>R12(config-if-range)#ip nat inside |

---
- Problema Encontrado (Descripcion Breve): No tiene configurado OSPF

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                       |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols<br>show ip ospf  | R12(config)#router ospf 1<br>R12(config-router)#router-id 12.12.12.12<br>R12(config-router)#network 172.16.102.12 0.0.0.0 area 1<br>R12(config-router)#network 172.16.106.12 0.0.0.0 area 1<br>R12(config-router)#exit |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---
## Equipo: R13

- Problema Detectado (Descripcion Breve): IPv6 configurada en e0/1, deberia estar en e0/0

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                     |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ipv6 int brief                | R13(config)#int e0/1<br>R13(config-if)#no ipv6 add<br>R13(config-if)#exit<br>R13(config)#int e0/0<br>R13(config-if)#ipv6 add 2019:ACAD:ACAD:3::1/112 |

---
- Problema Encontrado (Descripcion Breve): OSPF No configurado / Interfaz e0/3 temporizador personalizado habilitado

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                 |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show ip protocols<br>show ip ospf  | R13(config)#router ospf 1<br>R13(config-router)#router-id 13.13.13.13<br>R13(config-router)#network 172.16.106.13 0.0.0.0 area 1<br>R13(config-router)#network 172.16.103.13 0.0.0.0 area 1<br>R13(config-router)#exit<br>R13(config)#int e0/3<br>R13(config-if)#no ip ospf hello-interval 9<br> |

---
- Problema Encontrado (Descripcion Breve): NAT Deniega todo el trafico, deberia permitir

| Comando para encontrar el problema        | Comando Aplicado                                                                                                            |
| ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| show access-lists<br>show run \| s ip nat | R13(config)#ip access-list standard INTERNET<br>R13(config-std-nacl)#no deny 0.0.0.0<br>R13(config-std-nacl)#permit 0.0.0.0 |

---
- Problema Detectado (Descripcion Breve): No tiene BGP Funcionando

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                    |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols<br>show ip bgp   | R13(config)#router bgp 12345<br>R13(config-router)#neighbor 201.0.0.2 remote-as 10000<br>R13(config-router)#neighbor 172.16.106.12 remote-as 12345<br>R13(config-router)#neighbor 172.16.103.11 remote-as 12345<br> |

---
- Problema Encontrado (Descripcion Breve): NAT: Interfaz Overload incorrecta

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                             |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show run                           | R13(config)#ip nat inside source list INTERNET interface Ethernet0/3 overload<br><br>Dynamic mapping in use, do you want to delete all entries? \[no\]: yes<br>R13(config)#ip nat inside source list INTERNET interface Ethernet0/0 overload |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---
## Equipo: R10

- Problema Detectado (Descripcion Breve): OSPF: Temporizador activado en interfaz e0/1, no deberia estar

| Comando para encontrar el problema | Comando Aplicado                                                    |
| ---------------------------------- | ------------------------------------------------------------------- |
| show ip ospf                       | R10(config)#int e0/1<br>R10(config-if)#no ip ospf hello-interval 12 |

---
- Problema Encontrado (Descripcion Breve): OSPF: Area Incorrecta en la red 172.16.192.10

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
| show ip protocols                  |                  |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                                                     |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show ip bgp                        | R10(config)#router bgp 12345<br>R10(config-router)#neighbor 172.16.102.12 remote-as 12345<br>R10(config-router)#neighbor 172.16.102.12 update<br>R10(config-router)#neighbor 172.16.102.12 update-source l0<br>R10(config-router)#neighbor 172.16.100.9 remote-as 12345<br>R10(config-router)#neighbor 172.16.100.9 update-source l0 |

---
## Equipo: R11

- Problema Detectado (Descripcion Breve): Interfaz Loopback 0 sin IP

| Comando para encontrar el problema | Comando Aplicado                                                        |
| ---------------------------------- | ----------------------------------------------------------------------- |
| show ip int brief                  | R11(config)#int l0<br>R11(config-if)#ip add 11.11.11.11 255.255.255.255 |

---
- Problema Encontrado (Descripcion Breve): OSPF: Area incorrecta para red 172.16.103.11

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                    |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | R11(config)#router ospf 1<br>R11(config-router)#no network 172.16.103.11 0.0.0.0 area 10<br>R11(config-router)#network 172.16.103.11 0.0.0.0 area 1 |

---
- Problema Encontrado (Descripcion Breve): No tiene configurado BGP

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                 |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip bgp                        | R11(config)#router bgp 12345<br>R11(config-router)#neighbor 172.16.101.9 remote-as 12345<br>R11(config-router)#neighbor 172.16.101.9 update-source l0<br>R11(config-router)#neighbor 172.16.103.13 remote-as 12345<br>R11(config-router)#neighbor 172.16.103.13 update-source l0 |

---
- Problema Detectado (Descripcion Breve): VPNv4 BGP

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
| show                               |                  |

---
- Problema Encontrado (Descripcion Breve): VRF Ruta importadas incorrectas

| Comando para encontrar el problema | Comando Aplicado                                                                                                                     |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| show vrf detail                    | R11(config)#ip vrf SUCURSAL-PERA2<br>R11(config-vrf)#no route-target import 65000:17<br>R11(config-vrf)#route-target import 65001:17 |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---
## Equipo: R9

- Problema Detectado (Descripcion Breve): OSPF: Area incorrecta para la red 172.16.89.9, deberia ser area 0

| Comando para encontrar el problema | Comando Aplicado                                                                                                                             |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | R9(config)#router ospf 1<br>R9(config-router)#no network 172.16.89.9 0.0.0.0 area 10<br>R9(config-router)#network 172.16.89.9 0.0.0.0 area 0 |

---
- Problema Encontrado (Descripcion Breve): Lista ACL aplicando en OSPF que no deberia existir

| Comando para encontrar el problema                                 | Comando Aplicado                                                                                                                       |
| ------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| show access-list<br>show run \| s access<br>show run \| s ospf<br> | R9(config)#no ip access-list standard OPTIMIZACION<br>R9(config)#router ospf 1<br>R9(config-router)#no distribute-list OPTIMIZACION in |

---
- Problema Encontrado (Descripcion Breve): OSPF: IP red incorrecta 172.16.68.6, deberia ser 172.16.68.8

| Comando para encontrar el problema | Comando Aplicado                                                                                                                            |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | R8(config)#router ospf 1<br>R8(config-router)#no network 172.16.68.6 0.0.0.0 area 0<br>R8(config-router)#network 172.16.68.8 0.0.0.0 area 0 |

---
- Problema Detectado (Descripcion Breve): BGP: No esta configurado

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                              |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip bgp                        | R9(config)#router bgp 12345<br>R9(config-router)#neighbor 172.16.89.8 remote-as 12345<br>R9(config-router)#neighbor 172.16.100.10 remote-as 12345<br>R9(config-router)#neighbor 172.16.101.11 remote-as 12345 |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---

## Equipo: R7

- Problema Detectado (Descripcion Breve): Rutas importadas estan incorrectas

| Comando para encontrar el problema | Comando Aplicado                                                                                                                 |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| show vrf brief                     | R7(config)#ip vrf SUCURSAL-PERA1<br>R7(config-vrf)#no route-target import 65002:8<br>R7(config-vrf)#route-target import 65002:16 |

---
- Problema Encontrado (Descripcion Breve): OSPF: ACL No deberia existir

| Comando para encontrar el problema | Comando Aplicado                                                                                                                       |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| show access-lists<br>              | R7(config)#no ip access-list standard OPTIMIZACION<br>R7(config)#router ospf 1<br>R7(config-router)#no distribute-list OPTIMIZACION in |

---
- Problema Encontrado (Descripcion Breve): BGP: Faltan Redes de vecinos

| Comando para encontrar el problema       | Comando Aplicado                                                                                                                               |
| ---------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip bgp summary<br>show ip protocols | R7(config)#router bgp 12345<br>R7(config-router)#neighbor 172.16.67.8 remote-as 12345<br>R7(config-router)#neighbor 172.16.56.5 remote-as 2345 |

---
## Equipo: R8
- Problema Detectado (Descripcion Breve): BGP: No esta configurado

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                          |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip bgp                        | R8(config)#router bgp 12345<br>R8(config-router)#neighbor 172.16.89.9 remote-as 12345<br>R8(config-router)#neighbor 172.16.67.7 remote-as 12345<br>R8(config-router)#neighbor 172.16.68.6 remote-as 12345 |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---
## Equipo: R6
- Problema Detectado (Descripcion Breve): Loopback 0 no esta configurada

| Comando para encontrar el problema | Comando Aplicado                                                                                   |
| ---------------------------------- | -------------------------------------------------------------------------------------------------- |
| show ip int brief                  | R6(config)#int loopback 0<br>R6(config-if)#ip add 6.6.6.6 255.255.255.255<br>R6(config-if)#no shut |

---
- Problema Encontrado (Descripcion Breve): Interfaz e0/0 apagada

| Comando para encontrar el problema | Comando Aplicado                             |
| ---------------------------------- | -------------------------------------------- |
| show ip int brief                  | R6(config)#int e0/0<br>R6(config-if)#no shut |

---
- Problema Encontrado (Descripcion Breve): BGP: no esta configurado

| Comando para encontrar el problema | Comando Aplicado                                                                                                                               |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip bgp                        | R6(config)#router bgp 12345<br>R6(config-router)#neighbor 172.16.68.8 remote-as 12345<br>R6(config-router)#neighbor 172.16.56.5 remote-as 2345 |

---
## Equipo: R5
- Problema Detectado (Descripcion Breve): EIGRP: Configuracion Nombrada con autenticacion inexistente

| Comando para encontrar el problema              | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show ip protocols<br>show run \| s router eigrp | R5(config)#no router eigrp 200<br>R5(config)#key chain caso3<br>R5(config-keychain)#key 1<br>R5(config-keychain-key)#key-string caso3<br>R5(config-keychain-key)#exit<br>R5(config-keychain)#exit<br>R5(config)#router eigrp TSHOOT<br>R5(config-router)#address-family ipv4 autonomous-system 200<br>R5(config-router-af)#network 10.10.35.5 0.0.0.0<br>R5(config-router-af)#network 10.10.45.5 0.0.0.0<br>R5(config-router-af)#af-interface e0/1<br>R5(config-router-af-interface)#authentication key-chain caso3<br>R5(config-router-af-interface)#authentication mode hmac-sha-256 caso3<br>R5(config-router-af)#af-interface e0/2<br>R5(config-router-af-interface)#authentication key-chain caso3<br>R5(config-router-af-interface)#authentication mode hmac-sha-256 caso3<br> |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---
## Equipo: R4
- Problema Detectado (Descripcion Breve): EIGRP: Configuracion Nombrada Autenticada Inexistente

| Comando para encontrar el problema              | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ----------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols<br>show run \| s router eigrp | R4(config)#no router eigrp 200<br>R4(config)#key chain caso3<br>R4(config-keychain)#key 1<br>R4(config-keychain-key)#key-string caso3<br>R4(config-keychain-key)#exit<br>R4(config-keychain)#exit<br>R4(config)#router eigrp TSHOOT<br>R4(config-router)#address-family ipv4 autonomous-system 200<br>R4(config-router-af)#network 10.10.45.4 0.0.0.0<br>R4(config-router-af)#network 10.10.24.4 0.0.0.0<br>R4(config-router-af)#network 10.10.184.2 0.0.0.0<br>R4(config-router-af)#af-interface e0/2<br>R4(config-router-af-interface)#authentication key-chain caso3<br>R4(config-router-af-interface)#authentication mode hmac-sha-256 caso3<br>R4(config-router-af-interface)#exit<br>R4(config-router-af)#af-interface e0/3<br>R4(config-router-af-interface)#authentication key-chain caso3<br>R4(config-router-af-interface)#authentication mode hmac-sha-256 caso3 |

---
- Problema Encontrado (Descripcion Breve): Interfaz Multilink 2 apagado

| Comando para encontrar el problema | Comando Aplicado                                    |
| ---------------------------------- | --------------------------------------------------- |
| show ppp all                       | R4(config)#int multilink 2<br>R4(config-if)#no shut |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---
## Equipo: R2
- Problema Detectado (Descripcion Breve): ACL OPTIMIZACION No deberia existir

| Comando para encontrar el problema | Comando Aplicado                                   |
| ---------------------------------- | -------------------------------------------------- |
| show access-lists                  | R2(config)#no ip access-list standard OPTIMIZACION |

---
- Problema Encontrado (Descripcion Breve): EIGRP Nombrado Autenticacion SHA 256 no Existe

| Comando para encontrar el problema              | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ----------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols<br>show run \| s router eigrp | R2(config)#no router eigrp 200<br>R2(config)#key chain caso3<br>R2(config-keychain)#key 1<br>R2(config-keychain-key)#key-string caso3<br>R2(config-keychain-key)#exit<br>R2(config-keychain)#exit<br>R2(config)#router eigrp TSHOOT<br>R2(config-router)#address-family ipv4 autonomous-system 200<br>R2(config-router-af)#network 10.10.24.2 0.0.0.0<br>R2(config-router-af)#network 10.10.23.2 0.0.0.0<br>R2(config-router-af)#network 10.10.12.2 0.0.0.0<br>R2(config-router-af)#af-interface e0/0<br>R2(config-router-af-interface)#authentication key-chain caso3<br>R2(config-router-af-interface)#authentication mode hmac-sha-256 caso3<br>R2(config-router-af-interface)#exit<br>R2(config-router-af)#af-interface e0/3<br>R2(config-router-af-interface)#authentication key-chain caso3<br>R2(config-router-af-interface)#authentication mode hmac-sha-256 caso3 |

---
- Problema Encontrado (Descripcion Breve): Multilink con la ip incorrecta y apagafo

| Comando para encontrar el problema | Comando Aplicado                                                                                                                      |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| show ppp all                       | R2(config)#int multilink 1<br>R2(config-if)#no ip add<br>R2(config-if)#ip address 10.10.12.2 255.255.255.252<br>R2(config-if)#no shut |

---
- Problema Detectado (Descripcion Breve): EIGRP: Falta red Multilink

| Comando para encontrar el problema | Comando Aplicado                                                            |
| ---------------------------------- | --------------------------------------------------------------------------- |
| show ip protocols                  | R2(config)#router eigrp 200<br>R2(config-router)#network 10.10.12.2 0.0.0.0 |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---
## Equipo: R1
- Problema Detectado (Descripcion Breve): Multilink Mascara incorrecta

| Comando para encontrar el problema | Comando Aplicado                                                                                                 |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| show ppp all<br>show ip int mul 1  | R1(config)#int multilink 1<br>R1(config-if)#no ip address<br>R1(config-if)#ip address 10.10.12.1 255.255.255.252 |

---
- Problema Encontrado (Descripcion Breve): Interfaz s1/0 apagada

| Comando para encontrar el problema | Comando Aplicado                             |
| ---------------------------------- | -------------------------------------------- |
| show ip int brief<br>show ppp all  | R1(config)#int s1/0<br>R1(config-if)#no shut |

---
- Problema Encontrado (Descripcion Breve): EIGRP: Protocolo apagado y le falta la red multilink

| Comando para encontrar el problema | Comando Aplicado                                                                                         |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------- |
| show run \| s router eigrp         | R1(config)#router eigrp 200<br>R1(config-router)#no shut<br>R1(config-router)#network 10.10.12.1 0.0.0.0 |

---
## Equipo: R18

- Problema Detectado (Descripcion Breve): Interfaz s1/1 apagada para multilink

| Comando para encontrar el problema | Comando Aplicado                               |
| ---------------------------------- | ---------------------------------------------- |
| show ppp all                       | R18(config)#int s1/1<br>R18(config-if)#no shut |

---
- Problema Encontrado (Descripcion Breve): Multilink 2 con ip incorrecta

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                           |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ppp all<br>show ip int brief  | R18(config)#int multilink 2<br>R18(config-if)#no ip address 10.10.184.2 255.255.255.252<br>R18(config-if)#ip address 10.10.184.1 255.255.255.252<br>R18(config-if)#no shut |

---
- Problema Encontrado (Descripcion Breve): EIGRP: No esta configurado

| Comando para encontrar el problema | Comando Aplicado                                                                                                                |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | R18(config)#router eigrp 200<br>R18(config-router)#network 10.10.184.1 0.0.0.0<br>R18(config-router)#network 10.10.18.1 0.0.0.0 |

---
## Equipo: R3

- Problema Detectado (Descripcion Breve): Interfaz e1/0 apagada

| Comando para encontrar el problema | Comando Aplicado                             |
| ---------------------------------- | -------------------------------------------- |
| show ip int brief                  | R3(config)#int e1/0<br>R3(config-if)#no shut |

---
- Problema Encontrado (Descripcion Breve): BGP: No esta configurado

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                 |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip bgp                        | R3(config)#router bgp 2345<br>R3(config-router)#neighbor 10.10.35.5 remote-as 2345<br>R3(config-router)#neighbor 10.10.23.2 remote-as 2345<br>R3(config-router)#neighbor 180.180.181.5 remote-as 1415<br>R3(config-router)#neighbor 180.180.180.4 remote-as 1415 |

---
- Problema Encontrado (Descripcion Breve): EIGRP Autenticando con SHA-256

| Comando para encontrar el problema              | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ----------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols<br>show run \| s router eigrp | R3(config)#no router eigrp 200<br>R3(config)#key chain caso3<br>R3(config-keychain)#key 1<br>R3(config-keychain-key)#key-string caso3<br>R3(config-keychain-key)#exit<br>R3(config-keychain)#exit<br>R3(config)#router eigrp TSHOOT<br>R3(config-router)#address-family ipv4 autonomous-system 200<br>R3(config-router-af)#network 10.10.35.3 0.0.0.0<br>R3(config-router-af)#network 10.10.23.3 0.0.0.0<br>R3(config-router-af)#af-interface e0/1<br>R3(config-router-af-interface)#authentication key-chain caso3<br>R3(config-router-af-interface)#authentication mode hmac-sha-256 caso3<br>R3(config-router-af-interface)#exit<br>R3(config-router-af)#af-interface e0/0<br>R3(config-router-af-interface)#authentication key-chain caso3<br>R3(config-router-af-interface)#authentication mode hmac-sha-256 caso3 |

---
## Equipo: R16
- Problema Detectado (Descripcion Breve): VRF no estan creados

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---
## Equipo: R15
- Problema Detectado (Descripcion Breve): DHCP: Todas las ip estan excluidas

| Comando para encontrar el problema | Comando Aplicado                                                    |
| ---------------------------------- | ------------------------------------------------------------------- |
| show run \| ip dhcp                | R15(config)#no ip dhcp excluded-address 192.168.10.1 192.168.20.254 |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---
## Equipo: SW2
- Problema Detectado (Descripcion Breve): Mascara Incorrecta Interfaz e0/0

| Comando para encontrar el problema    | Comando Aplicado                                                                                            |
| ------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| show ip int brief<br>show ip int e0/0 | SW2(config)#int e0/0<br>SW2(config-if)#no ip add<br>SW2(config-if)#ip address 192.168.104.2 255.255.255.252 |

---
- Problema Encontrado (Descripcion Breve): VLAN Troncales no pueden pasar al otro switch mediante e0/2

| Comando para encontrar el problema | Comando Aplicado                                                           |
| ---------------------------------- | -------------------------------------------------------------------------- |
| show int trunk                     | SW2(config)#int e0/2<br>SW2(config-if)#switchport trunk allowed vlan 10,20 |

---
- Problema Encontrado (Descripcion Breve): Interfaz e0/3 Apagada y no tiene configurada la seguridad de puerto

| Comando para encontrar el problema   | Comando Aplicado                                                                                                                                                                                                                                                                                         |
| ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip int brief<br>show int status | SW2(config)#int e0/3<br>SW2(config-if)#no switchport port-security mac-address aaaa.bbbb.cccc<br>SW2(config-if)#switchport port-security violation shutdown<br>SW2(config-if)#switchport port-security maximum 3<br>SW2(config-if)#switchport port-security mac-address sticky<br>SW2(config-if)#no shut |

---
- Problema Detectado (Descripcion Breve): DHCP: No tiene IP-Helper ni IP

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                            |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip int brief                  | SW2(config)#int vlan 20<br>SW2(config-if)#ip helper-address 192.168.104.1<br>SW2(config-if)#ip address 192.168.20.1 255.255.255.0<br>SW2(config-if)#no shut |

---
- Problema Encontrado (Descripcion Breve): Red VLAN 20 falta en EIGRP y BGP

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                     |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show ip protocols                  | SW2(config)#router eigrp 100<br>SW2(config-router)#network 192.168.20.1 0.0.0.0<br>SW2(config-router)#exit<br>SW2(config)#router bgp 1415<br>SW2(config-router)#network 192.168.20.0 |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---
## Equipo: SW1
- Problema Detectado (Descripcion Breve): EIGRP: AS Incorrecto

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                         |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show ip protocols                  | SW1(config)#no router eigrp 1<br>SW1(config)#router eigrp 100<br>SW1(config-router)#network 192.168.10.1 0.0.0.0<br>SW1(config-router)#network 192.168.100.2 0.0.0.0<br>SW1(config-router)#network 192.168.101.2 0.0.0.0 |

---
- Problema Encontrado (Descripcion Breve): Falta configuracion VLAN 10 con IP y Helper Address

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                            |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show run                           | SW1(config)#int vlan 10<br>SW1(config-if)#ip address 192.168.10.1 255.255.255.0<br>SW1(config-if)#ip helper-address 192.168.101.1<br>SW1(config-if)#no shut |

---
- Problema Encontrado (Descripcion Breve): Enlace Troncal mal configurado

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show int status                    | SW1(config)#int e0/2<br>SW1(config-if)#no switchport trunk allowed vlan 30<br>SW1(config-if)#switchport trunk allowed vlan 10,20<br>SW1(config-if)#switchport trunk encapsulation dot1q<br>SW1(config-if)#switchport mode trunk |

---
- Problema Detectado (Descripcion Breve): Puerto de Acceso VLAN 10 le falta seguridad de puerto y sobra configuracion de mecanismos de estabilizacion

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                                                                           |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show int status<br>show run        | SW1(config)#int e0/3<br>SW1(config-if)#no spanning-tree portfast<br>SW1(config-if)#no spanning-tree bpduguard<br>SW1(config-if)#no switchport port-security mac-address aaaa.0bbb.cccc<br>SW1(config-if)#switchport port-security maximum 3<br>SW1(config-if)#switchport port-security mac-address sticky<br>SW1(config-if)#shut<br>SW1(config-if)#no shut |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---
