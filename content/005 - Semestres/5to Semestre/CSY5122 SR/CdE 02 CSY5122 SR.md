# Info
## Sobre/Objetivos
Conectividad Segura Capa 2 y Capa 3, integridad, proteccion de datos
## Requerimientos
### Implementación de Protocolos de Enrutamiento IPv4/IPv6
- Asignar direccionamiento IPv4/IPv6 según corresponda en la topología de red.
- Para IPv4, implemente solución de protocolo de estado de enlace multiarea. Es importante autenticar el establecimiento de relaciones de vecindades. También, dejar interfaces pasivas según corresponda.
- Para IPv6, implemente solución de protocolo vector distancia.
- RA debe proveer de direccionamiento IPv4 por DHCP a redes de VLAN10 y VLAN20.
- Desde router borde de empresa hacia Internet, implementar ruta por defecto en IPv4/IPv6. Debe garantizar el retorno solo para IPv6.
- En IPv4, debe permitir que el tráfico de todas las redes LAN de la empresa, sean traducidos por IP pública. Compruebe dicha traducción.
## Implementación Sistema de Prevención de Intrusos:
- En RA, implemente solución de IPS, donde el tráfico generado por RED LAN 192.168.200.0/24 pueda ser inspeccionado para llegar hacia zona de backbone de la empresa.
- Realizar las configuraciones necesarias, para crear el directorio en la memoria flash del dispositivo, la carga de las firmas y la habilitación de las mismas.
- Implemente modificación en firma asociada a los ICMP-REPLY, para que cualquier conectividad que requiera llegar a red 192.168.200.0/24, sea denegada en tiempo real. Es importante que los mensajes generados se vean reflejados en el servidor SYSLOG que se encuentra situado en dicha red.
## Implementación de Túnel 6 over 4:
- En routers apropiados, implementar solución de túnel 6 over 4, con la dirección IPv6 2021:ACAD:ACAD:AC::/112.
- En túnel debe permitir el tráfico encapsulado IPv6 a través de red IPv4.
- Debe existir conectividad entre Red LAN e Internet a través del túnel creado.
## Implementación de Seguridad en Capa 2:
- Crear VLANs correspondientes.
- Habilitar enlaces troncales correspondientes.
- Implementar solución de agregado de enlace, según protocolo propuesto.
- Interfaces que conectan a equipos finales para todos los dispositivos de capa 2, deben tener habilitado seguridad de puerto de forma dinámica, con un máximo de dos direcciones MAC. En caso que exceda, cantidad, debe generar mensaje de tipo SNMP.
- Interfaces que conectan a equipos finales no deben recibir mensajes BPDU.
- En SW que sea root, deberá asegurar dicho rol, con configuración apropiada para esto.
- Implementar solución de control de tormenta, donde este tipo de tráfico no supere el umbral del 10%.
- Implementar solución de DHCP Snooping, donde se asegure el servidor DHCP. Además, los equipos finales no deben solicitar más de 2 direcciones IP por minuto.
- Comprobar el funcionamiento de todos los mecanismos de seguridad de capa 2 solicitados.
## Topologia
![500](https://slink.proxylivy.work/image/253890e4-b8e1-4054-a35c-7b8b25b55e07.png)
## Descargar
Via Onedrive [Vacio](https://duoccl0-my.sharepoint.com/:u:/g/personal/ga_zunigam_duocuc_cl/EXM8NjXxPjhGmMYkBw_LT9cBAh15jcBSXaaz-TnH8RNOvA?e=aOANcu) | [Terminado](https://duoccl0-my.sharepoint.com/:u:/g/personal/ga_zunigam_duocuc_cl/EWmdLyIjklpIoE1h9HpyNkkB9sqkAQX3OCS8LHgYmOuqmQ?e=Ypb0ET)
# Configuracion
## Configuracion Dispositivos Finales
### PC-VLAN10
- IPv6: 2021:ACAD:ACAD:10::AA/64
- DGv6: 2021:ACAD:ACAD:10::1
- DNSv6: 2021:ACAD:ACAD:8::8
### PC-VLAN20
- IPv6: 2021:ACAD:ACAD:20::BB/64
- DGv6: 2021:ACAD:ACAD:20::1
- DNSv6: 2021:ACAD:ACAD:8::8
### PC-Prueba
- IP: 192.168.200.20
- MASK: 255.255.255.0
- DG: 192.168.200.1
- DNS: 8.8.8.8
### Server-Syslog
- IP: 192.168.200.10
- MASK: 255.255.255.0
- DG: 192.168.200.1
- DNS: 8.8.8.8
## ISP
```
enable
config t
ipv6 unicast-routing
int s0/0/0
ip address 209.50.0.2 255.255.255.252
ipv6 address 2021:ACAD:ACAD:DDD::2/64
no shut
exit
int loopback 0
ip address 8.8.8.8 255.255.255.255
ipv6 address 2021:ACAD:ACAD:8::8/128
no shut
exit
ipv6 route ::0/0 2021:ACAD:ACAD:DDD::1
```
## RC
```
enable
config t
ipv6 unicast-routing
int s0/0/0
ip address 209.50.0.1 255.255.255.252
ipv6 address 2021:ACAD:ACAD:DDD::1/64
no shut
exit
int s0/0/1
ip address 192.168.101.1 255.255.255.248
no shut
exit
router ospf 50
router-id 3.3.3.3
network 192.168.101.0 255.255.255.252 area 0
default-information originate
redistribute static
exit
int s0/0/1
ip ospf 50 area 0
ip ospf message-digest-key 1 md5 modoshorizo
ip ospf authentication message-digest
exit
!
ipv6 router eigrp 50
eigrp router-id 3.3.3.3
redistribute static
no shut
exit
!
int tunnel 0
ipv6 address 2021:ACAD:ACAD:AC::1/112
tunnel source s0/0/1
tunnel destination 192.168.100.1
tunnel mode ipv6ip
ipv6 eigrp 50
no shut
exit
!
ip route 0.0.0.0 0.0.0.0 209.50.0.2
ipv6 route ::0/0 2021:ACAD:ACAD:DDD::2
!
ip access-list standard INTERNET
permit 192.168.10.0 0.0.0.255
permit 192.168.20.0 0.0.0.255
permit 192.168.200.0 0.0.0.255
permit 192.168.101.0 0.0.0.7
permit 192.168.100.0 0.0.0.7
exit
ip nat inside source list INTERNET int s0/0/0 overload
int s0/0/0
ip nat outside
exit
int s0/0/1
ip nat inside
exit
exit
!
```
### Verification
```
show ip nat translations
```
## RB
```
enable
config t
ipv6 unicast-routing
int s0/0/1
ip address 192.168.101.2 255.255.255.248
no shut
exit
int s0/0/0
ip address 192.168.100.2 255.255.255.248
no shut
exit
router ospf 50
router-id 2.2.2.2
network 192.168.101.0 255.255.255.248 area 0
network 192.168.100.0 255.255.255.248 area 0
exit
int s0/0/1
ip ospf 50 area 0
ip ospf message-digest-key 1 md5 modoshorizo
ip ospf authentication message-digest
exit
int s0/0/0
ip ospf 50 area 0
ip ospf message-digest-key 1 md5 modoshorizo
ip ospf authentication message-digest
exit
```

## RA
```
enable
config t
ipv6 unicast-routing
int s0/0/0
ip address 192.168.100.1 255.255.255.248
no shut
exit
int g0/0
ip address 192.168.200.1 255.255.255.0
no shut
exit
int g0/1
no shut
exit
int g0/1.10
encapsulation dot1q 10
ip address 192.168.10.1 255.255.255.0
ipv6 address 2021:ACAD:ACAD:10::1/64
exit
int g0/1.20
encapsulation dot1q 20
ip address 192.168.20.1 255.255.255.0
ipv6 address 2021:ACAD:ACAD:20::1/64
exit
!
router ospf 50
router-id 1.1.1.1
network 192.168.100.0 255.255.255.248 area 0
network 192.168.200.0 255.255.255.0 area 1
network 192.168.10.0 255.255.255.0 area 2
network 192.168.20.0 255.255.255.0 area 2
passive-interface g0/1.10
passive-interface g0/1.20
exit
int s0/0/0
ip ospf 50 area 0
ip ospf message-digest-key 1 md5 modoshorizo
ip ospf authentication message-digest
exit
!
int g0/1.10
ip ospf 50 area 2
ipv6 eigrp 50
exit
int g0/1.20
ip ospf 50 area 2
ipv6 eigrp 50
exit
!
ipv6 router eigrp 50
eigrp router-id 1.1.1.1
passive-interface g0/1.10
passive-interface g0/1.20
no shut
exit
!
int tunnel 0
ipv6 address 2021:ACAD:ACAD:AC::2/112
tunnel source s0/0/0
tunnel destination 192.168.101.1
tunnel mode ipv6ip
ipv6 eigrp 50
no shut
exit
!
!
ip dhcp pool VLAN10
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 8.8.8.8
exit
ip dhcp pool VLAN20
network 192.168.20.0 255.255.255.0
default-router 192.168.20.1
dns-server 8.8.8.8
exit
exit
!
```
### Verifications
```
show ip route
ping 8.8.8.8
```
## RA IPS
Nota: Debes escribir esto a mano, xdn't
```
enable
config t
license boot module c2900 technology-package securityk9
!# Type "yes"
!# Enter
do wr
end
reload
!# Confirm
!# WAIT
!
enable
clock set 17:00:00 19 MAY 2024
mkdir DIRECTORIO-IPS
!# ENTER
!
config t
ip ips config location flash:DIRECTORIO-IPS
ip ips name REGLA-IPS
logging on
logging host 192.168.200.10
service timestamps log datetime msec
ip ips signature-category
category all
retired true
exit
category ios_ips basic
retired false
exit
exit
!# enter
!
int s0/0/0
ip ips REGLA-IPS in
exit
int g0/0
ip ips REGLA-IPS out
exit
int g0/1
ip ips REGLA-IPS in
exit
!
ip ips signature-definition
signature 2004 0
status
retired false
enabled true
exit
engine
event-action produce-alert
event-action deny-packet-inline
exit
exit
exit
!# Confirm
```
## SWC
```
enable
config t
vlan 10
name CASO
exit
vlan 20
name ESTUDIO-2
exit
int range g0/1,f0/6-7,f0/9
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20
switchport nonegotiate
no shut
exit
int range f0/6-7,f0/9
spanning-tree guard root
exit
int range f0/6-7
channel-group 1 mode on
exit
ip dhcp snooping
ip dhcp snooping vlan 10,20
no ip dhcp snooping information option
int g0/1
ip dhcp snooping trust
exit
int range g0/1,f0/6-7,f0/9
storm-control broadcast level 10
exit
```

## SWB
```
enable
config t
vlan 10
name CASO
exit
vlan 20
name ESTUDIO-2
exit
int range f0/6-7,f0/10
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20
switchport nonegotiate
exit
int range f0/6-7
channel-group 1 mode on
exit
ip dhcp snooping
ip dhcp snooping vlan 10,20
no ip dhcp snooping information option
int range f0/6-7
ip dhcp snooping trust
exit
```
## SWA
```
enable
config t
vlan 10
name CASO
exit
vlan 20
ESTUDIO-2
exit
int range f0/9-10
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20
switchport nonegotiate
exit
int f0/23
switchport mode access
switchport access vlan 10
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation restrict
spanning-tree bpduguard enable
spanning-tree portfast
switchport nonegotiate
exit
int f0/24
switchport mode access
switchport access vlan 20
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation restrict
spanning-tree bpduguard enable
spanning-tree portfast
switchport nonegotiate
exit
ip dhcp snooping
ip dhcp snooping vlan 10,20
no ip dhcp snooping information option
int range f0/9-10
ip dhcp snooping trust
exit
int f0/23
ip dhcp snooping limit rate 2
exit
int f0/24
ip dhcp snooping limit rate 2
exit
```
## SW-CONTROL
```
enable
config t
int f0/1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport nonegotiate
exit
int f0/22,f0/24
switchport mode access
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation restrict
spanning-tree bpduguard enable
spanning-tree portfast
switchport nonegotiate
exit
```

