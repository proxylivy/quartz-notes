# Info
## Descargar
[Onedrive Forma A Terminado](https://duoccl0-my.sharepoint.com/:u:/g/personal/ga_zunigam_duocuc_cl/EXdBFYHmBXJDhntPkKI3EhQBRDz27cIK1dTpEPMw7ANN_g?e=3hyJO9)
## Tabla VLSM

| Host | VLAN | Address       | Mask | Mask Decimal    | Espacio Asignable               |
| ---- | ---- | ------------- | ---- | --------------- | ------------------------------- |
| 850  | 3    | 192.168.144.0 | /22  | 255.255.252.0   | 192.168.144.1 - 192.168.147.254 |
| 500  | 6    | 192.168.148.0 | /23  | 255.255.254.0   | 192.168.148.1 - 192.168.149.254 |
| 200  | 9    | 192.168.150.0 | /24  | 255.255.255.0   | 192.168.150.1 - 192.168.150.254 |
| 50   | 12   | 192.168.151.0 | /26  | 255.255.255.192 | 192.168.151.1 - 192.168.151.62  |
# Configuracion
## Dispositivos Finales
### PC LAN 3
- IPv4: 192.168.147.9
- Subnet: 255.255.252.0
- Gateway: 192.168.147.254
- DNS: 192.168.151.32

### PC Vlan 6
- IPv4: 192.168.149.224
- Subnet:  255.255.254.0
- Gateway: 192.168.149.254
- DNS: 192.168.151.32

### PC Vlan 9
- IPv4: 
- DHCP
- Subnet: 
- Gateway: 
- DNS: 

### DNS
- IPv4: 192.168.151.32
- Subnet: 255.255.255.192
- Gateway: 192.168.151.62
- DNS: 192.168.151.32

### DHCP
- IPv4: 201.0.0.2
- Subnet: 255.255.255.252
- Gateway: 201.0.0.1
- DNS: 192.168.151.32

## ISP
```
int g0/2
no shut
exit
!
int g0/0
ip address 200.200.254.2 255.255.255.252
no shut
exit
!
int g0/1
ip address 200.200.255.2 255.255.255.252
no shut
exit
!
ip route 0.0.0.0 0.0.0.0 200.200.254.1
ip route 0.0.0.0 0.0.0.0 200.200.255.1
```

## OESTE
```
ip route 0.0.0.0 0.0.0.0 200.200.255.1
ip route 0.0.0.0 0.0.0.0 200.200.254.2
!
int g0/0
no shut
exit
!
int g0/1
no shut
exit
!
int g0/1.3
encapsulation dot1q 3
ip address 192.168.144.2 255.255.252.0
standby version 2
standby 3 ip 192.168.147.254
standby 3 priority 75
standby 3 preempt
no shut
exit

int g0/1.6
encapsulation dot1q 6
ip address 192.168.148.2 255.255.254.0
standby version 2
standby 6 ip 192.168.149.254
standby 6 priority 75
standby 6 preempt
no shut
exit


int g0/1.9
encapsulation dot1q 9
ip address 192.168.150.2 255.255.255.0
standby version 2
standby 9 ip 192.168.150.254
standby 9 priority 150
standby 9 preempt
no shut
exit


int g0/1.12
encapsulation dot1q 12
ip address 192.168.151.2 255.255.255.192
standby version 2
standby 12 ip 192.168.151.62
standby 12 priority 150
standby 12 preempt
no shut
exit
```

## ESTE
```
ip route 0.0.0.0 0.0.0.0 200.200.254.1
ip route 0.0.0.0 0.0.0.0 200.200.255.2
!
ip dhcp excluded-address 192.168.150.1 192.168.150.5
!
ip dhcp pool Profe_Genial
network 192.168.150.0 255.255.255.0
default-router 192.168.150.254
dns-server 192.168.151.32
exit
!
int g0/1
no shut
exit
!
int g0/2
no shut
exit
!
int g0/2.3
encapsulation dot1q 3
ip address 192.168.144.3 255.255.252.0
standby version 2
standby 3 ip 192.168.147.254
standby 3 priority 150
standby 3 preempt
no shut
exit
!
int g0/2.6
encapsulation dot1q 6
ip address 192.168.148.3 255.255.254.0
standby version 2
standby 6 ip 192.168.149.254
standby 6 priority 150
standby 6 preempt
no shut
exit
!
int g0/2.9
encapsulation dot1q 9
ip address 192.168.150.3 255.255.255.0
standby version 2
standby 9 ip 192.168.150.254
standby 9 priority 75
standby 9 preempt
no shut
exit
!
int g0/2.12
encapsulation dot1q 12
ip address 192.168.151.3 255.255.255.192
standby version 2
standby 12 ip 192.168.151.62
standby 12 priority 75
standby 12 preempt
no shut
exit
```
## DLS 1
```
vlan 3
name RRHH
exit
!
vlan 6
name VENTAS
exit
!
vlan 9
name FINANZAS
exit
!
vlan 12
name TI
exit
!
int range fa0/1-2,fa0/5-6,fa0/9-24
shut
exit
!
interface range fa0/7-8
channel-group 2 mode on
no shut
exit
!
interface Port-channel2
switchport mode trunk
switchport trunk allowed vlan 3,6,9,12
switchport nonegotiate
no shut
exit
!
interface range fa0/3-4
channel-group 3 mode desirable
no shut
exit
!
interface Port-channel3
switchport mode trunk
switchport trunk allowed vlan 3,6,9,12
switchport nonegotiate
no shut
exit
!
spanning-tree mode rapid-pvst
spanning-tree vlan 9,12 ROOT primary
spanning-tree vlan 3,6 ROOT secondary
!
int fa0/1
no shut
exit
```
## DLS 2
```
vlan 3
name RRHH
exit
!
vlan 6
name VENTAS
exit
!
vlan 9
name FINANZAS
exit
!
vlan 12
name TI
exit
!
int range fa0/1-4,fa0/9-24
shut
exit
!
interface range fa0/5-6
channel-group 4 mode active
no shut
exit
!
interface Port-channel4
switchport mode trunk
switchport trunk allowed vlan 3,6,9,12
switchport nonegotiate
no shut
exit
!
interface range fa0/7-8
channel-group 2 mode on
no shut
exit
!
interface Port-channel2
switchport mode trunk
switchport trunk allowed vlan 3,6,9,12
switchport nonegotiate
no shut
exit
!
spanning-tree mode rapid-pvst
!
spanning-tree vlan 3,6 ROOT primary
spanning-tree vlan 9,12 root secondary
!
int fa0/2
no shut
exit
```
## ALS 1
```
vlan 3
name RRHH
exit
!
vlan 6
name VENTAS
exit
!
vlan 9
name FINANZAS
exit
!
vlan 12
name TI
exit
!
int range fa0/20-24
switchport mode access
switchport access vlan 3
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky 
switchport port-security violation restrict
ip dhcp snooping limit rate 2
spanning-tree portfast
spanning-tree bpduguard enable
exit
!
int range fa0/10-14
switchport mode access
switchport access vlan 6
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky 
switchport port-security violation restrict
ip dhcp snooping limit rate 2
spanning-tree portfast
spanning-tree bpduguard enable
exit
!
int range fa0/5-9,fa0/11-23
shut
exit
!
interface range fa0/3-4
channel-group 3 mode auto
no shut
exit
!
interface Port-channel3
switchport mode trunk
switchport trunk allowed vlan 3,6,9,12
switchport nonegotiate
spanning-tree portfast
spanning-tree bpduguard enable
no shut
exit
!
interface range fa0/1-2
channel-group 1 mode on
no shut
exit
!
interface Port-channel1
switchport mode trunk
switchport trunk allowed vlan 3,6,9,12
switchport nonegotiate
spanning-tree portfast
spanning-tree bpduguard enable
no shut
exit
!
spanning-tree mode rapid-pvst
```
## ALS 2
```
vlan 3
name RRHH
exit
!
vlan 6
name VENTAS
exit
!
vlan 9
name FINANZAS
exit
!
vlan 12
name TI
exit
!
int range fa0/10-14
switchport mode access
switchport access vlan 9
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky 
switchport port-security violation protect
ip dhcp snooping limit rate 2
spanning-tree portfast
spanning-tree bpduguard enable
exit
!
int range fa0/20-24
switchport mode access
switchport access vlan 12
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky 
switchport port-security violation protect
ip dhcp snooping limit rate 2
spanning-tree portfast
spanning-tree bpduguard enable
exit
!
int range fa0/3-4,fa0/7-9,fa0/11-23
shut
exit
!
interface range fa0/5-6
channel-group 4 mode passive
no shut
exit
!
interface Port-channel4
switchport mode trunk
switchport trunk allowed vlan 3,6,9,12
switchport nonegotiate
spanning-tree portfast
spanning-tree bpduguard enable
no shut
exit
!
spanning-tree mode rapid-pvst
!
interface range fa0/1-2
channel-group 1 mode on
no shut
exit
!
interface Port-channel1
switchport mode trunk
switchport trunk allowed vlan 3,6,9,12
switchport nonegotiate
spanning-tree portfast
spanning-tree bpduguard enable
no shut
exit
!
```