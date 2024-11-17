# Info
## Imagen Topologia
![500](https://slink.proxylivy.work/image/9c0696a0-c4a4-4135-b98e-225064c8d829.png)
## Contexto
La empresa “MI NUEVA PERA S.A” lo ha solicitado de sus servicios para la implementación de protocolos de
enrutamiento IGP/EGP para lograr el acceso a una red externa que opera con direccionamiento IPv4/IPv6
# Requerimientos
## Configuración de Switching:
• Implementar microsegmentación de dominio de broadcast, según ID propuesto y nombre de referencia.
• Asignar las interfaces correspondientes a las VLANs de Datos.
• Implementar algún tipo de seguridad que permita aprender un máximo de 3 direcciones MAC. En caso de sobrepasar la cantidad, no deberá aceptar ningún frame adicional.
• Implementar solución de agregado de enlace L2, en base a protocolos propuestos. Es importante que un extremo negocie y otro escuche.
• Dentro de las interfaces lógicas resultantes deberá permitir solo el paso de VLAN de Datos creadas, y manteniendo el protocolo DTP desactivado.
• Implementar RPVST+ en todos los SW.
• Implementar mecanismo que equipos finales no puedan solicitar más de 3 IP. En caso de ocurrir, interfaz deberá pasar a condición de err-disable.
• Los equipos finales no deben enviar ni recibir mensajes BPDU, además de pasar automáticamente al estado de FWD.

## Configuración de Enrutamiento:
• Router R2 debe proveer de direccionamiento en IPv4 a equipos finales. Debe excluir las primeras 7 direcciones IP.
• Implementar protocolos de enrutamiento propuestos en IPv4/IPv6, procurando configurar router-id a libertad. Es importante configurar interfaces pasivas según sea necesario.
• Router R1 debe anunciar las rutas de las VLANs de manera sumarizada a R2 a nivel IPv4/IPv6.
• Redistribuir protocolo IGP a nivel de IPv4 con mapas de rutas. Las rutas que provienen desde EIGRP hacia OSPF se debe añadir una métrica base de 50 y la métrica no debe variar. Para el caso de las rutas de OSPF a EIGRP utilizarán los valores de la interfaz Ethernet 0/0. Para IPv6 la redistribución será de manera directa.
• Implementar solución de MP-BGP para garantizar que sucursal principal alcance a red remota de equipo EDGE en IPv4/IPv6. Las loopbacks deben participar como prefijos para BGP.
• En router BR, manipular atributo MED, donde la loopback tenga una métrica 75 para IPv4, mediante mapas de rutas.
• En router ISP, deberá manipular atributo AS-PATH para que enlace hacia BORDE tengan 3 saltos adicionales con el AS 100 a nivel de IPv4, realizar implementación mediante mapas de rutas.
• La redistribución entre protocolos de enrutamiento IGP y EGP a nivel de IPv4/IPv6 será de manera directa.
• Comprobar conectividad completa en la topología

## Sumarizacion de redes
## Tabla Sumarizacion 
IP 1: 172.16.10.0/24
IP 2: 172.16.20.0/24
IP 3: 172.16.30.0/24

| 1er <br>Octeto | 2do <br>Octeto | 128 | 64  | 32  | Corte | 16  | 8   | 4   | 2   | 1   | 4to <br>Octeto |
| -------------- | -------------- | --- | --- | --- | ----- | --- | --- | --- | --- | --- | -------------- |
| 172            | 16             | 0   | 0   | 0   | /     | 0   | 1   | 0   | 1   | 0   | 0              |
| 172            | 16             | 0   | 0   | 0   | /     | 1   | 0   | 1   | 0   | 0   | 0              |
| 172            | 16             | 0   | 0   | 0   | /     | 1   | 1   | 1   | 1   | 0   | 0              |
IP Sumarizada: `172.16.0.0/19`
## IPV6
0-9

| A   | 10  |
| --- | --- |
| B   | 11  |
| C   | 12  |
| D   | 13  |
| E   | 14  |
| F   | 15  |

| IPv6                  | 1er Octeto | 2do<br>Octeto | 3er Octeto | 8   | 4   | 2   | 1   | \|  | 8   | 4   | 2   | 1   | \|  | 8   | 4   | 2   | 1   | \|  | 8   | 4   | 2   | 1   | \|  |     |
| --------------------- | ---------- | ------------- | ---------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 3001:ACAD:ACAD:A::/64 |            |               |            |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |
| 3001:ACAD:ACAD:B::/64 |            |               |            |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |
| 3001:ACAD:ACAD:C::/64 |            |               |            |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |
La red Sumarizada es: ``

# Configuracion
## ALS1
```
enable
config t
ipv6 unicast-routing
vlan 10
name ROUTE
exit
vlan 20
name SWITCH
exit
vlan 30
name TSHOOT
exit
vtp mode transparent
int range e0/1-2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30
switchport nonegotiate
exit
int range e1/0-2
switchport mode access
switchport port-security
switchport port-security maximum 3
switchport port-security violation shutdown
switchport port-security mac-address sticky
spanning-tree portfast
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
exit
int e1/0
switchport access vlan 10
exit
int e1/1
switchport access vlan 20
exit
int e1/2
switchport access vlan 30
exit
int range e0/1-2
channel-group 1 mode passive
exit
spanning-tree mode rapid-pvst
ip dhcp snooping
ip dhcp snooping vlan 10,20,30
no ip dhcp snooping information option
int range e0/1-2
ip dhcp snooping trust
exit
int range e1/0-2
ip dhcp snooping limit rate 3
exit
copy running-config unix:
```
## ALS2
```
enable
config t
ipv6 unicast-routing
vlan 10
name ROUTE
exit
vlan 20
name SWITCH
exit
vlan 30
name TSHOOT
exit
vtp mode transparent
int range e0/0-2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30
switchport nonegotiate
exit
int range e0/1-2
channel-group 1 mode active
exit
spanning-tree mode rapid-pvst
ip dhcp snooping
ip dhcp snooping vlan 10,20,30
no ip dhcp snooping information option
int e0/0
ip dhcp snooping trust
exit
end
copy running-config unix:
```
## R1
```
enable
config t
ipv6 unicast-routing
int e0/0
no shut
exit
int e0/0.10
encapsulation dot1q 10
ip address 172.16.10.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:A::1/64
exit
int e0/0.20
encapsulation dot1q 20
ip address 172.16.20.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:B::1/64
exit
int e0/0.30
encapsulation dot1q 30
ip address 172.16.30.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:C::1/64
exit
router eigrp 123
router-id 1.1.1.1
network 192.168.12.2 0.0.0.0
network 172.16.10.0 255.255.255.0
network 172.16.20.0 255.255.255.0
network 172.16.30.0 255.255.255.0
passive-interface e0/0.10
passive-interface e0/0.20
passive-interface e0/0.30
exit
ipv6 router eigrp 123
router-id 1.1.1.1
passive-interface e0/0.10
passive-interface e0/0.20
passive-interface e0/0.30
exit
int e0/0.10
ipv6 eigrp 123
exit
int e0/0.20
ipv6 eigrp 123
exit
int e0/0.30
ipv6 eigrp 123
exit
!
ip dhcp excluded-address 172.16.10.1 172.16.10.7
ip dhcp excluded-address 172.16.20.1 172.16.20.7
ip dhcp excluded-address 172.16.30.1 172.16.30.7
!
ip dhcp pool VLAN10
network 172.16.10.0 255.255.255.0
default-router 172.16.10.1
dns-server 8.8.8.8
exit
ip dhcp pool VLAN20
network 172.16.20.0 255.255.255.0
default-router 172.16.20.1
dns-server 8.8.8.8
exit
ip dhcp pool VLAN30
network 172.16.30.0 255.255.255.0
default-router 172.16.30.1
dns-server 8.8.8.8
exit
end
copy running-config unix:
```
## R2
```
enable
config t
ipv6 unicast-routing
router ospf 1 
router-id 2.2.2.2
network 192.168.14.4 0.0.0.0 area 0
exit
!
router eigrp 123
router-id 2.2.2.2
network 192.168.12.1 0.0.0.0
exit
!
ipv6 router ospf 1
router-id 2.2.2.2
exit
!
ipv6 router eigrp 123
router-id 2.2.2.2
exit
int e0/0
ipv6 ospf 1 area 0
exit
int e0/1
ipv6 eigrp 123
exit
!# EIGRP -> OSPF
ip prefix-list EIGRP permit 192.168.12.0/24
route-map OSPF permit
match ip address prefix-list EIGRP
set metric 50
set metric-type type-1
exit
router ospf 1
redistribute eigrp 123 subnets route-map OSPF
exit
ip prefix-list OSPF permit 192.168.14.0/24
route-map EIGRP permit
match ip address prefix-list OSPF
set metric 10000 1000 255 1 1500
router eigrp 123
redistribute ospf 1 route-map EIGRP
exit
ipv6 router ospf 1
redistribute eigrp 123 include-connected
exit
ipv6 router eigrp 123
redistribute ospf 1 metric 10000 1000 255 1 1500 include-connected
exit
end
copy running-config unix:

```
## BORDE
```
enable
config t
ipv6 unicast-routing
no ip domain-name
router ospf 1
router-id 3.3.3.3
network 192.168.14.1 0.0.0.0 area 0
exit
router bgp 65000
bgp router-id 3.3.3.3
neighbor 3001:ACAD:ACAD:201::2 remote-as 100
neighbor 201.0.0.2 remote-as 100
!
address-family ipv4
no neighbor 3001:ACAD:ACAD:201::2 activate
neighbor 201.0.0.2 activate
exit-address-family
!
address-family ipv6
neighbor 3001:ACAD:ACAD:201::2 activate
exit-address-family
exit
end
copy running-config unix:
```
## ISP
```
enable
config t
ipv6 unicast-routing
interface Loopback0
ip address 8.8.8.8 255.255.255.255
ipv6 address 3001:ACAD:ACAD:8::8/128
exit
interface Ethernet0/0
ip address 200.0.0.2 255.255.255.252
ipv6 address 3001:ACAD:ACAD:200::2/112
!
interface Ethernet0/1
ip address 201.0.0.2 255.255.255.252
ipv6 address 3001:ACAD:ACAD:201::2/112
!
router bgp 100
bgp router-id 4.4.4.4
bgp log-neighbor-changes
!
address-family ipv4
network 8.8.8.8 mask 255.255.255.255
neighbor 200.0.0.1 remote-as 200
neighbor 201.0.0.1 remote-as 65000
exit-address-family
!
address-family ipv6
network 3001:ACAD:ACAD:8::8/128
neighbor 3001:ACAD:ACAD:200::1 remote-as 200
neighbor 3001:ACAD:ACAD:201::1 remote-as 65000
exit-address-family
!# AS-PATH
route-map ASv4 permit
set as-path prepend 100 100 100
router bgp 100
address-family ipv4
neighbor 200.0.0.1 route-map ASv4 out
exit
end
copy running-config unix:
```
## EDGE
```
enable
config t
ipv6 unicast-routing
interface Loopback0
 ip address 192.168.100.1 255.255.255.255
 ipv6 address 3001:ACAD:ACAD:100::1/128
!
interface Ethernet0/0
 ip address 200.0.0.1 255.255.255.252
 ipv6 address 3001:ACAD:ACAD:200::1/112
!
router bgp 200
 bgp router-id 5.5.5.5
 bgp log-neighbor-changes
 neighbor 3001:ACAD:ACAD:200::2 remote-as 100
 neighbor 200.0.0.2 remote-as 100
 !
 address-family ipv4
  network 192.168.100.1 mask 255.255.255.255
  no neighbor 3001:ACAD:ACAD:200::2 activate
  neighbor 200.0.0.2 activate
 exit-address-family
 !
 address-family ipv6
  network 3001:ACAD:ACAD:100::1/128
  neighbor 3001:ACAD:ACAD:200::2 activate
 exit-address-family

route-map MEDv4 permit
set metric 75
exit
router bgp 200
address-family ipv4
neighbor 192.168.100.1 route-map MEDv4 in
neighbor 200.0.0.2 route-map MEDv4 in
exit
end
copy running-config unix:
```