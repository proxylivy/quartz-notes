# Info
## Imagen Topologia
![500](https://slink.proxylivy.work/image/1fcb1671-b96d-47c8-84cb-c1afe7249543.png)
## Tabla Topologia

| Vlan | Nombre | IPv4           | IPv6                   |
| ---- | ------ | -------------- | ---------------------- |
| 10   | ENCOR  | 172.16.10.0/24 | 3001:ACAD:ACAD:10::/64 |
| 20   | ENARSI | 172.16.20.0/24 | 3001:ACAD:ACAD:20::/64 |
| 30   | ROUTE  | 172.16.30.0/24 | 3001:ACAD:ACAD:30::/64 |
| 40   | Switch | 172.16.40.0/24 | 3001:ACAD:ACAD:40::/64 |


## Objetivo
Configurar routing y switching avanzado que permita la conectividad a nivel empresarial y garantice acceso hacia Internet mediante EGP a nivel de IPv4/IPv6.
## Contexto
La empresa “MI SEGUNDA PERA EXTREME” ha implementado una red corporativa a través de una red de conmutación y enrutamiento que requiere un aumento en la eficiencia de su red, para el ordenamiento del tráfico a nivel de capa 2, y el posterior acceso a redes externas a nivel IPv4/IPv6, que permita que su red sea más robusta y resistente a posibles ataques en su red interna.
## Requerimientos
### Configuración de Switching Básico:
• [[010 - Protocolos/010.2 - Switching/VLAN|VLAN]] -> Implementar microsegmentación de dominio de broadcast, según ID propuesto y nombre de referencia.
• Permitir el paso de VLANs creadas por interfaces correspondientes.
• Desactivar [[010 - Protocolos/010.2 - Switching/DTP|DTP]] en todas las interfaces de los [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]].
• Asignar las interfaces correspondientes a las VLANs de Datos.
• Implementar algún tipo de [[010 - Protocolos/010.2 - Switching/Seguridad de Puerto|Seguridad de puerto]] que permita desactivar la puerta del SW en caso que exceda la cantidad máxima de [[020 - Conceptos/020.3 - Fundamentos/MAC|MAC]] por defecto que tiene los [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]].
• Implementar solución de [[010 - Protocolos/010.2 - Switching/Etherchannel|Agregado de enlaces]] L2, en base a protocolos propuestos. Toda conexión que comience en MLS deberá negociar de forma automática. Solo debe pasar las VLAN de datos respectivas.

### Configuración de Switching Intermedio:
• Implementar [[010 - Protocolos/010.2 - Switching/spanning-tree/802.1W (RSTP)|802.1W (RSTP)]] o RPVST+ en todos los [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]].
• Por políticas de la empresa la VLAN10 y VLAN20, deben ser root a través de ALS1 y root secondary a través de ALS2.
• Por políticas de la empresa la VLAN30 y VLAN40, deben ser root a través de ALS2 y root secondary a través de ALS1.
• Implementar mecanismo que equipos finales no puedan solicitar más de 2 IP por minuto. En caso de ocurrir, interfaz deberá pasar a condición de err-disable.
• Reducir a la mitad los temporizadores de protocolo RPVST+.
• Implementar mecanismos de estabilización de STP en interfaces de equipos finales, según corresponda.

### Configuración de Enrutamiento:
• Implementar protocolos de enrutamiento propuestos en IPv4/IPv6, configurando interfaces pasivas según corresponda.
• Utilizar a [[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]] RA para asignar IPv4 por [[010 - Protocolos/010.3 - Comunicaciones/DHCP|DHCP]] a los equipos finales. Se ha solicitado excluir las primeras 5 direcciones IP.
• Implementar solución de [[010 - Protocolos/010.1 - Routing/BGP/BGP-4|BGP-4]] para garantizar que sucursal principal alcance a red remota de equipo BR en IPv4/IPv6. Las loopbacks deben participar como prefijos para BGP.
• En router BR, manipular [[010 - Protocolos/010.1 - Routing/BGP/BGP#Atributo Med|Atributo MED]], donde la loopback tenga una métrica 50 para IPv4 y de 100 para IPv6. Realizar manipulación mediante mapas de rutas.
• En router ISP, deberá manipular [[010 - Protocolos/010.1 - Routing/BGP/BGP#Atributo AS-Path|BGP]] donde hacia BR tenga dos saltos adicionales en el atributo solicitado para IPv6.
• La redistribución entre protocolo estado de enlace y vector distancia a nivel IPv4 será mediante mapas de
rutas. Para el caso de IPv6 será de manera directa.
• La redistribución entre protocolos de enrutamiento IGP y EGP a nivel de IPv4/IPv6 será de manera directa.
• Comprobar conectividad completa en la topología.
# Configuracion
## ALS1
```
enable
config t
ipv6 unicast-routing
!# Troubleshooting
spanning-tree mode rapid-pvst
!# Config
vlan 10
name ENCOR
exit
vlan 20
name ENARSI
exit
vlan 30
name ROUTE
exit
vlan 40
name SWITCH
exit
vtp mode transparent
int e0/2
switchport mode access
switchport access vlan 10
switchport port-security
switchport port-security violation shutdown
switchport port-security mac-address sticky
spanning-tree portfast
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
exit
int e0/3
switchport mode access
switchport access vlan 20
switchport port-security
switchport port-security violation shutdown
switchport port-security mac-address sticky
spanning-tree portfast
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
exit
int range e0/0-1,e1/0-1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport nonegotiate
exit
int range e1/0-1
channel-group 2 mode passive
exit
int range e0/0-1
channel-group 6 mode on
exit
spanning-tree vlan 10,20 root primary
spanning-tree vlan 30,40 root secondary
ip dhcp snooping
ip dhcp snooping vlan 10,20,30,40
no ip dhcp snooping information option
int range e0/2-3
ip dhcp snooping limit rate 2
exit
int range e1/0-1
ip dhcp snooping trust
exit
!# Reducir temporizadores
spanning-tree vlan 10,20,30,40 hello-time 1
spanning-tree vlan 10,20,30,40 forward-time 8
spanning-tree vlan 10,20,30,40 max-age 10
exit
copy running-config unix:
```
## ALS2
```
enable
config t
ipv6 unicast-routing
!# Troubleshooting
spanning-tree mode rapid-pvst
!# Configuracion Normal
vlan 10
name ENCOR
exit
vlan 20
name ENARSI
exit
vlan 30
name ROUTE
exit
vlan 40
name SWITCH
exit
vtp mode transparent
int range e0/2-3
switchport mode access
switchport port-security
switchport port-security violation shutdown
switchport port-security mac-address sticky
spanning-tree portfast
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
exit
!
int e0/2
switchport access vlan 30
exit
int e0/3
switchport access vlan 40
exit
!
int range e0/0-1,e1/2-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport nonegotiate
exit
int range e0/0-1
channel-group 6 mode on
exit
int range e1/2-3
channel-group 4 mode auto
exit
spanning-tree vlan 30,40 root primary
spanning-tree vlan 10,20 root secondary
ip dhcp snooping
ip dhcp snooping vlan 10,20,30,40
no ip dhcp snooping information option
int range e1/2-3
ip dhcp snooping trust
exit
int range e0/2-3
ip dhcp snooping limit rate 2
exit
spanning-tree vlan 10,20,30,40 hello-time 1
spanning-tree vlan 10,20,30,40 forward-time 8
spanning-tree vlan 10,20,30,40 max-age 10
end
copy running-config unix:
```
## MLS
```
enable
config t
ipv6 unicast-routing
!# Troubleshooting
no vlan 10,11,12,13,14,110,120,130,140
spanning-tree mode rapid-pvst
!# Configuracion
vlan 10
name ENCOR
exit
vlan 20
name ENARSI
exit
vlan 30
name ROUTE
exit
vlan 40
name SWITCH
exit
vtp mode transparent
int range e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport nonegotiate
exit
ip dhcp snooping
ip dhcp snooping vlan 10,20,30,40
no ip dhcp snooping information option
int e0/2
duplex half
!
no switchport
!
ip address 192.168.13.1 255.255.255.252
ipv6 address 3001:ACAD:ACAD:13::1/112
exit
int range e1/0-1
channel-group 2 mode active
exit
int range e1/2-3
channel-group 4 mode desirable
exit
router ospf 1
router-id 2.2.2.2
network 192.168.13.2 0.0.0.0 area 0
network 172.16.10.0 0.0.0.255 area 0
network 172.16.20.0 0.0.0.255 area 0
network 172.16.30.0 0.0.0.255 area 0
network 172.16.40.0 0.0.0.255 area 0
exit
ipv6 router ospf 1
router-id 2.2.2.2
exit
int e0/2
ipv6 ospf 1 area 0
exit
ip routing
int vlan 10
ip address 172.16.10.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:10::1/64
ipv6 ospf 1 area 0
no shut
exit
int vlan 20
ip address 172.16.20.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:20::1/64
ipv6 ospf 1 area 0
no shut
exit
int vlan 30
ip address 172.16.30.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:30::1/64
ipv6 ospf 1 area 0
no shut
exit
int vlan 40
ip address 172.16.40.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:40::1/64
ipv6 ospf 1 area 0
no shut
exit
end
copy running-config unix:
```
## RA
```
enable
config t
ipv6 unicast-routing
!# Troubleshooting
hostname RA
int e0/0
no ip address 192.168.14.1 255.255.255.0
no ipv6 address 2022:ABCD:ABCD:14::1/64
no ipv6 ospf 1 area 0
exit
int e0/1
no ip address 192.168.12.1 255.255.255.0
no ipv6 address 2022:ABCD:ABCD:12::1/64
exit
no int loopback 1
router ospf 1
no network 192.168.12.1 0.0.0.0 area 0
no network 192.168.14.1 0.0.0.0 area 0
!# Configuracion Normal
int e0/0
ip address 172.16.100.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:100::1/64
no shut
int e0/2
ip address 192.168.13.2 255.255.255.252
ipv6 address 3001:ACAD:ACAD:ACAD:13::2/112
no shut
exit
router ospf 1
router-id 1.1.1.1
network 192.168.13.1 0.0.0.0 area 0
exit
ipv6 router ospf 1
router-id 1.1.1.1
exit
int e0/2
duplex half
ipv6 ospf 1 area 0
exit
router eigrp 123
eigrp router-id 1.1.1.1
network 172.16.100.2 0.0.0.0
exit
ipv6 router eigrp 123
eigrp router-id 1.1.1.1
exit
int e0/0
ipv6 eigrp 123
exit
ip dhcp excluded-address 172.16.10.1 172.16.10.5
ip dhcp excluded-address 172.16.20.1 172.16.20.5
ip dhcp excluded-address 172.16.30.1 172.16.30.5
ip dhcp excluded-address 172.16.40.1 172.16.40.5
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
ip dhcp pool VLAN40
network 172.16.40.0 255.255.255.0
default-router 172.16.40.1
dns-server 8.8.8.8
exit
!# EIGRP -> OSF
ip prefix-list EIGRP permit 172.16.100.0/24
route-map OSPF permit
match ip address prefix-list EIGRP
set metric 21
set metric-type type-1
router ospf 1
redistribute eigrp 123 subnets route-map OSPF
exit
!# OSFP -> EIGRP
ip prefix-list OSPF permit 192.168.13.0/30
route-map EIGRP permit
match ip address prefix-list OSPF
set metric 1000 1000 255 1 1500
router eigrp 123
redistribute ospf 1 route-map EIGRP
exit
!# Redistribucion Directa IPv6
ipv6 router ospf 1
redistribute eigrp 123 include-connected
exit
ipv6 router eigrp 123
redistribute ospf 1 metric 1000 1000 255 1 1500 include-connected
exit
end
copy running-config unix:
```
## RB
```
enable
config t
ipv6 unicast-routing
!# Troubleshooting
int e0/0
no ip address 172.16.100.2 255.255.255.252
exit
!# Ahora si empieza la configuracion
int e0/0
ip address 172.16.100.2 255.255.255.0
exit
router eigrp 123
eigrp router-id 3.3.3.3
network 172.16.100.1 0.0.0.0
exit
ipv6 router eigrp 123
eigrp router-id 3.3.3.3
exit
int e0/0
ipv6 eigrp 123
exit
!
router bgp 200
address-family ipv4
neighbor 219.50.0.2 remote-as 100
exit
address-family ipv6
neighbor 3001:ACAD:ACAD:219::2 remote-as 100
exit
end
copy running-config unix:
```
## ISP
```
enable
config t
ipv6 unicast-routing
no ip domain-lookup
router bgp 100
address-family ipv4
neighbor 219.50.0.1 remote-as 200
neighbor 220.50.0.1 remote-as 65500
exit
address-family ipv6
neighbor 3001:ACAD:ACAD:219::1 remote-as 200
neighbor 3001:ACAD:ACAD:220::1 remote-as 65500
exit
!# Contaminacion AS-PATH
route-map ASv6 permit
set as-path prepend 100 100
router bgp 100
neighbor 3001:ACAD:ACAD:220::1 route-map ASv6 out
exit
end
copy running-config unix:
```
## BR
```
enable
config t
ipv6 unicast-routing
router bgp 65500
address-family ipv4
neighbor 220.50.0.2 remote-as 100
neighbor 8.8.8.8 remote-as 65500
neighbor 8.8.8.8 update-source loopback 0
network 8.8.8.8 mask 255.255.255.255
exit
address-family ipv6
neighbor 3001:ACAD:ACAD:220::2 remote-as 100
neighbor 3001:ACAD:ACAD:8::8 remote-as 65500
network 3001:ACAD:ACAD:8::8/128
exit
!# Manipulacion de MED
route-map MEDv4 permit
set metric 50
exit
route-map MEDv6 permit
set metric 100
exit
router bgp 65500
address-family ipv4
neighbor 220.50.0.2 route-map MEDv4 in
exit
address-family ipv6
neighbor 3001:ACAD:ACAD:220::2 route-map MEDv6 in
exit
end
copy running-config unix:
```
