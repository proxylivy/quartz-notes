# Info
## Actividad:
- [Onedrive](https://duoccl0-my.sharepoint.com/:u:/g/personal/ga_zunigam_duocuc_cl/ESb1DDJ1QPVHuRrH4RqNg4oBXI1smLMsis_qYX3q25NXzA?e=gtPIJs) o [Onedrive](https://duoccl0-my.sharepoint.com/:u:/g/personal/ga_zunigam_duocuc_cl/EQpl_V2W7t1Eu1VLYfbyNx8B9byu48wxPW4IKtVjqi79qA?e=hlac5o)
## Apoyo:
- [Calculadora VLSM](http://vlsmcalc.com/)
- [[002 - Ordenar/3er Semestre/ARY3112 (RS)/Actividades/1.- Microsegmentacion en Redes LAN/1.1- Configuracion Basica de dispositivos]]
- [[002 - Ordenar/3er Semestre/ARY3112 (RS)/Actividades/1.- Microsegmentacion en Redes LAN/1.2- Microsegmentacion en Redes LAN]]
- [[002 - Ordenar/3er Semestre/ARY3112 (RS)/Actividades/1.- Microsegmentacion en Redes LAN/1.3- Enrutamiento InterVLAN]]
- [[002 - Ordenar/3er Semestre/ARY3112 (RS)/Actividades/1.- Microsegmentacion en Redes LAN/1.4- Evitando Loop de Capa 2]]

## Terminado:
![[extras/Adjuntos/CASO 1_ARY3112 resuelto.2.pka]]

Primero que nada, conchetumare .txt culiao, me cagaste la vida
y el video qliao igual

## Tabla VLSM

Red 192.168.0.0/19

Host, Vlan, Address, Mask, Dec Mask
(400)Vlan 20,  192,168.0.0,  /23, 255.255.254.0
(200)Vlan 10,  192.168.2.0,  /24, 255.255.255.0
(150)Vlan 30,  192.168.3.0,  /24, 255.255.255.0
(50) Vlan 40,  192.168.4.0,  /26, 255.255.255.192
(12) Vlan 100, 192.168.4.64, /28, 255.255.255.240
(5)  Vlan 90,  192.168.4.80, /29, 255.255.255.248

# Configuracion
## Dispositivos Finales
PC_Vlan 20
- IPv4: 192.168.0.12
- Mascara: 255.255.254.0
- Default Gateway: 192.168.0.1
- DNS: 176.16.1.8

PC_Vlan 30
- IPv4: 192.168.3.15
- Mascara: 255.255.255.0
- Default Gateway: 192.168.3.1
- DNS: 176.16.1.8

PC_Vlan 40
- IPv4: 192.168.4.18
- Mascara: 255.255.255.0
- Default Gateway: 192.168.4.1
- DNS: 176.16.1.8

DNS
- IPv4: 176.16.1.8
- Mascara:255.255.255.0
- Default Gateway: 176.16.1.1
- DNS: 176.16.1.8

DHCP
- IPv4: 176.16.1.100
- Mascara: 255.255.255.0
- Default Gateway: 176.16.1.1
- DNS: 176.16.1.8

## R1
```
hostname R1
!
line console 0
pass cisco
exit
!
line vty 0 4
pass oeste
exit
!
banner motd #Solo ingreso de personal autorizado#
!
int g0/1
ip add 15.15.15.5 255.255.255.252
no shut
exit
!
int g0/0
no shut
exit
!
int g0/0.10
encapsulation dot1q 10
ip address 192.168.2.1 255.255.255.0
no shut
exit
!
int g0/0.20
encapsulation dot1q 20
ip address 192.168.0.1 255.255.254.0
no shut
exit
!
int g0/0.30
encapsulation dot1q 30
ip address 192.168.3.1 255.255.255.0
no shut
exit
!
int g0/0.40
encapsulation dot1q 40
ip address 192.168.4.1 255.255.255.192
no shut
exit
!
int g0/0.90
encapsulation dot1q 90
ip address 192.168.4.81 255.255.255.248
no shut
exit
!
int g0/0.100
encapsulation dot1q 100
ip address 192.168.4.65 255.255.255.240
no shut
exit
!
ip route 0.0.0.0 0.0.0.0 15.15.15.6
```

## R2
```
hostname R2
!
line console 0
pass cisco
exit
!
line vty 0 4
pass oeste
exit
!
banner motd #Solo ingreso de personal autorizado#
!
int g0/0
ip add 176.16.1.1 255.255.255.0
no shut
exit
!
int g0/1
ip add 15.15.15.6 255.255.255.252
no shut
exit
!
ip route 0.0.0.0 0.0.0.0 15.15.15.5
```
## SW6
```
hostname SW6
!
line console 0
pass cisco
exit
!
line vty 0 4
pass oeste
exit
!
banner motd #Solo ingreso de personal autorizado#
!
vlan 10
name Invitados
exit
!
vlan 20
name Ventas
exit
!
vlan 30
name Soporte
exit
!
vlan 40
name RRHH
exit
!
vlan 90
name Operaciones
exit
!
vlan 100
name Nativa
exit
!
spanning-tree mode rapid-pvst
default-gateway 192.168.4.65
!
int F0/15
switchport mode access
switchport access vlan 40
spanning-tree portfast
spanning-tree bpduguard enable
no shut
exit
!
int range F0/1-F0/14
switchport mode access
switchport access vlan 40
shut
exit
!
int range F0/16-21
switchport mode access
switchport access vlan 40
shut
exit
!
int F0/24
switchport mode access
switchport access vlan 40
shut
exit
!
int range F0/22-F0/23
switchport mode trunk
switchport trunk allowed vlan 20,30,40,90
switchport trunk native vlan 100
switchport nonegotiate
no shut
exit
!
int vlan 100
ip add 192.168.4.70 255.255.255.240
exit
!
```
## Switch 5
```
hostname SW5
!
line console 0
pass cisco
exit
!
line vty 0 4
pass oeste
exit
!
banner motd #Solo ingreso de personal autorizado#
!
vlan 10
name Invitados
exit
!
vlan 20
name Ventas
exit
!
vlan 30
name Soporte
exit
!
vlan 40
name RRHH
exit
!
vlan 90
name Operaciones
exit
!
vlan 100
name Nativa
exit
!
spanning-tree mode rapid-pvst
ip default-gateway 192.168.4.65
!
int F0/10
switchport mode access
switchport access vlan 30
spanning-tree portfast
spanning-tree bpduguard enable
no shut
exit
!
int range F0/23-F0/24
switchport mode trunk
switchport trunk allowed vlan 20,30,40,90
switchport trunk native vlan 100
switchport nonegotiate
no shut
exit
!
int range F0/1-F0/9
switchport mode access
switchport access vlan 40
shut
exit
!
int range F0/11-F0/22
switchport mode access
switchport access vlan 40
shut
exit
!
int vlan 100
ip add 192.168.4.69 255.255.255.240
exit
!
```
## Switch 4
```
hostname SW4
!
line console 0
pass cisco
exit
!
line vty 0 4
pass oeste
exit
!
banner motd #Solo ingreso de personal autorizado#
!
vlan 10
name Invitados
exit
!
vlan 20
name Ventas
exit
!
vlan 30
name Soporte
exit
!
vlan 40
name RRHH
exit
!
vlan 90
name Operaciones
exit
!
vlan 100
name Nativa
exit
!
spanning-tree mode rapid-pvst
ip default-gateway 192.168.4.65
!
int F0/5
switchport mode access
switchport access vlan 20
spanning-tree portfast
spanning-tree bpduguard enable
no shut
exit
!
int F0/22
switchport mode trunk
switchport trunk allowed vlan 20,30,40,90
switchport trunk native vlan 100
switchport nonegotiate
no shut
exit
!
int F0/24
switchport mode trunk
switchport trunk allowed vlan 20,30,40,90
switchport trunk native vlan 100
switchport nonegotiate
no shut
exit
!
int range F0/1-F0/4
switchport mode access
switchport access vlan 40
shut
exit
!
int range F0/6-21
switchport mode access
switchport access vlan 40
shut
exit
!
int F0/23
switchport mode access
switchport access vlan 40
shut
exit
!
int vlan 100
ip add 192.168.4.68 255.255.255.240
exit
!
```
## Switch 3
```
hostname SW3
!
line console 0
pass cisco
exit
!
line vty 0 4
pass oeste
exit
!
banner motd #Solo ingreso de personal autorizado#
!
vlan 10
name Invitados
exit
!
vlan 20
name Ventas
exit
!
vlan 30
name Soporte
exit
!
vlan 40
name RRHH
exit
!
vlan 90
name Operaciones
exit
!
vlan 100
name Nativa
exit
!
spanning-tree mode rapid-pvst
ip default-gateway 192.168.4.65
spanning-tree vlan 40,90,100 root primary
!
int range F0/21-24
switchport mode trunk
switchport trunk allowed vlan 20,30,40,90
switchport trunk native vlan 100
switchport nonegotiate
no shut
exit
!
int vlan 100
ip add 192.168.4.67 255.255.255.240
exit
!
```
## Switch 2
```
hostname SW2
!
line console 0
pass cisco
exit
!
line vty 0 4
pass oeste
exit
!
banner motd #Solo ingreso de personal autorizado#
!
vlan 10
name Invitados
exit
!
vlan 20
name Ventas
exit
!
vlan 30
name Soporte
exit
!
vlan 40
name RRHH
exit
!
vlan 90
name Operaciones
exit
!
vlan 100
name Nativa
exit
!
spanning-tree mode rapid-pvst
ip default-gateway 192.168.4.65
spanning-tree vlan 10,20,30 root primary
!
int range F0/22-24
switchport mode trunk
switchport trunk allowed vlan 20,30,40,90
switchport trunk native vlan 100
switchport nonegotiate
no shut
exit
!
int F0/20
switchport mode trunk
switchport trunk allowed vlan 20,30,40,90
switchport trunk native vlan 100
switchport nonegotiate
no shut
exit
!
int vlan 100
ip add 192.168.4.66 255.255.255.240
exit
```
## Switch 1 (Multi-Capa)
```
hostname S1
!
line console 0
pass cisco
exit
!
line vty 0 4
pass oeste
exit
!
banner motd #Solo ingreso de personal autorizado#
!
vlan 10
name Invitados
exit
!
vlan 20
name Ventas
exit
!
vlan 30
name Soporte
exit
!
vlan 40
name RRHH
exit
!
vlan 90
name Operaciones
exit
!
vlan 100
name Nativa
exit
!
spanning-tree mode rapid-pvst
ip default-gateway 192.168.4.65
!
int range F0/20-21
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 20,30,40,90
switchport trunk native vlan 100
switchport nonegotiate
no shut
exit
!
int vlan 100
ip add 192.168.4.74 255.255.255.240
exit
!
int g0/1
no shut
exit
!
```
## SA
```
hostname SA
int g0/1
no shut
exit
!
```