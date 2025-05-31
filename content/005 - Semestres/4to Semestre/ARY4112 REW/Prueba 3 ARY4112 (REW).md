# Info
## Descargar
- [Evaluación N°3 - ARY4112.pka](https://duoccl0-my.sharepoint.com/:u:/g/personal/ga_zunigam_duocuc_cl/ER01XXrd4EVJpzw8VCvZZQsBeT5MAtdcIRQSwDj00WP4XA?e=Exeu8A)
- [(Hecho) Evaluación N°3 - ARY4112 Gabo.pka](https://duoccl0-my.sharepoint.com/:u:/g/personal/ga_zunigam_duocuc_cl/EfUsSQDcSiZMkh2bVkXwzH0BMC40YN1kJ77uMzK7DKmmAw?e=R1Tc62)
## Imagen Topologia
![](https://slink.proxylivy.work/image/808c9c59-5d77-4675-ad87-cd71e6d39ea8.png)
## Requerimientos
### Contexto
La empresa REDES S.A fue diseñada e implementada por un equipo de especialistas de Networking. Pero por motivos desconocidos, el trabajo fue interrumpido y dejado de manera inconclusa. Por desgracia la red no está operativa, por lo cual, sabiendo de sus habilidades ha sido requerido para la detección y corrección de problemas que tiene la red, así como, la funcionalidad de la misma para lograr salida hacia Internet a nivel de IPv4/IPv6.

### A. DETECCIÓN Y CORRECCIÓN DE PROBLEMAS DE CONECTIVIDAD:
- Se ha solicitado revisar el funcionamiento de las interfaces port-channels de la red. Por antecedentes se tiene que están implementado mediante configuración estática persistente. Revisar y corregir de ser necesario.
- Se ha solicitado revisar el funcionamiento del DHCP. Estos deben proporcionar IPv4 de manera dinámica a la VLAN10, VLAN20 y VLAN40. Estos pools deben estar ubicados en R2. La IP del DNS será la dirección 8.8.8.8, además de poder excluir las primeras 4 IP. Revisar y corregir de ser necesario.
- Se ha solicitado revisar el funcionamiento de OSPF a nivel de IPv4/IPv6, principalmente las interfaces pasivas. Revisar y corregir de ser necesario.

### B. IMPLEMENTACIÓN DE SOLUCIONES DE CAPA 2:
- Crear las VLANs propuestas en las porciones de red respectiva.
- Asignar interfaces de acceso a VLAN correspondiente.
- Interfaces de acceso deberán tener implementado seguridad de puerto dinámica, además de mecanismos de estabilización de STP respectivos.
- En los enlaces troncales deberá permitir el paso de VLAN respectivas, y desactivar protocolo DTP

### C. IMPLEMENTACIÓN DE SOLUCIONES DE CAPA 3:
- Deberá implementar OSPF Multiarea. Los routers utilizarán el router-id con formato “X.X.X.X” donde la “X” reemplaza número del router. Implementar interfaces pasivas respectivas.
- Autenticar protocolo OSPF a nivel de IPv4, mediante hashing MD5. Utilizar clave **prueba3**
- Permitir salida hacia Internet a nivel IPv4/IPv6. Debe garantizar el retorno solo para IPv6.
- Sumarizar a nivel IPv4/IPv6 el protocolo OSPF. Las redes sumarizadas deben verse reflejadas en los routers vecinos a nivel de tabla de enrutamiento.
- Implementar NAT con sobrecarga, donde deberá permitir las redes sumarizadas salgan a Internet. Utilice una ACL con el nombre de **INTERNET**, la cual debe salir traducido a través de la interfaz GigabitEthernet 0/0. Además, debe definir las zonas respectivas para la traducción IP.
- Servidor de VLAN30 funcionará como servidor Syslog y NTP, por lo cual todos los routers deberán enviar sus logs y sincronizarse en tiempo y hora con este dispositivo.
- Comprobar conectividad a nivel de IPv4/IPv6.


# Configuracion
## Servidor PT
- IPv4: 192.168.4.0
- Subnet: 255.255.255.0
- Default Gateway: 192.168.4.1
- DNS: 8.8.8.8
- IPv6: 3001:ABCD:ABCD:1:C::C/80
- DGv6: 3001:ABCD:ABCD:1:C::1
- DNSv6: 3001:ABCD:ABCD:8::8

## PC VLAN 40
- IPv4: 192.168.5.40
- Subnet: 255.255.255.0
- DGv4: 192.168.5.1
- DNSv4: 8.8.8.8

## PC VLAN 20
- IPv4: DHCP
- Subnet: 255.255.255.0
- DGv4: 172.16.3.1
- DNSv4: 8.8.8.8
- IPv6: 3001:ABCD:ABCD:1:B::B/80
- DGv6: 3001:ABCD:ABCD:1:B::1
- DNSv6: 3001:ABCD:ABCD:8::8

## PC VLAN 10
- IPv4: DHCP
- Subnet: 255.255.255.0
- DGv4: 172.16.2.1
- DNSv4: 8.8.8.8
- IPv6: 3001:ABCD:ABCD:1:A::A/80
- DGv6: 3001:ABCD:ABCD:1:A::1
- DNSv6: 3001:ABCD:ABCD:8::8

## SWA
```
enable
config t
hostname SWA
Vlan 10
name RRHH
Vlan 20
name VENTAS
Vlan 30
name TI
Vlan 40
name GERENCIA
int fa0/10
switchport mode access
switchport access vlan 10
spanning-tree portfast
spanning-tree bpduguard enable
switchport port-security
no shut
exit
int fa0/20
switchport mode access 
switchport access vlan 20
spanning-tree portfast
spanning-tree bpduguard enable
switchport port-security
no shut
exit
!
int port-channel 1
switchport mode trunk
switchport trunk allowed vlan 10,20
switchport nonegotiate
no shut
exit
```

## SWB
```
!
enable
config t
Vlan 10
name RRHH
Vlan 20
name VENTAS
Vlan 30
name TI
Vlan 40
name GERENCIA
exit
int fa0/3
switchport mode trunk
switchport trunk allowed vlan 10,20
switchport nonegotiate
no shut
exit
int port-channel 1
switchport mode trunk
switchport trunk allowed vlan 10,20
switchport nonegotiate
no shut
exit
```

## R2
```
!
ip access-list standard INTERNET
permit 172.16.2.0 0.0.1.255
permit 192.168.4.0 0.0.1.255
!
no ip dhcp excluded-address 172.16.2.1 172.16.2.254
ip dhcp excluded-address 172.16.2.1 172.16.2.4
!
no ip dhcp excluded-address 172.16.3.1 172.16.3.254
ip dhcp excluded-address 172.16.3.1 172.16.3.4
!
ip dhcp excluded-address 192.168.4.1 192.168.4.4
!
ip dhcp excluded-address 192.168.5.1 192.168.5.4
!
ip dhcp pool VLAN20
dns-server 8.8.8.8
!
ip dhcp pool VLAN40
no network 172.16.5.0 255.255.255.0
no default-router 172.16.5.1
network 192.168.5.0 255.255.255.0
default-router 192.168.5.1
dns-server 8.8.8.8
!
router ospf 1
router-id 2.2.2.2
network 10.0.0.0 0.0.0.255 area 0
no passive-interface s0/0/0
exit
!
no ipv6 router ospf 1
ipv6 router ospf 2
router-id 2.2.2.2
log-adjacency-changes
default-information originate
!
ip route 0.0.0.0 0.0.0.0 10.0.0.1
ip route 0.0.0.0 0.0.0.0 200.0.0.1
!
int s0/0/0
ipv6 ospf 2 area 0
no shut
exit
int s0/0/1
ipv6 ospf 2 area 0
no shut
exit
!
ip nat inside source list INTERNET interface x/x overload
int x/x
ip nat outside
int x/x
ip nat inside

loggin host
loggin console
loggin trap debugging
service timestamp 
```

## R1
```
!
enable
config t
!
ipv6 unicast-routing
!
router ospf 1
network 10.0.0.0 0.0.0.255 area 0
area 1 range 172.16.2.0 255.255.254.0
area 1 authentication message-digest
network 172.16.2.0 0.0.1.255
no passive-interface s0/0/0
passive-interface g0/0.10
passive-interface g0/0.20
exit
!
ipv6 router ospf 2
router-id 1.1.1.1
passive-interface g0/0.10
passive-interface g0/0.20
exit
!
ip route 0.0.0.0 0.0.0.0 10.0.0.2
!
int g0/0
no shut
exit
!
int g0/0.10
ip helper-address 10.0.0.2
exit
int g0/0.20
ip helper-address 10.0.0.2
exit
!
int s0/0/0
ipv6 ospf 2 area 0
no shut
exit
!
int s0/0/0
ip ospf authentication message-digest
ip ospf authentication-key prueba3
!
logging trap debugging
loggin 192.168.4.30
ntp server 192.168.4.30
!
```

## ISP
```
ip route 0.0.0.0 0.0.0.0 200.0.0.2
```