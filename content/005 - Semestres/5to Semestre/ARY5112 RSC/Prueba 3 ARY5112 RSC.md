# Info
## Imagen Topologia
![1000](https://slink.proxylivy.work/image/500984d8-65a8-4e94-93f6-67ad6a3f1193.png)
## Sobre

## Requerimientos

# Configuracion
## SWA
```
enable
config t
ipv6 unicast-routing
errdisable recovery interval 45
vlan 100
name ENCOR
exit
vlan 200
name ENARSI
exit
vtp mode transparent
int range e0/2-3
duplex half
switchport mode access
switchport port-security
switchport port-security mac-address sticky
switchport port-security violation restrict
switchport nonegotiate
spanning-tree portfast
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
no shut
exit
int e0/2
switchport access vlan 100
exit
int e0/3
switchport access vlan 200
exit
int range e0/0-1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 100,200
no shut
channel-group 6 mode on
exit
spanning-tree mode rapid-pvst
spanning-tree vlan 100,200 hello-time 2
spanning-tree vlan 100,200 forward-time 11
spanning-tree vlan 100,200 max-age 15
!# VACL
mac access-list extended VACL-L2
permit host aabb.cc00.0a20 host aabb.cc00.0b30
exit
ip access-list extended VACL-L3
permit ip host 172.17.100.11 host 172.16.200.11
exit
vlan access-map FILTRO 100
match mac address VACL-L2
match ip address VACL-L3
action drop
exit
vlan access-map FILTRO 200
action forward
exit
vlan filter FILTRO vlan-list 100,200
end
copy running-config unix:
```
## SWA Snooping
A veces falla
```
config t
ip dhcp snooping
ip dhcp snooping vlan 100,200
no ip dhcp snooping infomation option
int range e0/2-3
ip dhcp snooping limit rate 3
exit
int po6
ip dhcp snooping trust
exit
```
## SWB
```
enable
config t
ipv6 unicast-routing
errdisable recovery interval 45
vlan 100
name ENCOR
exit
vlan 200
name ENARSI
exit
vtp mode transparent
int range e0/0-1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 100,200
no shut
channel-group 6 mode on
exit
spanning-tree mode rapid-pvst
spanning-tree vlan 100,200 hello-time 2
spanning-tree vlan 100,200 forward-time 11
spanning-tree vlan 100,200 max-age 15
```
## R1
```
enable
config t
ipv6 unicast-routing
int e0/3
no shut
exit
!# Decrement
track 1 int e0/0 line-protocol
delay down 5 up 3
exit
int e0/3.100
encapsulation dot1q 100
ip address 172.16.100.1 255.255.255.0
ipv6 addres 3001:ABCD:ABCD:100::1/64
exit
int e0/3.200
encapsulation dot1q 200
ip address 172.16.200.1 255.255.255.0
ipv6 addres 3001:ABCD:ABCD:200::1/64
exit
int e0/3.100
standby version 2
standby 100 ip 172.16.100.5
standby 100 preempt
standby 100 priority 120
!# TRACK
standby 100 track 1 decrement 31
standby 1000 ipv6 3001:ABCD:ABCD:100::AA/64
standby 1000 preempt
standby 1000 priority 90
exit
int e0/3.200
standby version 2
standby 200 ip 172.16.200.5
standby 200 preempt
standby 200 priority 120
standby 200 track 1 decrement 31
standby 2000 ipv6 3001:ABCD:ABCD:200::AA/64
standby 2000 preempt
standby 2000 priority 90
exit
!# VPN-Site-To-Site
crypto isakmp policy 1
authentication pre-share
encryption aes 128
hash sha
group 16
lifetime 43200
exit
crypto isakmp key encor address 172.16.32.2
crypto ipsec transform-set PRUEBA-3 esp-aes 128 esp-sha-hmac
exit
ip access-list extended PROTECCION
permit ip host 1.1.1.1 host 2.2.2.2
exit
crypto map MAPA-PRUEBA3 1 ipsec-isakmp
match address PROTECCION
set transform-set PRUEBA-3
set peer 172.16.32.2
exit
int e0/0
crypto map MAPA-PRUEBA3
exit
!# OSPF
router ospf 1
router-id 1.1.1.1
network 172.16.31.0 255.255.255.0 area 0
network 172.16.100.0 255.255.255.0 area 0
network 172.16.200.0 255.255.255.0 area 0
network 1.1.1.1 0.0.0.0 area 0
passive-interface e0/3.100
passive-interface e0/3.200
exit
ipv6 router ospf 1
router-id 1.1.1.1
passive-interface e0/3.100
passive-interface e0/3.200
exit
int e0/3.100
ipv6 ospf 1 area 0
ip helper-address 172.16.31.3
exit
int e0/3.200
ipv6 ospf 1 area 0
ip helper-address 172.16.31.3
exit
int e0/0
ipv6 ospf 1 area 0
exit
int loopback 1
ipv6 ospf 1 area 0
exit

```
## R2
```
enable
config t
ipv6 unicast-routing
!# Decrement
track 1 int e0/1 line-protocol
delay down 5 up 3
exit
int e0/2
no shut
exit
int e0/2.100
encapsulation dot1q 100
ip address 172.16.100.2 255.255.255.0
ipv6 addres 3001:ABCD:ABCD:100::2/64
exit
int e0/2.200
encapsulation dot1q 200
ip address 172.16.200.2 255.255.255.0
ipv6 addres 3001:ABCD:ABCD:200::2/64
exit
int e0/2.100
standby version 2
standby 100 ip 172.16.100.5
standby 100 preempt
standby 100 priority 90
standby 1000 ipv6 3001:ABCD:ABCD:100::AA/64
standby 1000 preempt
standby 1000 priority 120
standby 1000 track 1 decrement 31
exit
int e0/2.200
standby version 2
standby 200 ip 172.16.200.5
standby 200 preempt
standby 200 priority 90
standby 2000 ipv6 3001:ABCD:ABCD:200::AA/64
standby 2000 preempt
standby 2000 priority 120
standby 2000 track 1 decrement 31
exit
!# VPN-Site-To-Site
crypto isakmp policy 1
authentication pre-share
encryption aes 128
hash sha
group 16
lifetime 43200
exit
crypto isakmp key encor address 172.16.31.1
crypto ipsec transform-set PRUEBA-3 esp-aes 128 esp-sha-hmac
exit
ip access-list extended PROTECCION
permit ip host 2.2.2.2 host 1.1.1.1
exit
crypto map MAPA-PRUEBA3 1 ipsec-isakmp
match address PROTECCION
set transform-set PRUEBA-3
set peer 172.16.31.1
exit
int e0/1
crypto map MAPA-PRUEBA3
exit
!# OSPF
router ospf 1
router-id 2.2.2.2
network 172.16.32.0 255.255.255.0 area 0
network 172.16.100.0 255.255.255.0 area 0
network 172.16.200.0 255.255.255.0 area 0
network 2.2.2.2 0.0.0.0 area 0
passive-interface e0/2.100
passive-interface e0/2.200
exit
ipv6 router ospf 1
router-id 2.2.2.2
passive-interface e0/2.100
passive-interface e0/2.200
exit
int e0/2.100
ipv6 ospf 1 area 0
ip helper-address 172.16.32.3
exit
int e0/2.200
ipv6 ospf 1 area 0
ip helper-address 172.16.32.3
exit
int e0/1
ipv6 ospf 1 area 0
exit
int loopback 2
ipv6 ospf 1 area 0
exit
```
## R2 Visualizacion VPN
```
ping 172.16.10.1 source loopback 0
show crypto ipsec sa
show crypto isakmp sa
```
## R3
```
enable
config t
ipv6 unicast-routing
router ospf 1
router-id 3.3.3.3
network 172.16.32.0 255.255.255.0 area 0
network 172.16.31.0 255.255.255.0 area 0
exit
ipv6 router ospf 1
router-id 3.3.3.3
exit
int e0/1
ipv6 router ospf 1
exit
int e0/0
ipv6 router ospf 1
exit
!# EIGRP
router eigrp 34
eigrp router-id 3.3.3.3
network 172.16.43.0 255.255.255.0
exit
ipv6 router eigrp 34
eigrp router-id 3.3.3.3
exit
int e0/2
ipv6 eigrp 34
exit
!# DHCP
ip dhcp excluded-address 172.16.100.1 192.16.100.10
ip dhcp excluded-address 172.16.200.1 192.16.200.10
ip dhcp pool VLAN100
network 172.16.100.0 255.255.255.0
default-router 172.16.100.5
dns-server 8.8.8.8 8.8.4.4
ip dhcp pool VLAN200
network 172.16.200.0 255.255.255.0
default-router 172.16.200.5
dns-server 8.8.8.8 8.8.4.4

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
ip route 0.0.0.0 0.0.0.0 205.0.0.2
ipv6 route ::/0 3001:ABCD:ABCD:205::2
ip route 0.0.0.0 0.0.0.0 206.0.0.2
ipv6 route ::/0 3001:ABCD:ABCD:206::2
!# EIGRP
router eigrp 34
eigrp router-id 4.4.4.4
network 172.16.43.0 255.255.255.0
redistribute static
exit
ipv6 router eigrp 34
eigrp router-id 4.4.4.4
redistribute static
exit
int e0/2
ipv6 eigrp 34
exit
!# BGP
router bgp 64900
bgp router-id 4.4.4.4
address-family ipv4
neighbor 205.0.0.2 remote-as 2000
neighbor 206.0.0.2 remote-as 1000
exit
address-family ipv6
neighbor 3001:ABCD:ABCD:205::2 remote-as 2000
neighbor 3001:ABCD:ABCD:206::2 remote-as 1000
exit
!# ATRIBUTOS
!# Preferir IPv4 a ISP-1 y IPV6 a ISP-2 con AS-PATH
route-map AS-PATH permit
set as-path prepend 64900 64900 64900
exit
router bgp 64900
address-family ipv4
neighbor 205.0.0.2 route-map AS-PATH out
exit
address-family ipv6
neighbor 3001:ABCD:ABCD:206::2 route-map AS-PATH out
exit
exit
do clear ip bgp * soft
```
## ISP-1
```
enable
config t
ipv6 unicast-routing
!# BGP
router bgp 1000
bgp router-id 5.5.5.5
address-family ipv4
neighbor 206.0.0.1 remote-as 64900
neighbor 216.0.0.1 remote-as 64800
network 8.8.8.8 mask 255.255.255.255
exit
address-family ipv6
neighbor 3001:ABCD:ABCD:206::1 remote-as 64900
neighbor 3001:ABCD:ABCD:216::1 remote-as 64800
network 3001:ABCD:ABCD:8::8/128
exit
!# ATRIBUTOS BGP
!# Vecinos IPv6 vecinos metrica 1000
route-map MED permit
set metric 1000
exit
router bgp 1000
address-family ipv6
neighbor 3001:ABCD:ABCD:206::1 route-map MED out
neighbor 3001:ABCD:ABCD:216::1 route-map MED out
exit
exit
do clear ip bgp * soft
```
## ISP-2
```
enable
config t
ipv6 unicast-routing
router bgp 2000
bgp router-id 6.6.6.6
address-family ipv4
neighbor 205.0.0.1 remote-as 64900
neighbor 215.0.0.1 remote-as 64800
network 8.8.4.4 mask 255.255.255.255
exit
address-family ipv6
neighbor 3001:ABCD:ABCD:205::1 remote-as 64900
neighbor 3001:ABCD:ABCD:215::1 remote-as 64800
network 3001:ABCD:ABCD:8::4/128
exit

```
## BR
```
enable
config t
ipv6 unicast-routing
router bgp 64800
bgp router-id 7.7.7.7
address-family ipv4
neighbor 215.0.0.2 remote-as 2000
neighbor 216.0.0.2 remote-as 1000
network 222.0.0.0 mask 255.255.255.252
exit
address-family ipv6
neighbor 3001:ABCD:ABCD:215::2 remote-as 2000
neighbor 3001:ABCD:ABCD:216::2 remote-as 1000
network 3001:ABCD:ABCD:222::/112
exit
exit
!# ATRIBUTOS
!# Preferir IPv4 a ISP-1 y IPV6 a ISP-2 con AS-PATH
route-map AS-PATH permit
set as-path prepend 64800
exit
router bgp 64800
address-family ipv4
neighbor 215.0.0.2 route-map AS-PATH out
exit
address-family ipv6
neighbor 3001:ABCD:ABCD:216::2 route-map AS-PATH out
exit
exit
do clear ip bgp * soft
```

## TCL + VPN
```
tclsh
foreach address {
[ip]
[ip]
} { ping $address repeat 5 size 1500 }
exit
```