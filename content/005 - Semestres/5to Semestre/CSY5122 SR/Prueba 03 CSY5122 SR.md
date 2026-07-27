# Info
## Imagen Topologia
![1000](https://slink.proxylivy.work/image/2cf6e53d-7c07-44a7-a215-d5a9211a4f88.png)
## Contexto
La empresa “MI ULTIMA PERA S.A”, se encuentra en la implementación de una red de conectividad con la implementación de firewall ASA, VPN Site-to-Site y VPN Clientless, para lo cual necesita la labor de un Ing. En Conectividad y Redes para la implementación de la red garantizar conectividad completa y funcionamiento de seguridad en esta infraestructura de red.
## Requerimientos
### A. Implementación en Capa 3:
- Direccionar con IPv4 los routers, interfaces de firewall ASA y equipos finales, según segmento de red correspondiente.
- Habilitar protocolo de enrutamiento vector distancia, según AS solicitado. No olvide configurar interfaces pasivas correspondiente.
- Autenticar protocolo de enrutamiento mediante hashing MD5.

### B. Implementación en Capa 2:
- En todos los equipos correspondientes, habilitar mecanismo para que interfaces que conecten a equipos finales al iniciarse las puertas queden habilitadas en el estado de FWD.
- En todos los equipos correspondientes, implementar solución para que en caso que interfaz reciba una BPDU por parte de un equipo final, este puerto quede en estado de err-disable.
- En equipo apropiado, implementar mecanismo de DHCP Snooping, además de evitar el ataque de hambruna en equipos correspondiente, acotando la petición solo hasta 3 IP por minuto.
- En las interfaces apropiada, configurar mecanismo de control de tormenta, permitiendo procesar un máximo de 12% de tráfico broadcast.

### C. Implementación de Seguridad:
- Entre los routers R1 y R3 deberá implementar VPN Site-to-Site, utilizando parámetros a elección. Es importante garantizar que el tráfico originado entre las redes 10.10.10.0/24 a la 10.10.40.0/24 sea protegido.
- En firewall ASA, implementar las zonas propuestas en la topología, para la zona Inside, considerar un nivel de seguridad de 80, para la DMZ el 50% de la zona Inside, y para el Outside, considerar el nivel mínimo de nivel de seguridad.
- Asignar las interfaces a VLANs correspondiente en el FW.
- Implementar DHCP para Inside, para un máximo de 10 direcciones IP.
- Permitir que zona inside salga traducido por IP que contiene SVI de zona Outside.
- Implementar MPF para permitir la inspección del tráfico ICMP.
- Realizar NAT Estático para que PC de zona DMZ pueda salir hacia afuera y tener conectividad de vuelta, a través de una IP libre de la zona Outside.
- Permitir que PC de red 10.10.40.0/24 pueda acceder por Telnet al ASA.
- Implementar solución de VPN Clientless donde los PC que están al exterior del firewall puedan acceder a la página web `www.prueba3.cl` a través del firewall. Utilizar como bookmark **PRUEBA3**

### D. Comprobación de Servicios:
- Comprobar el funcionamiento de VPN Site-to-Site generando conectividad entre la red 10.10.10.0/0 a 10.10.40.0/24, procurando que el tráfico sea cifrado.
- Comprobar que los PC anteriormente ocupado, puedan acceder a la página web [www.prueba3.cl](http://www.prueba3.cl/) a través del FW ASA.
- Comprobar conectividad completa en la topología.

# Configuracion
## Dispositivos Finales
### PC3
- IP: 10.10.10.2
- Mask: 255.255.255.0
- DG: 10.10.10.1
- DNS: 8.8.8.8
### PC2
- IP: 172.16.100.3
- Mask: 255.255.255.0
- DG: 172.16.100.2
- DNS: 8.8.8.8
### PC0
- DHCP
### SERVER-PT
- DHCP
### PC1
- IP: 10.10.40.2
- Mask: 255.255.255.0
- DG: 10.10.40.1
- DNS: 8.8.8.8

## SWA
```
enable
config t
int f0/14
switchport mode access
spanning-tree bpduguard enable
spanning-tree portfast
storm-control broadcast level 12.0
no shut
exit
int f0/7
storm-control broadcast level 12.0
no shut
exit

```
## SWB
```
enable
config t
ip dhcp snooping
no ip dhcp snooping information option
int range f0/12,f0/14
switchport mode access
spanning-tree bpduguard enable
spanning-tree portfast
ip dhcp snooping limit rate 3
storm-control broadcast level 12.0
no shut
exit
int f0/4
storm-control broadcast level 12.0
ip dhcp snooping trust
no shut
exit
```
## SWC
```
enable
config t
int f0/1
switchport mode access
spanning-tree bpduguard enable
spanning-tree portfast
storm-control broadcast level 12.0
no shut
exit
int f0/2
storm-control broadcast level 12.0
no shut
exit
```
## RA Licencia
```
enable
config t
license boot module c2900 technology-package securityk9
yes
do wr
exit
reload
```
## RA
```
enable
config t
hostname RA
int g0/0
ip address 10.10.20.1 255.255.255.0
no shut
exit
int g0/1
ip address 172.16.200.1 255.255.255.0
no shut
exit
int g0/2
ip address 10.10.10.1 255.255.255.0
no shut
exit
!# Autenticacion Clasica
key chain llavero
key 1
key-string perita
exit
exit
!# EIGRP
router eigrp 123
eigrp router-id 1.1.1.1
network 10.10.20.0 255.255.255.0
network 10.10.10.0 255.255.255.0
!# Interfaces Pasivas para despues
!# passive-interface g0/1
!# passive-interface g0/2
exit
int g0/0
ip authentication mode eigrp 123 md5
ip authentication key-chain eigrp 123 llavero
exit
!# Site-To-Site
crypto isakmp policy 1
authentication pre-share
encryption aes 256
hash sha
group 5
lifetime 86400
exit
crypto isakmp key perota address 10.10.30.1
crypto ipsec transform-set PRUEBA-3 esp-aes 256 esp-sha-hmac
ip access-list extended PROTECCION
permit ip 10.10.10.0 0.0.0.255 10.10.40.0 0.0.0.255
exit
crypto map MAPA-PRUEBA3 1 ipsec-isakmp
match address PROTECCION
set transform-set PRUEBA-3
set peer 10.10.30.1
exit
int g0/0
crypto map MAPA-PRUEBA3
exit
```
## RB
```
enable
config t
hostname RB
int g0/0
ip address 10.10.20.2 255.255.255.0
no shut
exit
int g0/1
ip address 10.10.30.2 255.255.255.0
no shut
exit
!# Autenticacion Clasica
key chain llavero
key 1
key-string perita
exit
exit
!# EIGRP
router eigrp 123
eigrp router-id 2.2.2.2
network 10.10.20.0 255.255.255.0
network 10.10.30.0 255.255.255.0
exit
int range g0/0-1
ip authentication mode eigrp 123 md5
ip authentication key-chain eigrp 123 llavero
exit
```
## RC Licencia
```
enable
config t
license boot module c2900 technology-package securityk9
yes
do wr
exit
reload
```
## RC
```
enable
config t
hostname RC
int g0/2
ip address 10.10.40.1 255.255.255.0
no shut
exit
!# Autenticacion Clasica
key chain llavero
key 1
key-string perita
exit
exit
!# EIGRP
router eigrp 123
eigrp router-id 3.3.3.3
network 10.10.30.0 255.255.255.0
network 10.10.40.0 255.255.255.0
!# passive-interface f0/2
exit
int g0/1
ip address 10.10.30.1 255.255.255.0
ip authentication mode eigrp 123 md5
ip authentication key-chain eigrp 123 llavero
exit
!# VPN Site-to-Site
crypto isakmp policy 1
authentication pre-share
encryption aes 256
hash sha
group 5
lifetime 86400
exit
crypto isakmp key perota address 10.10.20.1
crypto ipsec transform-set PRUEBA-3 esp-aes 256 esp-sha-hmac
ip access-list extended PROTECCION
permit ip 10.10.40.0 0.0.0.255 10.10.10.0 0.0.0.255 
exit
crypto map MAPA-PRUEBA3 1 ipsec-isakmp
match address PROTECCION
set transform-set PRUEBA-3
set peer 10.10.20.1
exit
int g0/1
crypto map MAPA-PRUEBA3
no shut
exit
```
## FW-ASA
`enable` y "enter"
```
config t
clock set 13:00:00 04 JUL 2024
int vlan 1
no ip address 192.168.1.1 255.255.255.0
exit
int vlan 2
no nameif outside
exit
hostname ASA
!# Zonas
int vlan 3
nameif DMZ
security-level 40 
ip address 172.16.100.2 255.255.255.0
no forward interface vlan 1
exit
int vlan 1
nameif INSIDE
security-level 80
ip address 172.16.50.2 255.255.255.0
exit
int vlan 2
nameif OUTSIDE
security-level 0
ip address 172.16.200.2 255.255.255.0
exit
int e0/5
!# OUTSIDE
switchport access vlan 2
no shut
exit
int e0/4
!# INSIDE
switchport access vlan 1
no shut
exit
int e0/2
!# DMZ
switchport access vlan 3
no shut
exit
!# Salto
route OUTSIDE 0.0.0.0 0.0.0.0 172.16.200.1
!# DHCPD
no dhcpd address 192.168.1.5-192.168.1.36 inside
dhcpd address 172.16.50.3-172.16.50.10 inside
dhcpd dns 8.8.8.8 int inside
dhcpd enable inside
!# NAT PAT INSIDE -> OUTSIDE
object network INTERNET
subnet 172.16.50.0 255.255.255.0
nat (INSIDE,OUTSIDE) dynamic interface
!
!# MPF PING
class-map inspection_default
match default-inspection-traffic
exit
policy-map global_policy
class inspection_default
inspect icmp
exit
service-policy global_policy global
!
!# NAT ESTATICO PC-DMZ -> OUTSIDE
object network DMZ
host 172.16.100.3
nat (DMZ,OUTSIDE) static 172.16.200.1
!
access-list DMZ permit ip any host 172.16.100.3
access-group DMZ in interface outside
!# Servicios Accede Telnet
username ASA password telnet
aaa authentication telnet console LOCAL
telnet 10.10.40.2 255.255.255.255 OUTSIDE
telnet timeout 5
!
!# VPN Clientless
webvpn
enable OUTSIDE
exit
!
username modopera password peron
!
!# Group Policy
group-policy POLITICA internal
group-policy POLITICA attributes
webvpn
url-list value PRUEBA3
exit
exit
!
!#CONEXION A NIVEL DE TUNEL
tunnel-group ACCESO type remote-access
tunnel-group ACCESO general-attributes
default-group-policy POLITICA
exit
!
username modopera attributes
vpn-group-policy POLITICA
exit
!
```
## BookMark Manager
- Titulo: PRUEBA3
- url: `http://172.16.50.4` o `www.prueba3.cl`