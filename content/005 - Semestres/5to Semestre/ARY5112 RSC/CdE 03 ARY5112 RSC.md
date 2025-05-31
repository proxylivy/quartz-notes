# Info
## Imagen Topologia
![2000](https://slink.proxylivy.work/image/d9c77123-2600-4eb1-a2dd-6669176e1822.png)
## Descargar
[Onedrive Topologia](https://duoccl0-my.sharepoint.com/:u:/g/personal/ga_zunigam_duocuc_cl/EYTnq9RpqLJGozca662Ye_QBRe6unRzdj4QIWnE5hyO8jQ?e=u3v169)
## Contexto
La empresa “MI PERA SENIOR S.A” ha pedido el servicio de un grupo de especialistas de redes avanzadas donde deberá hacer la implementación de la red, como también lograr la explicación de los servicios que esta tiene a nivel de tecnologías de capa 2 y capa 3, también con redes que operan a nivel de IPv4/IPv6.
## Requerimientos
### A.- Configuración de Switching Básico:
- Implementar microsegmentación de dominio de broadcast, según ID propuesto y nombre de referencia.
- Permitir el paso de VLANs creadas por interfaces correspondientes.
- Debe permitir el paso de VLAN Nativa, con configuración apropiada para este fin.
- Desactivar DTP en todas las interfaces de los SW.
- Las interfaces que no estén en uso deberán quedar desactivadas.
- Asignar las interfaces correspondientes a las VLANs de Datos.
- Implementar algún tipo de seguridad que permita ignorar los nuevos frames que lleguen a las interfaces de acceso de los SW en caso de que exceda la cantidad máxima de MAC por defecto que tiene los SW.
- Implementar solución de agregado de enlace L2 en base protocolo propuesto. Es importante considerar que los Port-Channel que están en los DLS son los que deben negociar y de los ALS escuchar, según corresponda.

### B.- Configuración de Switching Intermedio:
- Implementar MSTP en todos los switches.
- Deberá crear la región 1, y con el nombre ENCOR. La instancia 2, deberá mapear a la VLAN10 y VLAN 20. La instancia 4, deberá mapear a la VLAN30 y VLAN40.
- Realizar las modificaciones necesarias para que VLAN30 y VLAN40 sea root primary en DLS2 y root secondary en DLS1.
- Realizar las modificaciones necesarias para que VLAN10 y VLAN20 sea root primary en DLS1 y root secondary en DLS2.
- Reducir a la mitad los temporizadores de MSTP. En caso de ser decimal, aproximar al entero siguiente.
- Permitir autorecuperación de interfaces de 30 segundos. Realizar implementación para lograr este objetivo.
- Implementar DHCP Snooping donde permita asegurar el servicio DHCP en la red corporativa.
- Implementar mecanismo que equipos finales no puedan solicitar más de 3 IP. En caso de ocurrir, interfaz deberá pasar a condición de err-disable.
- Interfaces que conectan a equipos finales deberán tener mecanismo que permita pasar automáticamente al estado de FWD, además de no enviar ni recibir BPDU. 
- PC de VLAN10 y VLAN20 no pueden comunicarse a nivel de capa 2 ni capa 3, implemente solución adecuada para aquello.
- PC de VLAN30 y VLAN40 no pueden comunicarse a nivel de capa 2 ni capa 3, implemente solución adecuada para aquello.

### C.- Configuración de Alta Disponibilidad:
- Realizar enrutamiento intervlan en los equipos apropiados, según direccionamiento propuesto.
- Implementar protocolo de alta disponibilidad propietario Cisco en versión 2.Utilizar como VIPv4 IP10 y para IPv6 VIPv6 ::10.
- Realizar las configuraciones pertinentes, para que todo el tráfico IPv4 prefiera R2 y el tráfico IPv6 prefiera R1.
- Modificar los temporizadores los cuales deben quedar a 200 mseg el hello y 800 mseg en hold-time para IPv4. Para IPv6 los temporizadores deben queden equivalentes a protocolos VRRPv3.
- Implementar IP SLA, para que, en caso de falla de loopback del ISP-2, todo el tráfico automáticamente prefiera router R1.
- Implementar track para que enlace ascendente de R1 en caso de falla, todo el tráfico IPv6 prefiera el router R2.
### D. Configuración de Enrutamiento:
- Implementar protocolos de enrutamiento IGP propuestos en IPv4/IPv6.
- R1 y R2 debe mandar las rutas sumarizadas de redes LAN a nivel de IPv4/IPv6.
- Utilizar a router R3 para asignar IPv4 de manera automática en equipos finales. Debe excluir las primeras 7 IP. Debe proporcionar como DNS principal la IP 4.4.4.4 y DNS Secundario la 8.8.8.8.
- La redistribución de protocolos IGP a nivel IPv4 será mediante route-maps. Todas las rutas externas que sean inyectadas a protocolo estado de enlace deberá añadir un costo base de 35 y ser variable. Para el caso de protocolo vector distancia, debe asegurarse que la métrica sea asociada a los valores K de la interfaz.
- La redistribución de protocolos IGP a nivel IPv6 será de manera directa.
- Implementar MP-BGP para IPv4/IPv6. Debe generar las sesiones pares respectivas, según AS solicitados. La loopback de ambos ISP y red de sucursal BR deben participar como prefijos en protocolo de enrutamiento.
- Realizar redistribuciones entre IGP y EGP a nivel IPv4 mediante route-map. Para IPv6 la redistribución será de manera directa.
- Generar manipulación de atributos mediante mapas de rutas, donde el ISP-1 a nivel IPv6 le informe a sus routers vecinos que su prefijo tendrá un valor de 500 a nivel de métrica. Debe manipular el atributo adecuado para esto.
- Generar manipulación de atributos mediante mapas de rutas, donde el tráfico a nivel IPv6 prefiera el ISP-1 y para IPv4 el ISP-2. Debe manipular el atributo adecuado para esto.
- Implementar solución para que los AS 64600 y AS 65000 no sean considerados como sistemas autónomos de tránsito por parte de los ISP.
- Implementar solución de VPN Site-to-Site, donde el tráfico entre la red 172.16.10.0/24 y 172.16.20.0/24 viaje de forma protegida, utilizando los siguientes parámetros:

| Parametos                          | R1        | R2        |
| ---------------------------------- | --------- | --------- |
| Metodo de Distribucion<br>de Llave | ISAKMP    | ISAKMP    |
| Algoritmo de Cifrado               | AES 256   | AES 256   |
| Algoritmo de Hash                  | SHA-1     | SHA-1     |
| Metodo de Autenticacion            | Pre-Share | Pre-Share |
| Intercambio de Llave               | DH5       | DH5       |
| Tiempo de Vida IKE SA              | 86400     | 86400     |
| Llave ISAKMP                       | ccnpv8    | ccnpv8    |


| Parametros                  | R1             | R2             |
| --------------------------- | -------------- | -------------- |
| Transform-Set               | CASO-3         | CASO-3         |
| Cifrado ESP Transform       | esp-aes        | esp-aes        |
| Autenticacion ESP Transform | esp-sha-hmac   | esp-sha-hmac   |
| Trafico a Proteger          | 172.16.10.0/24 | 172.16.20.0/24 |
| Nombre Crypto MAP           | MAPA-PRUEBA3   | MAPA-PRUEBA3   |
| Establecimiento SA          | ipsec-isakmp   | ipsec-isakmp   |
|                             |                |                |
- Comprobar conectividad completa en la topología a nivel IPv4/IPv6.
### E. Demostración de Evidencias:
- Generar una captura de pantalla que demuestre la conectividad completa que existe desde los equipos finales al PC que está ubicado en BR a nivel de IPv4/IPv6 y mediante el uso del TRACERT.
- Generar una captura de pantalla que demuestre el cifrado del tráfico de la VPN Site-to-Site.
## Tabla Direccionamiento

| Nombre | VLAN-ID | IPv4            | IPv6                   |
| ------ | ------- | --------------- | ---------------------- |
| CCNP   | 10      | 192.168.10.0/24 | 3001:ABCD:ABCD:10::/64 |
| CCIE   | 20      | 192.168.20.0/24 | 3001:ABCD:ABCD:20::/64 |
| CCNA   | 30      | 192.168.30.0/24 | 3001:ABCD:ABCD:30::/64 |
| CCENT  | 40      | 192.168.40.0/24 | 3001:ABCD:ABCD:40::/64 |
| NATIVA | 90      | N/A             | N/A                    |
## Tabla Sumarizada IPv4
IP-network 1: `192.168.10.0/24`
IP-network 2: `192.168.20.0/24`
IP-network 3: `192.168.30.0/24`
IP-network 4: `192.168.40.0/24`

| 1er <br>Octeto | 2do <br>Octeto | 128 | 64  | Corte | 32  | 16  | 8   | 4   | 2   | 1   | 4to <br>Octeto |
| -------------- | -------------- | --- | --- | ----- | --- | --- | --- | --- | --- | --- | -------------- |
| 192            | 168            | 0   | 0   | /     | 0   | 0   | 1   | 0   | 1   | 0   | 0              |
| 192            | 168            | 0   | 0   | /     | 0   | 1   | 0   | 1   | 0   | 0   | 0              |
| 192            | 168            | 0   | 0   | /     | 0   | 1   | 1   | 1   | 1   | 0   | 0              |
| 192            | 168            | 0   | 0   | /     | 1   | 0   | 1   | 0   | 0   | 0   | 0              |

IP Sumarizada: `192.168.0.0/18`
## Tabla Sumarizada IPv6
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

| Network IPv6           | Diferencia | 8   | 4   | 2   | 1   | \|  | 8   | 4   | 2   | 1   | \|  | 8   | Corte | 4   | 2   | 1   | \|  | 8   | 4   | 2   | 1   | \|  |
| ---------------------- | ---------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | ----- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 3001:ABCD:ABCD:10::/64 | 0010       | 0   | 0   | 0   | 0   | \|  | 0   | 0   | 0   | 0   | \|  | 0   | /     | 0   | 0   | 1   | \|  | 0   | 0   | 0   | 0   | \|  |
| 3001:ABCD:ABCD:20::/64 | 0020       | 0   | 0   | 0   | 0   | \|  | 0   | 0   | 0   | 0   | \|  | 0   | /     | 0   | 1   | 0   | \|  | 0   | 0   | 0   | 0   | \|  |
| 3001:ABCD:ABCD:30::/64 | 0030       | 0   | 0   | 0   | 0   | \|  | 0   | 0   | 0   | 0   | \|  | 0   | /     | 0   | 1   | 1   | \|  | 0   | 0   | 0   | 0   | \|  |
| 3001:ABCD:ABCD:40::/64 | 0040       | 0   | 0   | 0   | 0   | \|  | 0   | 0   | 0   | 0   | \|  | 0   | /     | 1   | 0   | 0   | \|  | 0   | 0   | 0   | 0   | \|  |


La red Sumarizada es: `3001:ABCD:ABCD::/57`

# Configuracion
NOTA: La redistribucion esta mal a nivel IPv6 en R4 y Mal en IPv4 en R3, me dio flojera arreglarlo
## ALS1
```
enable
config t
errdisable recovery interval 30
ipv6 unicast-routing
int range e0/2-3
duplex half
exit
!# Configuracion Switching Basico
vlan 10
name CCNP
exit
vlan 20
name CCIE
exit
vlan 30
name CCNA
exit
vlan 40
name CCENT
exit
vlan 90
name NATIVA
exit
vtp mode transparent
int range e0/2-3
switchport mode access
switchport port-security
switchport port-security violation restrict
switchport port-security mac-address sticky
spanning-tree portfast
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
no shut
exit
int e0/2
switchport access vlan 10
exit
int e0/3
switchport access vlan 20
exit
int range e0/0-1,e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport trunk native vlan 90
switchport nonegotiate
no shut
exit
int range e0/0-1
channel-group 1 mode on
no shut
exit
int range e1/0-1
channel-group 4 mode auto
no shut
exit
int range e1/2-3
channel-group 2 mode auto
no shut
exit
!# Configuracion Switching Intermedio
spanning-tree mode mst
spanning-tree mst configuration
name ENCOR
revision 1
instance 2 vlan 10,20
instance 4 vlan 30,40
exit
spanning-tree mst hello-time 1
spanning-tree mst forward-time 8
spanning-tree mst max-age 10
!# VACL
mac access-list extended VACL-L2
permit host aabb.cc00.0c20 host aabb.cc00.0d30
exit
ip access-list extended VACL-L3
permit ip host 192.168.10.8 host 192.168.20.8
exit
vlan access-map FILTRO 10
match mac address VACL-L2
match ip address VACL-L3
action drop
exit
vlan access-map FILTRO 20
action forward
exit
vlan filter FILTRO vlan-list 10,20
end
copy running-config unix:
```
## ALS1 Snooping
Hay que revisar que funcione primero DHCP antes de configurar DHCP Snooping
```
config t
ip dhcp snooping
ip dhcp snooping vlan 10,20,30,40
no ip dhcp snooping information option
int range e0/2-3
ip dhcp snooping limit rate 3
exit
!# Esto debe ir al final por alguna razon
int range po1, po2, po3
ip dhcp snooping trust
exit
```
## ALS2
```
enable
config t
errdisable recovery interval 30
int range e0/2-3
duplex half
exit
ipv6 unicast-routing
vlan 10
name CCNP
exit
vlan 20
name CCIE
exit
vlan 30
name CCNA
exit
vlan 40
name CCENT
exit
vlan 90
name NATIVA
exit
vtp mode transparent
int range e0/2-3
switchport mode access
switchport port-security
switchport port-security violation restrict
switchport port-security mac-address sticky
spanning-tree portfast
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
no shut
exit
int e0/2
switchport access vlan 30
exit
int e0/3
switchport access vlan 40
exit
int range e0/0-1,e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport trunk native vlan 90
switchport nonegotiate
no shut
exit
int range e0/0-1
channel-group 1 mode on
no shut
exit
int range e1/0-1
channel-group 3 mode passive
no shut
exit
int range e1/2-3
channel-group 5 mode passive
no shut
exit
!# Configuracion Switching Intermedio
spanning-tree mode mst
spanning-tree mst configuration
name ENCOR
revision 1
instance 2 vlan 10,20
instance 4 vlan 30,40
exit
spanning-tree mst hello-time 1
spanning-tree mst forward-time 8
spanning-tree mst max-age 10
mac access-list extended VACL-L2
permit host aabb.cc00.0e20 host aabb.cc00.0f30
exit
ip access-list extended VACL-L3
permit ip host 192.168.30.8 host 192.168.40.8
exit
vlan access-map FILTRO 30
match mac address VACL-L2
match ip address VACL-L3
action drop
exit
vlan access-map FILTRO 40
action forward
exit
vlan filter FILTRO vlan-list 30,40
end
copy running-config unix:
```
## ALS2 Snooping
Revisa si funciona DHCP
```
ip dhcp snooping
ip dhcp snooping vlan 10,20,30,40
no ip dhcp snooping information option
int range e0/2-3
ip dhcp snooping limit rate 3
exit
!
!# Esto debe ir al final por alguna razon
int range po1,po3,po5
ip dhcp snooping trust
exit
```
## DLS1
```
enable
config t
errdisable recovery interval 30
int e0/3
duplex half
exit
ipv6 unicast-routing
hostname DLS1
vlan 10
name CCNP
exit
vlan 20
name CCIE
exit
vlan 30
name CCNA
exit
vlan 40
name CCENT
exit
vlan 90
name NATIVA
exit
vtp mode transparent
int range e0/0-1,e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport trunk native vlan 90
switchport nonegotiate
no shut
exit
int range e0/0-1
channel-group 6 mode on
no shut
exit
int range e1/0-1
channel-group 3 mode active
no shut
exit
int range e1/2-3
channel-group 2 mode desirable
no shut
exit
!# SWITCHING INTERMEDIO
spanning-tree mode mst
spanning-tree mst configuration
name ENCOR
revision 1
instance 2 vlan 10,20
instance 4 vlan 30,40
exit
spanning-tree mst 2 root primary
spanning-tree mst 4 root secondary
spanning-tree mst hello-time 1
spanning-tree mst forward-time 8
spanning-tree mst max-age 10
end
copy running-config unix:
```
## DLS1 Snooping
Maldito DHCP
```
ip dhcp snooping
ip dhcp snooping vlan 10,20,30,40
no ip dhcp snooping information option
int range po2,po3,po6
ip dhcp snooping trust
exit
```
## DLS2
```
enable
config t
errdisable recovery interval 30
int e0/3
duplex half
exit
ipv6 unicast-routing
vlan 10
name CCNP
exit
vlan 20
name CCIE
exit
vlan 30
name CCNA
exit
vlan 40
name CCENT
exit
vlan 90
name NATIVA
exit
vtp mode transparent
int range e0/0-1,e0/3,e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport trunk native vlan 90
switchport nonegotiate
no shut
exit
int range e0/0-1
channel-group 6 mode on
no shut
exit
int range e1/0-1
channel-group 4 mode desirable
no shut
exit
int range e1/2-3
channel-group 5 mode active
no shut
exit
spanning-tree mode mst
spanning-tree mst configuration
name ENCOR
revision 1
instance 2 vlan 10,20
instance 4 vlan 30,40
exit
spanning-tree mst 2 root secondary
spanning-tree mst 4 root primary
spanning-tree mst hello-time 1
spanning-tree mst forward-time 8
spanning-tree mst max-age 10
end
copy running-config unix:
```
## DLS2 Snooping
Vamos DHCP
```
ip dhcp snooping
ip dhcp snooping vlan 10,20,30,40
no ip dhcp snooping information option
int range po4,po5,po6
ip dhcp snooping trust
exit
```
## R1 Parte 1
Putty no copia los comandos enteros
```
enable
config t
ipv6 unicast-routing
int e0/3
no shut
exit
int e0/3.10
encapsulation dot1q 10
ip address 192.168.10.1 255.255.255.0
ipv6 address 3001:ABCD:ACBD:10::1/64
exit
int e0/3.20
encapsulation dot1q 20
ip address 192.168.20.1 255.255.255.0
ipv6 address 3001:ABCD:ACBD:20::1/64
exit
int e0/3.30
encapsulation dot1q 30
ip address 192.168.30.1 255.255.255.0
ipv6 address 3001:ABCD:ACBD:30::1/64
exit
int e0/3.40
encapsulation dot1q 40
ip address 192.168.40.1 255.255.255.0
ipv6 address 3001:ABCD:ACBD:40::1/64
exit
!
!# TRACK
track 1 int e0/0 line-protocol
delay down 5 up 3
exit
!
!# HSRP
int e0/3.10
standby version 2
standby 10 ip 192.168.10.10
standby 10 priority 90
standby 10 preempt
standby 10 timers msec 200 msec 800
standby 100 ipv6 3001:ABCD:ABCD:10::10/64
standby 100 priority 120
standby 100 preempt
standby 100 timers 1 3
standby 100 track 1 decrement 31
exit
int e0/3.20
standby version 2
standby 20 ip 192.168.20.10
standby 20 priority 90
standby 20 preempt
standby 20 timers msec 200 msec 800
standby 200 ipv6 3001:ABCD:ABCD:20::10/64
standby 200 priority 120
standby 200 preempt
standby 200 timers 1 3
standby 200 track 1 decrement 31
exit
int e0/3.30
standby version 2
standby 30 ip 192.168.30.10
standby 30 priority 90
standby 30 preempt
standby 30 timers msec 200 msec 800
standby 300 ipv6 3001:ABCD:ABCD:30::10/64
standby 300 priority 120
standby 300 preempt
standby 300 timers 1 3
standby 300 track 1 decrement 31
exit
int e0/3.40
standby version 2
standby 40 ip 192.168.40.10
standby 40 priority 90
standby 40 preempt
standby 40 timers msec 200 msec 800
standby 400 ipv6 3001:ABCD:ABCD:40::10/64
standby 400 priority 120
standby 400 preempt
standby 400 timers 1 3
standby 400 track 1 decrement 31
exit
!# VPN-SITE-TO-SITE
crypto isakmp policy 1
authentication pre-share
encryption aes 256
hash sha
group 5
lifetime 86400
exit
crypto isakmp key ccnpv8 address 192.168.23.2
crypto ipsec transform-set CASO-3 esp-aes 256 esp-sha-hmac
exit
ip access-list extended PROTECCION
permit ip 172.16.10.0 0.0.0.255 172.16.20.0 0.0.0.255
exit
```
## R1  Parte 2
Putty no copia todo correctamente
```
crypto map MAPA-PRUEBA3 1 ipsec-isakmp
match address PROTECCION
set transform-set CASO-3
set peer 192.168.23.2
exit
int e0/0
crypto map MAPA-PRUEBA3
exit
!# EIGRP
router eigrp 123
eigrp router-id 1.1.1.1
network 192.168.13.1 255.255.255.255
network 172.16.10.0 255.255.255.0
network 192.168.10.0 255.255.255.0
network 192.168.20.0 255.255.255.0
network 192.168.30.0 255.255.255.0
network 192.168.40.0 255.255.255.0
passive-interface e0/3.10
passive-interface e0/3.20
passive-interface e0/3.30
passive-interface e0/3.40
!# EIGRPv6
ipv6 router eigrp 123
router-id 1.1.1.1
passive-interface e0/3.10
passive-interface e0/3.20
passive-interface e0/3.30
passive-interface e0/3.40
exit
int e0/3.10
ipv6 eigrp 123
ip helper-address 192.168.13.3
exit
int e0/3.20
ipv6 eigrp 123
ip helper-address 192.168.13.3
exit
int e0/3.30
ipv6 eigrp 123
ip helper-address 192.168.13.3
exit
int e0/3.40
ipv6 eigrp 123
ip helper-address 192.168.13.3
exit
int loopback 0
ipv6 eigrp 123
exit
int e0/0
ipv6 eigrp 123
ip summary-address eigrp 123 192.168.0.0 255.255.192.0
ipv6 summary-address eigrp 123 3001:ABCD:ABCD::/57
exit
```
## R2 Parte 1
Putty no copia correctamente
```
enable
config t
ipv6 unicast-routing
int e0/3
no shut
exit
int e0/3.10
encapsulation dot1q 10
ip address 192.168.10.2 255.255.255.0
ipv6 address 3001:ABCD:ACBD:10::2/64
exit
int e0/3.20
encapsulation dot1q 20
ip address 192.168.20.2 255.255.255.0
ipv6 address 3001:ABCD:ACBD:20::2/64
exit
int e0/3.30
encapsulation dot1q 30
ip address 192.168.30.2 255.255.255.0
ipv6 address 3001:ABCD:ACBD:30::2/64
exit
int e0/3.40
encapsulation dot1q 40
ip address 192.168.40.2 255.255.255.0
ipv6 address 3001:ABCD:ACBD:40::2/64
exit
!# IP SLA
ip sla 1
icmp-echo 4.4.4.4
frecuency 5
exit
ip sla schedule 1 start-time now life forever
track 1 ip sla 1 reachability
delay down 5 up 3
exit
!# HSRP
int e0/3.10
standby version 2
standby 10 ip 192.168.10.10
standby 10 priority 120
standby 10 preempt
standby 10 timers msec 200 msec 800
standby 10 track 1 decrement 31
standby 100 ipv6 3001:ABCD:ABCD:10::10/64
standby 100 priority 90
standby 100 preempt
standby 100 timers 1 3
exit
int e0/3.20
standby version 2
standby 20 ip 192.168.20.10
standby 20 priority 120
standby 20 preempt
standby 20 track 1 decrement 31
standby 20 timers msec 200 msec 800
standby 200 ipv6 3001:ABCD:ABCD:20::10/64
standby 200 priority 90
standby 200 preempt
standby 200 timers 1 3
exit
int e0/3.30
standby version 2
standby 30 ip 192.168.30.10
standby 30 priority 120
standby 30 preempt
standby 30 track 1 decrement 31
standby 30 timers msec 200 msec 800
standby 300 ipv6 3001:ABCD:ABCD:30::10/64
standby 300 priority 90
standby 300 preempt
standby 300 timers 1 3
exit
int e0/3.40
standby version 2
standby 40 ip 192.168.40.10
standby 40 priority 120
standby 40 preempt
standby 40 track 1 decrement 31
standby 40 timers msec 200 msec 800
standby 400 ipv6 3001:ABCD:ABCD:40::10/64
standby 400 priority 90
standby 400 preempt
standby 400 timers 1 3
exit
!# VPN-SITE-TO-SITE
crypto isakmp policy 1
authentication pre-share
encryption aes 256
hash sha
group 5
lifetime 86400
exit
crypto isakmp key ccnpv8 address 192.168.13.1
crypto ipsec transform-set CASO-3 esp-aes 256 esp-sha-hmac
exit
ip access-list extended PROTECCION
permit ip 172.16.20.0 0.0.0.255 172.16.10.0 0.0.0.255
exit
```
## R2 Parte 2
Putty no se configura correctamente
```
crypto map MAPA-PRUEBA3 1 ipsec-isakmp
match address PROTECCION
set transform-set CASO-3
set peer 192.168.13.1
exit
int e0/1
crypto map MAPA-PRUEBA3
exit
!# EIGRP
router eigrp 123
eigrp router-id 2.2.2.2
network 172.16.20.0 255.255.255.0
network 192.168.10.0 255.255.255.0
network 192.168.20.0 255.255.255.0
network 192.168.30.0 255.255.255.0
network 192.168.40.0 255.255.255.0
network 192.168.23.2 255.255.255.255
passive-interface e0/3.10
passive-interface e0/3.20
passive-interface e0/3.30
passive-interface e0/3.40
exit
!# EIGRPv6
ipv6 router eigrp 123
router-id 2.2.2.2
passive-interface e0/3.10
passive-interface e0/3.20
passive-interface e0/3.30
passive-interface e0/3.40
exit
int e0/3.10
ipv6 eigrp 123
ip helper-address 192.168.23.3
exit
int e0/3.20
ipv6 eigrp 123
ip helper-address 192.168.23.3
exit
int e0/3.30
ipv6 eigrp 123
ip helper-address 192.168.23.3
exit
int e0/3.40
ipv6 eigrp 123
ip helper-address 192.168.23.3
exit
int loopback 0
ipv6 eigrp 123
exit
int e0/1
ipv6 eigrp 123
ip summary-address eigrp 123 192.168.0.0 255.255.192.0
ipv6 summary-address eigrp 123 3001:ABCD:ABCD::/57
exit
exit
```
## R2 Visualizacion VPN
```
ping 172.16.10.1 source loopback 0
show crypto ipsec sa
show crypto isakmp sa
```
## R3 Parte 1
Odio esto
```
enable
config t
ipv6 unicast-routing
!# EIGRP
router eigrp 123
eigrp router-id 3.3.3.3
network 192.168.13.3 255.255.255.255
network 192.168.23.3 255.255.255.255
exit
ipv6 router eigrp 123
eigrp router-id 3.3.3.3
exit
int range e0/0-1
ipv6 eigrp 123
exit
!# OSFP
router ospf 1
router-id 3.3.3.3
network 192.168.34.3 0.0.0.0 area 0
exit
!# OSFPv3
ipv6 router ospf 1
router-id 3.3.3.3
exit
int e0/2
ipv6 ospf 1 area 0
exit
!# DHCP
ip dhcp excluded-address 192.168.10.1 192.168.10.7
ip dhcp excluded-address 192.168.20.1 192.168.20.7
ip dhcp excluded-address 192.168.30.1 192.168.30.7
ip dhcp excluded-address 192.168.40.1 192.168.40.7
ip dhcp pool VLAN10
network 192.168.10.0 255.255.255.0
default-router 192.168.10.10
dns-server 4.4.4.4 8.8.8.8
exit
ip dhcp pool VLAN20
network 192.168.20.0 255.255.255.0
default-router 192.168.20.10
dns-server 4.4.4.4 8.8.8.8
exit
ip dhcp pool VLAN30
network 192.168.30.0 255.255.255.0
default-router 192.168.30.10
dns-server 4.4.4.4 8.8.8.8
exit
ip dhcp pool VLAN40
network 192.168.40.0 255.255.255.0
default-router 192.168.40.10
dns-server 4.4.4.4 8.8.8.8
exit
```
## R3 Parte 2
Putty un Desastre
```
!# MODO REDISTRIBUCION DESDE 0
!# EIGRP A OSPF
!# IPv4 Route-Map
!# IPv6 Directa
!
!# RUTAS EIGRP -> OSPFv2
ip prefix-list RUTAS-EIGRP permit 192.168.13.0/24
ip prefix-list RUTAS-EIGRP permit 192.168.23.0/24
ip prefix-list RUTAS-EIGRP permit 172.16.10.0/24
ip prefix-list RUTAS-EIGRP permit 172.16.20.0/24
ip prefix-list RUTAS-EIGRP permit 172.168.0.0/18
ip prefix-list RUTAS-EIGRP permit 172.168.10.0/24
ip prefix-list RUTAS-EIGRP permit 172.168.20.0/24
ip prefix-list RUTAS-EIGRP permit 172.168.30.0/24
ip prefix-list RUTAS-EIGRP permit 172.168.40.0/24
route-map OSPFv2 permit
match ip address prefix-list RUTAS-EIGRP
set metric 35
set metric-type type-1
exit
router ospf 1
redistribute eigrp 123 subnets route-map OSPFv2
exit
!# RUTAS OSPFv2 -> EIGRP
ip prefix-list RUTAS-OSPFv2 permit 192.168.34.0/24
route-map EIGRP permit
match ip address prefix-list RUTAS-OSPFv2
set metric 10000 100 255 1 1500
router eigrp 123
redistribute ospf 1 route-map EIGRP
exit
!# RUTAS EIGRP -> OSPFv3 Directa IPv6
ipv6 router ospf 1
redistribute eigrp 123 include-connected
exit
!# RUTAS OSPFv3 -> EIGRP Directa IPv6
ipv6 router eigrp 123
redistribute ospf 1 metric 10000 100 255 1 1500 include-connected
exit
```
## R3 Visualizar DHCP
```
show ip dhcp binding
```
## R4
```
enable
config t
ipv6 unicast-routing
!# OSPFv2
router ospf 1
router-id 4.4.4.4
default-information originate
network 192.168.34.1 0.0.0.0 area 0
exit
!# OSPFv3
ipv6 router ospf 1
router-id 4.4.4.4
default-information originate
exit
int e0/2
ipv6 ospf 1 area 0
exit
!# Rutas Estaticas
ip route 0.0.0.0 0.0.0.0 200.0.0.2
ipv6 route ::/0 3001:ABCD:ABCD:200::2
ip route 0.0.0.0 0.0.0.0 201.0.0.2
ipv6 route ::/0 3001:ABCD:ABCD:201::2
!# BGP
router bgp 64600
bgp router-id 4.4.4.4
address-family ipv4
neighbor 200.0.0.2 remote-as 100
neighbor 201.0.0.2 remote-as 200
exit
address-family ipv6
neighbor 3001:ABCD:ABCD:200::2 remote-as 100
neighbor 3001:ABCD:ABCD:201::2 remote-as 200
exit
!# Modo Redistribucion 
!# RUTAS OSPFv2 -> BGP
!# Route-MAP
ip prefix-list RUTAS-OSPF permit 192.168.34.0/24
route-map BGP permit
match ip address prefix-list RUTAS-OSPF
router bgp 64600
redistribute ospf 1 route-map BGP
exit
!# RUTAS BGP -> OSPFv2
!# Route-MAP
ip prefix-list RUTAS-BGP permit 200.0.0.0/30
ip prefix-list RUTAS-BGP permit 201.0.0.0/30
ip prefix-list RUTAS-BGP permit 202.0.0.0/30
ip prefix-list RUTAS-BGP permit 203.0.0.0/30
ip prefix-list RUTAS-BGP permit 205.0.0.0/30
ip prefix-list RUTAS-BGP permit 8.8.8.8/32
ip prefix-list RUTAS-BGP permit 4.4.4.4/32
route-map OSPF permit
match ip address prefix-list RUTAS-BGP
set metric 35
set metric-type type-1
router ospf 1
redistribute bgp 64600 subnets route-map OSPF
exit
!# RUTAS OSPFv3 -> MP-BGP
!# Directa
router bgp 64600
address-family ipv6
redistribute ospf 1 metric 35 include-connected
exit
!# RUTAS MP-BGP -> OSPFv3
!# Directa
ipv6 router ospf 1
redistribute bgp 64600
exit
!# MP-BGP IPv6 -> ISP-1
!# Local Preference, mas peso menor ruta
route-map IPv6-ISP-1 permit
set local-preference 200
exit
!# MP-BGP IPv4 -> ISP-2
route-map IPv4-ISP-2 permit
set local-preference 200
exit
router bgp 64600
address-family ipv4
neighbor 201.0.0.2 route-map IPv4-ISP-2 in
exit
address-family ipv6
neighbor 3001:ABCD:ABCD:200::2 route-map IPv6-ISP-1 in
exit
```
## ISP-1
```
enable
config t
ipv6 unicast-routing
!#BGP
router bgp 100
bgp router-id 5.5.5.5
address-family ipv4
network 8.8.8.8 mask 255.255.255.255
neighbor 200.0.0.1 remote-as 64600
neighbor 202.0.0.1 remote-as 65000
exit
address-family ipv6
network 3001:ABCD:ABCD:8::8/128
neighbor 3001:ABCD:ABCD:200::1 remote-as 64600
neighbor 3001:ABCD:ABCD:202::1 remote-as 65000
exit
!# MP-BGP MED IPv6 500 in
route-map MED permit
set metric 500
exit
router bgp 100
address-family ipv6
neighbor 3001:ABCD:ABCD:200::1 route-map MED in
neighbor 3001:ABCD:ABCD:202::1 route-map MED in
exit
do clear bgp ipv6 unicast * soft
```
## ISP-2
```
enable
config t
ipv6 unicast-routing
router bgp 200
bgp router-id 6.6.6.6
address-family ipv4
network 4.4.4.4 mask 255.255.255.255
neighbor 201.0.0.1 remote-as 64600
neighbor 203.0.0.1 remote-as 65000
exit
address-family ipv6
network 3001:ABCD:ABCD:4::4/128
neighbor 3001:ABCD:ABCD:201::1 remote-as 64600
neighbor 3001:ABCD:ABCD:203::1 remote-as 65000
exit
```
## BR
```
enable
config t
ipv6 unicast-routing
router bgp 65000
bgp router-id 7.7.7.7
address-family ipv4
network 205.0.0.0 mask 255.255.255.252
neighbor 202.0.0.2 remote-as 100
neighbor 203.0.0.2 remote-as 200
exit
address-family ipv6
network 3001:ABCD:ABCD:205::/112
neighbor 3001:ABCD:ABCD:202::2 remote-as 100
neighbor 3001:ABCD:ABCD:203::2 remote-as 200
exit
exit
exit

```