# Info
## Imagen Topologia
![1000](https://slink.proxylivy.work/image/630fa0ea-0876-4791-a7f1-9b7cd707ddff.png)
## INSTRUCCIONES GENERALES
1. Esta prueba será evaluada con una escala de 1 a 100 puntos.
2. El tiempo máximo disponible es el indicado por el docente en las instrucciones enviadas a su correo institucional.
3. Esta evaluación debe ser realizada en parejas (dos personas) o en forma individual.
4. Recuerde de guardar correctamente la configuración de la prueba en IOU WEB, según instrucciones mencionadas en clases y video tutorial. Es de su exclusiva responsabilidad que la configuración quede almacenada en forma correcta.
5. Cuando finalice la evaluación, debe subir el archivo exportado IOU WEB junto, el archivo Word con la documentación a la actividad del curso de AVA llamada “Evaluación Práctica 2“, mencionando en los comentarios de la actividad los nombres de los integrantes de la evaluación. Recuerde que tiene hasta las 12:00 horas para subir el archivo. NO SE PERMITEN ENVÍOS TARDÍOS.
6. Los contenidos evaluados corresponden a la segunda unidad de la asignatura (Resolución de problemas con protocolo EGP y capa 2)
Nota: Recuerde que el archivo que será revisado para la evaluación es el archivo Word con la resolución de cada uno de los tickets. El archivo .gz sólo será ejecutado como respaldo de la evaluación.

## Instrucciones para el inicio de la evaluación:
==Importante: Realizar los siguientes pasos previos antes de encender máquina virtual.==
- Clic derecho en Máquina Virtual “IOU Web”, Ir a Configuración y Red.
- Conectar a seleccionar “Adaptador Puente” y Renovar Dirección MAC.
- Encender Máquina Virtual. Notará que no da IP, por lo tal utilice las credenciales root/cisco,
- luego digite el comando dhclient y finalmente ifconfig, acá obtendrá una IP del segmento de la red de su PC Base.
- Importar y cargar evaluación
- Recuerde guardar cambios cada 10 minutos, en equipos colocar `R1#copy running-config unix:` luego en “Devices” guardar, para finalmente exportar.

## Contexto
La empresa “TECNOLOGIAS ASOCIADAS ROOROJASCL” lo ha contratado como Ingeniero de Networking, y entre sus condiciones contractuales será trabajar como outsourcing en dependencia de un cliente. En la dependencia del cliente utilizan plataforma de generación de ticket, y se han generado varias incidencias.

Su labor será la detección, documentación y resolución de problemas, que han sido agrupados en tickets.
## Requerimientos
### A. Ticket 1: Tecnologías en capa 2
- Revisar la microsegmentación de dominios de broadcast correspondientes.
- Cliente ha solicitado revisar el estado de sus Port-Channel, debido a que no se encuentra funcionando de forma correcta, permitiendo además el paso solo de las VLANs correspondientes.
- Cliente ha solicitado revisar el funcionamiento de su versión de MST, el cual debe estar operativo y optimizado.
- Mejora solicitada: Interfaces que conectan a equipos finales deben ser configurados con PortFast y deben estar protegidos en el caso que el usuario inserte un switch en dicha interface.
- Debe documentar la mejora en el ítem “Mejoras solicitadas” en la hoja de documentación (configuración aplicada)
- Debe documentar todos los errores encontrados en la hoja de documentación, señalando de forma correcta lo solicitado.

### B. Ticket 2: Alta disponibilidad
- El cliente ha solicitado revisar su protocolo de alta disponibilidad. Comenta que la distribución se encuentra realizada de la siguiente manera: todo el tráfico IPv4 pasa a través del router R2. La IP virtual será la primera del segmento IPv4 correspondiente.
- Mejora solicitada: El cliente ha solicitado formalmente, como plan de mejora, que se utilice por ahora la IP de DNS de su único ISP para que, en caso de inconveniente, todo el tráfico salga por R3. Esto se mantendrá así mientras se contrata otro ISP. Debe monitorear el tráfico y en caso de caída se debe utilizar del DNS del ISP, el tráfico del FHRP configurado debe tomar por R3.
- Debe documentar la mejora en el ítem “Mejoras solicitadas” en la hoja de documentación (configuración aplicada)
- Debe documentar todos los errores encontrados en la hoja de documentación, señalando de forma correcta lo solicitado.

### C. Ticket 3: Enrutamiento IGP
- El cliente tiene implementado protocolo estado de enlace. Las áreas están definidas por el cliente.
- Se ha solicitado, reestablecer el funcionamiento a una configuración por defecto.
- Mejora solicitada: Debido a un proceso de mejorar, se ha solicitado que los tiempos de saludo de la interface ethernet 1/0 del R1 y del R2 se encuentren configuradas con un valor de 7 y el tiempo de dead en el valor que corresponde.
- Debe documentar la mejora en el ítem “Mejoras solicitadas” en la hoja de documentación (configuración aplicada)
- Debe documentar todos los errores encontrados en la hoja de documentación, señalando de forma correcta lo solicitado.

### D. Ticket 4: Enrutamiento EGP
- La compañía tiene una sucursal remota, la cual debe poder ser alcanzable por BGP. Le han pedido poder resolver los inconvenientes para lograr reestablecer la conectividad con dicho lugar.
- Todas las Loopback deben participar como prefijos para BGP.
- La redistribución entre IGP – EGP puede ser realizada a través de mapas de rutas o puede ser ejecutada de forma directa.
- Debe documentar todos los errores encontrados en la hoja de documentación, señalando de forma correcta lo solicitado.

### E. Ticket 5: Túnel 6 over 4
- La empresa tiene implementado un túnel GRE 6 over 4, para pruebas de funcionamiento y seguridad. Se ha solicitado la revisión del túnel en donde el tráfico de las Loopback de R2 y R3 sea a través del túnel. Las Loopback y túnel deben estar en EIGRP para IPv6.
- Debe documentar todos los errores encontrados en la hoja de documentación, señalando de forma correcta lo solicitado.

### F. Ticket 6: Servicios avanzados de direccionamiento IPv4
- Recuerde que se encuentra funcionando NATEO por sobrecarga en el router BORDE de la topología y sólo las redes de los PC de cada una de las VLAN deben salir traducidos por IP pública.
- Equipos Finales deben recibir direccionamiento IPv4 de manera dinámica a través del router R2.
- Mejora solicitada: Se solicita que para cada pool DHCP se deben excluir las primeras 20 direcciones IPv4.
- Debe documentar la mejora en el ítem “Mejoras solicitadas” en la hoja de documentación (configuración aplicada)
- Los equipos finales deben ser capaces llegar al ISP y Edge.
- Debe documentar todos los errores encontrados en la hoja de documentación, señalando de forma correcta lo solicitado.

### Conectividad Basica
- Captura de pantalla prueba de conectividad
- En la parte final de la hoja de documentación, debe insertar una captura de pantalla mostrando la conectividad total desde el PCVLAN10 hacia el DNS del ISP (8.8.8.8) y hacia la Loopback del router EDGE (192.168.100.1). En la captura de pantalla se debe mostrar su fondo de pantalla (debe ser distintos todos) y la barra de tareas donde se pueda visualizar la fecha y hora del PC.

> Los nombres de la VLAN no se encuentran considerados como errores. Queda a su criterio si gusta configurar la topología con los nombres que corresponden (ideal sería que sí)

## Pauta de Evaluacion

| Criterios de Evaluacion                                                                                                                                                | CL<br>10 pts. | L<br>5 pts. | PL<br>3 pts. | NL<br>0 pts. |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ----------- | ------------ | ------------ |
| 1. Documenta los tickets solicitados indicando<br>- Enfoque<br>- Equipo y error correspondiente<br>- Comandos para encontrar error<br>- Comandos para solucionar error |               |             |              |              |
| 2. Resolver Ticket 1: Tecnologias en Capa 2                                                                                                                            |               |             |              |              |
| 3. Resolver Ticket 2: Alta disponibilidad                                                                                                                              |               |             |              |              |
| 4. Resolver Ticket 3: Enrutamiento IGP                                                                                                                                 |               |             |              |              |
| 5. Resolver Ticket 4: Enrutamiento EGP                                                                                                                                 |               |             |              |              |
| 6. Resolver Ticket 5: Tunel GRE over 4                                                                                                                                 |               |             |              |              |
| 7. Resolver Ticket 6: Servicios avanzados IP                                                                                                                           |               |             |              |              |
| 8. Realiza Mejoras de Ticket 1 y 2                                                                                                                                     |               |             |              |              |
| 9. Realiza Mejoras de Ticket 3 y 6                                                                                                                                     |               |             |              |              |
| 10. Conectividad Completa PC-VLAN 10 -> ISP y EDGE                                                                                                                     |               |             |              |              |

# Configuracion
## Equipo: ALS1
- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): La interfaz del ALS1 se encuentra apagada.

| Comando para encontrar el problema | Comando Aplicado                                                         |
| ---------------------------------- | ------------------------------------------------------------------------ |
| ALS1#sh ip int brief               | ALS1(config)#int e0/3<br>ALS1(config-if)#no shut<br>ALS1(config-if)#exit |

---
- Problema Encontrado (Descripcion Breve): No se encuentran ingresadas las vlan 10 y 30 en el ALS

| Comando para encontrar el problema | Comando Aplicado                                  |
| ---------------------------------- | ------------------------------------------------- |
| ALS1#show vlan brief               | ALS1(config)#vlan 10,30<br>ALS1(config-vlan)#exit |

---
- Problema Encontrado (Descripcion Breve): PAgp a LACP. Se encuentra configurado pagp en vez de LACP del ALS y se realiza la configuracion solicitada.

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                                                |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ALS1#show etherchannel summary     | ALS1(config)#no int po3<br>ALS1(config)#int range e0/1-2<br>ALS1(config-if-range)#channel-group 3 mode passive<br>ALS1(config)#int po3<br>ALS1(config-if)# switchport trunk encapsulation dot1q<br>ALS1(config-if)# switchport trunk allowed vlan 10,20,30<br>ALS1(config-if)# switchport mode trunk<br>ALS1(config-if)#no shut |

---
- Problema Detectado (Descripcion Breve): Se encontraba configurada la vlan 100 como nativa en el switchport trunk.

| Comando para encontrar el problema | Comando Aplicado                                                            |
| ---------------------------------- | --------------------------------------------------------------------------- |
| sh run                             | ALS1(config)#int po1<br>ALS1(config-if)#no switchport trunk native vlan 100 |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado                                                                                                  |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| sh run                             | ALS1(config)#int e0/3<br>ALS1(config-if)#spanning-tree portfast<br>ALS1(config-if)#spanning-tree bpduguard enable |

---
## Equipo: DSL2
> Nota: Aqui hay un error extra, el hostname deberia ser DSL2, pero esta configurado DLS2

- Problema Detectado (Descripcion Breve): Se encontraba configurado pvst en vez de mst.

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                     |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| DLS2#show spa sum                  | DSL2(config)#spanning-tree mode mst<br>DSL2(config)#spanning-tree mst configuration<br>DSL2(config-mst)# name TSHOOT<br>DSL2(config-mst)# revision 1<br>DSL2(config-mst)# instance 1 vlan 10, 20, 30 |

---
- Problema Encontrado (Descripcion Breve): Faltaba agregar las vlan 20 y 30 en el DLS2

| Comando para encontrar el problema | Comando Aplicado        |
| ---------------------------------- | ----------------------- |
| DLS2#sh vlan brief                 | DSL2(config)#vlan 20,30 |

---
- Problema Encontrado (Descripcion Breve): Se encontraba activado el filtro de bdpu en DLS

| Comando para encontrar el problema | Comando Aplicado                                                           |
| ---------------------------------- | -------------------------------------------------------------------------- |
| show run                           | DSL2(config)#int po2<br>DSL2(config-if)#no spanning-tree bpdufilter enable |

---

- Problema Encontrado (Descripcion Breve): Se encontraba configurada como vlan de acceso la vlan 30 en vez de vlan 20.

| Comando para encontrar el problema | Comando Aplicado                                                                                                   |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| show vlan brief                    | DSL2(config)#int e0/3<br>DSL2(config-if)#no switchport access vlan 30<br>DSL2(config-if)#switchport access vlan 20 |

---
- Problema Detectado (Descripcion Breve): (STATIC a LACP po3) Se encontraba configurado el portchannel 3 como estatico en vez de LACP.

| Comando para encontrar el problema | Comando Aplicado                                                                                                                               |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| show etherchannel sum              | DSL2(config)#no int po3<br>DSL2(config)#int range e0/1-2<br>DSL2(config-if-range)#channel-group 3 mode active<br>DSL2(config-if-range)#no shut |

---
- Problema Encontrado (Descripcion Breve): Faltaba la configuracion del portchannel-3.

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                   |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show int trunk                     | DSL2(config)#int po3<br>DSL2(config-if)# switchport trunk encapsulation dot1q<br>DSL2(config-if)# switchport trunk allowed vlan 10,20,30<br>DSL2(config-if)# switchport mode trunk |

---
- Problema Encontrado (Descripcion Breve): Interfaz e0/3 de acceso no tiene los mecanismos de estabilizacion habilitados

| Comando para encontrar el problema | Comando Aplicado                                                                                                  |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| sh run                             | DSL2(config)#int e0/3<br>DSL2(config-if)#spanning-tree portfast<br>DSL2(config-if)#spanning-tree bpduguard enable |

---
- Problema Detectado (Descripcion Breve): La interfaz e 0/0 no estaba configurada como troncal.

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                    |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show int trunk                     | DSL2(config)#int e0/0<br>DSL2(config-if)# switchport trunk encapsulation dot1q<br>DSL2(config-if)# switchport trunk allowed vlan 10,20,30<br>DSL2(config-if)# switchport mode trunk |

---
## Equipo: DSL1

- Problema Detectado (Descripcion Breve): Faltaba agregar la vlan 20

| Comando para encontrar el problema | Comando Aplicado     |
| ---------------------------------- | -------------------- |
| sh vlan brief                      | DSL1(config)#vlan 20 |

---
- Problema Encontrado (Descripcion Breve): Estaba configurada una MAC estatica en el DLS

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                   |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| DSL1#show port int e0/3            | DSL1(config)#int e0/3<br>DSL1(config-if)#no switchport port-security mac-address aaaa.1111.bbbb<br>DSL1(config-if)#shut<br>DSL1(config-if)#no shut |

---
- Problema Encontrado (Descripcion Breve): Se encontraba apagada la interfaz E1/2 del equipo.

| Comando para encontrar el problema | Comando Aplicado                                 |
| ---------------------------------- | ------------------------------------------------ |
| show ip int brief                  | DSL1(config)#int e1/2<br>DSL1(config-if)#no shut |

---

- Problema Encontrado (Descripcion Breve): (PAgP Ambos Auto) Se encontraban configurados en ambos lados del portchannel-2 como auto

| Comando para encontrar el problema                                    | Comando Aplicado                                                                                                                            |
| --------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| DSL1#show etherchannel sum<br>DLS2#sh etherchannel 2 detail \| s Mode | DSL1(config)#int range e1/0-1<br>DSL1(config-if-range)#no channel-group 2 mode auto<br>DSL1(config-if-range)#channel-group 2 mode desirable |

---
- Problema Encontrado (Descripcion Breve): La interfaz de acceso no tenia los mecanismos de estabilizacion activados

| Comando para encontrar el problema | Comando Aplicado                                                                                                  |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| show run                           | DSL1(config)#int e0/3<br>DSL1(config-if)#spanning-tree portfast<br>DSL1(config-if)#spanning-tree bpduguard enable |

---

- Problema Detectado (Descripcion Breve): Puerto e0/0 en modo acceso

| Comando para encontrar el problema | Comando Aplicado                                                                           |
| ---------------------------------- | ------------------------------------------------------------------------------------------ |
| show int trunk<br>show vlan brief  | DSL1(config)#int e0/0<br>DSL1(config-if)#no switchport mode access<br>DSL1(config-if)#exit |

---
- Problema Encontrado (Descripcion Breve): Esta configurada la la interfaz e0/0 como modo acceso.

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                 |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show int trunk<br>show vlan brief  | DSL1(config)#int e0/0<br>DSL1(config-if)#switchport trunk encapsulation dot1q<br>DSL1(config-if)#switchport trunk allowed vlan 10,20,30<br>DSL1(config-if)#switchport mode trunk |

---
## Equipo: R2

- Problema Detectado (Descripcion Breve): Estaban excluida todas las ip asignadas a la vlan 10 y faltaba configurar para las vlan 20 y 30

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                 |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| R2#sh run \| s excluded            | R2(config)#no ip dhcp excluded-address 172.16.10.1 172.16.10.254<br>R2(config)#ip dhcp excluded-address 172.16.10.1 172.16.10.20<br>R2(config)#ip dhcp excluded-address 172.16.20.1 172.16.20.20<br>R2(config)#ip dhcp excluded-address 172.16.30.1 172.16.30.20 |

---
- Problema Encontrado (Descripcion Breve): Se encontraba apagado el protocolo ospf y faltaba por agregar el network

| Comando para encontrar el problema                                    | Comando Aplicado                                                                                                |
| --------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| sh ip protocols<br>show ip ospf neighbor<br>show run \| s router ospf | R2(config)#router ospf 1<br>R2(config-router)#no shut<br>R2(config-router)#network 192.168.21.2 0.0.0.0 area 10 |

---
- Problema Encontrado (Descripcion Breve): OSPFv3 esta habilitado en el switch mientras que EIGRPv6 esta configurado

| Comando para encontrar el problema | Comando Aplicado                 |
| ---------------------------------- | -------------------------------- |
| sh ipv6 protocols                  | R2(config)#no ipv6 router ospf 1 |

---

- Problema Encontrado (Descripcion Breve): Crear IP SLA mejora Se configura IP SLA

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                      |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| R2#show track                      | R2(config)#ip sla 1<br>R2(config-ip-sla)#icmp-echo 8.8.8.8<br>R2(config-ip-sla-echo)#frequency 5<br>R2(config-ip-sla-echo)#exit<br>R2(config)#ip sla schedule 1 start-time now life forever<br>R2(config)#track 2 ip sla 1 reachability<br>R2(config-track)#delay down 5 up 10<br>R2(config-track)#exit<br>R2(config)#int e0/0.20<br>R2(config-subif)#standby 20 track 2 decrement 21 |

---
- Problema Detectado (Descripcion Breve): Ajustar tiempos OSPF interfaz hacia R1

| Comando para encontrar el problema     | Comando Aplicado                                                                                      |
| -------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| show ip ospf int e1/1 \| include Timer | R2(config)#int e1/0  R2(config-if)#ip ospf hello-interval 7<br>R2(config-if)#ip ospf dead-interval 28 |

---
- Problema Encontrado (Descripcion Breve): Se encuentra mal configurada la interfaz del túnel, la dirección ipv6 asignada a esta.

| Comando para encontrar el problema                                                | Comando Aplicado                                                                                                                                                                                                                                                                                                                            |
| --------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| R2#show int tunnel 1<br>R2#show int tunnel 0<br>R2#show run \| s interface Tunnel | R2(config)#no int tunnel0<br>R2(config)#int tunnel1<br>R2(config-if)#ipv6 eigrp 1<br>R2(config-if)#no ipv6 address 2019:ACAD:ACAD:100::10/64<br>R2(config-if)#ipv6 address 2019:ACAD:ACAD:100::3/64<br>R2(config-if)#no tunnel source Ethernet0/0<br>R2(config-if)#tunnel source e1/0<br>R2(config-if)#keepalive 5<br>R2(config-if)#no shut |

---
## Equipo: R3

- Problema Detectado (Descripcion Breve): La interfaz e0/1 se encuentra apagada.

| Comando para encontrar el problema | Comando Aplicado                             |
| ---------------------------------- | -------------------------------------------- |
| show ip int brief                  | R3(config)#int e0/1<br>R3(config-if)#no shut |

---
- Problema Encontrado (Descripcion Breve): La direccion ip de la interfaz esta mal configurada.

| Comando para encontrar el problema | Comando Aplicado                                                                                                                   |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| show ip int brief                  | R3(config)#int e1/1<br>R3(config-if)#no ip address 31.31.31.3 255.255.255.0<br>R3(config-if)#ip address 192.168.31.3 255.255.255.0 |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema                                      | Comando Aplicado                                      |
| ----------------------------------------------------------------------- | ----------------------------------------------------- |
| show ip protocols<br>show ip ospf neighbor<br>show run \| s router ospf | R3(config)#router ospf 1<br>R3(config-router)#no shut |

---
- Problema Detectado (Descripcion Breve): No se encontraba configurado el modo de del túnel.

| Comando para encontrar el problema                        | Comando Aplicado                                                                                                  |
| --------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| R3#show int tunnel 1<br>R3#show run \| s interface Tunnel | R3(config)#int tunnel 1<br>R3(config-if)#tunnel mode ipv6ip<br>R3(config-if)#keepalive 5<br>R3(config-if)#no shut |

---
- Problema Encontrado (Descripcion Breve): se reestablece la prioridad del standby 10 y 30

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                              |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show standby brief                 | R3(config)#int e0/1.10<br>R3(config-subif)#no standby 10 priority 90<br>R3(config-subif)#standby 10 priority 110<br>R3(config-subif)#exit<br>R3(config)#int e0/1.30<br>R3(config-subif)#no standby 30 priority 90<br>R3(config-subif)#standby 30 priority 110 |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show track                         | R3(config)#ip sla 1<br>R3(config-ip-sla)#icmp-echo 8.8.8.8<br>R3(config-ip-sla-echo)#frequency 5<br>R3(config-ip-sla-echo)#exit<br>R3(config)#ip sla schedule 1 start-time now life forever<br>R3(config)#track 2 ip sla 1 reachability<br>R3(config-track)#delay down 5 up 10<br>R3(config-track)#exit<br>R3(config)#int e0/1.10<br>R3(config-subif)#standby 10 track 2 decrement 21<br>R3(config-subif)#exit<br>R3(config)#int e0/1.30<br>R3(config-subif)#standby 10 track 2 decrement 21 |

---
## Equipo: R1

- Problema Detectado (Descripcion Breve): Se encuentra apagado el protocolo OSPF

| Comando para encontrar el problema                                      | Comando Aplicado                                      |
| ----------------------------------------------------------------------- | ----------------------------------------------------- |
| show ip protocols<br>show ip ospf neighbor<br>show run \| s router ospf | R1(config)#router ospf 1<br>R1(config-router)#no shut |

---
- Problema Encontrado (Descripcion Breve): Las redes del area 10 tienen mal configurada el area dentro de OSPF

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                                                            |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip protocols                  | R1(config)#router ospf 1<br>R1(config-router)#no network 192.168.21.1 0.0.0.0 area 0<br>R1(config-router)#network 192.168.21.1 0.0.0.0 area 10<br>R1(config-router)#no network 192.168.31.1 0.0.0.0 area 0<br>R1(config-router)#network 192.168.31.1 0.0.0.0 area 10<br>R1(config-router)#no network 1.1.1.1 0.0.0.0 area 0 |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema                                               | Comando Aplicado                                                                                                            |
| -------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| show ip ospf int e1/0 \| include Timer<br>show ip ospf int e1/1 \| include Timer | R1(config)#int range e1/0-1<br>R1(config-if-range)#ip ospf hello-interval 7<br>R1(config-if-range)#ip ospf dead-interval 28 |

---

- Problema Detectado (Descripcion Breve): Router-ID OSPF incorrecto

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                 |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show ip protocols                  | R1(config)#router ospf 1<br>R1(config-router)#no router-id 2.2.2.2<br>R1(config-router)#router-id 1.1.1.1<br>R1(config-router)#do clear ip ospf process<br>Reset ALL OSPF processes? \[no\]: yes |

---
- Problema Encontrado (Descripcion Breve): No se encuentra configurada la network hacia el router de borde

| Comando para encontrar el problema | Comando Aplicado                                                                  |
| ---------------------------------- | --------------------------------------------------------------------------------- |
| show ip protocols                  | R1(config)#router ospf 1<br>R1(config-router)#network 192.168.41.1 0.0.0.0 area 0 |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                                                                                           |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show access-lists                  | R1(config)#no ip access-list standard OPTIMIZACION<br>R1(config)#router ospf 1<br>R1(config-router)#no distribute-list OPTIMIZACION in Ethernet0/0<br>R1(config-router)#no distribute-list OPTIMIZACION in Ethernet1/0<br>R1(config-router)#no distribute-list OPTIMIZACION in Ethernet1/1 |

---
## Equipo: Borde

- Problema Detectado (Descripcion Breve): Flujo NAT Se encuentra invertido el flujo de nateo.

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                                          |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show run                           | BORDE(config)#int e0/0<br>BORDE(config-if)#no ip nat outside<br>BORDE(config-if)#ip nat inside<br>BORDE(config-if)#exit<br>BORDE(config)#int e0/1<br>BORDE(config-if)#no ip nat inside<br>BORDE(config-if)#ip nat outside |

---
## Equipo: BORDE

- Problema Detectado (Descripcion Breve): Red OSPF no existente

| Comando para encontrar el problema | Comando Aplicado                                                                      |
| ---------------------------------- | ------------------------------------------------------------------------------------- |
| show ip protocols                  | BORDE(config)#router ospf 1<br>BORDE(config-router)#no network 4.4.4.4 0.0.0.0 area 0 |

---
- Problema Encontrado (Descripcion Breve): Se encuentra mal configurado el sistema autónomo del vecino.

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                      |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip bgp<br>show ip protocols   | BORDE(config)#router bgp 65000<br>BORDE(config-router)#no neighbor 201.0.0.2 remote-as 65000<br>BORDE(config-router)#neighbor 201.0.0.2 remote-as 100 |

---
- Problema Encontrado (Descripcion Breve): La ruta por defecto tiene mal configurado la direcccion del siguiente salto hacia el isp.

| Comando para encontrar el problema | Comando Aplicado                                                                                         |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------- |
| show ip route                      | BORDE(config)#no ip route 0.0.0.0 0.0.0.0 201.0.0.20<br>BORDE(config)#ip route 0.0.0.0 0.0.0.0 201.0.0.2 |

---

- Problema Encontrado (Descripcion Breve): Falta la lista de acceso del nateo.

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                                                               |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show run                           | BORDE(config)#ip access-list standard NAT<br>BORDE(config-std-nacl)#permit 172.16.10.0 0.0.0.255<br>BORDE(config-std-nacl)#permit 172.16.20.0 0.0.0.255<br>BORDE(config-std-nacl)#permit 172.16.30.0 0.0.0.255 |

---
- Problema Encontrado (Descripcion Breve): La lista de acceso da error para distribuir entre el router 1 y el router de borde 

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                            |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show access-lists<br>show run      | BORDE(config)#no ip access-list standard OPTIMIZACION<br>BORDE(config)#router ospf 1<br>BORDE(config-router)#no distribute-list OPTIMIZACION in Ethernet0/0 |

---
## Equipo: ISP

- Problema Detectado (Descripcion Breve): Se encuentra mal configurado el sistema autonomo del vecino hacia el router de borde.

| Comando para encontrar el problema | Comando Aplicado                                                                                                                                                           |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip bgp summary                | ISP(config)#router bgp 100<br>ISP(config-router)#no neighbor 201.0.0.1 remote-as 6500<br>ISP(config-router)# neighbor 201.0.0.1 remote-as 65000<br>ISP(config-router)#exit |

---
- Problema Encontrado (Descripcion Breve): Redistribucion IGP → EGP No se encontraba realizada la redistribucion de ospf en bgp

| Comando para encontrar el problema                     | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show ip route<br>show route-map<br>show ip prefix-list | BORDE(config)#router ospf 1<br>BORDE(config-router)#no redistribute static subnets<br>BORDE(config-router)#default-information originate<br>BORDE(config)#ip prefix-list IGP permit 192.168.41.0/24<br>BORDE(config)#ip prefix-list IGP permit 192.168.21.0/24<br>BORDE(config)#ip prefix-list IGP permit 192.168.31.0/24<br>BORDE(config)#ip prefix-list IGP permit 172.16.10.0/24<br>BORDE(config)#ip prefix-list IGP permit 172.16.20.0/24<br>BORDE(config)#ip prefix-list IGP permit 172.16.30.0/24<br>BORDE(config)#route-map EGP permit<br>BORDE(config-route-map)#match ip address prefix-list IGP<br>BORDE(config-route-map)#set metric 20<br>BORDE(config-route-map)#exit<br>BORDE(config)#router bgp 65000<br>BORDE(config-router)#redistribute ospf 1 route-map EGP |

---
- Problema Encontrado (Descripcion Breve): Redistribucion EGP → IGP

| Comando para encontrar el problema                     | Comando Aplicado                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| show ip route<br>show route-map<br>show ip prefix list | BORDE(config)#ip prefix-list EGP permit 201.0.0.0/30<br>BORDE(config)#ip prefix-list EGP permit 200.0.0.0/30<br>BORDE(config)#ip prefix-list EGP permit 8.8.8.8/32<br>BORDE(config)#ip prefix-list EGP permit 192.168.100.1/32<br>BORDE(config)#route-map IGP permit<br>BORDE(config-route-map)#match ip address prefix-list EGP<br>BORDE(config-route-map)#set metric 20  BORDE(config-route-map)#set metric-type type-1<br>BORDE(config-route-map)#exit<br>BORDE(config)#router ospf 1<br>BORDE(config-router)#redistribute bgp 65000 subnets route-map IGP<br>BORDE(config-router)#exit |
