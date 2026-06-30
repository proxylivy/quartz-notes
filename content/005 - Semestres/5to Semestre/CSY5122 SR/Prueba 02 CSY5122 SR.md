# Info
## Imagen Topologia
![500](https://slink.proxylivy.work/image/63422d06-2daa-452e-a89b-77d5b0b6e9a9.png)
## Objetivo
La empresa “MI SEGUNDA RED S.A” necesita segurizar su red empresarial a nivel de capa 2 y 3, debido a que ha tenido inconvenientes y robo de información especialmente desde el interior de la empresa.
Para lo cual, necesita un Ing. En Conectividad y Redes, con conocimientos en seguridad, que le permita proteger esta red. Usted, ha sido contactado para liderar este proyecto de optimización de red, dejando esta red con los mejores estándares de conectividad y de seguridad permitidos en esta solución.
## Requerimientos
## 1. Implementación de Protocolos de Enrutamiento IPv4/IPv6
- Asignar direccionamiento IPv4/IPv6 según corresponda en la topología de red.
- Para IPv4, implemente solución de protocolo vector distancia propietario Cisco. Es importante autenticar el establecimiento de relaciones de vecindades. También, dejar interfaces pasivas según corresponda.
- Para IPv6, implemente solución de protocolo estado de enlace. Debe autenticar las sesiones correspondientes.
- En R1 deberá mandar ruta sumarizada de VLANs y segmento de RED LAN, para reducir tamaños de RIB de R2 y R3.
- R1 debe proveer de direccionamiento IPv4 por DHCP a redes de VLAN20 y VLAN30.
- Desde router borde de empresa hacia Internet, implementar ruta por defecto en IPv4/IPv6. Debe garantizar el retorno solo para IPv6.
- En IPv4, debe permitir que el tráfico de todas las redes LAN de la empresa, sean traducidos por IP pública. Compruebe dicha traducción.

## 2. Implementación Sistema de Prevención de Intrusos
- En R1, implemente solución de IPS, donde el tráfico generado por RED LAN 172.16.11.0/24 pueda ser inspeccionado para llegar hacia zona de backbone de la empresa.
- Realizar las configuraciones necesarias, para crear el directorio en la memoria flash del dispositivo, la carga de las firmas y la habilitación de las mismas.
- Implemente modificación en firma asociada a los ICMP-ECHO, para que si los equipos de la RED-LAN 172.16.11.0/24 desean salir fuera de la red, sea denegada en tiempo real. Es importante que los mensajes generados se vean reflejados en el servidor SYSLOG que se encuentra situado en dicha red.

## 3. Implementación de Túnel 6 over 4
- En routers apropiados, implementar solución de túnel 6 over 4, con la dirección IPv6 2021:ABCD:DCBA:1000::/112.
- En túnel debe permitir el tráfico encapsulado IPv6 a través de red IPv4.
- Debe existir conectividad entre Red LAN e Internet a través del túnel creado.

## 4. Implementación de Seguridad en Capa 2
- Crear VLANs correspondientes.
- Habilitar enlaces troncales correspondientes.
- Implementar VLAN administrativa, para que los equipos de capa 2 sean administrado mediante Telnet.
- Interfaces que conectan a equipos finales para todos los dispositivos de capa 2, deben tener habilitado seguridad de puerto de forma dinámica, con un máximo de tres direcciones MAC. En caso que exceda cantidad, no deberá aceptar más frames.
- Interfaces que conectan a equipos finales no deben recibir mensajes BPDU.
- En SW que sea root, deberá asegurar dicho rol, con configuración apropiada para esto.
- Implementar solución de control de tormenta, donde este tipo de tráfico no supere el umbral del 9%.
- Implementar solución de DHCP Snooping, donde se asegure el servidor DHCP. Además, los equipos finales no deben solicitar más de 3 direcciones IP por minuto.
- Comprobar el funcionamiento de todos los mecanismos de seguridad de capa 2 solicitados.

# Configuracion
## Dispositivos Finales
### RED-LAN
IPv4: 172.16.11.10
Mask: 255.255.255.0
DG: 172.16.11.1
DNS: 4.4.4.4

### SYSLOG
IP: 172.16.11.20
Mask: 255.255.255.0
DG: 172.16.11.1
DNS: 4.4.4.4

### VLAN20
IPv6: 2021:ACAD:ACAD:20::BBBB/64
DGv6: 2021:ACAD:ACAD:20::1
DNSv6: 2021:ACAD:ACAD:4::4

### VLAN30
IPv6: 2021:ACAD:ACAD:30::AAAA/64
DGv6: 2021:ACAD:ACAD:30::1
DNSv6: 2021:ACAD:ACAD:4::4

## ISP
```
enable
config t
ipv6 unicast-routing
int g0/1
ip address 220.0.0.1 255.255.255.252
ipv6 address 2021:ACAD:ACAD:220::2/112
no shut
exit
int loopback 0
ip address 4.4.4.4 255.255.255.255
ipv6 address 2021:ACAD:ACAD:4::4/128
no shut
exit
ipv6 route ::0/0 2021:ACAD:ACAD:220::1
```
## R3
```
enable
config t
ipv6 unicast-routing
int g0/1
ip address 220.0.0.1 255.255.255.252
ipv6 address 2021:ACAD:ACAD:220::1/112
no shut
exit
int g0/0
ip address 172.16.23.3 255.255.255.0
no shut
exit
router eigrp 123
eigrp router-id 3.3.3.3
network 172.16.23.0 255.255.255.0
redistribute static
key chain llavero
key 1
key-string perita
exit
int g0/0
ip authentication mode eigrp 123 md5
ip authentication key-chain eigrp 123 llavero
exit
ipv6 router ospf 123
router-id 3.3.3.3
default-information originate
exit
int tunnel 0
ipv6 address 2021:ABCD:DCBA:1000::3/112
tunnel source g0/0
tunnel destination 172.16.12.1
tunnel mode ipv6ip
ipv6 ospf 123 area 0
no shut
exit
ip route 0.0.0.0 0.0.0.0 220.0.0.2
ipv6 route ::0/0 2021:ACAD:ACAD:220::2
ip access-list standard INTERNET
permit 172.16.23.0 0.0.0.255
permit 172.16.12.0 0.0.0.255
permit 172.16.11.0 0.0.0.255
permit 172.16.10.0 0.0.0.255
permit 172.16.20.0 0.0.0.255
permit 172.16.30.0 0.0.0.255
exit
ip nat inside source list INTERNET int g0/1 overload
int g0/0
ip nat inside
exit
int g0/1
ip nat outside
exit
int g0/0
ip authentication mode eigrp 123 md5
ip authentication key-chain eigrp 123 llavero
exit
```
## R2
```
enable
config t
ipv6 unicast-routing
int g0/0
ip address 172.16.23.2 255.255.255.0
no shut
exit
int s0/0/0
ip address 172.16.12.2 255.255.255.0
no shut
exit
router eigrp 123
eigrp router-id 2.2.2.2
network 172.16.23.0 255.255.255.0
network 172.16.12.0 255.255.255.0
key chain llavero
key 1
key-string perita
exit
int g0/0
ip authentication mode eigrp 123 md5
ip authentication key-chain eigrp 123 llavero
no shut
exit
int s0/0/0
ip authentication mode eigrp 123 md5
ip authentication key-chain eigrp 123 llavero
no shut
exit
```
## R1
```
enable
config t
ipv6 unicast-routing
int s0/0/0
ip address 172.16.12.1 255.255.255.0
no shut
exit
router eigrp 123
eigrp router-id 1.1.1.1
network 172.16.12.0 255.255.255.0
network 172.16.0.0 255.255.254.0
passive-interface g0/0.20
passive-interface g0/0.30
passive-interface g0/0.10
key chain llavero
key 1
key-string perita
int s0/0/0
ip authentication mode eigrp 123 md5
ip authentication key-chain eigrp 123 llavero
no shut
exit
int g0/1
ip 172.16.11.1 255.255.255.0
no shut
exit
int g0/0
no shut
exit
int g0/0.10
encapsulation dot1q 10
ip address 172.16.10.1 255.255.255.0
no shut
exit
int g0/0.20
encapsulation dot1q 20
ip address 172.16.20.1 255.255.255.0
ipv6 address 2021:ACAD:ACAD:20::1/64
router eigrp 123
no shut
exit
int g0/0.30
encapsulation dot1q 30
ip address 172.16.30.1 255.255.255.0
ipv6 address 2021:ACAD:ACAD:30::1/64
no shut
exit
ipv6 router ospf 123
router-id 1.1.1.1
no shut
exit
int tunnel 0
ipv6 address 2021:ABCD:DCBA:1000::1/112
tunnel source s0/0/0
tunnel destination 172.16.23.3
tunnel mode ipv6ip
ipv6 ospf 123 area 0
no shut
exit
ip dhcp pool VLAN20
network 172.16.20.0 255.255.255.0
default-router 172.16.20.1
dns-server 4.4.4.4
exit
ip dhcp pool VLAN30
network 172.16.30.0 255.255.255.0
default-router 172.16.30.1
dns-server 4.4.4.4
exit
```
## R1-IPS
```
enable
config t
license boot module c2900 technology-package securityk9
do wr
end
reload
!# OKEY
enable
clock set 14:00:00 23 MAY 2024
mkdir DIRECTORIO-IPS
config t
ip ips config location flash:DIRECTORIO-IPS
ip ips name REGLA-IPS
logging on
logging host 172.16.11.20
service timestamps log datetime msec
ip ips signature-category
category all
retired true
exit
category ios_ips basic
retired false
exit
exit
int s0/0/0
ip ips REGLA-IPS in
exit
int g0/1
ip ips REGLA-IPS out
exit
int g0/0
ip ips REGLA-IPS in
exit
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
## SW-LAN
```
enable
config t
vlan 10
name ADMINISTRATIVA
vlan 20
name PERA
vlan 30
name MAXIMA
int range f0/12, f0/24
switchport mode access
switchport nonegotiate
no shut
exit
int f0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30
switchport nonegotiate
no shut
exit
```
## SWC
```
enable
config t
vlan 10
name ADMINISTRATIVA
vlan 20
name PERA
vlan 30
name MAXIMA
int range f0/1, f0/23-24
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30
switchport nonegotiate
no shut
exit
int vlan 10
ip address 172.16.10.2 255.255.255.0
no shut
exit
ip dhcp snooping
ip dhcp snooping vlan 20,30
no ip dhcp snooping information option
int f0/1
ip dhcp snooping trust
exit
int range f0/1, f0/23-24
storm-control broadcast level 9
```
## SWB
```
enable
config t
vlan 10
name ADMINISTRATIVA
vlan 20
name PERA
vlan 30
name MAXIMA
int range f0/22,f0/24
switchport mode trunk
switchport trunk allowed vlan 10,20,30
switchport nonegotiate
no shut
exit
int f0/10
switchport mode access
switchport access vlan 20
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation restrict
switchport nonegotiate
spanning-tree bpduguard enable
spanning-tree portfast
exit
ip dhcp snooping
ip dhcp snooping vlan 20,30
no ip dhcp snooping information option
int range f0/22,f0/24
ip dhcp snooping trust
exit
int f0/10
ip dhcp snooping limit rate 3
exit
int vlan 10
ip address 172.16.10.3 255.255.255.0
exit
```
## SWA
```
enable
config t
vlan 10
name ADMINISTRATIVA
vlan 20
name PERA
vlan 30
name MAXIMA
int range f0/22,f0/23
switchport mode trunk
switchport trunk allowed vlan 10,20,30
switchport nonegotiate
no shut
exit
int f0/4
switchport mode access
switchport access vlan 30
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation restrict
switchport nonegotiate
spanning-tree bpduguard enable
spanning-tree portfast
no shut
exit
ip dhcp snooping
ip dhcp snooping vlan 20,30
no ip dhcp snopoping information option
int range f0/22,f0/23
ip dhcp snooping trust
exit
int f0/4
ip dhcp snooping limit rate 3
exit
```