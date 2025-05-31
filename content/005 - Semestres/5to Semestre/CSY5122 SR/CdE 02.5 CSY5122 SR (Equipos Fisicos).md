Gane las 3 decimas solo con esto ^^
# Topologia
![500](https://slink.proxylivy.work/image/338dba7a-18fe-4c5d-9a6a-06cd2319e08c.png)

| Nombre   | VLAN | IPv4            | IPv6                   |
| -------- | ---- | --------------- | ---------------------- |
| Soporte  | 10   | 192.168.10.0/24 | 3001:ABCD:ACAD:10::/64 |
| Finanzas | 20   | 192.168.20.0/24 | 3001:ABCD:ACAD:20::/64 |

# Configuracion

## R1
```
enable
configure replace flash:ccna.txt force
config t
int g0/0
no shut
exit
!
int s0/0/0
ip address 200.200.0.1 255.255.255.248
ipv6 address 3001:ABCD:ABCD:200::1/112
exit
!
int g0/0.10
encapsulation dot1q 10
ip address 192.168.10.1 255.255.255.0
ipv6 address 3001:ABCD:ACAD:10::1/64
exit
int g0/0.20
encapsulation dot1q 20
ip address 192.168.20.1 255.255.255.0
ipv6 address 3001:ABCD:ACAD:20::1/64
exit
ip dhcp excluded-address 192.168.10.1 192.168.10.4
ip dhcp excluded-address 192.168.20.1 192.168.20.4
ip dhcp pool VLAN10
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 8.8.8.8
exit
ip dhcp pool VLAN20
network 192.168.20.0 255.255.255.0
default-router 192.168.20.1
dns-server 8.8.8.8
exit
!
ip route 0.0.0.0 0.0.0.0 200.200.0.3
ipv6 route ::/0 3001:ABCD:ABCD:200::3
```

## R3
```
enable
configure replace flash:ccna.txt force
config t
ipv6 unicast-routing
int s0/0/0
ip address 200.200.0.3 255.255.255.248
ipv6 address 3001:ABCD:ABCD:200::3/112
exit
int loopback 0
ip address 8.8.8.8 255.255.255.252
ipv6 address 3001:ABCD:ABCD:8::8/128
exit
ip route 0.0.0.0 0.0.0.0 200.200.0.1
ipv6 route ::/0 3001:ABCD:ABCD:200::1
```

## SWA
```
enable
config t
vlan 10,20
exit
spanning-tree mode rapid-pvst
int range f0/6,f0/12
switchport mode trunk
switchport trunk allowed vlan 10,20
switchport nonegotiate
exit
int f0/5
switchport mode access
switchport access vlan 10
switchport port-security
switchport port-security maximum 3
switchport port-security mac-address sticky
switchport port-security violation restrict
spanning-tree portfast
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
exit
!
ip dhcp snooping
ip dhcp snooping vlan 10,20
no ip dhcp snooping information option
int f0/6
ip dhcp snooping trust
ip dhcp snooping limit rate 3
```

## SWB
```
enable
config t
vlan 10,20
exit
spanning-tree mode rapid-pvst
!
int range f0/12,f0/18
switchport mode trunk
switchport trunk allowed vlan 10,20
switchport nonegotiate
exit
!
int f0/5
switchport mode access
switchport access vlan 20
switchport port-security
spanning-tree portfast
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
exit
!
ip dhcp snooping
ip dhcp snooping vlan 10,20
no ip dhcp snooping information option
int f0/18
ip dhcp snooping trust
ip dhcp snooping limit rate 3
```

## SWC
```
vlan 10,20
exit
!
spanning-tree mode rapid-pvst
!
int range f0/18,f0/6,f0/1
switchport mode trunk
switchport trunk allowed vlan 10,20
switchport nonegotiate
exit
!
ip dhcp snooping
ip dhcp snooping vlan 10,20
no ip dhcp snooping information option
int f0/1
ip dhcp snooping trust
ip dhcp snooping limit rate 3
```