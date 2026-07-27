

| Host?? | Vlan/Wan | Network      | CIDR | DEC MASK        | IPv6                  |
| ------ | -------- | ------------ | ---- | --------------- | --------------------- |
|        | 30       | 172.16.0.0   | /22  | 255.255.252.0   | 3001:ACAD:ACAD:3::/64 |
|        | 10       | 172.16.4.0   | /23  | 255.255.254.0   | 3001:ACAD:ACAD:1::/64 |
|        | 20       | 172.16.6.0   | /23  | 255.255.254.0   | 3001:ACAD:ACAD:2::/64 |
|        | 40       | 172.16.8.0   | /24  | 255.255.255.0   | 3001:ACAD:ACAD:4::/64 |
|        | 60       | 172.16.9.0   | /26  | 255.255.255.192 | 3001:ACAD:ACAD:6::/64 |
|        | 50       | 172.16.9.64  | /27  | 255.255.255.224 | 3001:ACAD:ACAD:5::/64 |
|        | WAN-1    | 172.16.9.96  | /30  | 255.255.255.252 | 3001:ACAD:ACAD:A::/64 |
|        | WAN-2    | 172.16.9.100 | /30  | 255.255.255.252 | 3001:ACAD:ACAD:B::/64 |
# Configuracion
## Dispositivos Finales
### Server-PT Correo
- Ipv4: 172.16.7.24
- Subnet: 255.255.254.0
- DG: 172.16.6.1
- DNS: 210.0.0.2
- IPv6: 3001:ACAD:ACAD:2::A/64
- DGv6: 3001:ACAD:ACAD:2::1
- DNSv6: 3001:ACAD:ACAD:210::2

### PC-PT Vlan 30
- Ipv4: DHCP
- Subnet: DHCP
- DG: DHCP
- DNS: 210.0.0.2
- IPv6: 3001:ACAD:ACAD:3::B/64
- DGv6: 3001:ACAD:ACAD:3::1
- DNSv6: 3001:ACAD:ACAD:210::2

### Server PT caso1.cl
- Ipv4: 210.0.0.2
- Subnet: 255.255.255.0
- DG: 210.0.0.1
- DNS: 210.0.0.2
- IPv6: 3001:ACAD:ACAD:210::2/112
- DGv6: 3001:ACAD:ACAD:210::1
- DNSv6: 3001:ACAD:ACAD:210::2

### PC-PT VLAN 60
- IPv4: DHCP
- SubNet: DHCP
- DG: DHCP
- DNS: 210.0.0.2
- IPv6: 3001:ACAD:ACAD:6::D/64
- DGv6: 3001:ACAD:ACAD:6::1
- DNSv6: 3001:ACAD:ACAD:210::2

### SERVER-PT FTP/TFTP
- IPv4: 172.16.9.74
- SubNet: 255.255.255.224
- DG: 172.16.9.65
- DNS: 210.0.0.2
- IPv6: 3001:ACAD:ACAD:5::C/64
- DGv6: 3001:ACAD:ACAD:5::1
- DNSv6: 3001:ACAD:ACAD:210::2

## CORE
```
enable
config t
ipv6 unicast-routing
!
ip dhcp excluded-address 172.16.4.1 172.16.4.4
!
ip dhcp pool VLAN10
network 172.16.4.0 255.255.254.0
default-router 172.16.4.1
dns-server 210.0.0.2
exit
!
ip dhcp excluded-address 172.16.0.1 172.16.0.4
ip dhcp pool VLAN30
network 172.16.0.0 255.255.252.0
default-router 172.16.0.1
dns-server 210.0.0.2
exit
!
ip dhcp excluded-address 172.16.8.1 172.16.8.4
ip dhcp pool VLAN40
network 172.16.8.0 255.255.255.0
default-router 172.16.8.1
dns-server 210.0.0.2
exit
!
ip dhcp excluded-address 172.16.9.1 172.16.9.4 
ip dhcp pool VLAN60
network 172.16.9.0 255.255.255.192
default-router 172.16.9.1
dns-server 210.0.0.2
exit
!
router ospf 5
router-id 3.3.3.3
network 221.0.0.0 0.0.0.7 area 0
network 172.16.0.0 0.0.255.255 area 0
default-information originate
exit
!
ipv6 router ospf 10
router-id 3.3.3.3
default-information originate
exit
!
int s0/0/0
ip add 172.16.9.98 255.255.255.252
ipv6 add 3001:ACAD:ACAD:A::2/64
ipv6 ospf 10 area 0
no shut
exit
!
int s0/0/1
ip add 172.16.9.102 255.255.255.252
ipv6 add 3001:ACAD:ACAD:B::2/64
ipv6 ospf 10 area 0
no shut
exit
!
ip route 0.0.0.0 0.0.0.0 221.0.0.2
ipv6 route ::/0 3001:ACAD:ACAD:221::2
exit
!
```
## RA
```
enable
config t
ipv6 unicast-routing
!
int g0/0
no shut
exit
!
int g0/0.10
encapsulation dot1q 10
ip add 172.16.4.1 255.255.254.0
ipv6 add 3001:ACAD:ACAD:1::1/64
ip helper-address 172.16.9.98
ipv6 ospf 10 area 0
no shut
exit
!
int g0/0.20
encapsulation dot1q 20
ip add 172.16.6.1 255.255.254.0
ipv6 add 3001:ACAD:ACAD:2::1/64
ipv6 ospf 10 area 0
no shut
exit
!
int g0/0.30
encapsulation dot1q 30
ip add 172.16.0.1 255.255.252.0
ipv6 add 3001:ACAD:ACAD:3::1/64
ip helper-address 172.16.9.98
ipv6 ospf 10 area 0
no shut
exit
!
router ospf 5
router-id 1.1.1.1
passive-interface g0/0
passive-interface g0/0.10
passive-interface g0/0.20
passive-interface g0/0.30
exit
!
ipv6 router ospf 10
router-id 1.1.1.1
passive-interface g0/0
passive-interface g0/0.10
passive-interface g0/0.20
passive-interface g0/0.30
exit
!
int s0/0/0
ip add 172.16.9.97 255.255.255.252
ipv6 add 3001:ACAD:ACAD:A::1/64
ipv6 ospf 10 area 0
no shut
exit
!
ip access-list extended NO-CORREO
deny tcp host "ipequipo" host 172.16.7.24 eq 110 eq 25
exit
!
```
## RB
```
ipv6 unicast-routing
!
int g0/0
no shut
exit
!
int g0/0.40
encapsulation dot1q 40
ip add 172.16.8.1 255.255.255.0
ipv6 add 3001:ACAD:ACAD:4::1/64
ip helper-address 172.16.9.102
ipv6 ospf 10 area 0
no shut
exit
!
int g0/0.50
encapsulation dot1q 50
ip add 172.16.9.65 255.255.255.224
ipv6 add 3001:ACAD:ACAD:5::1/64
ipv6 ospf 10 area 0
no shut
exit
!
int g0/0.60
encapsulation dot1q 60
ip add 172.16.9.1 255.255.255.192
ipv6 add 3001:ACAD:ACAD:6::1/64
ip helper-address 172.16.9.102
ipv6 ospf 10 area 0
no shut
exit
!
router ospf 5
router-id 2.2.2.2
network 172.16.9.100 0.0.0.3 area 0
network 172.16.9.96 0.0.0.3 area 0
network 221.0.0.0 0.0.0.7 area 0
network 210.0.0.0 0.0.0.3 area 0
passive-interface g0/0.40
passive-interface g0/0.50
passive-interface g0/0.60
exit
!
ipv6 router ospf 10
router-id 2.2.2.2
passive-interface g0/0.40
passive-interface g0/0.50
passive-interface g0/0.60
exit
!
int s0/0/1
ip add 172.16.9.101 255.255.255.252
ipv6 add 3001:ACAD:ACAD:B::1/64
ipv6 ospf 10 area 0
no shut
exit
!
```
## ISP_MALESTAR
```
enable
config t
ipv6 unicast-routing
```
## SWA-1
```
enable
config t
vlan 10
name TI
exit
!
vlan 20
name CORREO
exit
!
vlan 30
name RRHH
exit
!
vlan 40
name VENTAS
exit
!
vlan 50
name RESPALDO
exit
!
vlan 60
name ASESORIA
exit
!
vlan 1000
name NATIVA
exit
!
int range fa0/1,fa0/24
switchport mode trunk
switchport trunk native vlan 1000
switchport nonegotiate
no shut
exit
!
int range fa0/2-23
switchport mode access
switchport access vlan 1000
shut
exit
!
exit
```
## SWA-2
```
enable
config t
vlan 10
name TI
exit
!
vlan 20
name CORREO
exit
!
vlan 30
name RRHH
exit
!
vlan 40
name VENTAS
exit
!
vlan 50
name RESPALDO
exit
!
vlan 60
name ASESORIA
exit
!
vlan 1000
name NATIVA
exit
!
int fa0/24
switchport mode trunk
switchport trunk native vlan 1000
switchport nonegotiate
no shut
exit
!
int range f0/3-f0/5
switchport mode access
switchport access vlan 10
switchport port-security 
no shut
exit
!
int range fa0/7-fa0/10
switchport mode access
switchport access vlan 20
switchport port-security
no shut
exit
!
int range fa0/16-fa0/20
switchport mode access
switchport access vlan 30
switchport port-security
no shut
exit
!
int range fa0/1-fa0/2
switchport mode access
switchport access vlan 1000
shut
exit
!
int range fa0/11-15
switchport mode access
switchport access vlan 1000
shut
exit
!
int range fa0/21-fa0/23
switchport mode access
switchport access vlan 1000
shut
exit
!
int fa0/6
switchport mode access
switchport access vlan 1000
shut
exit
!
exit
!
```
## SWB-1
```
enable
config t
vlan 10
name TI
exit
!
vlan 20
name CORREO
exit
!
vlan 30
name RRHH
exit
!
vlan 40
name VENTAS
exit
!
vlan 50
name RESPALDO
exit
!
vlan 60
name ASESORIA
exit
!
vlan 1000
name NATIVA
exit
!
int range fa0/1,fa0/24
switchport mode trunk
switchport trunk native vlan 1000
switchport nonegotiate
no shut
exit
!
int range f0/2-23
switchport mode access
switchport access vlan 1000
switchport port-security 
no shut
exit
!
```
## SWB-2
```
enable
config t
vlan 10
name TI
exit
!
vlan 20
name CORREO
exit
!
vlan 30
name RRHH
exit
!
vlan 40
name VENTAS
exit
!
vlan 50
name RESPALDO
exit
!
vlan 60
name ASESORIA
exit
!
vlan 1000
name NATIVA
exit
!
int fa0/24
switchport mode trunk
switchport trunk native vlan 1000
switchport nonegotiate
no shut
exit
!
int range f0/18-f0/23
switchport mode access
switchport access vlan 40
switchport port-security 
no shut
exit
!
int fa0/10
switchport mode access
switchport access vlan 50
switchport port-security
no shut
exit
!
int range fa0/2-9
switchport mode access
switchport access vlan 60
switchport port-security
no shut
exit
!
int range fa0/1,fa0/11-17
switchport mode access
switchport access vlan 1000
switchport port-security
shut
exit
!
exit
```