## AD
La vlan nativa se configura como yo quiera
al trabajar, igual revisa la IP de los dispositivos

## Tabla Topologia

| Nombre          | VLAN | IPv4           | IPv6                   |
| --------------- | ---- | -------------- | ---------------------- |
| PERAEXTREME     | 10   | 172.16.10.0/24 | 3001:ACAD:ACAD:10::/64 |
| TERRIBLEBRIGIDO | 11   | 172.16.11.0/24 | 3001:ACAD:ACAD:20::/64 |
| UNLIKE          | 12   | 172.16.12.0/24 | 3001:ACAD:ACAD:30::/64 |
| NOLEDICOLOR     | 13   | 172.16.13.0/24 | 3001:ACAD:ACAD:40::/64 |
| ADMINISTRATIVA  | 14   | 172.16.14.0/24 | 3001:ACAD:ACAD:50::/64 |
| NATIVA          | 15   | N/A            | N/A                    |
## Imagen Topologia
![500](https://slink.proxylivy.work/image/1818cc4d-30b6-4d81-bbcf-57a26e33e061.png)
## Anexo MST
Nota: El Port-channel 1(DLS1 a DLS2 en las puertas e0/0-1) se apaga
Nota2: VLAN Administrativa y Nativa quedaran en la instancia 0 por defecto
![500](https://slink.proxylivy.work/image/fc4a43d5-2859-454a-8157-5776549ed644.png)
## Recuerda
- user:root/pass:cisco
- Poner la maquina en `adaptador puente` y otras configuraciones
- `dhclient` / `loadkeys es` / `ip a | grep inet` permite acceder a la pagina web
`vtp mode transparent` -> Guarda las vlans
`copy running-config unix:` -> Crea una copia de las configuraciones, luego en "Devices" lo guarda para luego exportarla

## Configuracion
### ALS1
```
vlan 10
name PERAEXTREME
exit
vlan 11
name TERRIBLEBRIGIDO
exit
vlan 12
name UNLIKE
exit
vlan 13
name NOLEDICOLOR
exit
vlan 14
name ADMINISTRATIVA
exit
vlan 15
name NATIVA
exit
vtp mode transparent
int e0/2
switchport mode access
switchport access vlan 10
switchport port-security
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int e0/3
switchport mode access
switchport access vlan 11
switchport port-security
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int range e0/0-1, e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,11,12,13,14
switchport trunk native vlan 15
switchport nonegotiate
exit
int range e1/0-1
channel-group 2 mode auto
exit
int range e1/2-3
channel-group 3 mode passive
exit
int range e0/0-1
channel-group 6 mode on
exit
int vlan 14
ip address 172.16.14.4 255.255.255.0
no shut
exit
ip default-gateway 172.16.14.1
spanning-tree mode mst
spanning-tree mst configuration
name REGION1
revision 1
instance 1 vlan 10,11
instance 2 vlan 12,13
exit
spanning-tree mst hello-time 1
spanning-tree mst forwarding-time 6
spanning-tree mst max-age 8
int port-channel 2
spanning-tree mst 0 cost 200000000
end
copy running-config unix:
```
## ALS2
```
vlan 10
name PERAEXTREME
exit
vlan 11
name TERRIBLEBRIGIDO
exit
vlan 12
name UNLIKE
exit
vlan 13
name NOLEDICOLOR
exit
vlan 14
name ADMINISTRATIVA
exit
vlan 15
name NATIVA
exit
vtp mode transparent
int e0/2
switchport mode access
switchport access vlan 12
switchport port-security
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int e0/3
switchport mode access
switchport access vlan 13
switchport port-security
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int range e0/0-1, e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,11,12,13,14
switchport trunk native vlan 15
switchport nonegotiate
exit
int range e1/0-1
channel-group 5 mode passive
exit
int range e1/2-3
channel-group 4 mode on
exit
int range e0/0-1
channel-group 6 mode on
exit
int vlan 14
ip address 172.16.14.3 255.255.255.0
no shut
exit
ip default-gateway 172.16.14.1
spanning-tree mode mst
spanning-tree mst configuration
name REGION1
revision 1
instance 1 vlan 10,11
instance 2 vlan 12,13
exit
spanning-tree mst hello-time 1
spanning-tree mst forwarding-time 6
spanning-tree mst max-age 8
spanning-tree mst 0 root primary
int port-channel 4
spanning-tree mst 1,2 cost 200000000
exit
```
## DLS1
```
vlan 10
name PERAEXTREME
exit
vlan 11
name TERRIBLEBRIGIDO
exit
vlan 12
name UNLIKE
exit
vlan 13
name NOLEDICOLOR
exit
vlan 14
name ADMINISTRATIVA
exit
vlan 15
name NATIVA
exit
vtp mode transparent
no ipv6 unicast-routing
ipv6 unicast-routing
int range e0/0-1, e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,11,12,13,14
switchport trunk native vlan 15
switchport nonegotiate
exit
int range e0/0-1
channel-group 1 mode auto
exit
int range e1/2-3
channel-group 4 mode on
exit
int range e1/0-1
channel-group 2 mode desirable
exit
int port-channel 1
shutdown
exit
int vlan 14
ip address 172.16.14.1 255.255.255.0
no shut
exit
ip default-gateway 172.16.14.1
spanning-tree mode mst
spanning-tree mst configuration
name REGION1
revision 1
instance 1 vlan 10,11
instance 2 vlan 12,13
exit
spanning-tree mst hello-time 1
spanning-tree mst max-age 8
spanning-tree mst forwarding-time 6
exit
copy running-config unix:
```
## DLS2
```
vlan 10
name PERAEXTREME
exit
vlan 11
name TERRIBLEBRIGIDO
exit
vlan 12
name UNLIKE
exit
vlan 13
name NOLEDICOLOR
exit
vlan 14
name ADMINISTRATIVA
exit
vlan 15
name NATIVA
exit
vtp mode transparent
no ipv6 unicast-routing
ipv6 unicast-routing
int range e0/0-2, e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,11,12,13,14
switchport trunk native vlan 15
switchport nonegotiate
exit
int range e0/0-1
channel-group 1 mode desirable
exit
int range e1/2-3
channel-group 3 mode active
exit
int range e1/0-1
channel-group 5 mode active
exit
int port-channel 1
shutdown
exit
int vlan 14
ip address 172.16.14.2 255.255.255.0
no shut
exit
ip default-gateway 172.16.14.1
spanning-tree mode mst
spanning-tree mst configuration
name REGION1
revision 1
instance 1 vlan 10,11
instance 2 vlan 12,13
exit
spanning-tree mst hello-time 1
spanning-tree mst forwarding-time 6
spanning-tree mst max-age 8
int vlan 10
ip address 172.16.10.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:10::1/64
no shut
exit
int vlan 11
ip address 172.16.11.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:11::1/64
no shut
exit
int vlan 12
ip address 172.16.12.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:12::1/64
no shut
exit
int vlan 13
ip address 172.16.13.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:13::1/64
no shut
exit
int vlan 14
ip address 172.16.14.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:14::1/64
no shut
exit
int e0/2
no switchport
ip address 172.16.21.1 255.255.255.252
ipv6 address 3001:ACAD:ACAD:21::1/112
exit
router ospf 1
router-id 2.2.2.2
area 1 stub no-summary
network 172.16.10.0 0.0.0.255 area 1
network 172.16.11.0 0.0.0.255 area 1
network 172.16.12.0 0.0.0.255 area 1
network 172.16.13.0 0.0.0.255 area 1
network 172.16.14.0 0.0.0.255 area 1
network 172.16.21.0 0.0.0.3 area 1
passive-interface vlan 10
passive-interface vlan 11
passive-interface vlan 12
passive-interface vlan 13
passive-interface vlan 14
exit
ipv6 router ospf 1
router-id 2.2.2.2
area 1 stub no-summary
passive-interface vlan 10
passive-interface vlan 11
passive-interface vlan 12
passive-interface vlan 13
passive-interface vlan 14
exit
int vlan 10
ipv6 ospf 1 area 1
exit
int vlan 11
ipv6 ospf 1 area 1
exit
int vlan 12
ipv6 ospf 1 area 1
exit
int vlan 13
ipv6 ospf 1 area 1
exit
int vlan 14
ipv6 ospf 1 area 1
exit
int e0/2
ipv6 ospf 1 area 1
exit
```
### Router Borde
```
no ipv6 unicast-routing
ipv6 unicast-routing
router ospf 1
router-id 1.1.1.1
network 172.16.21.0 0.0.0.3 area 1
area 1 stub no-summary
default-information originate
exit
ipv6 router ospf 1
router-id 1.1.1.1
default-information originate
area 1 stub no-summary
exit
int e0/2
ipv6 ospf 1 area 1
exit
ip route 0.0.0.0 0.0.0.0 200.0.0.1
ipv6 route ::/0 3001:ACAD:ACAD:200::1
```
### Router ISP
```
no ipv6 unicast-routing
ipv6 unicast-routing
ip route 0.0.0.0 0.0.0.0 200.0.0.2
ipv6 route ::/0 3001:ACAD:ACAD:200::2
```
## Visualizacion/Troubleshooting
