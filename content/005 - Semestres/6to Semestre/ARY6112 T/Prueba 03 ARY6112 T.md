# Info
## Imagen Topologia
![1000](https://slink.proxylivy.work/image/0f3ad3fd-4de1-4162-86ef-7f33478a9941.png)
## Contexto
La empresa “RROJASC LTDA.” con ubicación en la Región Metropolitana con varios años en el mercado, se dedica principalmente a la venta de muebles y decoración hacia las grandes cadenas de supermercados a nivel nacional e internacional, actualmente cuenta con dos sucursales en la RM.
Debido a algunas circunstancias de ingresos mal intencionados a sus sistemas hace unos días, surgió una gran problemática de conectividad en sus dependencias generando múltiples pérdidas. Es por ello que se requiere de manera urgente la intervención de un ingeniero Networking para que la red vuelva a tener la conectividad total.
## Requerimientos
### A. Ticket 1: DMVPN
El PC-casa-Matriz debe tener conectividad por una DMVPN al PC-Móvil. Su tarea es poder verificar si la conectividad se encuentra OK. En caso contrario, debe solucionar los inconvenientes para que la comunicación se ejecute por la DMVPN.
- Identifique el o los dispositivos que causan la falla.
- Qué comandos de verificación utilizo para determinar el problema.
- Qué comandos de configuración utilizo para resolver la falla.
- Justifique técnicamente la solución que implemento para resolver la falla

### B. Ticket 2: MPLS VPN Capa 2
El PC-Norte y el PC-Sur se deben conectar por medio de una MPLS VPN de capa2. Lamentablemente al realizar una prueba de conectividad básica (ping), dicha prueba falla.
- Identifique el o los dispositivos que causan la falla.
- Qué comandos de verificación utilizo para determinar el problema.
- Qué comandos de configuración utilizo para resolver la falla.
- Justifique técnicamente la solución que implemento para resolver la falla

### C. Ticket 3: MPLS VPN Capa 3
El PC-Cliente-1A debe tener comunicación con el PC-Cliente-1B por medio de una MPLS VPN de capa 3. Lamentablemente al realizar una prueba de conectividad básica (ping), dicha prueba falla.
- Identifique el o los dispositivos que causan la falla.
- Qué comandos de verificación utilizo para determinar el problema.
- Qué comandos de configuración utilizo para resolver la falla.
- Justifique técnicamente la solución que implemento para resolver la falla

### Pruebas de conectividad
Debe llamar al docente en el aula de clases cuando tenga un ticket con la conectividad y/o con la conectividad de los tres ítems, de manera que se pueda evaluar de inmediato los criterios de evaluación asociados a la conectividad total.

## Pauta de Evaluacion

| Criterios                                                | CL<br>3 pts. | L<br>2 pts. | PL<br>1 pts. | NL<br>0 pts |
| -------------------------------------------------------- | ------------ | ----------- | ------------ | ----------- |
| T1: Indica los equipos con problemas                     |              |             |              |             |
| T1: Indica los comandos de verificacion para el problema |              |             |              |             |
| T1: Indica los comandos de resolver el problema          |              |             |              |             |
| T1: Justifica tecnicamente la solucion implementada      |              |             |              |             |
| T2: Indica los equipos con problemas                     |              |             |              |             |
| T2: Indica los comandos de verificacion para el problema |              |             |              |             |
| T2: Indica los comandos de resolver el problema          |              |             |              |             |
| T2: Justifica tecnicamente la solucion implementada      |              |             |              |             |
| T3: Indica los equipos con problemas                     |              |             |              |             |
| T3: Indica los comandos de verificacion para el problema |              |             |              |             |
| T3: Indica los comandos de resolver el problema          |              |             |              |             |
| T3: Justifica tecnicamente la solucion implementada      |              |             |              |             |
| Demuestra conectividad total de todos los tickets        |              |             |              |             |


# Configuracion
## Ticket 1: DMVPN
- Equipo: **DM**
- Justifica tecnicamente la solucion implementada: La ruta estatica que va hacia la RED Central a traves de la loopback para el tunnel DMVPN es incorrecta debido a que el 3er octeto no corresponde al segmento correspondiente, se cambio modifico para que reconociera correctamente la ip de la otra sucursal

| Comando para encontrar el problema | Comando Aplicado                                                                                                         |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Show ip route                      | DM(config)#no ip route 172.168.88.0 255.255.255.0 10.10.10.1<br>DM(config)#ip route 172.168.0.0 255.255.255.0 10.10.10.1 |

---
## Ticket 2: MPLS VPN Capa 2
- Equipo: CE31
- Justifica tecnicamente la solucion implementada: En CE31 la interfaz que conecta hacia PE3 se encuentra apagada, lo que imposibilita la conexión IPv4 para MPLS, se procede a encender para restablecer la conexión

| Comando para encontrar el problema | Comando Aplicado                                 |
| ---------------------------------- | ------------------------------------------------ |
| Show ip int brief                  | CE31(config)#int e0/0<br>CE31(config-if)#no shut |

---
- Equipo: PE4
- Justifica tecnicamente la solucion implementada: En PE4 se encuentra mal configurado MPLS L2 debido a que la ip que intenta conectar a la otra sucursal es erronea, tenia configurada la 33.33.33.33, se procedio a modificar el valor a la loopback de del router PE3 con la nueva ip 3.3.3.3

| Comando para encontrar el problema | Comando Aplicado                                                                                                                              |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| Show xconnect all                  | PE4(config)#int e0/0<br>PE4(config-if)#no xconnect 33.33.33.33 34 encapsulation mpls<br>PE4(config-if)#xconnect 3.3.3.3 34 encapsulation mpls |

---
## Ticket 3: MPLS VPN de Capa 3
- Equipo: PE1
- Justifica tecnicamente la solucion implementada: En PE1, VRF no está configurado en la interfaz e0/1, lo que no permite que sea reconocido dentro de la VRF y no se conecta, se procede a configurar VRF 1-A y la ip correspondiente al segmento debido a que VRF borra la configuración de la interfaz.

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                  |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| show vrf<br>show run int e0/1      | PE1(config)#int e0/1<br>PE1(config-if)#no ip address<br>PE1(config-if)#vrf forwarding 1-A<br>PE1(config-if)#ip address 223.0.11.1 255.255.255.248 |

---
- Equipo: PE1
- Justifica tecnicamente la solucion implementada: En PE1, dentro de BGP, en la sección address-family ipv4 se encuentra desactivado el vecino 2.2.2.2 que corresponde al vecino PE2, por lo que se procede a habilitar el vecino con el comando activate

| Comando para encontrar el problema | Comando Aplicado                                 |
| ---------------------------------- | ------------------------------------------------ |
| show ip bgp summary                | R1-B(config)#int e0/1<br>R1-B(config-if)#no shut |

---
- Equipo: R1-B
- Justifica tecnicamente la solucion implementada: En R1-B la interfaz e0/1 que conecta hacia el PC-cliente-1B se encuentra apagada, por lo que no podrá configurar DHCP correctamente hacia esa interfaz, se procede a encender

| Comando para encontrar el problema    | Comando Aplicado                                                                                                          |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| Show ip int brief<br>Show ip int e0/1 | PE1(config)#router bgp 65530<br>PE1(config-router)#address-family ipv4<br>PE1(config-router-af)#neighbor 2.2.2.2 activate |

---
## Evidencias
### Ticket 2: Conectividad entre sucursales MPLS L2
![1000](https://slink.proxylivy.work/image/0a3678ba-8956-4f51-998f-66527c6e0a67.png)