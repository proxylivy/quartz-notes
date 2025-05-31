# Sobre
Seguridad en Capa 2
## Topologia
![500](https://slink.proxylivy.work/image/004e314e-dfc6-4fc1-94eb-8c94c9e31152.png)
# Requerimientos:
## Protocolos de Enrutamiento IGP:
- Habilitar protocolos de enrutamiento IGP en IPv4/IPv6 dejando interfaces pasivas según corresponda.
- Autenticar protocolo de enrutamiento.
- Router R1 debe proveer de IP dinámica a PC de VLAN20. 
- Permitir salida hacia red externa en IPv4/IPv6.
- Para IPv4 todas las redes de equipos finales, deben salir traducido por IP pública. Comprobar traducción IP.
## Seguridad en Capa 2:
- Crear VLANs propuestas.
- Implementar solución de agregado de enlace, en base a protocolos propuestos.
- Implementar mecanismo de seguridad que evite el VLAN Prunning.
- Implementar mecanismo de seguridad que asegure el root de la capa 2.
- Interfaces que conectan a equipos finales, no deben aprender más de dos direcciones MAC.
- Interfaces que conectan a equipos finales NO deben enviar ni recibir BPDU.
- Implementar mecanismo de DHCP Snooping en la topología. Además equipos finales no deben pedir más de 2 IP por segundo.
## Seguridad Perimetral:
- Crear zonas propuestas.
- Permitir que el tráfico que salga desde zona INTERNA hacia zona EXTERNA sea inspeccionado.
- Realizar las configuraciones necesarias para que desde la red EXTERNA los mensajes ICMP no sean respondidos.
- Comprobar funcionamiento en la topología.
# Configuracion
## R1
```
enable
config t
int e0/0
no shut
exit
int e0/0.10
encapsulation dot1q 10
ip address 172.16.10.1 255.255.255.0
ipv6 address 33001:ACAD:ACAD:10::1/64
exit
int e0/0.20
encapsulation dot1q 20
ip address 172.16.20.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:20::1/64
exit
router eigrp 123
network 172.16.10.0 255.255.255.0
network 172.16.20.0 255.255.255.0
network 192.168.12.0 255.255.255.0
passive-interface e0/0.10
passive-interface e0/0.20
exit
ipv6 unicast-routing
ipv6 router eigrp 123
passive-interface e0/0.10
passive-interface e0/0.20
exit
int e0/0.10
ipv6 eigrp 123
exit
int e0/0.20
ipv6 eigrp 123
exit
int s1/1
ipv6 eigrp 123
exit
key chain LLAVERO
key 1
key-string perita
exit
exit
int s1/1
ip authentication key-chain eigrp 123 LLAVERO
ip authentication mode eigrp 123 md5
exit
ip dhcp excluded-address 172.16.20.1 172.16.20.4
ip dhcp pool VLAN20
network 172.16.20.0 255.255.255.0
default-router 172.16.20.1
dns-server 8.8.8.8
exit
ip route 0.0.0.0 0.0.0.0 209.50.0.2
ipv6 route ::/0 3001:ACAD:ACAD:209::2
router eigrp 123
redistribute static
exit
ipv6 router eigrp 123
redistribute static
exit
ip access-list standard INTERNET
permit 172.16.10.0 0.0.0.255
permit 172.16.20.0 0.0.0.255
permit 192.168.44.0 0.0.0.255
exit
ip nat inside source list INTERNET int s1/2 overload
int s1/2
ip nat outside
exit
int e0/0.10
ip nat inside
exit
int e0/0.20
ip nat inside
exit
int s1/1
ip nat inside
exit
```

## R2
```
enable
config t
router eigrp 123
network 192.168.23.0 255.255.255.0
network 192.168.12.0 255.255.255.0
exxit
ipv6 unicast-routing
ipv6 router eigrp 123
exit
int s1/0
ipv6 eigrp 123
exit
int s1/1
ipv6 eigrp 123
exit
key chain LLAVERO
key 1
key-string perita
exit
exit
int s1/1
ip authentication key-chain eigrp 123 LLAVERO
ip authentication mode eigrp 123 md5
exit
int s1/0
ip authentication key-chain eigrp 123 LLAVERO
ip authentication mode eigrp 123 md5
exit
```

## R3
```
enable
config t
router eigrp 123
network 192.168.23.0 255.255.255.0
exit
router ospf 1
router-id 3.3.3.3
network 192.168.34.0 255.255.255.0 area 0
exit
ipv6 unicast-routing
ipv6 router eigrp 123
exit
int s1/0
ipv6 eigrp 123
exit
ipv6 router ospf 1
router-id 3.3.3.3
exit
int s1/1
ipv6 ospf 1 area 0
exit
key chain LLAVERO
key 1
key-string perita
exit
exit
int s1/0
ip authentication key-chain eigrp 123 LLAVERO
ip authentication mode eigrp 123 md5
exit
int s1/1
ip ospf authentication message-digest
ip ospf message-digest-key 1 md5 modoshorizo
exit
router ospf 1
redistribute eigrp 123 subnets
default-information originate
exit
router eigrp 123
redistribute ospf 1 metric 1544 2000 255 1 1500
exit
ipv6 router ospf 1
redistribute eigrp 123 include-connected
default-information originate
exit
ipv6 router eigrp 123
redistribute ospf 1 metric 1544 2000 255 1 1500 include-connected
exit
```

## R4
```
enable
config t
router ospf 1
router-id 4.4.4.4
network 192.168.44.0 255.255.255.0 area 0
network 192.168.34.0 255.255.255.0 area 0
passive-interface e0/3
exit
ipv6 unicast-routing
ipv6 router ospf 1
router-id 4.4.4.4
exit
int e0/3
ipv6 ospf 1 area 0
exit
int s1/1
ipv6 ospf 1 area 0
ip authentication key-chain eigrp 123 LLAVERO
ip authentication mode eigrp 123 md5
exit
zone security INTERNA
exit
zone security EXTERNA
exit
ip access extended LAN
permit ip 192.168.44.0 0.0.0.255 any
exit
class-map type inspect match-all CLASE
match access-group name LAN
exit
policy-map type inspect POLITICA
class type inspect CLASE
inspect
exit
exit
zone-pair security ZONA-PAR source INTERNA destination EXTERNA
service-policy type inspect POLITICA
exit
int e0/3
zone-member security INTERNA
exit
int s1/1
zone-member security EXTERNA
exit
```

## ISP
```
enable
config t
ipv6 unicast-routing
ipv6 route ::/0 3001:ACAD:ACAD:209::1
```

# SWA
```
enable
config t
int e0/0
duplex half
exit
vlan 10,20
exit
vtp mode transparent
int range e0/0-2, e1/0-1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20
switchport nonegotiate
exit
int range e0/1-2
channel-group 1 mode on
exit
int range e1/0-1
channel-group 3 mode desirable
exit
ip dhcp snooping
ip dhcp snooping vlan 20
no ip dhcp snooping information option
int e0/0
ip dhcp snooping trust
```

## SWB
```
enable
config t
int e0/3
duplex half
exit
vlan 10,20
exit
vtp mode transparent
int e0/3
switchport mode access
switchport access vlan 10
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation restrict
spanning-tree portfast
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
int range e0/1-2, e1/2-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20
switchport nonegotiate
exit
int range e0/1-2
channel-group 1 mode on
exit
int range e1/2-3
channel-group 2 mode passive
exit
```

## SWC
```
enable
config t
int e0/3
duplex half
exit
vlan 10,20
exit
vtp mode transparent
int e0/3
switchport mode access
switchport access vlan 20
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation restrict
spanning-tree portfast
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
exit
int range e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20
switchport nonegotiate
exit
int range e1/2-3
channel-group 2 mode active
exit
int range e1/0-1
channel-group 3 mode auto
exit
ip dhcp snooping
ip dhcp snooping vlan 20
no ip dhcp snooping information option
int range port-channel 2-3
ip dhcp snooping trust
exit
ip dhcp snooping limit rate 2
```