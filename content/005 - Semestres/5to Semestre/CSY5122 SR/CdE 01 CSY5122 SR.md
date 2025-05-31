## Imagen Topologia
![500](https://slink.proxylivy.work/image/8e3eee5c-28e4-40c5-bf39-585b4d444aeb.png)
Nota, No Hago NAT debido al problema del congelamiento sino ruta estatica
## R1
```
enable
config t
router rip
version 2
no auto-summary
network 172.16.12.0
network 172.16.23.0
default-information originate
exit
ipv6 unicast-routing
ipv6 router rip GPT
exit
int s1/1
ipv6 rip GPT enable
exit
!
ip route 0.0.0.0 0.0.0.0 209.50.0.2
ipv6 route ::/0 3001:ACAD:ACAD:209::2
!
exit
copy running-config unix:
```
## R2
```
enable
config t
router rip
version 2
no auto-summary
network 172.16.12.0
network 172.16.23.0
exit
ipv6 unicast-routing
ipv6 router rip GPT
exit
int s1/1
ipv6 rip GPT enable
exit
int s1/0
ipv6 rip GPT enable
exit

```
### Verificar Rutas RIP R2
```
show ip route rip
show ipv6 route rip
```
## R3
```
enable
config t
router ospf 1
router-id 3.3.3.3
network 172.16.34.0 255.255.255.0 area 0
network 172.16.45.0 255.255.255.0 area 0
network 172.16.55.0 255.255.255.0 area 0
passive-interface e0/0
exit
!
ipv6 unicast-routing
ipv6 router ospf 1
router-id 3.3.3.3
passive-interface e0/0
exit
!
router rip
version 2
no auto-summary
network 172.16.23.0
network 172.16.12.0
passive-interface e0/0
exit
!
ipv6 router rip GPT
exit
!
int s1/0
ipv6 rip GPT enable
exit
int s1/1
ipv6 ospf 1 area 0
exit
!
router ospf 1
redistribute rip subnets
exit
!
router rip
redistribute ospf 1 metric 1
exit
!
ipv6 router ospf 1
redistribute rip GPT include-connected
exit
!
ipv6 router rip GPT
redistribute ospf 1 include-connected metric 1
exit
```

### Revisar Tablas de RIP/OSPF
```
show ip route rip
show ipv6 route rip
```

## R4
```
enable
config t
router ospf 1
router-id 4.4.4.4
network 172.16.34.0 255.255.255.0 area 0
network 172.16.45.0 255.255.255.0 area 0
exit
ipv6 unicast-routing
ipv6 router ospf 1
router-id 4.4.4.4
exit
int s1/1
ipv6 ospf 1 area 0
exit
int s1/2
ipv6 ospf 1 area 0
exit
```

## R5
```
enable
config t
router ospf 1
router-id 5.5.5.5
network 172.16.45.0 255.255.255.0 area 0
network 172.16.34.0 255.255.255.0 area 0
passive-interface e0/1
exit
ipv6 unicast-routing
ipv6 router ospf 1
router-id 5.5.5.5
passive-interface e0/1
exit
int s1/2
ipv6 ospf 1 area 0
```

### Probar ping desde PC
```
enable
ping 8.8.8.8
```

## ISP
```
enable
config t
ipv6 unicast-routing
ip route 0.0.0.0 0.0.0.0 209.50.0.1
ipv6 route ::/0 3001:ACAD:ACAD:209::1
```