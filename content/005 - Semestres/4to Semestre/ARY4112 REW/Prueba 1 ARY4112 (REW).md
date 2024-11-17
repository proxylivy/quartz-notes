
Dispositivos Finales
> .100
```
Ipv4: 172.16.1.100
Subnet: 255.255.255.0
DG: 172.16.1.1
DNS: 201.212.111.5
IPv6: 2021:AC:AC:1::100/64
DGv6: 2021:AC:AC:1::1
DNSv6: 2001:212:ACAD:A::5
```

---
> Soporte
```
Ipv4: 172.17.1.100
Subnet: 255.255.255.0
DG: 172.17.1.1ospf
DNS: 201.212.111.5
IPv6: 2021:AC:AC:30::100/64
DGv6: 2021:AC:AC:30::1
DNSv6: 2001:212:ACAD:A::5
```

---
> Jefe Area
```
Ipv4: 172.17.1.110
Subnet: 255.255.255.0
DG: 172.17.1.1
DNS: 201.212.111.5
IPv6: 2021:AC:AC:30::110/64
DGv6: 2021:AC:AC:30::1
DNSv6: 2001:212:ACAD:A::5
```

---
> .10
```
Ipv4: 10.100.100.10
Subnet: 255.255.255.0
DG: 10.100.100.1
DNS: 201.212.111.5
IPv6: 2021:AC:AC:A100::10/64
DGv6: 2021:AC:AC:A100::1
DNSv6: 2001:212:ACAD:A::5
```

---
> .50
```
Ipv4: 10.200.200.50
Subnet: 255.255.255.0
DG: 10.200.200.1
DNS: 201.212.111.5
IPv6: 2021:AC:AC:A200::50/64
DGv6: 2021:AC:AC:A200::1
DNSv6: 2001:212:ACAD:A::5
```

---
> .120
```
Ipv4: 10.220.220.120
Subnet: 255.255.255.0
DG: 10.220.220.1
DNS: 201.212.111.5
IPv6: 2021:AC:AC:A220::120/64
DGv6: 2021:AC:AC:A220::1
DNSv6: 2001:212:ACAD:A::5
```

---
> redes3.cl
```
Ipv4: 201.212.111.5
Subnet: 255.255.255.248
DG: 201.212.111.1
DNS: 201.212.111.5
IPv6: 2001:212:ACAD:A::5/64
DGv6: 2001:212:ACAD:A::1
DNSv6: 2001:212:ACAD:A::5
```

---
---
Router
> Gerencia
```
!
enable
config terminal
hostname Gerencia
ipv6 unicast-routing
!
interface GigabitEthernet0/0
ip address 172.20.20.1 255.255.255.248
ipv6 address 2021:AC:AC:20::1/64
ipv6 ospf 500 area 0
no shutdown
exit
!
interface GigabitEthernet0/1
ip address 172.16.1.1 255.255.255.0
ipv6 address 2021:AC:AC:1::1/64
ipv6 ospf 500 area 0
no shutdown
exit
!
router ospf 100
router-id 1.1.1.1
log-adjacency-changes
default-information originate
redistribute static
network 172.16.1.0 0.0.0.255 area 0
network 172.20.20.0 0.0.0.7 area 0
passive-interface g0/1
passive-interface g0/0
exit
!
ipv6 router ospf 500
router-id 1.1.1.1
log-adjacency-changes
default-information originate
redistribute static
passive-interface g0/1
passive-interface g0/0
exit
!
```

---
> Area TI
```
enable
config t
ipv6 unicast-routing
!
int g0/0
ip add 172.20.20.3 255.255.255.248
ipv6 add 2021:ac:ac:20::3/64
ipv6 ospf 500 area 0
ip ospf priority 255
ipv6 ospf priority 255
no shut
exit
!
int g0/1
ip add 172.17.1.1 255.255.255.0
ipv6 add 2021:ac:ac:30::1/64
ipv6 ospf 500 area 0
ip ospf priority 255
ipv6 ospf priority 255
no shut
exit
!
router ospf 100
router-id 2.2.2.2
default-information originate
redistribute static
log-adjacency-changes
network 172.17.1.0 0.0.0.255 area 0
network 172.20.20.0 0.0.0.7 area 0
passive-interface g0/0
passive-interface g0/1
!
ipv6 router ospf 500
router-id 2.2.2.2
log-adjacency-changes
default-information originate
redistribute static
passive-interface g0/0
passive-interface g0/1

```

---
> RRHH
```
enable
config t
ipv6 unicast-routing
!
interface GigabitEthernet0/0
ip address 172.20.20.2 255.255.255.248
ipv6 address 2021:AC:AC:20::2/64
ipv6 ospf 500 area 0
no shut
exit
!
interface GigabitEthernet0/1
ip address 192.168.10.2 255.255.255.0
ipv6 address 2021:AC:AC:40::2/64
ipv6 ospf 500 area 0
no shut
exit
!
interface GigabitEthernet0/2
ip address 192.168.30.2 255.255.255.0
ipv6 address 2021:AC:AC:50::2/64
ipv6 ospf 500 area 0
no shut
exit
!
router ospf 100
router-id 3.3.3.3
log-adjacency-changes
default-information originate
redistribute static
network 192.168.30.0 0.0.0.255 area 0
network 172.20.20.0 0.0.0.7 area 0
network 192.168.10.0 0.0.0.255 area 0
passive-interface g0/0
passive-interface g0/1
passive-interface g0/2
!
ipv6 router ospf 500
router-id 3.3.3.3
log-adjacency-changes
default-information originate
redistribute static
passive-interface g0/0
passive-interface g0/1
passive-interface g0/2
!
```


---
>LAN
```
enable
config t
hostname LAN
ipv6 unicast-routing
!
int g0/1
ip address 190.88.88.2 255.255.255.252
ipv6 address 2021:AC:AC:60::2/64
ipv6 ospf 500 area 0
no shut
exit
!
interface GigabitEthernet0/2
ip address 192.168.30.1 255.255.255.0
ipv6 address 2021:AC:AC:50::1/64
ipv6 ospf 500 area 0
no shut
exit
!
int g0/0
no shut
exit
!
interface GigabitEthernet0/0.100
encapsulation dot1Q 100
ip address 10.100.100.1 255.255.255.0
ipv6 address 2021:AC:AC:A100::1/64
ipv6 ospf 500 area 0
no shut
exit
!
interface GigabitEthernet0/0.200
encapsulation dot1Q 200
ip address 10.200.200.1 255.255.255.0
ipv6 address 2021:AC:AC:A200::1/64
ipv6 ospf 500 area 0
no shut
exit
!
interface GigabitEthernet0/0.220
encapsulation dot1Q 220
ip address 10.220.220.1 255.255.255.0
ipv6 address 2021:AC:AC:A220::1/64
ipv6 ospf 500 area 0
no shut
exit
!
router ospf 100
router-id 5.5.5.5
log-adjacency-changes
default-information originate
redistribute static
network 192.168.30.0 0.0.0.255 area 0
network 190.88.88.0 0.0.0.3 area 0
network 10.100.100.0 0.0.0.255 area 0
network 10.200.200.0 0.0.0.255 area 0
network 10.220.220.0 0.0.0.255 area 0
passive-interface g0/1
passive-interface g0/0
passive-interface g0/0.100
passive-interface g0/0.200
passive-interface g0/0.220
exit
!
ipv6 router ospf 500
router-id 5.5.5.5
log-adjacency-changes
default-information originate
redistribute static
passive-interface g0/1
passive-interface g0/0
passive-interface g0/0.100
passive-interface g0/0.200
passive-interface g0/0.220
```


---
>Produccion
```
enable
config t
ipv6 unicast-routing
!
interface GigabitEthernet0/0
ip address 192.168.10.1 255.255.255.0
ipv6 address 2021:AC:AC:40::1/64
ipv6 ospf 500 area 0
no shut
exit
!
interface GigabitEthernet0/1
ip address 190.99.99.2 255.255.255.252
ipv6 ospf 500 area 0
no shutdown
exit
!
interface GigabitEthernet0/2
ipv6 ospf 500 area 0
no shut
exit
!
router ospf 100
router-id 4.4.4.4
log-adjacency-changes
default-information originate
redistribute static
network 192.168.10.0 0.0.0.255 area 0
network 190.99.99.0 0.0.0.3 area 0
passive-interface g0/0
passive-interface g0/1
exit
!
ipv6 router ospf 500
router-id 4.4.4.4
log-adjacency-changes
default-information originate
redistribute static
passive-interface g0/0
passive-interface g0/1
exit
!
```

---
> ISP ENTEL
```
enable
config t
ipv6 unicast-routing
!
int g0/0
ip add 190.99.99.1 255.255.255.252
ipv6 add 2021:ac:ac:70::1/64
ipv6 ospf 500 area 0
no shut
exit
!
int g0/1
ip add 190.88.88.1 255.255.255.252
ipv6 add 2021:ac:ac:60::1/64
ipv6 ospf 500 area 0
no shut
exit
!
int g0/2
ip add 201.212.111.1 255.255.255.248
ipv6 add 2001:212:acad:a::1/64
ipv6 ospf 500 area 0
no shut
exit
!
!#Aqui le va la ruta estativa

```

---
---
Switch

>Switch0
```
enable
config t
vlan 100
exit
vlan 200
exit
vlan 220
exit
!
int range fa0/1-3
switchport mode trunk
switchport trunk allowed vlan 100,200,220
switchport nonegotiate
no shut
exit
```

> Switch2
```
enable
config t
vlan 100
exit
vlan 200
exit
vlan 220
exit
!
int f0/1
switchport mode trunk
switchport trunk allowed vlan 100,200,220
switchport nonegotiate
no shut
exit
!
int range f0/2-3
switchport mode access
switchport access vlan 100
no shut
exit
```

>Switch1
```
enable
config t
vlan 100
exit
vlan 200
exit
vlan 220
exit
!
int G0/2
switchport mode trunk
switchport trunk allowed vlan 100,200,220
switchport nonegotiate
no shut
exit
!
int f0/1
switchport mode access
switchport access vlan 100
no shut
exit
!
int f0/2
switchport mode access
switchport access vlan 200
no shut
exit
!
int f0/3
switchport mode access
switchport access vlan 220
no shut
exit
```