Comandos De Apoyo
[[anexos/PKT CME.pkt]] | [Online](https://duoccl0-my.sharepoint.com/:u:/g/personal/ga_zunigam_duocuc_cl/ESv6M88CRk1PnNEWXzWfdWIB5lRHkxZjVOpHQHKVjuE1Qw?e=tPkb6b)

[Video Apoyo Onedrive](https://duoccl0-my.sharepoint.com/:v:/g/personal/ga_zunigam_duocuc_cl/ESTdNOvngotOug3QUdteSxEBnJx-7-uNH8EGNYY59OxLwQ?nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJPbmVEcml2ZUZvckJ1c2luZXNzIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXciLCJyZWZlcnJhbFZpZXciOiJNeUZpbGVzTGlua0RpcmVjdCJ9fQ&e=KDwscH)

Se usan Routers 2911 y Switch multicapa 3560-24PS

HQ = HeadQuarter (Casa Matriz)
BR = Branch (Sucursal)

---
> Activando licencia
> Router HQ y Todos los BR
> NOTA: Recuerda agregar la tarjeta HWIC-2T y esperar el tiempo de reload
```
!
enable
conf terminal
license boot module c2900 technology-package uck9
yes
!
do wr
do show license all
do reload
```

---
> HQ-PO
```
!
enable
config terminal
hostname HQ-BR-PO
!
!# Creamos DHCP
!# Se excluyen las ip desde la .1 a la .10
!
ip dhcp excluded-address 10.10.10.1 10.10.10.10
ip dhcp excluded-address 10.10.11.1 10.10.11.10
!
ip dhcp pool HQDATA
network 10.10.10.0 255.255.255.0
default-router 10.10.10.1
exit
!
ip dhcp pool HQVOZ
network 10.10.11.0 255.255.255.0
default-router 10.10.11.1
option 150 ip 10.10.11.1
exit
!
!# Declaramos la IP en las interfaces
interface GigabitEthernet0/1
no shutdown
exit
! 
interface GigabitEthernet0/1.10
encapsulation dot1Q 10
ip address 10.10.10.1 255.255.255.0
exit
!
interface GigabitEthernet0/1.11
encapsulation dot1Q 11
ip address 10.10.11.1 255.255.255.0
exit
!
interface serial 0/0/0
ip address 192.168.50.1 255.255.255.252
clock rate 2000000
no shutdown
exit
!
interface serial 0/0/1
ip address 192.168.53.2 255.255.255.252
no shutdown
exit
!
!# Routing OSPF
router ospf 192
log-adjacency-changes
network 10.10.10.0 0.0.0.255 area 0
network 10.10.11.0 0.0.0.255 area 0
network 192.168.50.0 0.0.0.3 area 0
network 192.168.51.0 0.0.0.3 area 0
network 192.168.52.0 0.0.0.3 area 0
network 192.168.53.0 0.0.0.3 area 0
exit
!
!# Creacion de Telefonos HQ
telephony-service
max-ephones 2
max-dn 2
ip source-address 10.10.11.1 port 2000
auto assign 1 to 2
!
ephone-dn 1
number 1001
!
ephone-dn 2
number 1002
!
exit
telephony-service
create cnf-files
exit
do wr
!
!## DiAL PEERS
!## HQ -> Todos los BR (1000, 2000, 3000, 4000)
!
dial-peer voice 1 voip
destination-pattern 1...
session target ipv4:10.10.11.1
!
dial-peer voice 2 voip
destination-pattern 2...
session target ipv4:20.20.22.1
!
dial-peer voice 3 voip
destination-pattern 3...
session target ipv4:30.30.33.1
!
dial-peer voice 4 voip
destination-pattern 4...
session target ipv4:40.40.44.1
exit
do wr
!
```

---
> Solo BR-PN
```
!
enable
config terminal
hostname BR-PN
!
ip dhcp excluded-address 20.20.20.1 20.20.20.10
ip dhcp excluded-address 20.20.22.1 20.20.22.10
!
ip dhcp pool BRDATA
network 20.20.20.0 255.255.255.0
default-router 20.20.20.1
exit
ip dhcp pool BRVOZ
network 20.20.22.0 255.255.255.0
default-router 20.20.22.1
option 150 ip 20.20.22.1
exit
!
!# Declaramos la IP en las interfaces
interface GigabitEthernet0/1
no shutdown
exit
! 
interface GigabitEthernet0/1.20
encapsulation dot1Q 20
ip address 20.20.20.1 255.255.255.0
exit
!
interface GigabitEthernet0/1.22
encapsulation dot1Q 22
ip address 20.20.22.1 255.255.255.0
exit
!
interface serial 0/0/1
ip address 192.168.50.2 255.255.255.252
no shutdown
exit
!
interface serial 0/0/0
ip address 192.168.51.1 255.255.255.252
clock rate 2000000
no shutdown
exit
!
!# Routing OSPF
router ospf 192
log-adjacency-changes
network 20.20.20.0 0.0.0.255 area 0
network 20.20.22.0 0.0.0.255 area 0
network 192.168.50.0 0.0.0.3 area 0
network 192.168.51.0 0.0.0.3 area 0
network 192.168.52.0 0.0.0.3 area 0
network 192.168.53.0 0.0.0.3 area 0
exit
!# Creacion De Telefonos
telephony-service
max-ephones 3
max-dn 3
ip source-address 20.20.22.1 port 2000
auto assign 1 to 3
!
ephone-dn 1
number 2001
!
ephone-dn 2
number 2002
!
ephone-dn 3
number 2003
exit
telephony-service
create cnf-files
!
!#DiAL PEERS
!# BR-PN -> PO(1000)/PN(2000)/PV(3000)

dial-peer voice 1 voip
destination-pattern 1...
session target ipv4:10.10.11.1
 
dial-peer voice 2 voip
destination-pattern 2...
session target ipv4:20.20.22.1

dial-peer voice 3 voip
destination-pattern 3...
session target ipv4:30.30.33.1
exit
!
telephony-service
create cnf-files
exit
!
do wr
!
```

---
>BR-PV
```
!
enable
config terminal
hostname BR-PV
!
ip dhcp excluded-address 30.30.30.1 30.30.30.10
ip dhcp excluded-address 30.30.33.1 30.30.33.10
!
ip dhcp pool BRDATA
network 30.30.30.0 255.255.255.0
default-router 30.30.30.1
exit
ip dhcp pool BRVOZ
network 30.30.33.0 255.255.255.0
default-router 30.30.33.1
option 150 ip 30.30.33.1
exit
!
!# Declaramos la IP en las interfaces
interface GigabitEthernet0/1
no shutdown
exit
! 
interface GigabitEthernet0/1.30
encapsulation dot1Q 30
ip address 30.30.30.1 255.255.255.0
exit
!
interface GigabitEthernet0/1.33
encapsulation dot1Q 33
ip address 30.30.33.1 255.255.255.0
exit
!
interface serial 0/0/1
ip address 192.168.51.2 255.255.255.252
no shutdown
exit
!
interface serial 0/0/0
ip address 192.168.52.1 255.255.255.252
clock rate 2000000
no shutdown
exit
!
!# Routing OSPF
router ospf 192
log-adjacency-changes
network 30.30.30.0 0.0.0.255 area 0
network 30.30.33.0 0.0.0.255 area 0
network 192.168.50.0 0.0.0.3 area 0
network 192.168.51.0 0.0.0.3 area 0
network 192.168.52.0 0.0.0.3 area 0
network 192.168.53.0 0.0.0.3 area 0
exit
!# Creacion De Telefonos
telephony-service
max-ephones 2
max-dn 2
ip source-address 30.30.33.1 port 2000
auto assign 1 to 2
!
ephone-dn 1
number 3001
!
ephone-dn 2
number 3002
!
exit
telephony-service
create cnf-files
!
!#DiAL PEERS
!# BR-PV -> PO(1000)/PV(3000)/PS(4000)

dial-peer voice 1 voip
destination-pattern 1...
session target ipv4:10.10.11.1

dial-peer voice 3 voip
destination-pattern 3...
session target ipv4:30.30.33.1

dial-peer voice 4 voip
destination-pattern 4...
session target ipv4:40.40.44.1
exit
telephony-service
create cnf-files
exit
!
do wr
!
```

---
>BR-PS
```
!
enable
config terminal
hostname BR-PS
!
ip dhcp excluded-address 40.40.40.1 40.40.40.10
ip dhcp excluded-address 40.40.44.1 40.40.44.10
!
ip dhcp pool BRDATA
network 40.40.40.0 255.255.255.0
default-router 40.40.40.1
exit
ip dhcp pool BRVOZ
network 40.40.44.0 255.255.255.0
default-router 40.40.44.1
option 150 ip 40.40.44.1
exit
!
!# Declaramos la IP en las interfaces
interface GigabitEthernet0/1
no shutdown
exit
! 
interface GigabitEthernet0/1.40
encapsulation dot1Q 40
ip address 40.40.40.1 255.255.255.0
exit
!
interface GigabitEthernet0/1.44
encapsulation dot1Q 44
ip address 40.40.44.1 255.255.255.0
exit
!
interface serial 0/0/1
ip address 192.168.52.2 255.255.255.252
no shutdown
exit
!
interface serial 0/0/0
ip address 192.168.53.1 255.255.255.252
clock rate 2000000
no shutdown
exit
!
!# Routing OSPF
router ospf 192
log-adjacency-changes
network 40.40.40.0 0.0.0.255 area 0
network 40.40.44.0 0.0.0.255 area 0
network 192.168.50.0 0.0.0.3 area 0
network 192.168.51.0 0.0.0.3 area 0
network 192.168.52.0 0.0.0.3 area 0
network 192.168.53.0 0.0.0.3 area 0
exit
!# Creacion De Telefonos
telephony-service
max-ephones 3
max-dn 3
ip source-address 40.40.44.1 port 2000
auto assign 1 to 3
!
ephone-dn 1
number 4001
!
ephone-dn 2
number 4002
!
ephone-dn 3
number 4003
exit
telephony-service
create cnf-files
!
!#DiAL PEERS
!# BR-PS -> PO(1000)/PN(2000)/PS(4000)

dial-peer voice 1 voip
destination-pattern 1...
session target ipv4:10.10.11.1
 
dial-peer voice 2 voip
destination-pattern 2...
session target ipv4:20.20.22.1

dial-peer voice 4 voip
destination-pattern 4...
session target ipv4:40.40.44.1
exit
telephony-service
create cnf-files
exit
do wr
!
```

---
>Declarando TRUNK
>Switch Multicapa SW-HQ-PO
```
!
enable
config terminal
hostname SW-HQ-PO
interface GigabitEthernet0/1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,11
no shutdown
exit
!#Declarando Accesos
interface FastEthernet0/1
switchport access vlan 10
switchport mode access
switchport nonegotiate
switchport voice vlan 11
exit
!
interface FastEthernet0/2
switchport access vlan 10
switchport mode access
switchport nonegotiate
switchport voice vlan 11
exit
!
do wr
!
```

---
>Declarando TRUNK
>Switch Multicapa SW-BR-PN
```
enable
config terminal
hostname SW-BR-PN
interface GigabitEthernet0/1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 20,22
no shutdown
exit
!# Declarando Accesos
interface FastEthernet0/1
switchport access vlan 20
switchport mode access
switchport nonegotiate
switchport voice vlan 22
exit
!
interface FastEthernet0/2
switchport access vlan 20
switchport mode access
switchport nonegotiate
switchport voice vlan 22
exit Switch Multicapa SW-BR-PV
!
do wr
!
```

---
> Declarando Trunk
> Switch Multicapa SW-BR-PV
```
!
enable
config terminal
hostname SW-BR-PV
interface GigabitEthernet0/1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 30,33
no shutdown
exit
!# Declarando Accesos
interface FastEthernet0/1
switchport access vlan 30
switchport mode access
switchport nonegotiate
switchport voice vlan 33
exit
!
interface FastEthernet0/2
switchport access vlan 30
switchport mode access
switchport nonegotiate
switchport voice vlan 33
exit
!
do wr
!
```

---
> Declarando Trunk
> Switch Multicapa SW-BR-PS
```
!
enable
config terminal
hostname SW-BR
interface GigabitEthernet0/1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 40,44
no shutdown
exit
!# Declarando Accesos
interface FastEthernet0/1
switchport access vlan 40
switchport mode access
switchport nonegotiate
switchport voice vlan 44
exit
!
interface FastEthernet0/2
switchport access vlan 40
switchport mode access
switchport nonegotiate
switchport voice vlan 44
exit
!
do wr
!
```

---
Fotos:
Ejemplo Topologia
![[anexos/topo ejemplo ip.png]]

Condiciones CME
![[anexos/Condicion CME.png]]
