# Sobre
Tunnel Gre
# Requerimientos:
- Habilitar [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP|EIGRP]] para IPv4. Dejar interfaces pasivas según corresponda.
- Habilitar [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv3|OSPFv3]] para IPv6. Dejar interfaces pasivas según corresponda.
- Garantizar salida hacia ISP en IPv4/IPv6.
- Para IPv4, los PC deben poder salir con IP pública hacia Internet.
- Habilitar servicio Syslog en todos los routers, donde solo se capture desde el nivel 5. Estos mensajes deben ser reflejados en el programa TFTP64 de su PC.
- Implementar seguridad de puerto más restrictiva para los equipos finales.
- Los equipos finales no deben participar en STP además de no recibir BPDU.
- Implementar DHCP Snooping en interfaces adecuadas para aquello. Equipos finales no deben pedir más de 2 IP por minuto.
- Implemente Túnel 6 over 4, para permitir que los equipos finales lleguen a Internet en ambiente IPv6. Deberá comprobar funcionamiento.
- Levante y desactive túnel, donde los mensajes quedarán capturados en TFTP64.
# Imagen Topologia
![500](https://slink.proxylivy.work/image/07f382f7-1088-4b62-8715-14d1354e08dc.png)
# Configuracion
## R1
```
enable
config t
int e0/0
no shutdown
int e0/0.10
encapsulation do1q 10
ip address 172.16.10.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:10::1/64
exit
int e0/0.20
encapsulation do1q 20
ip address 172.16.20.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:20::1/64
exit
router eigrp 20
network 172.16.10.0 255.255.255.0
network 172.16.20.0 255.255.255.0
network 172.16.12.0 255.255.255.0
passive-interface e0/0.10
passive-interface e0/0.20
exit
ip dhcp excluded-address 172.16.10.1 172.16.10.4
ip dhcp pool VLAN10
network 172.16.10.0 255.255.255.0
default-router 172.16.10.1
dns-server 8.8.8.8
exit
!
logging on
logging host 192.168.56.1
logging source-interface s1/0
!
int tunnel 1
ipv6 address 3001:ACAD:ACAD:100::1/64
tunnel source s1/0
tunnel destination 172.16.32.3
tunnel mode ipv6ip
keepalive 10
exit
ipv6 unicast-routing
ipv6 router ospf 1
router-id 1.1.1.1
passive-interface e0/0.10
passive-interface e0/0.20
exit
int e0/0.10
ipv6 ospf 1 area 0
exit
int e0/0.20
ipv6 ospf 1 area 0
exit
int tunnel 1
ipv6 ospf 1 area 0
exit
```

## R2
```
enable
config t
router eigrp 20
network 172.16.12.0 255.255.255.0
network 172.16.32.0 255.255.255.0
network 192.168.56.0 255.255.255.0
passive-interface e0/0
logging on
logging host 192.168.56.1
logging trap 5
logging source-interface e0/0
```

## R3
```
enable
config t
ip route 0.0.0.0 0.0.0.0 209.50.0.2
router eigrp 20
network 172.16.32.0 255.255.255.0
redistribute static
exit
ip access-list standard INTERNET
permit 172.16.10.0 0.0.0.255
permit 172.16.20.0 0.0.0.255
exit
ip nat inside source list INTERNET int e0/0 overload
int e0/0
ip nat outside
exit
int s1/1
ip nat inside
exit
int tunnel 1
ipv6 address 3001:ACAD:ACAD:100::3/64
tunnel source s1/1
tunnel destination 172.16.12.1
tunnel mode ipv6ip
keepalive 5
exit
ipv6 unicast-routing
ipv6 router ospf 1
router-id 3.3.3.3
default-information originate
exit
int tunnel 1
ipv6 ospf 1 area 0
exit
ipv6 route ::/0 3001:ACAD:ACAD:209::2
```
## ISP
```
enable
config t
ipv6 unicast-routing
ipv6 route ::/0 3001:ACAD:ACAD:209::1
```
## SWA
```
enable
config t
int e0/0
duplex half
exit
int range e0/0,e0/2-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20
switchport nonegotiate
exit
int range e0/2-3
channel-group 1 mode active
exit
ip dhcp snooping
ip dhcp snooping vlan 10
no ip dhcp snooping information option
int e0/0
ip dhcp snooping trust
exit
```

## SWB
```
enable
config t
int range e0/0-1
duplex half
exit
int e0/1
switchport mode access
switchport access vlan 10
switchport port-security
switchport port-security maximum 3
switchport port-security mac-address sticky
switchport port-security violation shutdown
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int e0/0
switchport mode access
switchport access vlan 20
switchport port-security
switchport port-security maximum 3
switchport port-security mac-address sticky
switchport port-security violation shutdown
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int range e0/2-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20
switchport nonegotiate
exit
int range r0/2-3
channel-group 1 mode passive
exit
ip dhcp snooping
ip dhcp snooping vlan 10
no ip dhcp snooping information option
int port-channel 1
ip dhcp snooping trust
int e0/1
ip dhcp snooping limit rate 3
```
## PCVLAN10
```
enable
config t
int e0/1
no ipv6 address
ipv6 address 3001:ACAD:ACAD:10::A/64
exit
no ip domain-lookup
```
## TFTP64
Recuerda configurarlo en Syslog y con la interfaz Virtualbox que corresponde