# Info
## Imagen Topologia
![500](https://slink.proxylivy.work/image/582d6523-80ab-4f84-9f7e-f003fe8d2a47.png)
## Sobre
Implementacion de PACL y VACL
## Requerimientos
- Crear dominios de broadcast en todos los equipos propuestos, según ID y nombres propuestos.
- Implementar agregado de enlace en base a los números de grupos solicitados.
- Habilitar PVST+ Rápido en todos los equipos de capa 2 de la topología.
- En los switches de acceso implementar algún tipo de seguridad donde aprenda un máximo de 2 direcciones MAC, desactivando el puerto en caso de error.
- Interfaces que conectan a equipos finales no deben enviar ni recibir BPDU.
- PC de VLAN80 no puede comunicarse con ningún equipo de la infraestructura de red. Esta solución debe ser implementado a nivel de interfaz.
- PC de VLAN90 no puede comunicarse con PC de VLAN100. Esta solución debe ser implementado a nivel de VLAN.
- Realizar enrutamiento intervlan en equipos apropiados.
- Implementar protocolos de enrutamiento propuesto a nivel de IPv4/IPv6.
- Considerar a R2 como servidor DHCP para IPv4/IPv6.
- Permitir salida hacia Internet en IPv4/IPv6. A nivel IPv4 deben salir traducidos por IP pública.
- Comprobar conectividad y funcionamiento de las soluciones propuestas.
# Configuracion
## PC-VLAN80
```
enable
config t
hostname PC-VLAN80
ipv6 unicast-routing
int e0/3
ip address dhcp
ipv6 address 3001:ABCD:ABCD:75::B/64
no shut
exit
ip route 0.0.0.0 0.0.0.0 172.16.80.1
ipv6 route ::/0 3001:ABCD:ABCD:75::1
exit
```
## PC-VLAN90
```
enable
config t
hostname PC-VLAN90
ipv6 unicast-routing
int e0/3
ip address dhcp
ipv6 address dhcp
no shut
exit
ip route 0.0.0.0 0.0.0.0 172.16.90.1
ipv6 route ::/0 3001:ABCD:ABCD:80::1
!# Cuando configures DHCPv6 tendras que ejecutar esto
int e0/3
ipv6 address autoconfig
end
```
## PC-VLAN100
```
enable
config t
hostname PC-VLAN100
ipv6 unicast-routing
int e0/3
ip address dhcp
ipv6 address 3001:ABCD:ABCD:85::D/64
no shut
exit
ip route 0.0.0.0 0.0.0.0 172.16.100.1
ipv6 route ::/0 3001:ABCD:ABCD:85::1
```
## SWC
```
enable
config t
spanning-tree mode rapid-pvst
vlan 70,80,90,100,200
exit
vtp mode transparent
int e0/3
duplex half
exit
int e0/3
switchport mode access
switchport access vlan 80
switchport port-security
switchport port-security mac-address sticky
switchport port-security violation shutdown
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int range e1/2-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trun allowed vlan 70,80,90,100
switchport trunk native vlan 200
switchport nonegotiate
exit
int range e1/2-3
channel-group 3 mode on
exit
int vlan 70
ip address 172.16.70.4 255.255.255.0
no shut
exit
ip default-gateway 172.16.70.1
!# PACL
exit
!# VER MAC STICA a e0/3
show mac address-table
config t
mac access-list extended L2
deny host aabb.cc00.0a30 any
permit any any
exit
ip access-list extended L3
deny ip host 172.16.80.5 any
permit any any
exit
int e0/3
mac access-group L2 in
ip access-group L3 in
exit
```
## SWD
```
enable
config t
spanning-tree mode rapid-pvst
vlan 70,80,90,100,200
exit
vtp mode transparent
int e0/3
duplex half
exut
int e0/3
switchport mode access
switchport access vlan 90
switchport port-security
switchport port-security mac-address sticky
switchport port-security violation shutdown
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int range e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trun allowed vlan 70,80,90,100
switchport trunk native vlan 200
switchport nonegotiate
exit
int range e1/0-1
channel-group 4 mode passive
exit
int range e1/2-3
channel-group 5 mode on
exit
int vlan 70
ip address 172.16.70.5 255.255.255.0
no shut
exit
ip default-gateway 172.16.70.1
int vlan 70
ip address 172.16.70.5 255.255.255.0
no shut
exit
ip default-gateway 172.16.70.1
!# VACL
end
!# Ver Mac Estatica del pc final
show mac address-table
config t
mac access-list extended VACL-L2
permit host aabb.cc00.0b30 host aabb.cc00.0c30
exit
ip access-list extended VACL-L3
permit ip host 172.16.90.5 host 172.16.100.5
exit
vlan access-map BLOQUEO 10
match mac address VACL-L2
match ip address VACL-L3
action drop
exit
vlan access-map BLOQUEO 20
action forward
exit
vlan filterr BLOQUEO vlan-list 90,100
```
## SWE
```
enable
config t
spanning-tree mode rapid-pvst
vlan 70,80,90,100,200
exit
vtp mode transparent
int e0/3
duplex half
exit
int e0/3
switchport mode access
switchport access vlan 100
switchport port-security
switchport port-security mac-address sticky
switchport port-security violation shutdown
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int range e0/1-2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trun allowed vlan 70,80,90,100
switchport trunk native vlan 200
switchport nonegotiate
exit
int range e0/1-2
channel-group 6 mode auto
exit
int vlan 70
ip address 172.16.70.6 255.255.255.0
no shut
exit
ip default-gateway 172.16.70.1
```
## SWA
```
enable
config t
spanning-tree mode rapid-pvst
vlan 70,80,90,100,200
exit
vtp mode transparent
int range e0/1-2,e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trun allowed vlan 70,80,90,100
switchport trunk native vlan 200
switchport nonegotiate
exit
int range e1/2-3
channel-group 3 mode on
exit
int range e1/0-1
channel-group 4 mode active
exit
int range e0/1-2
channel-group 1 mode auto
exit
int vlan 70
ip address 172.16.70.2 255.255.255.0
no shut
exit
ip default-gateway 172.16.70.1
```
## SWB
```
enable
config t
spanning-tree mode rapid-pvst
vlan 70,80,90,100,200
exit
vtp mode transparent
int range e0/1-2,e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trun allowed vlan 70,80,90,100
switchport trunk native vlan 200
switchport nonegotiate
exit
int range e1/2-3
channel-group 5 mode on
exit
int range e0/1-2
channel-group 6 mode desirable
exit
int range e1/0-1
channel-group 2 mode passive
exit
int vlan 70
ip address 172.16.70.3 255.255.255.0
no shut
exit
ip default-gateway 172.16.70.1
```
## MLS
```
enable
config t
ip routing
no ipv6 unicast-routing
ipv6 unicast-routing
spanning-tree mode rapid-pvst
vlan 70,80,90,100,200
exit
vtp mode transparent
int e0/0
duplex half
exit
int range e0/1-2,e1/0-1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trun allowed vlan 70,80,90,100
switchport trunk native vlan 200
switchport nonegotiate
exit
int range e0/1-2
channel-group 1 mode desirable
exit
int range e1/0-1
channel-group 2 mode active
exit
int vlan 70
ip address 172.16.70.1 255.255.255.0
no shut
exit
ip default-gateway 172.16.70.1
int vlan 80
ip address 172.16.80.1 255.255.255.0
ipv6 address 3001:ABCD:ABCD:75::1/64
no shut
exit
int vlan 90
ip address 172.16.90.1 255.255.255.0
ipv6 address 3001:ABCD:ABCD:80::1/64
no shut
exit
int vlan 100
ip address 172.16.100.1 255.255.255.0
ipv6 address 3001:ABCD:ABCD:85::1/64
no shut
exit
int e0/0
no switchport
ip address 172.16.12.1 255.255.255.0
ipv6 address 3001:ABCD:ABCD:12::1/112
exit
router eigrp 1
network 172.16.80.1 0.0.0.0
network 172.16.90.1 0.0.0.0
network 172.16.100.1 0.0.0.0
network 172.16.12.1 0.0.0.0
passive-interface vlan 80
passive-interface vlan 90
passive-interface vlan 100
exit
ipv6 router eigrp 1
passive-interface vlan 80
passive-interface vlan 90
passive-interface vlan 100
exit
int vlan 80
ipv6 eigrp 1
exit
int vlan 90
ipv6 eigrp 1
exit
int vlan 100
ipv6 eigrp 1
exit
int e0/0
ipv6 eigrp 1
exit
int vlan 80
ip helper-address 172.16.12.2
exit
int vlan 90
ip helper-address 172.16.12.2
exit
int vlan 100
ip helper-address 172.16.12.2
exit
int vlan 90
ipv6 nd managed-config-flag
ipv6 dhcp relay destination 3001:ABCD:ABCD:12::2
exit
```
## R2
```
enable
config t
ipv6 unicast-routing
router eigrp 1
network 172.16.12.2 0.0.0.0
exit
ipv6 router eigrp 1
exit
int e0/0
ipv6 eigrp 1
exit
router ospf 1
router-id 2.2.2.2
network 172.16.23.2 0.0.0.0 area 0
exit
ipv6 router ospf 1
router-id 2.2.2.2
exit
int e0/1
ipv6 ospf 1 area 0
exit
ipv6 router ospf 1
redistribute eigrp 1 include-connected
exit
!# Obtener parametros K
end
show int e0/1
config t
!# Configura con los Parametros K
ipv6 router eigrp 1
redistribute ospf 1 metric 10000 100 255 1 1500 1 include-connected
exit
!# EIGRP -> OSPF
ip prefix-list RUTAS-EIGRP permit 172.16.80.0/24
ip prefix-list RUTAS-EIGRP permit 172.16.90.0/24
ip prefix-list RUTAS-EIGRP permit 172.16.100.0/24
ip prefix-list RUTAS-EIGRP permit 172.16.12.0/24
route-map OSPF permit
match ip address prefix-list RUTAS-EIGRP
set metric-type type-1
exit
router ospf 1
redistribute eigrp 1 route-map OSPF subnets
exit
!# OSFP -> EIGRP
ip prefix-list RUTAS-OSPF permit 172.16.23.0/24
ip prefix-list RUTAS-OSPF permit 0.0.0.0/0
route-map EIGRP permit
match ip address prefix-list RUTAS-OSPF
set metric 10000 100 255 1 1500
exit
router eigrp 1
redistribute ospf 1 route-map EIGRP
exit
ip dhcp excluded-address 172.16.80.1 172.16.80.4
ip dhcp excluded-address 172.16.90.1 172.16.80.4
ip dhcp excluded-address 172.16.100.1 172.16.80.4
ip dhcp pool VLAN80
network 172.16.80.0 255.255.255.0
default-router 172.16.80.1
dns-server 8.8.8.8
exit
ip dhcp pool VLAN90
network 172.16.90.0 255.255.255.0
default-router 172.16.90.1
dns-server 8.8.8.8
exit
ip dhcp pool VLAN100
network 172.16.100.0 255.255.255.0
default-router 172.16.100.1
dns-server 8.8.8.8
exit
ipv6 dhcp pool DHCP-V6
address prefix 3001:ABCD:ABCD:80::/64
dns-server ::8
exit
int e0/0
ipv6 dhcp server DHCP-V6
exit

```
## R3
```
enable
config t
ipv6 unicast-routing
router ospf 1
router-id 3.3.3.3
network 172.16.23.3 0.0.0.0 area 0
default-information originate
exit
ip route 0.0.0.0 0.0.0.0 205.0.0.1
ipv6 route ::/0 3001:ABCD:ABCD:205::1
ipv6 router ospf 1
router-id 3.3.3.3
default-information originate
exit
int e0/1
ipv6 ospf 1 area 0
exit
ip access-list standard INTERNET
permit 172.16.80.0 0.0.0.255
permit 172.16.90.0 0.0.0.255
permit 172.16.100.0 0.0.0.255
exit
ip nat inside source list INTERNET interface e0/2 overload
int e0/1
ip nat inside
exit
int e0/2
ip nat outside
exit
```
## ISP
```
enable
config t
ipv6 unicast-routing
ipv6 route 3001:ABCD:ABCD:205::2
```