# Info
## Imagen Topologia
![500](https://slink.proxylivy.work/image/c4747c35-a8fe-43a1-bf2e-d81e8d32f572.png)
## Objetivo
Implementar una solución de conectividad segura mediante la implementación de firewall ASA y solución de Easy VPN.
## Contexto
La empresa “MODO SHORIZO XD S.A” necesita la implementación de un firewall ASA para comenzar con la segurización de su infraestructura de red. Por lo cual han solicitado la labor de Ing. En Conectividad y Redes para la realización de esa implementación, permitiendo además la conectividad completa en toda la red.
## Requerimientos
## A.- Implementación en Capa 3:
- La topología y equipos finales correspondiente se encuentran direccionado con IPv4.
- Implementar Protocolo de Enrutamiento de Estado de Enlace, usando área de backbone. Configurar interfaces pasivas correspondiente.
- Implementar autenticación de protocolo de enrutamiento a nivel de interfaz.
## B.- Implementación en Capa 2:
- Realizar configuración correspondiente para que las interfaces de los SW no queden asignadas en la VLAN1, además que estas estén apagadas.
- En interfaces correspondientes implementar seguridad de puerto por aprendizaje con un máximo de 2 direcciones MAC, además que en caso de exceder la interfaz debe desactivarse.
- En interfaces correspondientes, implementar mecanismos de estabilización de STP.
- En interfaces correspondiente, implementar control de tormenta para permitir solo el 15% de tráfico broadcast.
- En equipo apropiado implementar DHCP Snooping y mecanismo para evitar el ataque de hambruna, permitiendo solo 2 IP por minuto.
## C.- Implementación de Seguridad:
- En Firewall ASA, definir los nombres de las zonas. Los niveles de seguridad serán los siguientes: Para la Zona Inside el nivel de seguridad será el máximo permitido, para la DMZ será el 40% de la zona Inside, y para la zona Outside será la mitad de la DMZ.
- Implementar pool de DHCP para proporcionar IP de forma dinámica a zona inside. El número máximo de IPv4 serán 16.
- Implementar PAT para que Inside pueda salir por zona Outside, no olvidando implementar MPF para permitir el paso del ICMP.
- Permitir que en PC-DMZ pueda salir por NAT Estático hacia Outside. Utilizar IP a elección de dicho segmento de red. Realizar configuraciones pertinentes para permitir el retorno del ICMP hacia la DMZ.
- Permitir que el servidor SERVICIOS pueda acceder por Telnet hacia ASA.
- Implementar Easy VPN para que PC pueda llegar al servidor SERVICIOS mediante VPN. Debe comprobar que el PC establezca conexión por VPN y la conectividad sea mediante la VPN y no por el protocolo de enrutamiento.
## D.- Implementación de Servicios:
- El servidor SERVICIOS será considerado como servidor NTP, por ende todos los routers y el firewall ASA deben sincronizarse con el equipo en fecha y hora.
- El servidor SERVICIOS será considerado como servidor SYSLOG, donde deberá realizar las configuraciones pertinentes para que los routers envíen sus logs a dicho equipo. Se solicita que el tiempo de registro sea expresando en milisegundos.
# Configuracion
INSIDE: Vlan 1
OUTSIDE: Vlan 2
DMZ: Vlan 3
## Dispositivos Finales
### PC-DMZ
Listo
### Servicios
NOTA: NTP y Syslog(Msec) Server
Listo
### PC-EasyVPN
Listo
- GroupName: ACCESO
- Group Key: cisco
- Host IP: 172.16.10.1
- Username: RA
- Password: 12345
### PC-INSIDE
- IP: DHCP

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
ntp server 172.16.250.3
ntp update-calendar
logging host 172.16.250.3
logging trap
logging on
service timestamps log datetime msec
int g0/2
no shut
exit
int g0/0
no shut
exit
router ospf 1
router-id 1.1.1.1
network 172.16.100.0 255.255.255.252 area 0
network 172.16.10.0 255.255.255.0 area 0
network 172.16.5.0 255.255.255.0 area 0
passive-interface g0/2
exit
int s0/0/0
ip ospf 1 area 0
ip ospf message-digest-key 1 md5 modoshorizo
ip ospf authentication message-digest
no shut
exit
!# EasyVPN
aaa new-model
!
!# GroupName
!
aaa authentication login ACCESO local
aaa authorization network ACCESO local
!
!# Username & Pasword
!
username RA password 12345
crypto isakmp policy 1
authentication pre-share
encryption aes 256
hash sha
group 2
lifetime 86400
crypto isakmp client configuration group ACCESO
!# KEY
key cisco
pool ASA
crypto ipsec transform-set T esp-aes 256 esp-sha-hmac
crypto dynamic-map MAPA-D 1
set transform-set T
reverse-route
crypto map MAPA-D client authentication list ACCESO
crypto map MAPA-D isakmp authorization list ACCESO
crypto map MAPA-D client configuration address respond
crypto map MAPA-D 1 ipsec-isakmp dynamic MAPA-D
!# IP Destino
ip local pool ASA 172.16.250.20 172.16.250.30
!# Int PC EasyVPN
int g0/2
crypto map MAPA-D
exit
exit
```
## RB
```
enable
config t
ntp server 172.16.250.3
ntp update-calendar
logging host 172.16.250.3
logging trap
logging on
service timestamps log datetime msec
router ospf 1
router-id 2.2.2.2
network 172.16.100.0 255.255.255.252 area 0
network 172.16.200.0 255.255.255.252 area 0
exit
int s0/0/0
ip ospf 1 area 0
ip ospf message-digest-key 1 md5 modoshorizo
ip ospf authentication message-digest
no shut
exit
int s0/0/1
ip ospf 1 area 0
ip ospf message-digest-key 1 md5 modoshorizo
ip ospf authentication message-digest
no shut
exit
```
## RC
```
enable
config t
ntp server 172.16.250.3
logging host 172.16.250.3
logging trap
logging on
service timestamps log datetime msec
router ospf 1
router-id 3.3.3.3
network 172.16.200.0 255.255.255.252 area 0
!# Trampilla
network 172.16.250.0 255.255.255.0 area 0
passive-interface g0/0
exit
int g0/0
no shut
exit
int s0/0/1
ip ospf 1 area 0
ip ospf message-digest-key 1 md5 modoshorizo
ip ospf authentication message-digest
no shut
exit
```
## SWA
```
enable
config t
int f0/20
no shut
exit
int f0/10
switchport mode access
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation shutdown
spanning-tree bpduguard enable
spanning-tree portfast
switchport nonegotiate
no shut
exit
int f0/20
storm-control broadcast level 15.0
exit
```
## SWB
```
enable
config t
int f0/20
no shut
exit
int f0/10
switchport mode access
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation shutdown
spanning-tree bpduguard enable
spanning-tree portfast
switchport nonegotiate
no shut
exit
ip dhcp snooping
ip dhcp snooping vlan 1
no ip dhcp snooping information option
int f0/20
ip dhcp snooping trust
exit
int f0/10
ip dhcp snooping limit rate 2
exit
!
!# Storm Control
int f0/20
storm-control broadcast level 15.0
exit
!
```
## SWC
```
enable
config t
int f0/20
no shut
exit
int f0/1
switchport mode access
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation shutdown
spanning-tree bpduguard enable
spanning-tree portfast
switchport nonegotiate
no shut
exit
!# Storm Control
int f0/20
storm-control broadcast level 15.0
exit
!
```
## FW-ASA
`enable` y despues "Enter"
```
config t
clock set 13:00:00 28 JUN 2024
!# Troubleshooting
int vlan 1
no ip address 192.168.1.1 255.255.255.0
exit
int vlan 2
no nameif outside
exit
hostname FW-ASA
ntp server 172.16.250.3
!# Zonas
int vlan 3
nameif DMZ
security-level 40
ip address 172.16.20.2 255.255.255.0
no forward interface vlan 1
exit
int vlan 1
nameif INSIDE
security-level 100
ip address 172.16.15.2 255.255.255.0
exit
int vlan 2
nameif OUTSIDE
security-level 20
ip address 172.16.5.2 255.255.255.0
exit
int e0/3
switchport access vlan 2
no shut
exit
int e0/5
switchport access vlan 3
no shut
exit
int e0/0
switchport access vlan 1
no shut
exit
!# Salto a afuera
route OUTSIDE 0.0.0.0 0.0.0.0 172.16.5.1
!# DHCPD
no dhcpd address 192.168.1.5-192.168.1.36 inside
dhcpd address 172.16.15.1-172.16.15.16 inside
dhcpd dns 8.8.8.8 int inside
dhcpd enable inside
!
!# Nat Pat Inside a Outside
object network INTERNET
subnet 172.16.15.0 255.255.255.0
nat (INSIDE,OUTSIDE) dynamic interface
!
!# MPF PING Inside a Outside
class-map inspection_default
match default-inspection-traffic
exit
policy-map global_policy
class inspection_default
inspect icmp
exit
service-policy global_policy global
!
!# NAT ESTATICO PC-DMZ a OUTSIDE
object network DMZ
host 172.16.20.3
nat (DMZ,OUTSIDE) static 172.16.5.1
!
access-list DMZ permit ip any host 172.16.20.3
access-group DMZ in interface outside
!# Servicios Accede Telnet
username ASA password telnet
aaa authentication telnet console LOCAL
telnet 172.16.250.3 255.255.255.255 OUTSIDE
telnet timeout 5
exit
```