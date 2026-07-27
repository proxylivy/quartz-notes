# Info
## Imagen Topologia
![1000](https://slink.proxylivy.work/image/805caeb8-9644-463a-97ca-68a914f2ca68.png)
## Sobre
Implementación de Servicios de Service Provider en Redes Corporativas
## Requerimientos
- Crear microsegementación de dominios de broadcast, según lo propuesto en la topología.
- A nivel de interfaz de acceso configurar seguridad de puerto dinámica, además de que los equipos eventualmente solo podrán solicitar un máximo de 2 IP, además de tener implementado mecanismos de estabilización de STP.
- A nivel de troncales, este no deberá tener negociación automática.
- En los routers PE crear dos VRF, con nombres a elección.
- Asociar las interfaces a las VRF solicitadas.
- Asignar direccionamiento IP.
- Implementar EVN, asignando número de TAG a las VRF creadas.
- Enlaces que conectan entre PE e Internet deberán tener configuración de EVN trunk.
- Implementar protocolos de enrutamiento IGP, procurando asociar a la VRF respectiva, enrutando de la manera más exacta posible.
- No olvide configurar interfaces pasivas según corresponda.
- Comprobar conectividad completa en la topología.
# Configuracion
## Equipo: SW-1

- Problema Detectado (Descripcion Breve): VLANS no estan creadas

| Comando para encontrar el problema | Comando Aplicado                                                                       |
| ---------------------------------- | -------------------------------------------------------------------------------------- |
| show vlan brief                    | SW-1(config)#vlan 10,20<br>SW-1(config-vlan)#exit<br>SW-1(config)#vtp mode transparent |

---

- Problema Detectado (Descripcion Breve): Interfaz e0/0 y e0/3 no estan en modo de acceso

| Comando para encontrar el problema                                                   | Comando Aplicado                                                                 |
| ------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------- |
| show int e0/0 switchport \| s Mode<br>show int e0/3 switchport \| s Mode<br>show run | SW-1(config)#int range e0/0,e0/3<br>SW-1(config-if-range)#switchport mode access |

---

- Problema Detectado (Descripcion Breve): Interfaz e0/0 y e0/3 no estan con sus VLAN aplicadas correspondientes

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                     |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show int status                    | SW-1(config)#int e0/0<br>SW-1(config-if)#switchport access vlan 10<br>SW-1(config-if)#exit<br>SW-1(config)#int e0/3<br>SW-1(config-if)#switchport access vlan 20<br> |

---

- Problema Encontrado (Descripcion Breve): Interfaz e0/2 no es troncal, ni deja pasar solamente las VLANS necesarias

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                              |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show int trunk                     | SW-1(config)#int e0/2<br>SW-1(config-if)#switchport trunk encapsulation dot1q<br>SW-1(config-if)#switchport mode trunk<br>SW-1(config-if)#switchport trunk allowed vlan 10,20 |

---
- Problema Encontrado (Descripcion Breve): Interfaces de acceso le falta seguridad de puerto habilitada

| Comando para encontrar el problema                                               | Comando Aplicado                                                                       |
| -------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| show port-security<br>show port-security int e0/0<br>show port-security int e0/3 | SW-1(config)#int range e0/0,e0/3<br>SW-1(config-if-range)#switchport port-security<br> |

---
- Problema Detectado (Descripcion Breve): Interfaces de acceso no tienen mecanismos de estabilizacion habilitados

| Comando para encontrar el problema                                                                                 | Comando Aplicado                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------- |
| SW-1#show spa int e0/0 detail<br>SW-1#show spa int e0/3 detail<br>SW-1#show run int e0/0<br>SW-1#show run int e0/3 | SW-1(config)#int range e0/0,e0/3<br>SW-1(config-if-range)#spanning-tree portfast<br>SW-1(config-if-range)#spanning-tree bpduguard enable |

---
- Problema Encontrado (Descripcion Breve): Interfaces troncales no deberian negociar su estado

| Comando para encontrar el problema     | Comando Aplicado                                                |
| -------------------------------------- | --------------------------------------------------------------- |
| show dtp int e0/2<br>show run int e0/2 | SW-1(config)#int e0/2<br>SW-1(config-if)#switchport nonegotiate |

---
## Equipo: SW-2

- Problema Encontrado (Descripcion Breve): Vlans no estan credas

| Comando para encontrar el problema | Comando Aplicado                                                                       |
| ---------------------------------- | -------------------------------------------------------------------------------------- |
| show vlan brief                    | SW-2(config)#vlan 10,20<br>SW-2(config-vlan)#exit<br>SW-2(config)#vtp mode transparent |

---

- Problema Detectado (Descripcion Breve): Puertos de Acceso con las interfaces e0/2 y e0/3, no estan configuradas

| Comando para encontrar el problema                                                   | Comando Aplicado                                                              |
| ------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------- |
| show int e0/2 switchport \| s Mode<br>show int e0/3 switchport \| s Mode<br>show run | SW-2(config)#int range e0/2-3<br>SW-2(config-if-range)#switchport mode access |

---
- Problema Encontrado (Descripcion Breve): Interfaces de acceso no aplicadas por VLAN

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                     |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show int status                    | SW-2(config)#int e0/2<br>SW-2(config-if)#switchport access vlan 10<br>SW-2(config-if)#exit<br>SW-2(config)#int e0/3<br>SW-2(config-if)#switchport access vlan 20<br> |

---
- Problema Encontrado (Descripcion Breve): Puertos de Acceso no tienen seguridad de puerto activada

| Comando para encontrar el problema                                               | Comando Aplicado                                                                |
| -------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| show port-security<br>show port-security int e0/2<br>show port-security int e0/3 | SW-2(config)#int range e0/2-3<br>SW-2(config-if-range)#switchport port-security |

---
- Problema Encontrado (Descripcion Breve): Interfaces de acceso sin los mecanismos de estabilizacion activados

| Comando para encontrar el problema                                                             | Comando Aplicado                                                                                                                      |
| ---------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| show spa int e0/2 detail<br>show spa int e0/3 detail<br>show run int e0/2<br>show run int e0/3 | SW-2(config)#int range e0/2-3<br>SW-2(config-if-range)#spanning-tree portfast<br>SW-2(config-if-range)#spanning-tree bpduguard enable |

---
- Problema Encontrado (Descripcion Breve): Interfaz Troncal (e0/0) no esta configurada correctamente

| Comando para encontrar el problema | Comando Aplicado                                                                                                               |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| show int trunk                     | SW-2(config)#int e0/0<br>SW-2(config-if)#switchport trunk encapsulation dot1q<br>SW-2(config-if)#switchport mode trunk<br><br> |

---
- Problema Detectado (Descripcion Breve): Interfaz Troncal (e0/0) tiene la negociacion de puerto activa y no deberia

| Comando para encontrar el problema     | Comando Aplicado                                                |
| -------------------------------------- | --------------------------------------------------------------- |
| show dtp int e0/2<br>show run int e0/2 | SW-2(config)#int e0/0<br>SW-2(config-if)#switchport nonegotiate |

---

## Equipo: PE1

- Problema Detectado (Descripcion Breve): No existen Definiciones VRF 

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                 |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show vrf                           | PE1(config)#vrf definition CLIENTE-A<br>PE1(config-vrf)#address-family ipv4<br>PE1(config-vrf-af)#exit<br>PE1(config-vrf)#exit<br>PE1(config)#vrf definition CLIENTE-B<br>PE1(config-vrf)#address-family ipv4<br>PE1(config-vrf-af)#exit<br>PE1(config-vrf)#exit |

---
- Problema Encontrado (Descripcion Breve): Sub-interfaces no creadas y configuradas con VRF

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip int brief                  | PE1(config)#int e0/2<br>PE1(config-if)#no shut<br>PE1(config-if)#exit<br>PE1(config)#int e0/2.10<br>PE1(config-subif)#encapsulation dot1q 10<br>PE1(config-subif)#vrf forwarding CLIENTE-A<br>PE1(config-subif)#ip add 192.168.10.1 255.255.255.0<br>PE1(config-subif)#exit<br>PE1(config)#int e0/2.20<br>PE1(config-subif)#encapsulation dot1q 20<br>PE1(config-subif)#vrf forwarding CLIENTE-B<br>PE1(config-subif)#ip add 192.168.10.1 255.255.255.0<br>PE1(config-subif)#exit |

---
- Problema Encontrado (Descripcion Breve): Etiquetas VRF No estan asignadas

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                             |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show vrf detail                    | PE1(config)#vrf definition CLIENTE-A<br>PE1(config-vrf)#vnet tag 100<br>PE1(config-vrf)#exit<br>PE1(config)#vrf definition CLIENTE-B<br>PE1(config-vrf)#vnet tag 200<br>PE1(config-vrf)#exit |

---
- Problema Detectado (Descripcion Breve): Interfaz Troncal no esta participando de VRF

| Comando para encontrar el problema | Comando Aplicado                                                         |
| ---------------------------------- | ------------------------------------------------------------------------ |
| show vrf ipv4 interfaces           | PE1(config)#int e0/0<br>PE1(config-if)#vnet trunk<br>PE1(config-if)#exit |

---
- Problema Encontrado (Descripcion Breve): Configuracion OSPF para el Cliente-B

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                         |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols vrf CLIENTE-B    | PE1(config)#router ospf 1 vrf CLIENTE-B<br>PE1(config-router)#network 192.168.10.0 0.0.0.255 area 0<br>PE1(config-router)#network 20.20.20.0 0.0.0.255 area 0<br>PE1(config-router)#exit |

---
- Problema Encontrado (Descripcion Breve): EIGRP no esta configurado para el segmento CLIENTE-A

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                           |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show ip protocols vrf CLIENTE-A    | PE1(config)#router eigrp TSHOOT<br>PE1(config-router)#address-family ipv4 vrf CLIENTE-A autonomous-system 1<br>PE1(config-router-af)#network 192.168.10.0 255.255.255.0<br>PE1(config-router-af)#network 20.20.20.0 255.255.255.0<br>PE1(config-router-af)#exit<br>PE1(config-router)#exit |

---
## Equipo: Internet

- Problema Detectado (Descripcion Breve): Definiciones VRF no existen

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                         |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show vrf brief                     | INTERNET(config)#vrf definition CLIENTE-A<br>INTERNET(config-vrf)#address-family ipv4<br>INTERNET(config-vrf-af)#exit<br>INTERNET(config-vrf)#exit<br>INTERNET(config)#vrf definition CLIENTE-B<br>INTERNET(config-vrf)#address-family ipv4<br>INTERNET(config-vrf-af)#exit<br>INTERNET(config-vrf)#exit |

---
- Problema Encontrado (Descripcion Breve): VRF no tienen tag VNET Asignado

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                           |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show vrf detail                    | INTERNET(config)#vrf definition CLIENTE-A<br>INTERNET(config-vrf)#vnet tag 100<br>INTERNET(config-vrf)#exit<br>INTERNET(config)#vrf definition CLIENTE-B<br>INTERNET(config-vrf)#vnet tag 200<br>INTERNET(config-vrf)#exit |

---
- Problema Encontrado (Descripcion Breve): Falta agregar vecinos troncales VRF

| Comando para encontrar el problema | Comando Aplicado                                                                                            |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| show vrf ipv4 interfaces           | INTERNET(config)#int range e0/0-1<br>INTERNET(config-if-range)#vnet trunk<br>INTERNET(config-if-range)#exit |

---
- Problema Detectado (Descripcion Breve): Falta configurar OSPF para VRF CLIENTE-B

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                           |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols vrf CLIENTE-B    | INTERNET(config)#router ospf 1 vrf CLIENTE-B<br>INTERNET(config-router)#network 20.20.20.0 0.0.0.255 area 0<br>INTERNET(config-router)#network 30.30.30.0 0.0.0.255 area 0<br>INTERNET(config-router)#exit |

---
- Problema Encontrado (Descripcion Breve): Falta configurar EIGRP para VRF CLIENTE-A

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                                       |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols vrf CLIENTE-A    | INTERNET(config)#router eigrp TSHOOT<br>INTERNET(config-router)#address-family ipv4 vrf CLIENTE-A autonomous-system 1<br>INTERNET(config-router-af)#network 20.20.20.0 255.255.255.0<br>INTERNET(config-router-af)#network 30.30.30.0 255.255.255.0<br>INTERNET(config-router-af)#exit<br>INTERNET(config-router)#exit |

---
## Equipo: PE2

- Problema Detectado (Descripcion Breve): Definiciones VRF No existen

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                 |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show vrf brief                     | PE2(config)#vrf definition CLIENTE-A<br>PE2(config-vrf)#address-family ipv4<br>PE2(config-vrf-af)#exit<br>PE2(config-vrf)#exit<br>PE2(config)#vrf definition CLIENTE-B<br>PE2(config-vrf)#address-family ipv4<br>PE2(config-vrf-af)#exit<br>PE2(config-vrf)#exit |

---

- Problema Encontrado (Descripcion Breve): Sub-interfaces no creadas y configuradas con VRF

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip int brief                  | PE2(config)#int e0/0<br>PE2(config-if)#no shut<br>PE2(config-if)#exit<br>PE2(config)#int e0/0.10<br>PE2(config-subif)#encapsulation dot1q 10<br>PE2(config-subif)#vrf forwarding CLIENTE-A<br>PE2(config-subif)#ip add 192.168.11.1 255.255.255.0<br>PE2(config-subif)#exit<br>PE2(config)#int e0/2.20<br>PE2(config-subif)#encapsulation dot1q 20<br>PE2(config-subif)#vrf forwarding CLIENTE-B<br>PE2(config-subif)#ip add 192.168.11.1 255.255.255.0<br>PE2(config-subif)#exit |

---

- Problema Encontrado (Descripcion Breve): VRF no tienen tag VNET Asignado

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                             |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show vrf detail                    | PE2(config)#vrf definition CLIENTE-A<br>PE2(config-vrf)#vnet tag 100<br>PE2(config-vrf)#exit<br>PE2(config)#vrf definition CLIENTE-B<br>PE2(config-vrf)#vnet tag 200<br>PE2(config-vrf)#exit |

---
- Problema Encontrado (Descripcion Breve): Falta agregar vecinos troncales VRF

| Comando para encontrar el problema | Comando Aplicado                                                         |
| ---------------------------------- | ------------------------------------------------------------------------ |
| show vrf ipv4 interfaces           | PE2(config)#int e0/1<br>PE2(config-if)#vnet trunk<br>PE2(config-if)#exit |

---

- Problema Detectado (Descripcion Breve): No esta configurado OSPF para VRF CLIENTE-B

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                         |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols vrf CLIENTE-B    | PE2(config)#router ospf 1 vrf CLIENTE-B<br>PE2(config-router)#network 192.168.11.0 0.0.0.255 area 0<br>PE2(config-router)#network 30.30.30.0 0.0.0.255 area 0<br>PE2(config-router)#exit |

---
- Problema Encontrado (Descripcion Breve): No esta configurado EIGRP para VRF CLIENTE-A

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                           |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show ip protocols vrf CLIENTE-A    | PE2(config)#router eigrp TSHOOT<br>PE2(config-router)#address-family ipv4 vrf CLIENTE-A autonomous-system 1<br>PE2(config-router-af)#network 192.168.11.0 255.255.255.0<br>PE2(config-router-af)#network 30.30.30.0 255.255.255.0<br>PE2(config-router-af)#exit<br>PE2(config-router)#exit |

---