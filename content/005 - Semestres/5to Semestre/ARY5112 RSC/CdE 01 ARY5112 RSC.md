## TODO
- Modifica las puertas bloqueadas en MST
- Tengo Problemas con la 100.5 y la 40.5

Link Ayuda [Google Drive](https://drive.google.com/file/d/1n5p-IW5K749FqzMcpzNI3x5OQHR84UC6/view?usp=sharing)
[Video 3](https://duoccl0-my.sharepoint.com/personal/v_aranedas_profesor_duoc_cl/_layouts/15/onedrive.aspx?ga=1&id=%2Fpersonal%2Fv%5Faranedas%5Fprofesor%5Fduoc%5Fcl%2FDocuments%2FARY5112%20%2D%20Routing%20y%20Switching%20Corporativo%2FVideosTutoriales) es lo mismo
## Tabla Topologia

| Nombre          | VLAN | IPv4             | IPv6                   |
| --------------- | ---- | ---------------- | ---------------------- |
| CONLAPERABELICA | 30   | 192.168.30.0/24  | 3001:ACAD:ACAD:A1::/64 |
| PUROMIEDO       | 40   | 192.168.40.0/24  | 3001:ACAD:ACAD:A3::/64 |
| NOPASANA        | 50   | 192.168.50.0/24  | 3001:ACAD:ACAD:A5::/64 |
| IZIPIZI         | 60   | 192.168.60.0/24  | 3001:ACAD:ACAD:A7::/64 |
| ADM             | 100  | 192.168.100.0/24 | 3001:ACAD:ACAD:A9::/64 |
| NATIVA          | 200  | N/A              | N/A                    |
## Imagen Topologia
![500](https://slink.proxylivy.work/image/d0562689-4b79-44db-b9b1-a3e2cbd286b9.png)

## Recuerda
- user:root/pass:cisco
- Poner la maquina en `adaptador puente` y otras configuraciones
- `dhclient` / `loadkeys es` / `ip a | grep inet` permite acceder a la pagina web
`vtp mode transparent` -> Guarda las vlans
`copy running-config unix:` -> Crea una copia de las configuraciones, luego en "Devices" lo guarda para luego exportarla

# Configuracion
## Switching
### SW1
```
vlan 30
name CONLAPERABELICA
exit
vlan 40
name PUROMIEDO
exit
vlan 50
name NOPASANA
exit
vlan 60
name IZIPIZI
exit
vlan 100
name ADM
exit
vlan 200
name NATIVA
exit
vtp mode transparent
int e0/2
switchport mode access
switchport access vlan 30
switchport port-security
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int e0/3
switchport mode access
switchport access vlan 40
switchport port-security
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int range E1/0-3, E0/0-1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 30,40,50,60,100
switchport trunk native vlan 200
switchport nonegotiate
exit
int range E1/0-1
channel-group 3 mode auto
exit
int range E1/2-3
channel-group 5 mode on
exit
int range E0/0-1
channel-group 1 mode active
exit
spanning-tree mode mst
spanning-tree mst configuration
name REGION34
revision 1
instance 1 vlan 30,60
instance 2 vlan 40,50
instance 3 vlan 100,200
exit
spanning-tree mst hello-time 1
spanning-tree mst forward-time 8
spanning-tree mst max-age 10
interface port-channel 1
shutdown
exit
int port-channel 5
spanning-tree mst 0 cost 20000000
exit
interface vlan 100
ip address 192.168.100.4 255.255.255.240
no shut
exit
ip default-gateway 192.168.100.1
exit
copy running-config unix:
```
### MLS1
```
int 0/0
duplex half
exit
vlan 30
name CONLAPERABELICA
exit
vlan 40
name PUROMIEDO
exit
vlan 50
name NOPASANA
exit
vlan 60
name IZIPIZI
exit
vlan 100
name ADM
exit
vlan 200
name NATIVA
exit
vtp mode transparent
ip routing
no ipv6 unicast-routing
ipv6 unicast-routing
int range e1/0-3, e0/1-2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 30,40,50,60,100
switchport trunk native vlan 200
switchport nonegotiate
exit
int e0/0
no switchport
exit
int range e1/0-1
channel-group 3 mode desirable
exit
int range e1/2-3
channel-group 6 mode on
exit
int range e0/1-2
channel-group 2 mode active
exit
spanning-tree mode mst
spanning-tree mst configuration
name REGION34
revision 1
instance 1 vlan 30,60
instance 2 vlan 40,50
instance 3 vlan 100,200
exit
spanning-tree mst hello-time 1
spanning-tree mst forward-time 8
spanning-tree mst max-age 10
spanning-tree mst 0 root primary
!
interface vlan 30
ip address 192.168.30.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:A1::1/64
no shut
exit
interface vlan 40
ip address 192.168.40.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:A3::1/64
no shut
exit
interface vlan 50
ip address 192.168.50.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:A5::1/64
no shut
exit
interface vlan 60
ip address 192.168.60.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:A7::1/64
no shut
exit
interface vlan 100
ip address 192.168.100.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:A9::1/64
no shut
exit
ip default-gateway 192.168.100.1
!
int e0/0
no switchport
ip address 192.168.10.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:10::1/64
exit
!
ip dhcp excluded-address 192.168.30.1 192.168.30.4
ip dhcp excluded-address 192.168.40.1 192.168.40.4
ip dhcp excluded-address 192.168.50.1 192.168.50.4
ip dhcp excluded-address 192.168.60.1 192.168.60.4
ip dhcp pool VLAN30
network 192.168.30.0 255.255.255.0
default-router 192.168.30.1
dns-server 8.8.8.8
exit
ip dhcp pool VLAN40
network 192.168.40.0 255.255.255.0
default-router 192.168.30.1
dns-server 8.8.8.8
exit
ip dhcp pool VLAN50
network 192.168.50.0 255.255.255.0
default-router 192.168.30.1
dns-server 8.8.8.8
exit
ip dhcp pool VLAN60
network 192.168.60.0 255.255.255.0
default-router 192.168.30.1
dns-server 8.8.8.8
exit
!
router ospf 1
router-id 3.3.3.3
network 192.168.10.0 0.0.0.255 area 10
network 192.168.30.0 0.0.0.255 area 10
network 192.168.40.0 0.0.0.255 area 10
network 192.168.50.0 0.0.0.255 area 10
network 192.168.60.0 0.0.0.255 area 10
network 192.168.100.0 0.0.0.255 area 10
passive-interface vlan 30
passive-interface vlan 40
passive-interface vlan 50
passive-interface vlan 60
passive-interface vlan 100
exit
int vlan 30
ipv6 ospf 1 area 10
exit
int vlan 40
ipv6 ospf 1 area 10
exit
int vlan 50
ipv6 ospf 1 area 10
exit
int vlan 60
ipv6 ospf 1 area 10
exit
int vlan 100
ipv6 ospf 1 area 10
exit
int e0/0
ipv6 ospf 1 area 10
end
copy running-config unix:
```
### SW2
```
int range e0/2-3
duplex half
exit
vlan 30
name CONLAPERABELICA
exit
vlan 40
name PUROMIEDO
exit
vlan 50
name NOPASANA
exit
vlan 60
name IZIPIZI
exit
vlan 100
name ADM
exit
vlan 200
name NATIVA
exit
vtp mode transparent
int e0/2
switchport mode access
switchport access vlan 50
switchport port-security
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int e0/3
switchport mode access
switchport access vlan 60
switchport port-security
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int range e0/0-1, e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 30,40,50,60,100
switchport trunk native vlan 200
switchport nonegotiate
exit
int range e0/0-1
channel-group 1 mode passive
exit
int range e1/2-3
channel-group 6 mode on
exit
int range e1/0-1
channel-group 4 mode desirable
exit
interface port-channel 1
shutdown
exit
spanning-tree mode mst
spanning-tree mst configuration
name REGION12
revision 2
instance 1 vlan 30,60
instance 2 vlan 40,50
instance 3 vlan 100,200
exit
spanning-tree mst hello-time 1
spanning-tree mst forward-time 8
spanning-tree mst max-age 10
interface vlan 100
ip address 192.168.100.3 255.255.255.240
no shut
exit
ip default-gateway 192.168.100.1
exit
copy running-config unix:
```
### MLS2
```
int e0/3
duplex half
exit
vlan 30
name CONLAPERABELICA
exit
vlan 40
name PUROMIEDO
exit
vlan 50
name NOPASANA
exit
vlan 60
name IZIPIZI
exit
vlan 100
name ADM
exit
vlan 200
name NATIVA
exit
vtp mode transparent
no ipv6 unicast-routing
ipv6 unicast-routing
int e0/3
switchport mode access
switchport access vlan 100
switchport port-security
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int range e0/1-2, e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 30,40,50,60,100
switchport trunk native vlan 200
switchport nonegotiate
exit
int range e0/1-2
channel-group 2 mode passive
exit
int range e1/0-1
channel-group 4 mode auto
exit
int range e1/2-3
channel-group 5 mode on
exit
spanning-tree mode mst
spanning-tree mst configuration
name REGION34
revision 1
instance 1 vlan 30,60
instance 2 vlan 40,50
instance 3 vlan 100,200
exit
spanning-tree mst hello-time 1
spanning-tree mst forward-time 8
spanning-tree mst max-age 10
spanning-tree mst 1-2 root primary
interface vlan 100
ip address 192.168.100.2 255.255.255.240
no shut
exit
ip default-gateway 192.168.100.1
exit
copy running-config unix:
```

R1
```
no ipv6 unicast-routing
ipv6 unicast-routing
router ospf 1
router-id 1.1.1.1
network 192.168.10.0 0.0.0.255 area 10
network 192.168.12.0 0.0.0.255 area 0
exit
ipv6 router ospf 1
router-id 1.1.1.1
exit
int e0/0
ipv6 ospf 1 area 10
exit
int e0/1
ipv6 ospf 1 area 0
end
copy running-config unix:
```

R2
```
no ipv6 unicast-routing
ipv6 unicast-routing
router ospf 1
router-id 2.2.2.2
network 192.168.12.0 0.0.0.255 area 0
network 192.168.0.0 0.0.127.255 area 10
default-information originate
exit
ipv6 router ospf 1
router-id 2.2.2.2
default-information originate
exit
int e0/1
ipv6 ospf 1 area 0
exit
ip route 0.0.0.0 0.0.0.0 215.0.0.2
ip route 0.0.0.0 0.0.0.0 205.0.0.2 10
ipv6 route ::/0 3001:ACAD:ACAD:215::2
ipv6 route ::/0 3001:ACAD:ACAD:205::2 10
end
copy running-config unix:
```

### ISP
```
no ipv6 unicast-routing
ipv6 unicast-routing
ip route 0.0.0.0 0.0.0.0 215.0.0.1
ip route 0.0.0.0 0.0.0.0 205.0.0.1 10
ipv6 route ::/0 3001:ACAD:ACAD:215::1
ipv6 route ::/0 3001:ACAD:ACAD:205::1 10
end
copy running-config unix:
```

## Extras
### Truco de feria
Se usa con las instancias que no sean 0, ese es el CSIT Root
Primero SW1
```
spanning-tree mst 1-2 root primary
```
Despues MLS2 se lo quita
```
spanning-tree mst 1-2 root primary
```
y deberia bloquear la ruta al SW1
???
Luego volver al SW1 y desactivarlo con
```
no spanning-tree mst 1-2 root primary
```

### Problemas de mascara
Si los routers tienen mal configurada la ip-mask no hay adyacencia
`show running-config interface [int S/S/P]`

### Red Sumarizada
NOTA: La red tiene los 2 primeros octetos iguales

| Red              | 128 | Linea Corte | 64  | 32  | 16  | 8   | 4   | 2   | 1   |
| ---------------- | --- | ----------- | --- | --- | --- | --- | --- | --- | --- |
| 192.168.30.0/24  | 0   | /           | 0   | 0   | 1   | 1   | 1   | 1   | 0   |
| 192.168.40.0/24  | 0   | /           | 0   | 1   | 0   | 1   | 0   | 0   | 0   |
| 192.168.50.0/24  | 0   | /           | 0   | 1   | 1   | 0   | 0   | 1   | 0   |
| 192.168.60.0/24  | 0   | /           | 0   | 1   | 1   | 1   | 1   | 0   | 0   |
| 192.168.100.0/28 | 0   | /           | 1   | 1   | 0   | 0   | 1   | 0   | 0   |
IP: 192.168.0.0
MasK: 255.255.128.
Wildcard: 0.0.127.255

