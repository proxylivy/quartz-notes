# Info
## Imagen Topologia
![500](https://slink.proxylivy.work/image/7cd47732-3923-48fe-8ae7-5dbc43a22cb6.png)
## Anexo MST
![500](https://slink.proxylivy.work/image/11072ffc-8570-4c86-a4b4-a1eb4c38b49e.png)
## Sobre
Implementacion MSTP y Etherchannel
## Fe de errata
La topologia muestra que MLS1 lleva la .1 pero no es asi debido a que el [[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]] se encargara con [[020 - Conceptos/020.1 - Administracion/Enrutamiento Inter-Vlan|Enrutamiento Inter-Vlan]], se usara la ip .5
Los computadores finales tienen un problema de IPv6, estan mal configuradas, se deben cambiar, el comando para revisar esa ip es `show running-config | section ipv6 route`, si aparece `ipv6 route ::/0 [int S/S/P]` hay que eliminarla con `no` y cambiarla a `ipv6 route ::/0 [ipv6/prefix]`
## Requerimientos
- Crear las VLANs en todos los switches de la topología.
- Implementar solución de agregado de enlace de capa 2, en base a protocolos propuestos. Comprobar que estén operativos y en óptimo funcionamiento.
- En interfaces lógicas de agregado de enlaces, permitir el paso de las VLANs de Datos correspondiente. La VLAN Nativa debe ser permitida, pero utilizando configuración apropiada para este fin.
- Desactivar protocolo DTP en todas las interfaces correspondientes.
- Asignar interfaces a VLANs correspondientes.
- Implementar algún tipo de seguridad que permita desactivar la puerta del SW en caso que exceda la cantidad de direcciones MAC por defecto que aprenden los SW.
- Implementar solución de MST en todos los switches. Utilizar nombres según anexo, el número de revisión será a elección para las regiones.
- Crear las instancias, en donde se encuentre mapeadas las siguientes VLANs:
	- Instancia 1: VLAN30 y VLAN60
	- Instancia 2: VLAN40 y VLAN50
	- Instancia 3: VLAN100 y VLAN200
- Se ha solicitado modificar el funcionamiento de MST según la figura, para esto desactive el Port-Channel 1.
- Interfaces que conectan a equipos finales no deben enviar ni recibir BPDU.
- Asignar IP mediante la VLAN de Administración a los SW de la topología, además de configurar gateway en estos equipos.
- Router R1 deberá proporcionar IP de forma automática a los equipos de VLANs correspondiente.
- Realizar enrutamiento intervlan en equipo correspondiente.
- Habilitar OSPF con área propuesta. No olvide configurar interfaces pasivas.
- Permitir conectividad hacia Internet. Todo el tráfico saliente deberá utilizar enlace superior, encaso de falla, utilizar enlace de respaldo.
- Comprobar conectividad completa en IPv4/IPv6.
# Configuracion
## SW1
```
enable
config t
interface range e0/2-3
duplex half
exit
vlan 30,40,50,60,100,200
exit
vtp mode transparent
int e0/2
switchport mode access
switchport access vlan 30
switchport port-security
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int e0/3
switchport mode access
switchport access vlan 40
switchport port-security
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int range e0/0-1,e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 30,40,50,60,100
switchport trunk native vlan 100
switchport nonegotiate
exit
int range e0/0-1
channel-group 1 mode active
exit
int range e1/0-1
channel-group 3 mode desirable
exit
int range e1/2-3
channel-group 5 mode on
exit
int port-channel 1
shut
exit
!# Mantener Bloqueo Regional
int port-channel 5
spanning-tree mst 0 cost 200000000
exit
interface vlan 100
ip address 192.168.100.4 255.255.255.240
no shut
exit
ip default-gateway 192.168.100.1
end
copy running-config unix:
```
## SW2
```
enable
config t
interface range e0/2-3
duplex half
exit
vlan 30,40,50,60,100,200
exit
vtp mode transparent
int e0/2
switchport mode access
switchport access vlan 50
switchport port-security
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int e0/3
switchport mode access
switchport access vlan 60
switchport port-security
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int range e0/1,e1/0-3 
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 30,40,50,60,100
switchport trunk native vlan 100
switchport nonegotiate
exit
int range e0/0-1
channel-group 1 mode passive
exit
int range e1/0-1
channel-group 4 mode desirable
exit
int range e1/2-3
channel-group 6 mode on
exit
!# MST
spanning-tree mode mst
spanning-tree mst configuration
name REGION56
revision 2
instance 1 vlan 30,60
instance 2 vlan 40,50
instance 3 vlan 100,200
exit
int port-channel 1
shut
exit
interface vlan 100
ip address 192.168.100.3 255.255.255.240
no shut
exit
ip default-gateway 192.168.100.1
end
copy running-config unix:
```
## MLS2
```
enable
config t
interface e0/3
duplex half
exit
vlan 30,40,50,60,100,200
exit
vtp mode transparent
int e0/3
switchport mode access
switchport access vlan 100
switchport port-security
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int range e0/1-2,e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 30,40,50,60,100
switchport trunk native vlan 100
switchport nonegotiate
exit
int range e0/1-2
channel-group 2 mode passive
exit
int range e1/0-1
channel-group 4 mode auto
exit
int range e1/2-3
channel-group 5 mode on
exit
!# MST
spanning-tree mode mst
spanning-tree mst configuration
name REGION78
revision 1
instance 1 vlan 30,60
instance 2 vlan 40,50
instance 3 vlan 100,200
exit
!# CSIT Root (Todas las instancias)
spanning-tree mst 1-2 root primary
interface vlan 100
ip address 192.168.100.2 255.255.255.240
no shut
exit
ip default-gateway 192.168.100.1
end
copy running-config unix:
```
## MLS1
```
enable
config t
interface e0/0
duplex half
exit
vlan 30,40,50,60,100,200
exit
vtp mode transparent
int range e0/0-2,e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 30,40,50,60,100
switchport trunk native vlan 100
switchport nonegotiate
exit
int range e0/1-2
channel-group 2 mode active
exit
int range e1/0-1
channel-group 3 mode auto
exit
int range e1/2-3
channel-group 6 mode on
exit
!# MST
spanning-tree mode mst
spanning-tree mst configuration
name REGION78
revision 1
instance 1 vlan 30,60
instance 2 vlan 40,50
instance 3 vlan 100,200
exit
!# CSIT Regional Root
spanning-tree mst 0 root primary
!# Bloqueo CSIT
int port-channel 3
spanning-tree mst 1-2 cost 200000000
!# Aplica el truco de feria
interface vlan 100
ip address 192.168.100.5 255.255.255.240
no shut
exit
ip default-gateway 192.168.100.1
end
copy running-config unix:
```
## R1
```
enable
config t
ipv6 unicast-routing
int e0/0
no shut
exit
int e0/0.30
encapsulation dot1q 30
ip address 192.168.30.1 255.255.255.0
ipv6 address 3001:ABCD:ABCD:A1::1/64
exit
int e0/0.40
encapsulation dot1q 40
ip address 192.168.40.1 255.255.255.0
ipv6 address 3001:ABCD:ABCD:A3::1/64
exit
int e0/0.50
encapsulation dot1q 50
ip address 192.168.50.1 255.255.255.0
ipv6 address 3001:ABCD:ABCD:A5::1/64
exit
int e0/0.60
encapsulation dot1q 60
ip address 192.168.60.1 255.255.255.0
ipv6 address 3001:ABCD:ABCD:A7::1/64
exit
ip dhcp excluded-address 192.168.30.1 192.168.30.4
ip dhcp excluded-address 192.168.40.1 192.168.40.4
ip dhcp excluded-address 192.168.50.1 192.168.50.4
ip dhcp excluded-address 192.168.60.1 192.168.60.4
ip dhcp pool VLAN30
network 192.168.30.0 255.255.255.0
default-router 192.168.30.1
dns-server 8.8.8.8
exit
ip dhcp pool VLAN40
network 192.168.40.0 255.255.255.0
default-router 192.168.40.1
dns-server 8.8.8.8
exit
ip dhcp pool VLAN50
network 192.168.50.0 255.255.255.0
default-router 192.168.50.1
dns-server 8.8.8.8
exit
ip dhcp pool VLAN60
network 192.168.60.0 255.255.255.0
default-router 192.168.60.1
dns-server 8.8.8.8
exit
router ospf 1
router-id 1.1.1.1
network 192.168.30.0 255.255.255.0 area 0
network 192.168.40.0 255.255.255.0 area 0
network 192.168.50.0 255.255.255.0 area 0
network 192.168.60.0 255.255.255.0 area 0
network 192.168.12.0 255.255.255.0 area 0
passive-interface e0/0.30
passive-interface e0/0.40
passive-interface e0/0.50
passive-interface e0/0.60
exit
ipv6 router ospf 1
router-id 1.1.1.1
passive-interface e0/0.30
passive-interface e0/0.40
passive-interface e0/0.50
passive-interface e0/0.60
int e0/0.30
ipv6 router ospf 1 area 0
exit
int e0/0.40
ipv6 router ospf 1 area 0
exit
int e0/0.50
ipv6 router ospf 1 area 0
exit
int e0/0.60
ipv6 router ospf 1 area 0
exit
int e0/1
ipv6 router ospf 1 area 0
exit
end
copy running-config unix:
```
## R2
```
enable
config t
ipv6 unicast-routing
router ospf 1
router-id 2.2.2.2
network 192.168.12.0 255.255.255.0 area 0
default-information originate
exit
ip route 0.0.0.0 0.0.0.0 205.0.0.2
ip route 0.0.0.0 0.0.0.0 215.0.0.2 10
ipv6 router ospf 1
router-id 2.2.2.2
default-information originate
exit
int e0/1
ipv6 ospf 1 area 0
exit
ipv6 route ::/0 3001:ABCD:ABCD:205::2
ipv6 route ::/0 3001:ABCD:ABCD:215::2 20
end
copy running-config unix:
```
## ISP
```
enable
config t
ipv6 unicast-routing
ip route 0.0.0.0 0.0.0.0 205.0.0.1
ip route 0.0.0.0 0.0.0.0 215.0.0.1 10
ipv6 route ::/0 3001:ABCD:ABCD:205::1
ipv6 route ::/0 3001:ABCD:ABCD:215::1 20
end
copy running-config unix: 
```
## PCVLAN30
```
enable
config t
no ipv6 route ::/0 e0/2
ipv6 route ::/0 3001:ABCD:ABCD:A1::1
end
copy running-config unix:
```
## PCVLAN40
```
enable
config t
no ipv6 route ::/0 e0/3
ipv6 route ::/0 3001:ABCD:ABCD:A3::1
end
copy running-config unix:
```
## PCVLAN50
```
enable
config t
no ipv6 route ::/0 e0/2
ipv6 route ::/0 3001:ABCD:ABCD:A5::1
exit
end
copy running-config unix:
```
## PCVLAN60
```
enable
config t
no ipv6 route ::/0 e0/3
ipv6 route ::/0 3001:ABCD:ABCD:A7::1
exit
end
copy running-config unix:
```
# Extra
## Truco de feria
Mover CSIT ROOT para que se bloquee la puerta de MLS1
### SW1
```
spanning-tree mst 1-2 root primary
```
### MLS2
```
spanning-tree mst 1-2 root primary
```