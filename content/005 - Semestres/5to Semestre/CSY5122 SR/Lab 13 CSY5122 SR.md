# Info
## Imagen Topologia
![800](https://slink.proxylivy.work/image/50bdc227-48dd-44ff-af3a-3d9f3143e4f2.png)
## Sobre
Implementacion Firewall ASA
## Requerimientos
- Asignar direccionamiento IPv4 en las interfaces correspondiente de routers y firewall.
- Implementar seguridad de puerto dinámica, con máximo de dos direcciones MAC, y en caso de exceder la interfaz debe apagar.
- Interfaces que conectan a equipos finales implementar mecanismos de estabilización de STP.
- Implementar solución de DHCP Snooping, donde equipos finales no pidan más de 3 IP.
- Crear las zonas en Firewall, con prioridades a elección.
- Firewall debe asignar DHCP a VLAN1.
- Modificar MPF para permitir la conectividad hacia zona outside.
- Crear un PAT en firewall para que red LAN de VLAN1 salga por red 192.168.1.0/29.
- Crear un NAT estático para que DMZ salga por IP sobrante en red 192.168.1.0/29.
- Implementar protocolo estado de enlace, autenticando a nivel de interfaz.
- Realizar configuraciones para permitir llegar a Internet.
- Debe existir conectividad completa en la topologia.

# Configuracion
## Configuracion Dispositivos Finales
### PC0
- IP: DHCP

### SERVER
- IP: 172.16.20.20
- dec-mask: 255.255.255.0
- DG: 172.16.20.1
- DNS: 8.8.8.8

## RA
```
enable
config t
hostname RA
int g0/0
ip address 192.168.1.2 255.255.255.248
no shut
exit
int s0/0/0
ip address 192.168.2.2 255.255.255.0
no shut
exit
router ospf 1
router-id 1.1.1.1
network 192.168.1.0 255.255.255.248 area 0
network 192.168.2.0 255.255.255.252 area 0
exit
int s0/0/0
ip ospf authentication message-digest
ip ospf message-digest-key 1 md5 modopera
exit
```
## BORDE
```
enable
config t
hostname BORDE
int s0/0/0
ip address 192.168.2.1 255.255.255.252
no shut
exit
int s0/0/1
ip address 200.0.0.1 255.255.255.252
no shut
exit
router ospf 1
router-id 2.2.2.2
network 192.168.2.0 255.255.255.252 area 0
default-information originate
exit
ip route 0.0.0.0 0.0.0.0 200.0.0.2
int s0/0/0
ip ospf authentication message-digest
ip ospf message-digest-key 1 md5 modopera
exit
ip access-list standard INTERNET
permit 192.168.1.0 0.0.0.7
exit
ip nat inside source list INTERNET int s0/0/1 overload
int s0/0/0
ip nat inside
exit
int s0/0/1
ip nat outside
exit
```
## ISP
```
enable
config t
hostname ISP
int s0/0/1
ip address 200.0.0.2 255.255.255.252
no shut
exit
int loopback 0
ip address 8.8.8.8 255.255.255.255
no shut
exit
```
## SWA
Nota: En esta topologia estos switch no llevan vlan
```
enable
config t
hostname SWA
int f0/10
switchport mode access
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation shutdown
spanning-tree bpduguard enable
spanning-tree portfast
ip dhcp snooping limit rate 3
exit
```
## SWB
Nota: No lleva ip
```
enable
config t
hostname SWB
int f0/1
switchport mode access
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation shutdown
spanning-tree bpduguard enable
spanning-tree portfast
ip dhcp snooping limit rate 3
exit
ip dhcp snooping
ip dhcp snooping vlan 1
no ip dhcp snooping information option
int f0/18
ip dhcp snooping trust
exit
```
## ASA
```
enable
!# Password: !# Enter with no password
config t
hostname ASA
enable password perita
domain-name www.duoc.cl
clock set 17:10:00 01 JUN 2024
no dhcpd address 192.168.1.5-192.168.1.36 inside
int vlan 1
nameif INSIDE
security-level 90
ip address 172.16.10.1 255.255.255.0
exit
int vlan 2
nameif OUTSIDE
security-level 8
ip address 192.168.1.1 255.255.255.248
exit
int vlan 3
no forward interface vlan 1
nameif DMZ
security-level 60
ip address 172.16.20.1 255.255.255.0
exit
!# revisar las interfaces con las vlan
do show switch vlan
!# int OUTSIDE
int e0/1
switchport access vlan 2
exit
int e0/4
switchport access vlan 3
exit
route OUTSIDE 0.0.0.0 0.0.0.0 192.168.1.2
dhcpd address 172.16.10.2-172.16.10.30 INSIDE
dhcpd dns 8.8.8.8 interface INSIDE
dhcpd enable INSIDE
exit
!# Implementar NAT
object network INTERNET
subnet 172.16.10.0 255.255.255.0
nat (INSIDE,OUTSIDE) dynamic interface
exit
config t
class-map inspection_default
match default-inspection-traffic
exit
policy-map global_policy
class inspection_default
inspect icmp
exit
service-policy global_policy global
!
object network DMZ
host 172.16.20.20
nat (DMZ,OUTSIDE) static 192.168.1.3
exit
config t
access-list DMZ permit ip any host 172.16.20.20
access-group DMZ in interface OUTSIDE
end
config t
username ASA password telnet
aaa authentication telnet console LOCAL
telnet 172.16.20.20 255.255.255.255 DMZ
telnet 172.16.10.0 255.255.255.0 INSIDE
telnet timeout 5
```


# Visualizacion
## PC0
Desde Desktop > Command Prompt
```
ping 192.168.1.2
```
## Ver NAT desde ASA
```
show xlate
```
## Probar telnet desde Server y PC0
```
telnet 172.16.10.1
!# Username: ASA
!# Password: telnet
enable
!# Password: perita
```