## Topologia Prueba
![500](https://slink.proxylivy.work/image/0df67766-cc6c-4318-93f6-6543c6e31486.png)
## R1
```
enable
config t
ipv6 unicast-routing
router ospf 1
router-id 1.1.1.1
network 192.168.21.0 255.255.255.0 area 0
network 192.168.32.0 255.255.255.0 area 0
default-information originate
exit
!
ipv6 router ospf 1
router-id 1.1.1.1
default-information originate
exit
int s1/1
ipv6 ospf 1 area 0
exit
!
ip route 0.0.0.0 0.0.0.0 219.50.0.2
ipv6 route ::/0 3001:ACAD:ACAD:209::2
!
username R1 privilege 15 secret seguridad
username USER4 privilege 4 secret seguridad4
privilege exec level 2 show version
privilege exec level 2 show
privilege exec level 2 show address
privilege exec level 2 show ip
privilege exec level 2 show int
privilege exec level 2 no
privilege exec level 2 show arp
privilege exec level 2 show mac
!
```

## R2
```
enable
config t
router ospf 1
router-id 2.2.2.2
network 192.168.21.0 255.255.255.0 area 0
network 192.168.32.0 255.255.255.0 area 0
exit
ipv6 unicast-routing
ipv6 router ospf 1
router-id 2.2.2.2
exit
int s1/1
ipv6 ospf 1 area 0
exit
int s1/0
ipv6 ospf 1 area 0
```

## R3
```
enable
config t
router ospf 1
router-id 3.3.3.3
network 192.168.32.0 255.255.255.0 area 0
network 192.168.21.0 255.255.255.0 area 0
network 192.168.43.0 255.255.255.0 area 1
network 192.168.45.0 255.255.255.0 area 1
network 192.168.44.0 255.255.255.0 area 1
passive-interface e0/0
exit
ipv6 unicast-routing
ipv6 router ospf 1
router-id 3.3.3.3
passive-interface e0/0
exit
int s1/1
ipv6 ospf 1 area 0
exit
int s1/0
ipv6 ospf 1 area 0
exit
line console 0
login local
exit
username R3 privilege 15 secret seguridad
aaa new-model
radius-server host 192.168.56.102 auth-port 1812 acct-port 1813 key WinRadius
radius-server host 192.168.56.102 auth-port 1812 acct-port 1813 key WinRadius
aaa authentication login default group radius local-case
enable secret seguridad5
parser view VISTA-PRUEBA
secret seguridad5
commands exec include show ip
commands exec include show int
commands exec include configure terminal
commands exec include show ip ospf database
commands exec include show ip eigrp topology
end
```

## R4
```
enable
config t
router ospf 1
router-id 4.4.4.4
network 192.168.43.0 255.255.255.0 area 1
network 192.168.45.0 255.255.255.0 area 1
network 192.168.44.0 255.255.255.0 area 1
exit
ipv6 unicast-routing
ipv6 router ospf 1
router-id 4.4.4.4
exit
int range e0/1-2
ipv6 ospf 1 area 1
exit
int s1/1
ipv6 ospf 1 area 0
end

```
## ISP
```
enable
config t
ipv6 unicast-routing
ip route 0.0.0.0 0.0.0.0 219.50.0.1
ipv6 route ::/0 3001:ACAD:ACAD:209::1
end
```