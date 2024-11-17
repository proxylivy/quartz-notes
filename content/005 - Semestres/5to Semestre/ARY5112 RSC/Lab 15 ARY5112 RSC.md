# Info
## Imagen Topologia
![500](https://slink.proxylivy.work/image/01e68458-d588-43fd-9e17-4591e95716ba.png)
## Sobre
Implementacion de Seguridad Sobre Tuneles
> [!IMPORTANT] Importante
> Este parece ser el lab mas importante, ya que es un resumen de todo lo antes visto
## Tabla Direccionamiento

| Nombre        | VLAN-ID | IPv4         | IPv6                  |
| ------------- | ------- | ------------ | --------------------- |
| FINANZAS      | 21      | 10.0.21.0/24 | 3001:ABCD:ABCD:A::/64 |
| GESTION       | 22      | 10.0.31.0/24 | 3001:ABCD:ABCD:B::/64 |
| FUERZA-VENTAS | 23      | 10.0.41.0/24 | 3001:ABCD:ABCD:C::/64 |
| RRHH          | 24      | 10.0.51.0/24 | 3001:ABCD:ABCD:D::/64 |
| ADM           | 25      | 10.0.61.0/24 | 3001:ABCD:ABCD:E::/64 |
## Requerimientos
- Crear dominios de broadcast en todos los equipos propuestos, según ID y nombres propuestos.
- Implementar agregado de enlace en base a los números de grupos solicitados. (Lado Derecho Negocia, Lado Izquierdo Escucha)
- Habilitar PVST+ Rápido en todos los equipos de capa 2 de la topología.
- En los switches de acceso implementar algún tipo de seguridad donde aprenda un máximo de 2 direcciones MAC, desactivando el puerto en caso de error.
- En interfaces apropiadas, implementar mecanismos de estabilización de STP.
- VLAN21 no debe comunicarse con VLAN22 mediante PACL.
- VLAN23 no debe comunicarse con VLAN24 mediante VACL.
- Implementar enrutamiento intervlan tanto en R2 como R3.
- Implementar alta disponibilidad de capa 3, donde el tráfico de la VLAN21 y VLAN22 prefiera R2 y la VLAN23 y VLAN24 prefiera R3. Utilizar la ultima IP asignable como default-gateway para IPv4 y para IPv6 la ::FF.
- Implementar una VPN Site-to-Site entre R2 y R3. Utilizar parámetros a elección.
- Implementar protocolo de estado de enlace a nivel de IPv4/IPv6.
- Sumarizar las rutas del área 100, las cuales deben verse reflejadas en el área 0.
- Implementar MP-BGP, según AS correpondiente. Las loopbacks debe actuar como prefijos para este protocolo de enrutamiento.
- Influenciar atributo enlace entre ISP y BR, a través de route-map para que path aparezca con dos AS 200 en IPv4 y tres AS 200 en IPv6. 
- Realizar la redistribución entre IGP y EGP para ambos protocolos de enrutamiento a nivel de IPv4/IPv6.
- Comprobar conectividad completa en IPv4/IPv6.
# Configuracion
## SWA
```
enable
config t
vlan 21-25
exit
vtp mode transparent
int 0/3
duplex half
exit
int range e0/0-1, e1/0-3, e0/3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 21-25
switchport nonegotiate
exit
int range e1/2-3
channel-group 5 mode on
exit
int range e0/0-1
channel-group 2 mode active
exit
int range e1/0-1
channel-group 6 mode passive
exit
spanning-tree mode rapid-pvst
```
## SWB
```
enable
config t
vlan 21-25
exit
vtp mode transparent
int 0/3
duplex half
exit
int range e0/0-1, e1/0-3, e0/3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 21-25
switchport nonegotiate
exit
int range e0/0-1
channel-group 4 mode desirable
exit
int range e1/2-3
channel-group 3 mode on
exit
int range e1/0-1
channel-group 6 mode active
exit
spanning-tree mode rapid-pvst
```
## SWC
```
enable
config t
vlan 21-25
exit
vtp mode transparent
int range e0/2-3
duplex half
exit
int range e0/0-1, e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 21-25
switchport nonegotiate
exit
int range e1/2-3
channel-group 5 mode on
exit
int range e1/0-1
channel-group 1 mode auto
exit
int range e0/0-1
channel-group 4 mode auto
exit
spanning-tree mode rapid-pvst
int range e0/2-3
switchport mode access
switchport port-security
switchport port-security mac-address sticky
switchport port-security maximum 2
switchport port-security violation shutdown
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int e0/2
switchport access vlan 21
exit
int e0/2
switchport access vlan 22
exit
!# PACL
mac access-list extended NO-L2
deny host aabb.cc00.0920 host aabb.cc00.0a30
permit any any
exit
ip access-list extended NO-L3
deny ip host 10.0.21.5 host 10.0.31.5
permit ip any any
exit
int e0/2
mac access-group NO-L2 in
ip access-group NO-L3 in
exit
```
## SWD
```
enable
config t
vlan 21-25
exit
vtp mode transparent
int range e0/2-3
duplex half
exit
int range e0/0-1, e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 21-25
switchport nonegotiate
exit
int range e1/0-1
channel-group 1 mode desirable
exit
int range e1/2-3
channel-group 3 mode on
exit
int range e0/0-1
channel-group 2 mode passive
exit
spanning-tree mode rapid-pvst
int range e0/2-3
switchport mode access
switchport port-security
switchport port-security mac-address sticky
switchport port-security maximum 2
switchport port-security violation shutdown
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int e0/2
switchport access vlan 23
exit
int e0/2
switchport access vlan 24
exit
!# VACL
mac access-list extended VACL-L2
permit host aabb.cc00.0b20 host aabb.cc00.0c30
exit
ip access-list extended VACL-L3
permit ip host 10.0.41.5 host 10.0.51.5
exit
vlan access-map FILTRO 1
match mac address VACL-L2
match ip address VACL-L3
action drop
exit
vlan access-map FILTRO 2
action forward
exit
vlan filter FILTRO vlan-list 23-24
```
## R1
```
enable
config t
ipv6 unicast-routing
!# OSPFv2
router ospf 1
router-id 1.1.1.1
default-information originate
network 192.168.12.1 0.0.0.0 area 0
network 192.168.13.1 0.0.0.0 area 0
!# OSPFv3
ipv6 router ospf 1
router-id 1.1.1.1
default-information originate
exit
int range e0/1-2
ipv6 ospf 1 area 0
exit
ip route 0.0.0.0 0.0.0.0 209.50.0.2
ipv6 route ::/0 3001:ABCD:ABCD:209::2
!# BGP
router bgp 100
bgp router-id 1.1.1.1
address-family ipv4
neighbor 209.50.0.2 remote-as 200
exit
address-family ipv6
neighbor 3001:ABCD:ABCD:209::2 remote-as 200
exit
!# REDISTRIBUCION DIRECTA OSPF y BGP
router ospf 1
redistribute bgp 100 subnets
exit
router bgp 100
address-family ipv4
redistribute ospf 1 metric 1
exit
exit
ipv6 router ospf 1
redistribute bgp 100 metric 1
exit
router bgp 100
address-family ipv6
redistribute ospf 1 metric 1 include-connected
exit
exit
ip dhcp excluded-address 10.0.21.1 10.0.21.4
ip dhcp excluded-address 10.0.31.1 10.0.31.4
ip dhcp excluded-address 10.0.41.1 10.0.41.4
ip dhcp excluded-address 10.0.51.1 10.0.51.4
ip dhcp pool VLAN21
network 10.0.21.0 255.255.255.0
default-router 10.0.21.254
exit
ip dhcp pool VLAN22
network 10.0.31.0 255.255.255.0
default-router 10.0.31.254
exit
ip dhcp pool VLAN23
network 10.0.41.0 255.255.255.0
default-router 10.0.41.254
exit
ip dhcp pool VLAN24
network 10.0.51.0 255.255.255.0
default-router 10.0.51.254
exit
```
## R2
```
enable
config t
ipv6 unicast-routing
int e0/3
no switchport
no shut
exit
int e0/3.21
encapsulation dot1q 21
ip address 10.0.21.2 255.255.255.0
ipv6 address 3001:ABCD:ABCD:A::2/64
exit
int e0/3.22
encapsulation dot1q 22
ip address 10.0.31.2 255.255.255.0
ipv6 address 3001:ABCD:ABCD:B::2/64
exit
int e0/3.23
encapsulation dot1q 23
ip address 10.0.41.2 255.255.255.0
ipv6 address 3001:ABCD:ABCD:C::2/64
exit
int e0/3.24
encapsulation dot1q 24
ip address 10.0.51.2 255.255.255.0
ipv6 address 3001:ABCD:ABCD:D::2/64
exit
!# HSRP
int e0/3.21
standby version 2
standby 21 ip 10.0.21.254
standby 21 priority 120
standby 21 preempt
standby 211 ipv6 3001:ABCD:ABCD:A::FF/64
standby 211 priority 120
standby 211 preempt
exit
int e0/3.22
standby version 2
standby 22 ip 10.0.31.254
standby 22 priority 120
standby 22 preempt
standby 222 ipv6 3001:ABCD:ABCD:B::FF/64
standby 222 priority 120
standby 222 preempt
exit
int e0/3.23
standby version 2
standby 23 ip 10.0.41.254
standby 23 priority 90
standby 23 preempt
standby 233 ipv6 3001:ABCD:ABCD:C::FF/64
standby 233 priority 90
standby 233 preempt
exit
int e0/3.24
standby version 2
standby 24 ip 10.0.51.254
standby 24 priority 90
standby 24 preempt
standby 244 ipv6 3001:ABCD:ABCD:D::FF/64
standby 244 priority 90
standby 244 preempt
exit
!# VPN SITE-TO-SITE
crypto isakmp policy 1
authentication pre-share
encryption aes 256
hash sha
group 14
lifetime 86400
exit
crypto isakmp key perita address 192.168.13.3
crypto ipsec transform-set T esp-aes 256 esp-sha-hmac
exit
ip access-list extended PROTECCION
permit ip host 2.2.2.2 host 3.3.3.3
exit
crypto map MAPA 1 ipsec-isakmp
match address PROTECCION
set transform-set T
set peer 192.168.13.3
exit
int e0/1
crypto map MAPA
exit
!# OSPFv2
router ospf 1
router-id 2.2.2.2
network 2.2.2.2 0.0.0.0 area 100
network 10.0.21.2 0.0.0.0 area 100
network 10.0.31.2 0.0.0.0 area 100
network 10.0.41.2 0.0.0.0 area 100
network 10.0.51.2 0.0.0.0 area 100
network 10.0.21.254 0.0.0.0 area 100
network 10.0.31.254 0.0.0.0 area 100
network 10.0.41.254 0.0.0.0 area 100
network 10.0.51.254 0.0.0.0 area 100
network 192.168.12.1 0.0.0.0 area 0
passive-interface e0/3.21
passive-interface e0/3.22
passive-interface e0/3.23
passive-interface e0/3.24
!# OSPFv3
ipv6 router ospf 1
router-id 2.2.2.2
passive-interface e0/3.21
passive-interface e0/3.22
passive-interface e0/3.23
passive-interface e0/3.24
exit
int e0/3.21
ipv6 ospf 1 area 100
exit
int e0/3.22
ipv6 ospf 1 area 100
exit
int e0/3.23
ipv6 ospf 1 area 100
exit
int e0/3.24
ipv6 ospf 1 area 100
exit
int e0/1
ipv6 ospf 1 area 100
exit
int e0/3.21
ip helper-address 192.168.12.1
exit
int e0/3.22
ip helper-address 192.168.12.1
exit
int e0/3.23
ip helper-address 192.168.12.1
exit
int e0/3.24
ip helper-address 192.168.12.1
exit
```
## R3
```
enable
config t
ipv6 unicast-routing
int e0/3
no switchport
no shut
exit
int e0/3.21
encapsulation dot1q 21
ip address 10.0.21.3 255.255.255.0
ipv6 address 3001:ABCD:ABCD:A::3/64
exit
int e0/3.22
encapsulation dot1q 22
ip address 10.0.31.3 255.255.255.0
ipv6 address 3001:ABCD:ABCD:B::3/64
exit
int e0/3.23
encapsulation dot1q 23
ip address 10.0.41.3 255.255.255.0
ipv6 address 3001:ABCD:ABCD:C::3/64
exit
int e0/3.24
encapsulation dot1q 24
ip address 10.0.51.3 255.255.255.0
ipv6 address 3001:ABCD:ABCD:D::3/64
exit
!# HSRP
int e0/3.21
standby version 2
standby 21 ip 10.0.21.254
standby 21 priority 90
standby 21 preempt
standby 211 ipv6 3001:ABCD:ABCD:A::FF/64
standby 211 priority 90
standby 211 preempt
exit
int e0/3.22
standby version 2
standby 22 ip 10.0.31.254
standby 22 priority 90
standby 22 preempt
standby 222 ipv6 3001:ABCD:ABCD:B::FF/64
standby 222 priority 90
standby 222 preempt
exit
int e0/3.23
standby version 2
standby 23 ip 10.0.41.254
standby 23 priority 120
standby 23 preempt
standby 233 ipv6 3001:ABCD:ABCD:C::FF/64
standby 233 priority 120
standby 233 preempt
exit
int e0/3.24
standby version 2
standby 24 ip 10.0.51.254
standby 24 priority 120
standby 24 preempt
standby 244 ipv6 3001:ABCD:ABCD:D::FF/64
standby 244 priority 120
standby 244 preempt
exit
!# VPN SITE-TO-SITE
crypto isakmp policy 1
authentication pre-share
encryption aes 256
hash sha
group 14
lifetime 86400
exit
crypto isakmp key perita address 192.168.12.2
crypto ipsec transform-set T esp-aes 256 esp-sha-hmac
exit
ip access-list extended PROTECCION
permit ip host 3.3.3.3 host 2.2.2.2
exit
crypto map MAPA 1 ipsec-isakmp
match address PROTECCION
set transform-set T
set peer 192.168.12.2
exit
int e0/2
crypto map MAPA
exit
!# OSPF
router ospf 1
router-id 3.3.3.3
passive-interface e0/3.21
passive-interface e0/3.22
passive-interface e0/3.23
passive-interface e0/3.24
network 10.0.21.3 0.0.0.0 area 100
network 10.0.31.3 0.0.0.0 area 100
network 10.0.41.3 0.0.0.0 area 100
network 10.0.51.3 0.0.0.0 area 100
network 10.0.21.254 0.0.0.0 area 100
network 10.0.31.254 0.0.0.0 area 100
network 10.0.41.254 0.0.0.0 area 100
network 10.0.51.254 0.0.0.0 area 100
network 192.168.13.3 0.0.0.0 area 0
exit
!# OSPFv3
ipv6 router ospf 1
router-id 3.3.3.3
passive-interface e0/3.21
passive-interface e0/3.22
passive-interface e0/3.23
passive-interface e0/3.24
exit
int e0/3.21
ipv6 ospf 1 area 100
exit
int e0/3.22
ipv6 ospf 1 area 100
exit
int e0/3.23
ipv6 ospf 1 area 100
exit
int e0/3.24
ipv6 ospf 1 area 100
exit
int e0/2
ipv6 ospf 1 area 100
exit
int e0/3.21
ip helper-address 192.168.13.1
exit
int e0/3.22
ip helper-address 192.168.13.1
exit
int e0/3.23
ip helper-address 192.168.13.1
exit
int e0/3.24
ip helper-address 192.168.13.1
exit

```
## ISP
```
enable
config t
ipv6 unicast-routing
router bgp 200
bgp router-id 2.2.2.2
address-family ipv4
neighbor 209.50.0.1 remote-as 100
neighbor 220.50.0.1 remote-as 300
network 8.8.8.8 mask 255.255.255.255
exit
address-family ipv6
neighbor 3001:ABCD:ABCD:220::1 remote-as 300
neighbor 3001:ABCD:ABCD:209::1 remote-as 100
network 3001:ABCD:ABCD:8::8/128
exit
exit
route-map ATRIBUTO-IPV4
set as-path prepend 200 200
exit
route-map ATRIBUTO-IPV6
set as-path prepend 200 200 200
exit
router bgp 200
address-family ipv4
neighbor 220.50.0.1 route-map ATRIBUTO-IPV4 in
do clear ip bgp * soft
exit
address-family ipv6
neighbor 3001:ABCD:ABCD:220::1 route-map ATRIBUTO-IPV6 in
do clear ip bgp * soft
```
## BR
```
enable
config t
ipv6 unicast-routing
bgp router bgp 300
bgp router-id 3.3.3.3
address-family ipv4
neighbor 220.50.0.2 remote-as 200
network 192.168.100.1 mask 255.255.255.255
exit
address-family ipv6
neighbor 3001:ABCD:ABCD:220::2 remote-as 200
network 3001:ABCD:ABCD:100:1/128
exit

```