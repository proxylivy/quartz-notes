# Info
## Imagen Topologia
![500](https://slink.proxylivy.work/image/51b89bf2-329d-4035-9598-9f20f89cc031.png)
## Sobre

## Requerimientos

# Configuracion
VLAN 1: Inside
VLAN 2: OUTIDE
VLAN 3: DMZ
## SWA
```
enable
config t
configure replace flash:ccna.txt force
vlan 3
exit
int range f0/18
switchport mode access
switchport port-security
switchport port-security mac-address sticky
switchport port-security maximum 3
spanning-tree portfast
spanning-tree bpdufilter enable
spanning-tree bpduguard enable
no shut
exit
```
## SWB
```
enable
config t
configure replace flash:ccna.txt force
vlan 1
exit
int f0/18
switchport mode access
switchport port-security
switchport port-security mac-address sticky
switchport port-security maximum 3
spanning-tree portfast
spanning-tree bpduguard enable
no shut
exit
ip dhcp snooping
ip dhcp snooping vlan 1
no ip dhcp snooping information option
int f0/12
ip dhcp snooping trust
exit
int f0/18
ip dhcp snooping limit rate 3
exit

```
## SWC
```
enable
config t
configure replace flash:ccna.txt force
int g1/0/18
switchport mode access
switchport port-security
switchport port-security mac-address sticky
switchport port-security maximum 3
spanning-tree portfast
spanning-tree bpdufilter enable
spanning-tree bpduguard enable
no shut
exit
int g1/0/12
no shut
exit
```
## R1
```
enable
config t
configure replace flash:ccna.txt force
int g0/0
ip address 
```
## R2
```
enable
config t
configure replace flash:ccna.txt force
int s0/0/1
ip address 192.168.32.2 255.255.255.0
no shut
exit
router ospf 1
router-id 2.2.2.2
network 192.168.32.2 0.0.0.0 area 0
exit
int g0/0
ip address 172.16.30.2 255.255.255.0
no shut
exit
```
## R3
```
enable
config t
configure replace flash:ccna.txt force
int s0/0/0
ip address 192.168.13.3 255.255.255.0
no shut
exit
int s0/0/1
ip address 192.168.32.3 255.255.255.0
no shut
exit
router ospf 1
router-id 2.2.2.2
network 192.168.13.3 0.0.0.0 area 0
network 192.168.32.3 0.0.0.0 area 0
exit
```
## ASA
```
enable
!
config t
int vlan 1
nameif INSIDE
security-level 95
ip address 172.16.20.1 255.255.255.0
exit
!
interface vlan 2
nameif OUTSIDE
security-level 5
ip address 201.0.0.2 255.255.255.224
exit
!
interface ethernet 0/0
switchport access vlan 2
exit
!
route OUTSIDE 0.0.0.0 0.0.0.0 201.0.0.1
!
dhcpd address 172.16.20.2-172.16.20.20 INSIDE
dhcpd dns 8.8.8.8 interface INSIDE
dhcpd enable INSIDE
!
object network INTERNET
subnet 172.16.20.0 255.255.255.0
nat (INSIDE,OUTSIDE) dynamic interface
!
!#DMZ
int vlan 3
ip address 172.16.10.1 255.255.255.0
no forward interface vlan 1
nameif DMZ
security-level 55
exit
!
interface ethernet 0/2
switchport access vlan 3
exit
!
!#NAT ESTATICO
object network NAT
host 172.16.10.3
nat (DMZ,OUTSIDE) static 201.0.0.1
exit
!
access-list INTERNET permit ip any host 172.16.10.3
access-group INTERNET in interface OUTSIDE
!
```
# Visualizacion