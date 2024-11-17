# Info
## PacketTracer
[Onedrive](https://duoccl0-my.sharepoint.com/:u:/g/personal/ga_zunigam_duocuc_cl/ETc01RzyIQVIrkB5RhKMzQ0BrFB9VvfQJCYSzJjbjNWEeA?e=IWKVIS)
## Apoyo
[Calculadora VLSM](http://www.vlsmcalc.com/)
[[002 - Ordenar/3er Semestre/ARY3112 (RS)/Actividades/2.- Confiabilidad y disponibilidad en redes de pequeña y mediana embergadura/2.1- Agregacion de enlace EtherChannel|2.1- Agregacion de enlace EtherChannel]]
[[002 - Ordenar/3er Semestre/ARY3112 (RS)/Actividades/2.- Confiabilidad y disponibilidad en redes de pequeña y mediana embergadura/2.2- Protocolo de configuración dinámica de host DHCP|2.2- Protocolo de configuración dinámica de host DHCP]]
[[002 - Ordenar/3er Semestre/ARY3112 (RS)/Actividades/2.- Confiabilidad y disponibilidad en redes de pequeña y mediana embergadura/2.3- Protocolo de redundancia de primer salto FHRP|2.3- Protocolo de redundancia de primer salto FHRP]]
[[002 - Ordenar/3er Semestre/ARY3112 (RS)/Actividades/2.- Confiabilidad y disponibilidad en redes de pequeña y mediana embergadura/2.5- Seguridad de Capa 2|2.5- Seguridad de Capa 2]]
[[002 - Ordenar/3er Semestre/ARY3112 (RS)/Actividades/2.- Confiabilidad y disponibilidad en redes de pequeña y mediana embergadura/2.6- Conceptos de Seguridad de LAN|2.6- Conceptos de Seguridad de LAN]]
## Notas
NOTA: No lleva IPv6, ni configuracion basica
NOTA: Si la Ip es mayor a 256, se divide la ip que se necesita por 256, la parte entera se va al tercer octeto y la decimal se multiplica por 256 y se queda en el cuarto octeto
NOTA: En todos los port-channel de los switch, debe ir el switchport mode trunk, las vlan allowed y el nonegotiate

## VLSM
172.16.0.0/19

| Nombre  | Hosts | Address    | Mas | Masc decimal  |
| ------- | ----- | ---------- | --- | ------------- |
| Vlan 30 | 700   | 172.16.0.0 | /22 | 255.255.252.0 |
| Vlan 20 | 500   | 172.16.4.0 | /23 | 255.255.254.0 |
| Vlan 40 | 300   | 172.16.6.0 | /23 | 255.255.254.0 |
| Vlan 10 | 150   | 172.16.8.0 | /24 | 255.255.255.0 |

# Configuracion
## Dispositivos Finales
### Server NTP
- IPv4: 172.16.8.50
- SubNet: 255.255.255.0
- Default Gateway: 172.16.8.254
- DNS: 209.50.0.2

### PC Vlan20
- Ipv4: 172.16.5.194
- Subnet: 255.255.254.0
- Default Gateway: 172.16.6.254
- DNS: 209.50.0.2

### PC VLan30
- Ipv4: 172.16.2.158
- Subnet: 255.255.252.0
- Default Gateway: 172.16.3.254
- DNS: 209.50.0.2

### PC Vlan40 (DHCP)
- Deverian ser sus valores

### DNS Extremo
- Activar DNS
- IP: 209.50.0.2
- Subnet: 255.255.255.248
- Default Gateway: 209.50.0.1
- DNS: 209.50.0.2

## ISP
```
ntp server 172.16.8.50
!
ip route 0.0.0.0 0.0.0.0 200.200.1.2 
ip route 0.0.0.0 0.0.0.0 200.200.2.2 
```
## MLS1
```
hostname MLS1
!
vlan 10
name SERVIDORES
exit
!
vlan 20
name RRHH
exit
!
vlan 30
name VENTAS
exit
!
vlan 40
name CORPORATIVO
exit
!
interface range fa0/21-22
channel-group 1 mode active
no shut
exit
!
interface Port-channel1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport nonegotiate
no shut
exit
!
interface fa0/24
no switchport
ip address 200.200.1.2 255.255.255.0
no shut
EXIT
!
ip routing
!
INT VLAN 10
ip address 172.16.8.2 255.255.255.0
standby version 2
standby 10 ip 172.16.8.254
standby 10 priority 120
standby 10 preempt
EXIT
!
INT VLAN 20
ip address 172.16.4.2 255.255.254.0
standby version 2
standby 20 ip 172.16.5.254
standby 20 priority 120
standby 20 preempt
EXIT
!
INT VLAN 30
ip address 172.16.0.2 255.255.252.0
standby version 2
standby 30 ip 172.16.3.254
standby 30 priority 90
standby 30 preempt
EXIT
!
INT VLAN 40
ip address 172.16.6.2 255.255.254.0
standby version 2
standby 40 ip 172.16.7.254
standby 40 priority 90
standby 40 preempt
EXIT
!
ip route 0.0.0.0 0.0.0.0 200.200.1.1
!
ntp server 172.16.8.50
!
spanning-tree mode rapid-pvst
spanning-tree vlan 10,20 ROOT PRIMARY
spanning-tree vlan 30,40 ROOT SECONDARY
!
```
## MLS2
```
hostname MLS2
!
vlan 10
name SERVIDORES
exit
!
vlan 20
name RRHH
exit
!
vlan 30
name VENTAS
exit
!
vlan 40
name CORPORATIVO
exit
!
ip dhcp excluded-address 172.16.6.1 172.16.6.9
!
ip dhcp pool VLAN40
network 172.16.6.0 255.255.254.0
default-router 172.16.7.254
dns-server 209.50.0.2
exit
!
interface range fa0/21-22
channel-group 4 mode desirable
no shut
!
interface Port-channel4
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport nonegotiate
no shut
exit
!
interface FastEthernet0/24
no switchport
ip address 200.200.2.3 255.255.255.0
exit
!
ntp server 172.16.8.50
!
interface Vlan10
ip address 172.16.8.3 255.255.255.0
standby version 2
standby 10 ip 172.16.8.254
standby 10 priority 90
standby 10 preempt
exit
!
interface Vlan20
ip address 172.16.4.3 255.255.254.0
standby version 2
standby 20 ip 172.16.5.254
standby 20 priority 90
standby 20 preempt
exit
!
interface Vlan30
ip address 172.16.0.3 255.255.252.0
standby version 2
standby 30 ip 172.16.3.254
standby 30 priority 120
standby 30 preempt
exit
!
interface Vlan40
ip address 172.16.6.3 255.255.254.0
standby version 2
standby 40 ip 172.16.7.254
standby 40 priority 120
standby 40 preempt
exit
!
ip route 0.0.0.0 0.0.0.0 200.200.2.1 
!
!
ip routing
!
spanning-tree mode rapid-pvst
spanning-tree vlan 10,20 ROOT SECONDARY
spanning-tree vlan 30,40 ROOT PRIMARY
!
```
## SWA
```
hostname SWA
!
vlan 10
name SERVIDORES
exit
!
vlan 20
name RRHH
exit
!
vlan 30
name VENTAS
exit
!
vlan 40
name CORPORATIVO
exit
!
int range fa0/5-9
switchport mode access
switchport access vlan 10
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky 
switchport port-security violation restrict 
ip dhcp snooping limit rate 2
spanning-tree portfast
spanning-tree bpduguard enable
exit
!
int range fa0/15-17
switchport mode access
switchport access vlan 20
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky 
switchport port-security violation restrict 
ip dhcp snooping limit rate 2
spanning-tree portfast
spanning-tree bpduguard enable
exit
!
interface range fa0/1-2
channel-group 2 mode desirable
no shut
exit
!
!
interface range fa0/21-22
channel-group 1 mode on
no shut
exit
!
interface range fa0/23-24
channel-group 4 mode active
no shut
exit
!
interface Port-channel1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport nonegotiate
no shut
exit
!
interface Port-channel2
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport nonegotiate
no shut
exit
!
interface Port-channel4
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport nonegotiate
no shut
exit
!
spanning-tree mode rapid-pvst
```
## SWB
```
hostname SWB
!
vlan 10
name SERVIDORES
exit
!
vlan 20
name RRHH
exit
!
vlan 30
name VENTAS
exit
!
vlan 40
name CORPORATIVO
exit
!
ip dhcp snooping
!
int range fa0/10-11
switchport mode access
switchport access vlan 30
switchport port-security
switchport port-security maximum 3
switchport port-security mac-address sticky 
switchport port-security violation protect
ip dhcp snooping limit rate 2
spanning-tree portfast
spanning-tree bpduguard enable
exit
!
int range fa0/8-9
switchport mode access
switchport access vlan 40
switchport port-security
switchport port-security maximum 3
switchport port-security mac-address sticky 
switchport port-security violation protect 
ip dhcp snooping limit rate 2
spanning-tree portfast
spanning-tree bpduguard enable
exit
!
interface range fa0/5-6
channel-group 5 mode desirable
no shut
exit
!
!
interface range fa0/21-22
channel-group 1 mode on
no shut
exit
!
interface range fa0/23-24
channel-group 3 mode active
no shut
exit
!
interface Port-channel1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport nonegotiate
no shut
exit
!
interface Port-channel5
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport nonegotiate
no shut
exit
!
interface Port-channel3
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport nonegotiate
no shut
exit
!
spanning-tree mode rapid-pvst
```
## SWC
```
hostname SWC
!
vlan 10
name SERVIDORES
exit
!
vlan 20
name RRHH
exit
!
vlan 30
name VENTAS
exit
!
vlan 40
name CORPORATIVO
exit
!
interface range fa0/3-4
channel-group 6 mode on	
no shut
exit
!
interface range fa0/5-6
channel-group 5 mode auto	
no shut
exit
!
interface range fa0/21-22
channel-group 1 mode passive
no shut
exit
!
interface range fa0/23-24
channel-group 4 mode passive
no shut
exit
!
interface Port-channel1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport nonegotiate
no shut
exit
!
interface Port-channel5
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport nonegotiate
no shut
exit
!
interface Port-channel4
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport nonegotiate
no shut
exit
!
interface Port-channel6
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport nonegotiate
no shut
exit
!
spanning-tree mode rapid-pvst
!
```
## SWD
```
hostname SWD
!
vlan 10
name SERVIDORES
exit
!
vlan 20
name RRHH
exit
!
vlan 30
name VENTAS
exit
!
vlan 40
name CORPORATIVO
exit
!
interface range fa0/3-4
channel-group 6 mode on
no shut
exit
!
interface range fa0/1-2
channel-group 2 mode auto
no shut	
exit
!
interface range fa0/21-22
channel-group 4 mode AUTO
no shut
exit
!
interface range fa0/23-24
channel-group 3 mode passive
no shut
exit
!
interface Port-channel2
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport nonegotiate
no shut
exit
!
interface Port-channel3
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport nonegotiate
no shut
exit
!
interface Port-channel4
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport nonegotiate
no shut
exit
!
interface Port-channel6
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport nonegotiate
no shut
exit
!
spanning-tree mode rapid-pvst
!
```
