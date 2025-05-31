# Info
## Errata:
- Las vlan de direccionamiento estan mal, van 100 y 200 como direccionamiento y Mascara 24 como subred
- Y hay una instruccion repetida
## Imagen Topologia
![]()
## Sobre
Implementacion VPN Clientless
## Requerimientos

# Configuracion
## Dispositivos Finales
### PC-VLAN100
IP: DHCP

### PC-VLAN200
IP: DHCP

## R1 Licencia
```
enable
config t
licence boot module c1900 technology-package securityk9
yes
do wr
exit
reload
```
## R1
```
enable
config t
ipv6 unicast-routing
int g0/0
no shut
exit
int g0/0.100
encapsulation dot1q 100
ip address 192.168.100.1 255.255.255.0
no shut
exit
int g0/0.200
encapsulation dot1q 200
ip address 192.168.200.1 255.255.255.0
no shut
exit
router ospf 1
router-id 1.1.1.1
network 192.168.12.0 255.255.255.0 area 0
network 192.168.100.0 255.255.255.0 area 0
network 192.168.200.0 255.255.255.0 area 0
passive-interface g0/0.100
passive-interface g0/0.200
exit
ipv6 router ospf 1
router-id 1.1.1.1
exit
int s0/0/0
ipv6 ospf 1 area 0
exit
ip dhcp excluded-address 192.168.100.1 192.168.100.4
ip dhcp excluded-address 192.168.200.1 192.168.200.4
ip dhcp pool VLAN100
network 192.168.100.0 255.255.255.0
default-router 192.168.100.1
dns-server 8.8.8.8
exiit
ip dhcp VLAN200
network 192.168.200.0 255.255.255.0
default-router 192.168.200.1
dns-server 8.8.8.8
exit
!# Recuerda Licenciar o nada de esto va a funcionar xd
crypto isakmp policy 1
encpryption aes
authentication pre-share
hash sha
group 5
lifetime 86400
exit
crypto isakmp key perita address 192.168.23.3
crypto ipsec transform-set T esp-aes esp-sha-hmac
ip access-list extended VPN
permit ip 192.168.100.0 0.0.0.255 any
permit ip 192.168.200.0 0.0.0.255 any
exit
crypto map MAPA 1 ipsec-isakmp
match address VPN
set transform-set T
set peer 192.168.23.3
exit
int s0/0/0
crypto map MAPA
```
## R2
```
enable
config t
!# Troubleshooting: Tiene mala la IPv6
int s0/0/0
no ip address
ipv6 address 2021:ACAD:ACAD:12::2/64
exit
!# Fin del troubleshooting
ipv6 unicast-routing
router ospf 1
router-id 2.2.2.2
network 192.168.12.0 255.255.255.0 area 0
network 192.168.20.0 255.255.255.0 area 0
network 192.168.23.0 255.255.255.0 area 0
exit
ipv6 router ospf 1
router-id 2.2.2.2
exit
int s0/0/1
ipv6 ospf 1 area 0
exit
int s0/0/0
ipv6 ospf 1 area 0
exit
```
## R3 Licencia
```
enable
config t
licence boot module c1900 technology-package securityk9
!# Yes
do wr
exit
reload
```
## R3
```
enable
config t
ipv6 unicast-routing
router ospf 1
router-id 3.3.3.3
network 192.168.23.0 255.255.255.0 area 0
default-information originate
exit
ip route 0.0.0.0 0.0.0.0 209.50.0.2
ipv6 route ::/0 2021:ACAD:ACAD:209::2
ipv6 router ospf 1
router-id 3.3.3.3
default-information originate
exit
int s0/0/1
ipv6 ospf 1 area 0
exit
!# Recuerda licencia el router o todo esto fallara
crypto isakmp policy 1
encpryption aes
authentication pre-share
hash sha
group 5
lifetime 86400
exit
crypto isakmp key perita address 192.168.12.1
crypto ipsec transform-set T esp-aes esp-sha-hmac
ip access-list extended VPN
permit ip any 192.168.100.0 0.0.0.255
permit ip any 192.168.200.0 0.0.0.255
exit
crypto map MAPA 1 ipsec-isakmp
match address VPN
set transform-set T
set peer 192.168.12.1
exit
int s0/0/1
crypto map MAPA
exit
```
## ISP
```
enable
config t
ipv6 unicast-routing
ip route 0.0.0.0 0.0.0.0 209.50.0.1
ipv6 route ::/0 2021:ACAD:ACAD:209::1
```
## SWA
```
enable
config t
vlan 100
exit
vlan 200
exit
int range g0/1-2
channel-group 1 mode desirable
exit
int port-channel 1
switchport mode trunk
switchport trunk allowed vlan 100,200
switchport nonegotiate
exit
int f0/1
switchport mode trunk
switchport trunk allowed vlan 100,200
switchport nonegotiate
exit
ip dhcp snooping vlan 100,200
no ip dhcp snooping information option
int f0/1
ip dhcp snooping trust
exit
```
## SWB
```
enable
config t
vlan 100
exit
vlan 200
exit
int f0/10
switchport mode access
switchport access vlan 100
switchport port-security
spanning-tree bpduguard enable
spanning-tree portfast
exit
int f0/20
switchport mode access
switchport access vlan 200
switchport port-security
spanning-tree bpduguard enable
spanning-tree portfast
exit
int range g0/1-2
channel group 1 mode auto
exit
int port-channel 1
switchport mode trunk
switchport trunk allowed vlan 100,200
switchport nonegotiate
exit
ip dhcp snooping
ip dhcp snooping vlan 100,200
no ip dhcp information option
int range g0/1-2
ip dhcp snooping
exit
int range f0/10,f0/20
ip dhcp snooping limit rate 2
exit
```
## ASA
```
enable
!# Enter
config t
no dhcpd address 192.168.1.5-192.168.1.35 inside
int vlan 1
ip address 192.168.22.1 255.255.255.0
nameif INSIDE
security-level 90
exit
int vlan 2
ip address 192.168.20.1 255.255.255.0
nameif OUTSIDE
security-level 10
end
show switch vlan
config t
hostname ASA
class-map inspection_default
match default-inspection-traffic
exit
policy-map global_policy
class inspection_default
inspect icmp
exit
service-policy global_policy global
!# Ruta por defecto por alguna razon??
route OUTSIDE 0.0.0.0 0.0.0.0 192.168.20.2
webvpn
enable OUTSIDE
username security password cisco
!# Hace el paso de ASA GUI
exit
group-policy POLITICA internal
group-policy POLITICA attributes
webvpn
url-list value pagina
exit
exit
tunnel-group TUNEL type remote-access
tunnel-group TUNEL general-attributes
default-group-policy POLITICA
exit
username security attributes
vpn-group-policy POLITICA

```
## ASA GUI
Ir a `Config > Bookmark Manager` con el titulo de `Pagina` y URL `http://192.168.22.20`

# Visualizacion
## Ver configuracion VPN Funcionando
### R1
`show crypto isakmp SA`
`show crypto ipsec SA`