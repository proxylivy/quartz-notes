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

| Comando para encontrar el problema                                                             | Comando Aplicado                                                                                                                         |
| ---------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| show spa int e0/0 detail<br>show spa int e0/3 detail<br>show run int e0/0<br>show run int e0/3 | SW-1(config)#int range e0/0,e0/3<br>SW-1(config-if-range)#spanning-tree portfast<br>SW-1(config-if-range)#spanning-tree bpduguard enable |

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
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                     |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show int status                    | SW-2(config)#int e0/2<br>SW-2(config-if)#switchport access vlan 10<br>SW-2(config-if)#exit<br>SW-2(config)#int e0/3<br>SW-2(config-if)#switchport access vlan 20<br> |

---
- Problema Encontrado (Descripcion Breve): Puertos de Acceso no tienen seguridad de puerto activada

| Comando para encontrar el problema                                               | Comando Aplicado                                                                |
| -------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| show port-security<br>show port-security int e0/2<br>show port-security int e0/3 | SW-2(config)#int range e0/2-3<br>SW-2(config-if-range)#switchport port-security |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema                                                             | Comando Aplicado                                                                                                                      |
| ---------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| show spa int e0/2 detail<br>show spa int e0/3 detail<br>show run int e0/2<br>show run int e0/3 | SW-2(config)#int range e0/2-3<br>SW-2(config-if-range)#spanning-tree portfast<br>SW-2(config-if-range)#spanning-tree bpduguard enable |

---
- Problema Encontrado (Descripcion Breve): Interfaz Troncal (e0/0) no esta configurada y esta en modo DTP

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

- Problema Detectado (Descripcion Breve): 

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
