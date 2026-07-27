# Info
## Descargar Packet Tracer:
- [OneDrive PKT](https://duoccl0-my.sharepoint.com/:u:/g/personal/ga_zunigam_duocuc_cl/Eb6mJ-hre19FkvL7878StX8BgDOa19thetk-uQDhYkAofA?e=Wm5vbg)
## Topologia:
![500](https://slink.proxylivy.work/image/b9a92b85-d1b2-423a-a5e2-1da73084e507.png)
## Dispositivos Finales
### Server-PT www/dns
Nota: **Habilitar DNS, HTTP y configurar**
- IPv4: 220.0.0.2
- Subnet: 255.255.255.252
- DG: 220.0.0.1
- DNS: 220.0.0.2
IPV6
- IPv6: 3001:ACAD:ACAD:220::2 /112
- DGv6: 3001:ACAD:ACAD:220::1
- DNSv6: 3001:ACAD:ACAD:220::2

### Server-PT SMTP
Nota: **Habilitar Servidor de Correo**
- IPv4: 192.168.80.100
- Subnet: 255.255.255.0
- DG: 192.168.80.1
- DNS: 220.0.0.2
IPV6
- IPv6: 3001:ACAD:ACAD:80::100/64
- DGv6: 3001:ACAD:ACAD:80::1
- DNSv6: 3001:ACAD:ACAD:220::2

### Server-PT Intranet
- IPv4: 172.16.10.100
- Subnet: 255.255.255.0
- DG: 172.16.10.1
- DNS: 220.0.0.2

### PC-Vlan40
- IPv4: DHCP
- Subnet: DHCP
- DG: DHCP
- DNS: 220.0.0.2
IPv6
- IPv6: 3001:ACAD:ACAD:40::C/64
- DGv6: 3001:ACAD:ACAD:40::1
- DNSv6: 3001:ACAD:ACAD:220::2

### PC-Vlan50
- IPv4: DHCP
- Subnet: DHCP
- DG: DHCP
- DNS: 220.0.0.2
IPv6
- IPv6: 3001:ACAD:ACAD:50::D/64
- DGv6: 3001:ACAD:ACAD:50::1
- DNSv6: 3001:ACAD:ACAD:220::2

### PC-Vlan60
- IPv4: DHCP
- Subnet: DHCP
- DG: DHCP
- DNS: 220.0.0.2
IPv6
- IPv6: 3001:ACAD:ACAD:60::E/64
- DGv6: 3001:ACAD:ACAD:60::1
- DNSv6: 3001:ACAD:ACAD:220::2

### PC-Vlan70
- IPv4: DHCP 
- Subnet: DHCP
- DG: DHCP
- DNS: 220.0.0.2
IPv6
- IPv6: 3001:ACAD:ACAD:70::F/64
- DGv6: 3001:ACAD:ACAD:70::1
- DNSv6: 3001:ACAD:ACAD:220::2

## ISP
```
enable
config t
ipv6 unicast-routing
!
ip route 0.0.0.0 0.0.0.0 209.50.0.1
ipv6 route ::/0 3001:ACAD:ACAD:209::1
```
## R2
```
enable
config t
ipv6 unicast-routing
!
router ospf 10
default-information originate
passive-interface s0/0/1
router-id 2.2.2.2
network 192.168.200.0 0.0.0.255 area 0
!
ip route 0.0.0.0 0.0.0.0 209.50.0.2
ipv6 route ::/0 3001:ACAD:ACAD:209::2
!
ipv6 route ospf 20
router-id 0.0.0.2
passive-interface serial 0/0/1
!
ip nat inside source static 172.16.10.100 209.50.0.1
int s0/0/1
ip nat outside
int s0/0/0
ip nat inside
```
## R1
```
enable
config t
ipv6 unicast-routing
!# OSPFv2
router ospf 10
router-id 1.1.1.1
network 172.16.10.0 0.0.0.255 area 0
network 192.168.200.0 0.0.0.255 area 0
network 192.168.80.0 0.0.0.255 area 0
network 192.168.40.0 0.0.0.255 area 0
network 192.168.50.0 0.0.0.255 area 0
network 192.168.60.0 0.0.0.255 area 0
network 192.168.70.0 0.0.0.255 area 0
passive-interface g0/0.40
passive-interface g0/0.50
passive-interface g0/0.60
passive-interface g0/0.70
passive-interface g0/0.80
passive-interface g0/1
exit
!
!# OSPFv3
ipv6 router ospf 20
router-id 1.1.1.1
passive-interface g0/0.40
passive-interface g0/0.50
passive-interface g0/0.60
passive-interface g0/0.70
passive-interface g0/0.80
passive-interface g0/1
exit
!
int g0/0
no shut
exit
!
int g0/0.40
encapsulation dot1q 40
ip add 192.168.40.1 255.255.255.0
ipv6 add 3001:ACAD:ACAD:40::1/64
ipv6 ospf 20 area 0
no shut
exit
!
int g0/0.50
encapsulation dot1q 50
ip add 192.168.50.1 255.255.255.0
ipv6 add 3001:ACAD:ACAD:50::1/64
ipv6 ospf 20 area 0
no shut
exit
!
int g0/0.60
encapsulation dot1q 60
ip add 192.168.60.1 255.255.255.0
ipv6 add 3001:ACAD:ACAD:60::1/64
ipv6 ospf 20 area 0
no shut
exit
!
int g0/0.70
encapsulation dot1q 70
ip add 192.168.70.1 255.255.255.0
ipv6 add 3001:ACAD:ACAD:70::1/64
ipv6 ospf 20 area 0
no shut
exit
!
int g0/1
ip add 172.16.10.1 255.255.255.0
no shut
exit
!
int s0/0/0
ip add 192.168.200.1 255.255.255.0
ipv6 add 3001:ACAD:ACAD:200::1/112
ipv6 ospf 20 area 0
no shut
exit
!
!# DHCP
ip dhcp exluded-address 192.168.40.1 192.168.40.9
ip dhcp pool vlan40
network 192.168.40.0 255.255.255.0
domain-name lajoya.com
default-router 192.168.40.1
dns-server 220.0.0.2
exit
!
ip dhcp excluded-address 192.168.50.1 192.168.50.9
ip dhcp pool vlan50
network 192.168.50.0 255.255.255.0
domain-name lajoya.com
default-router 192.168.50.1
dns-server 220.0.0.2
exit
!
ip dhcp excluded-address 192.168.60.1 192.168.60.9
ip dhcp pool vlan60
network 192.168.60.0 255.255.255.0
domain-name lajoya.com
default-router 192.168.60.1
dns-server 220.0.0.2
exit
!
ip dhcp excluded-address 192.168.70.1 192.168.70.9
ip dhcp pool vlan70
network 192.168.70.0 255.255.255.0
domain-name lajoya.com
default-router 192.168.70.1
dns-server 220.0.0.2
exit
!
ip dhcp excluded-address 192.168.80.1 192.168.80.9
ip dhcp pool vlan80
network 192.168.80.0 255.255.255.0
domain-name lajoya.com
default-router 192.168.80.1
dns-server 220.0.0.2
exit
```
## Switch0
```
enable
config t
vlan 40
vlan 50
vlan 60
vlan 70
vlan 80
int fa0/24
switchport mode trunk
switchport nonegotiate
switchport trunk allowed vlan 40,50,60,70,80
no shut
exit
int fa0/1
switchport mode access
switchport access vlan 40 
no shut
exit
int fa0/2
switchport mode access
switchport access vlan 50
no shut
exit
int fa0/3
switchport mode access
switchport access vlan 60
no shut
exit
int fa0/4
switchport mode access
switchport access vlan 70
no shut
exit
int fa0/5
switchport mode access
switchport access vlan 80
no shut
exit
```