# Info
## Topologia
![500](https://slink.proxylivy.work/image/68b0ef7a-6466-431f-8b6a-d71abeca1f6a.png)
## Sobre
Implementacion de Listas de Control de Acceso IPv4/IPv6
## Requerimientos
### A.- Configuración Capa 3
- Habilitar protocolo estado de enlace en IPv4/IPv6 en área de tipo backbone. Dejar interfaces pasivas según sea necesario. 
- Realizar enrutamiento intervlan según requerimiento propuesto en topología.
- VLAN20 debe recibir direccionamiento IPv4 de manera dinámica.
- Permitir que VLAN10 y VLAN20 salgan hacia ISP mediante una única dirección IP pública.

### B.- Configuración Capa 2
- Habilitar enlaces troncales, permitiendo solo el paso de VLAN de Datos.
- Desactivar protocolo DTP en interfaces correspondiente.
- Aplicar seguridad de puerto dinámica con máximo de 2 MAC, en caso de exceder, el puerto debe deshabilitarse.

### C.- Implementación de ACL IPv4/IPv6
- VLAN10 y VLAN20 no pueden comunicarse entre ellas mediante IPv4/IPv6.
- PC de VLAN10 debe ser el único equipo que administre los routers en nivel de IPv4. Debe generar mensajes LOG. 
- Implemente seguridad donde mitigue los potenciales ataques de spoofing en la red.

### C.- Monitorización y Conectividad
- Router R3 debe ser autenticado por servidor RADIUS. Realizar configuraciones pertinentes para este fin.
- Router R3 cuando genere mensajes LOG, deberán ser enviados a servidor Syslog.
- R2 debe crear una vista que permita verificar RIB, tabla topológica y LSDB.
- Compruebe que exista conectividad completa en IPv4/IPv6.
# Configuracion
## R1
```
enable
config t
int e0/0
duplex half
no shut
exit
int e0/0.10
encapsulation dot1q 10
ip address 172.16.10.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:10::1/64
exit
int e0/0.20
encapsulation dot1q 20
ip address 172.16.20.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:20::1/64
exit
router ospf 1
router-id 1.1.1.1
network 172.16.10.0 255.255.255.0 area 0
network 172.16.20.0 255.255.255.0 area 0
network 192.168.12.0 255.255.255.0 area 0
passive-interface e0/0.10
passive-interface e0/0.20
default-information originate
exit
!
ipv6 unicast-routing
ipv6 router ospf 1
router-id 1.1.1.1
passive-interface e0/0.10
passive-interface e0/0.20
default-information originate
exit
int e0/0.10
ipv6 ospf 1 area 0
exit
int e0/0.20
ipv6 ospf 1 area 0
exit
int s1/1
ipv6 ospf 1 area 0
exit
!
ip route 0.0.0.0 0.0.0.0 209.50.0.2
ipv6 route ::/0 3001:ACAD:ACAD:209::2
!
ip access-list standard INTERNET
permit 172.16.10.0 0.0.0.255
permit 172.16.20.0 0.0.0.255
exit
ip nat inside source list INTERNET interface s1/2 overload
int e0/0.10
ip nat inside
exit
int e0/0.20
ip nat inside
exit
int s1/2
ip nat outside
exit
ip dhcp excluded-address 172.16.20.1 172.16.20.4
ip dhcp pool VLAN20
network 172.16.20.0 255.255.255.0
default-router 172.16.20.1
exit
ip access-list extended NO-VLAN20
deny ip 172.16.10.0 0.0.0.255 172.16.20.0 0.0.0.255
permit ip any any
exit
int e0/0.10
ip access-group NO-VLAN20
exit
ipv6 access-list NO-VLAN20-6
deny ipv6 3001:ACAD:ACAD:10::/64 3001:ACAD:ACAD:20::/64
permit ipv6 any any
exit
int e0/0.10
ipv6 traffic-filter NO-VLAN20-6 in
exit
ip access-list standard VTY
permit host 172.16.10.5 log
deny any
exit
line vty 0 1
access-class VTY in
exit
ip access-list extended SEGURIDAD
deny ip host 0.0.0.0 any
deny ip 10.0.0.0 0.255.255.255 any
deny ip 127.0.0.0 0.255.255.255 any
deny ip 172.16.0.0 0.15.255.255 any
deny ip 192.168.0.0 0.0.255.255 any
deny ip 224.0.0.0 15.255.255.255 any
deny ip host 255.255.255.255 any
permit ip any any
exit
int s1/2
ip access-group SEGURIDAD in
exit
```

## R2
```
enable
config t
router ospf 1
router-id 2.2.2.2
network 192.168.12.0 255.255.255.0 area 0
network 192.168.23.0 255.255.255.0 area 0
exit
!
ipv6 unicast-routing
ipv6 router ospf 1
router-id 2.2.2.2
exit
int s1/0
ipv6 ospf 1 area 0
exit
int s1/1
ipv6 ospf 1 area 0
exit
enable secret perita
aaa new-model
parser view VISTA1
secret cisco1
commands exec include show ip route
commands exec include show ip eigrp topology all-links
commands exec include show ip ospf database
end
!
enable view VISTA1
# enter password (cisco1)
show parser view
show ?
!
enable view
# enter password (perita)
!
```

## R3
NOTA: Aqui se configura WinRadius, se usa la ip 192.168.56.1 y TFTP64, en la seccion de Syslog y con la interfaz del virtualbox
```
enable
config t
router ospf 1
router-id 3.3.3.3
network 192.168.23.0 255.255.255.0 area 0
network 192.168.56.0 255.255.255.0 area 0
passive-interface e0/0
ipv6 unicast-routing
ipv6 router ospf 1
router-id 3.3.3.3
exit
int s1/0
ipv6 ospf 1 area 09
exit
!
username user02 password cisco
aaa new-model
radius-server host 192.168.56.1 auth-port 1812 acct-port 1813 key WinRadius
enable secret perita
aaa authentication login default group radius local-case
exit
logging on
logging host 192.168.56.1
logging trap 6
logging source-interface e0/0
int loopback 1
```

## ISP
```
enable
config t
ipv6 unicast-routing
ipv6 route ::/0 3001:ACAD:ACAD:209::1

```

## SWA
```
enable
config t
int 0/0
duplex half
exit
vlan 10,20
exit
vtp mode transparent
int range e0/0-1,e1/0
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20
switchport nonegotiate
exit

```

## SWB
```
enable
config t
int e0/3
duplex half
interface range e0/1,e1/2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20
switchport nonegotiate
exit
int e0/3
switchport mode access
switchport access vlan 10
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation shutdown
exit
```

## SWC
```
enable
config t
int e0/3
duplex half
vlan 10,20
exit
vtp mode transparent
int range e1/0,e1/2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20
switchport nonegotiate
exit
int e0/3
switchport mode access
switchport access vlan 10
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation shutdown
exit

```

## PCVLAN20
```
enable
config t
int e0/3
no ipv6 address
ipv6 address 3001:ACAD:ACAD:20::C/64
```