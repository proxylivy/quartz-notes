# Info
- ERRATA: El Switch que se usa es un "3560", el cual no permite usar bien IPv6 ni VLANs dentro de OSPF como Pasivas
- Si no funciona VPN, guarda el ejercicio y reincialo
## Imagen Topologia
![1000](https://slink.proxylivy.work/image/b08e0609-efce-40f0-9767-032ca2f94fa4.png)
## Sobre
Implementacion de Easy VPN
## Requerimientos
- Gran parte de la topología se encuentra direccionada con IPv4/IPv6.
- Habilitar protocolo estado de enlace. Configurar interfaces pasivas según corresponda.
- Autenticar protocolo estado de enlaces.
- Los SW deben tener seguridad de puerto por aprendizaje con un máximo de 2 MAC y violación de puerto restrict.
- Implementar mecanismos de estabilización de STP.
- Debe aplicar control de tormenta que no supere el 20%.
- Interfaces de equipos finales no deben solicitar más de 3 IP por minuto. parámetros a elección.
- Implemente Easy VPN donde red 192.168.20.0/24 tenga acceso seguro a 192.168.10.0/24. Utilice prámetros a elección.
- Comprobar funcionamiento de VPN de Acceso Remoto.
- Comprobar conectividad completa en la topología.
# Configuracion
## Dispositivos Finales
### PC-VLAN30
- IPv4: 192.168.30.20
- Mask: 255.255.255.0
- DG: 192.168.30.4
- DNS: 8.8.8.8
- DGv6: 2021:ACAD:ACAD:30::4

### PC-VLAN40
- IPv4: 192.168.40.10
- Mask: 255.255.255.0
- DG: 192.168.40.4
- DNS: 8.8.8.8
- DGv6: 2021:ACAD:ACAD:40::4

### PCV-VLAN10 VPN Configuration
- GroupName: REMOTO
- GroupKey: modoperita
- Host IP (Server IP): 192.168.10.1
- Username: RA
- Password: cisco

## RA
```
enable
config t
ipv6 unicast-routing
router ospf 1
router-id 1.1.1.1
network 192.168.10.0 255.255.255.0 area 0
network 192.168.12.0 255.255.255.0 area 0
passive-interface f0/0
ipv6 router ospf 1
router-id 1.1.1.1
passive-interface f0/0
exit
int f0/0
ipv6 ospf 1 area 0
exit
int s0/0/0
ipv6 ospf 1 area 0
exit
int s0/0/0
ip ospf authentication message-digest
ip ospf message-digest-key 1 md5 pera
exit
ip dhcp exluded-address 192.168.10.1 192.168.10.4
ip dhcp pool RED-1
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 8.8.8.8
exit
!# EASYVPN
aaa new-model
!# GroupName
aaa authentication login REMOTO local
aaa authorization network REMOTO local
!# Username & Password
username RA password cisco
crypto isakmp policy 1
authentication pre-share
encryption aes 256
hash sha
group 5
lifetime 86400
crypto isakmp client configuration group REMOTO
key modoperita
pool POOL
exit
crypto ipsec transform-set T esp-aes 256 esp-sha-hmac
crypto dynamic-map MAPA-D 1
set transform-set T
reverse-route
exit
crypto map MAPA client authentication list REMOTO
crypto map MAPA isakmp authorization list REMOTO
crypto map MAPA client configuration address respond
crypto map MAPA 1 ipsec-isakmp dynamic MAPA-D
ip local pool POOL 192.168.20.100 192.168.20.110
int f0/0
crypto map MAPA
exit
router ospf 1
redistribute static subnets
redistribute connected subnets
default-information originate
exit
```
## RB
```
enable
config t
ipv6 unicast-routing
router ospf 1
router-id 2.2.2.2
network 192.168.12.0 255.255.255.0 area 0
network 192.168.23.0 255.255.255.0 area 0
network 192.168.20.0 255.255.255.0 area 0
passive-interface f0/0
ipv6 router ospf 1
router-id 2.2.2.2
exit
int s0/0/0
ipv6 ospf 1 area 0
exit
int s0/0/1
ipv6 ospf 1 area 0
exit
int f0/0
ipv6 ospf 1 area 0
exit
int s0/0/0
ip ospf authentication message-digest
ip ospf message-digest-key 1 md5 pera
exit
int s0/0/1
ip ospf authentication message-digest
ip ospf message-digest-key 1 md5 pera
exit
ip dhcp excluded-address 192.168.20.1 192.168.20.4
ip dhcp pool RED-2
network 192.168.20.0 255.255.255.0
default-router 192.168.20.2
dns-server 8.8.8.8
exit
```
## RC
```
enable
config t
ipv6 unicast-routing
router ospf 1
router-id 3.3.3.3
network 192.168.100.0 255.255.255.0 area 0
network 192.168.23.0 255.255.255.0 area 0
exit
ipv6 router ospf 1
router-id 3.3.3.3
int s0/0/1
ipv6 ospf 1 area 0
exit
int s0/0/1
ip ospf authentication message-digest
ip ospf message-digest-key 1 md5 pera
exit
f0/0
ip ospf authentication message-digest
ip ospf message-digest-key 1 md5 pera
exit
```
## L3-1
```
enable
config t
hostname L3-1
ip routing
ipv6 unicast-routing
vlan 30
exit
vlan 40
exit
int f0/1
no switchport
ip address 192.168.100.4 255.255.255.0
ipv6 address 2021:ACAD:ACAD:100::4/64
no shut
exit
int vlan 30
ip address 192.168.30.4 255.255.255.0
ipv6 address 2021:ACAD:ACAD:30::4/64
no shut
exit
int vlan 40
ip address 192.168.40.4 255.255.255.0
ipv6 address 2021:ACAD:ACAD:40::4/64
exit
router ospf 1
router-id 4.4.4.4
network 192.168.100.0 255.255.255.0 area 0
network 192.168.30.0 255.255.255.0 area 0
network 192.168.40.0 255.255.255.0 area 0
passive-interface vlan 30
passive-interface vlan 40
exit
ipv6 router ospf 1
router-id 4.4.4.4
exit
int vlan 30
ipv6 ospf 1 area 0
exit
int vlan 40
ipv6 ospf 1 area 0
exit
int f0/1
ipv6 ospf 1 area 0
exit
int f0/2
switchport mode access
switchport access vlan 30
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation restrict
spanning-tree bpduguard enable
spanning-tree portfast
ip dhcp snooping limit-rate 3
storm-control broadcast level 20.0
exit
int f0/3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 30,40
switchport nonegotiate
exit
int f0/1
ip ospf authentication message-digest
ip ospf message-digest-key 1 md5 pera
exit
```
## SWD
```
enable
config t
vlan 30
exit
vlan 40
exit
int f0/2
switchport mode access
switchport access vlan 40
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation restrict
spanning-tree bpduguard enable
spanning-tree portfast
ip dhcp snooping limit-rate 3
storm-control broadcast level 20.0
int f0/1
switchport mode trunk
switchport trunk allowed vlan 30,40
switchport nonegotiate
exit
```
## SWC
```
enable
config t
hostname SWC
int f0/1
switchport mode access
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation restrict
spanning-tree bpduguard enable
spanning-tree portfast
ip dhcp snooping limit-rate 3
storm-control broadcast level 20.0
exit
```
## SWA
```
enable
config t
hostname SWA
vlan 10
exit
int f0/2
switchport mode access
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation restrict
spanning-tree bpduguard enable
spanning-tree portfast
ip dhcp snooping limit-rate 3
storm-control broadcast level 20.0
exit
```
# Visualizacion
## Ping PC-VLAN10 -> PC-VLAN20
```
ping 192.168.20.5
```
## Paquetes VPN
```
enable
show crypto ipsec sa
```