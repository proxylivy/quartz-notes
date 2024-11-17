# Info
NOTA
1.- Para ip mas grandes, se divide en 256, la parte entera al tercer octeto y lo que viene despues de la coma, se multiplica por 256
## Tabla VLSM

| VLAN  | Host | Adress       | Mask | Mask Decimal    | Rango Asignable             |
| ----- | ---- | ------------ | ---- | --------------- | --------------------------- |
| 5     | 500  | 172.16.32.0  | /23  | 255.255.254.0   | 172.16.32.1 - 172.16.33.254 |
| 10    | 200  | 172.16.34.0  | /24  | 255.255.255.0   | 172.16.34.1 - 172.16.34.254 |
| 15    | 160  | 172.16.35.0  | /24  | 255.255.255.0   | 172.16.35.1 - 172.16.35.254 |
| 20    | 60   | 172.16.36.0  | /26  | 255.255.255.192 | 172.16.36.1 - 172.16.36.62  |
| Wan-1 | 2    | 172.16.36.64 | /30  | 255.255.255.252 | 172.16.36.65 - 172.16.36.66 |
| Wan-2 | 2    | 172.16.36.68 | /30  | 255.255.255.252 | 172.16.36.69 - 172.16.36.70 |
# Configuracion
## Dispositivos Finales
### Vlan5 - NTP Server (350) (1.94)
- IPv4: 172.16.33.94
- SubNet: 255.255.254.0
- DG: 172.16.33.254
- DNS: 205.0.0.2

### Vlan10 (190)
- IPv4: 172.16.34.190
- SubNet: 255.255.255.0
- DG: 172.16.34.254
- DNS: 205.0.0.2

### Vlan15 (100)
- IPv4: 172.16.35.100
- SubNet: 255.255.255.0
- DG: 172.16.35.254
- DNS: 205.0.0.2

### WLC
- IPv4: 172.16.36.20
- Subnet: 255.255.255.192
- DG: 172.16.36.62
- DNS: 205.0.0.2

### DHCP-WIRELESS (30)
- IPv4: 172.16.36.30
- SubNet: 255.255.255.192
- DG: 172.16.36.62
- DNS: 205.0.0.2

## SW1
```
hostname SW1
!
vlan 5
name SERVIDORES
exit
!
vlan 10
name VENTAS
exit
!
vlan 15
name RR.HH
exit
!
vlan 20
name INALAMBRICA
exit
!
interface range fa0/21-22
channel-group 2 mode desirable
no shut
exit
!
interface range fa0/23-24
channel-group 1 mode on
no shut
exit
!
interface Port-channel 1
switchport mode trunk
switchport trunk allowed vlan 5,10,15,20
switchport nonegotiate
no shut
exit
!
interface Port-channel 2
switchport mode trunk
switchport trunk allowed vlan 5,10,15,20
switchport nonegotiate
no shut
exit
!
ntp server 172.16.33.94
!
spanning-tree mode rapid-pvst
spanning-tree vlan 5,10 ROOT primary
spanning-tree vlan 15,20 root secondary
!
int fa0/5
switchport mode access
switchport access vlan 5
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky 
switchport port-security violation protect
ip dhcp snooping limit rate 2
spanning-tree portfast
spanning-tree bpduguard enable
no shut
exit
!
int fa0/10
switchport mode access
switchport access vlan 10
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky 
switchport port-security violation protect
ip dhcp snooping limit rate 2
spanning-tree portfast
spanning-tree bpduguard enable
no shut
exit
```
## SW2
```
hostname SW2
!
vlan 5
name SERVIDORES
exit
!
vlan 10
name VENTAS
exit
!
vlan 15
name RR.HH
exit
!
vlan 20
name INALAMBRICA
exit
!
int fa0/1
switchport mode access
switchport access vlan 15
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky 
switchport port-security violation protect
ip dhcp snooping limit rate 2
spanning-tree portfast
spanning-tree bpduguard enable
no shut
exit
!
int range fa0/3,fa0/5,fa0/15
switchport mode access
switchport access vlan 20
no shut
exit
!
interface range fa0/23-24
channel-group 1 mode on
no shut
exit
!
interface Port-channel 1
switchport mode trunk
switchport trunk allowed vlan 5,10,15,20
switchport nonegotiate
no shut
exit
!
interface range fa0/2,fa0/14
channel-group 3 mode passive 
no shut
exit
!
interface Port-channel 3
switchport mode trunk
switchport trunk allowed vlan 5,10,15,20
switchport nonegotiate
no shut
exit
!
spanning-tree vlan 5,10 ROOT secondary
spanning-tree vlan 15,20 root primary
!
ntp server 172.16.33.94
```
## SW3
```
hostname SW3
!
vlan 5
name SERVIDORES
exit
!
vlan 10
name VENTAS
exit
!
vlan 15
name RR.HH
exit
!
vlan 20
name INALAMBRICA
exit
!
interface range fa0/21-22
channel-group 2 mode auto
no shut
exit
!
interface Port-channel 2
switchport mode trunk
switchport trunk allowed vlan 5,10,15,20
switchport nonegotiate
no shut
exit
!
interface range fa0/23,fa0/2
channel-group 3 mode active
no shut
exit
!
interface Port-channel 3
switchport mode trunk
switchport trunk allowed vlan 5,10,15,20
switchport nonegotiate
no shut
exit
!
ntp server 172.16.33.94
```
## Router
```
HOSTNAME RB
!
ipv6 unicast-routing
!
INT G0/1
NO SHUT
EXIT
!
INT G0/1.5
Encapsulation dot1q 5
ip address 172.16.32.2 255.255.254.0
standby version 2
standby 5 ip 172.16.33.254
standby 5 priority 120
standby 5 preempt
EXIT
!
INT G0/1.10
Encapsulation dot1q 10
ip address 172.16.34.2 255.255.255.0
standby version 2
standby 10 ip 172.16.34.254
standby 10 priority 120
standby 10 preempt
EXIT
!
INT G0/1.15
Encapsulation dot1q 15
ip address 172.16.35.2 255.255.255.0
standby version 2
standby 15 ip 172.16.35.254
standby 15 priority 90
standby 15 preempt
EXIT
!
INT G0/1.20
Encapsulation dot1q 20
ip address 172.16.36.2 255.255.255.192
standby version 2
standby 20 ip 172.16.36.62
standby 20 priority 90
standby 20 preempt
EXIT
!
router rip
version 2
passive-interface g0/0
passive-interface g0/1
passive-interface g0/2
passive-interface g0/1.5
passive-interface g0/1.10
passive-interface g0/1.15
passive-interface g0/1.20
no auto-summary
network 172.16.32.0
network 172.16.34.0
network 172.16.35.0
network 172.16.36.0
network 172.16.36.68
default-information originate
exit
!
int g0/2
ipv6 rip PRUEBA3 enable
no shut
exit
!
IPV6 ROUTER RIP PRUEBA3
redistribute static
exit
!
!
ip route 0.0.0.0 0.0.0.0 172.16.36.70
!
IPV6 ROUTE ::/0 2021:ABCD:ABCD:200::2
!
ntp server 172.16.33.94
```
## RA
```
HOSTNAME RA
!
ipv6 unicast-routing
!
INT G0/0
NO SHUT
EXIT
!
INT G0/0.5
Encapsulation dot1q 5
ip address 172.16.32.1 255.255.254.0
standby version 2
standby 5 ip 172.16.33.254
standby 5 priority 90
standby 5 preempt
EXIT
!
INT G0/0.10
Encapsulation dot1q 10
ip address 172.16.34.1 255.255.255.0
standby version 2
standby 10 ip 172.16.34.254
standby 10 priority 90
standby 10 preempt
EXIT
!
INT G0/0.15
Encapsulation dot1q 15
ip address 172.16.35.1 255.255.255.0
standby version 2
standby 15 ip 172.16.35.254
standby 15 priority 120
standby 15 preempt
EXIT
!
INT G0/0.20
Encapsulation dot1q 20
ip address 172.16.36.1 255.255.255.192
standby version 2
standby 20 ip 172.16.36.62
standby 20 priority 120
standby 20 preempt
EXIT
!
router rip
version 2
passive-interface g0/2
passive-interface g0/1
passive-interface g0/0
passive-interface g0/0.5
passive-interface g0/0.10
passive-interface g0/0.15
passive-interface g0/0.20
no auto-summary
network 172.16.32.0
network 172.16.34.0
network 172.16.35.0
network 172.16.36.0
network 172.16.36.64
default-information originate
exit
!
int g0/1
ipv6 rip PRUEBA3 enable
no shut
exit
!
IPV6 ROUTER RIP PRUEBA3
redistribute static
exit
!
!
ip route 0.0.0.0 0.0.0.0 172.16.36.66
!
IPV6 ROUTE ::/0 2021:ABCD:ABCD:100::1
!
ntp server 172.16.33.94
```
## ISP
```
ip route 0.0.0.0 0.0.0.0 210.0.0.2
ip route 0.0.0.0 0.0.0.0 210.0.0.2 100
!
IPV6 ROUTE ::/0 2021:ABCD:ABCD:210::2
IPV6 ROUTE ::/0 2021:ABCD:ABCD:210::2 200
!
ipv6 unicast-routing
```
## BORDE
```
ip route 0.0.0.0 0.0.0.0 210.0.0.1
ip route 0.0.0.0 0.0.0.0 210.0.0.1 100
!
IPV6 ROUTE ::/0 2021:ABCD:ABCD:210::1
IPV6 ROUTE ::/0 2021:ABCD:ABCD:210::1 200
!
ipv6 unicast-routing
```