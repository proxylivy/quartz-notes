# Info
## Imagen Topologia
![500](https://slink.proxylivy.work/image/009525e2-b9d4-431d-a506-db376fc8f653.png)
## Imagen Anexo MST
![500](https://slink.proxylivy.work/image/8419371b-842c-443f-9243-402c30d57340.png)
## Sobre
Enrutamiento Intervlan con OSPF
## Requerimientos
- Crear las VLANs en todos los switches de la topología.
- Implementar solución de agregado de enlace de capa 2, en base a protocolos propuestos. Comprobar que estén operativos y en óptimo funcionamiento.
- En interfaces lógicas de agregado de enlace, permitir el paso de las VLANs de Datos correspondientes. La VLAN Nativa debe ser permitida, pero utilizando configuración apropiada para ese fin.
- Desactivar protocolo DTP en todas las interfaces correspondientes.
- Asignar interfaces a VLANs correspondientes.
- Implementar algún tipo de seguridad que permite desactivar la puerta del SW en caso que exceda la cantidad de direcciones MAC por defecto que aprenden los SW.
- Implementar solución de MST en todos los switches. Utilizar nombres según anexo, el número de revisión será a elección para todas las regiones.
- Crear las instancias, en donde se encuentren mapeadas las siguientes VLANs:
	- Instancia 1: VLAN12 y VLAN17
	- Instancia 2: VLAN13 y VLAN15
	- Instancia 3: VLAN14 y VLAN16
- Se solicita modificar el funcionamiento de MST según la figura, dejando puertas bloqueadas.
- Interfaces que conectan a equipos finales no deben enviar ni recibir BPDU.
- Los switches serán administrados a través de la VLAN12. Asignar direccionamiento IPv4.
- Realizar enrutamiento intervlan por SVI a nivel de IPv4/IPv6.
- En equipo señalado configurar DHCPv4 para que equipos finales reciban IP de manera dinámica, procurando excluir las primeras 10 IP asignables.
- Habilitar protocolo de enrutamiento OSPF para IPv4/IPv6. Configurar interfaces pasivas.
- Ajustar temporizadores de OSPF en IPv4/IPv6, dejándolo al doble de su configuración por defecto.
- Modificar el tipo de enlace entre R1 y R2 el cual debe quedar como P2P, a nivel de IPv4.
- Realizar salida hacia Internet a nivel IPv4/IPv6.
- Comprobar conectividad completa en la topología.

# Configuracion
## SW1
```
enable
config t
vlan 12,13,14,15,16,17
exit
vtp mode transparent
int range e0/2-3
duplex half
exit
int range e0/2-3
switchport mode access
switchport port-security
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int e0/2
switchport access vlan 13
exit
int e0/3
switchport access vlan 14
exit
int range e0/0-1,e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 12-16
switchport trunk native vlan 17
switchport nogetiate
exit
int range 1/0-1
channel-group 1 mode on
exit
int range e0/0-1
channel-group 4 mode active
exit
int range e1/2-3
channel-group 5 mode auto
exit
spanning-tree mode mst
spanning-tree mst configuration
name REGION13
revision 1
instance 1 vlan 12,17
instance 2 vlan 13,15
instance 3 vlan 14,16
exit
int port-channel 5
no shut
exit
int port-channel 4
spanning-tree mst 0 cost 200000000
exit
!# CSIT Root
spanning-tree mst 1-3 root primary
int vlan 12
ip address 192.168.10.2 255.255.255.0
no shut
exit
ip default-gateway 192.168.10.1
end
copy running-config unix:
```
## SW2
```
enable
config t
vlan 12,13,14,15,16,17
exit
vtp mode transparent
int range e0/2-3
duplex half
exit
int range e0/2-3
switchport mode access
switchport port-security
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int e0/2
switchport access vlan 15
exit
int e0/3
switchport access vlan 16 
exit
int range e0/0-1,e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 12-16
switchport trunk native vlan 17
switchport nogetiate
exit
int range e1/2-3
channel-group 3 mode auto
exit
int range e0/0-1
channel-group 4 mode passive
exit
int range e1/0-1
channel-group 6 mode on
exit
spanning-tree mode mst
spanning-tree mst configuration
name REGION13
revision 1
instance 1 vlan 12,17
instance 2 vlan 13,15
instance 3 vlan 14,16
exit
int vlan 12
ip address 192.168.10.3 255.255.255.0
no shut
exit
ip default-gateway 192.168.10.1
end
copy running-config unix:
```
## SW3
```
enable
config t
vlan 12,13,14,15,16,17
exit
vtp mode transparent
int range e0/0-1,e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 12-16
switchport trunk native vlan 17
switchport nogetiate
exit
int range e0/0-1
channel-group 2 mode auto
exit
int range e1/2-3
channel-group 5 mode desirable
exit
int range e1/0-1
channel-group 6 mode on
exit
spanning-tree mode mst
spanning-tree mst configuration
name REGION31
revision 2
instance 1 vlan 12,17
instance 2 vlan 13,15
instance 3 vlan 14,16
exit
int port-channel 5
no shut
exit
int vlan 12
ip address 192.168.10.4 255.255.255.0
no shut
exit
ip default-gateway 192.168.10.1
end
copy running-config unix:
```
## SW4
```
enable
config t
ip routing
no ipv6 unicast-routing
ipv6 unicast-routing
vlan 12,13,14,15,16,17
exit
vtp mode transparent
int e0/2
duplex half
exit
int range e0/0-2,e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 12-16
switchport trunk native vlan 17
switchport nogetiate
exit
int range 1/0-1
channel-group 1 mode on
exit
int range e0/0-1
channel-group 2 mode desirable
exit
int range e1/2-3
channel-group 3 mode desirable
exit
spanning-tree mode mst
spanning-tree mst configuration
name REGION13
revision 1
instance 1 vlan 12,17
instance 2 vlan 13,15
instance 3 vlan 14,16
exit
!# CSIT Regional Root
spanning-tree mst 0 root primary
!
!# Hace el truco de feria
!
!# Asegurar bloqueo puerta CSIT Root
int port-channel 3
spanning-tree mst 1-3 cost 200000000
exit
int vlan 12
ip address 192.168.10.1 255.255.255.0
no shut
exit
ip default-gateway 192.168.10.1
int vlan 13
ip address 192.168.20.1 255.255.255.0
ipv6 address 3001:ABCD:ABCD:A6::1/64
no shut
exit
int vlan 14
ip address 192.168.22.1 255.255.255.0
ipv6 address 3001:ABCD:ABCD:B2::1/64
no shut
exit
int vlan 15
ip address 192.168.24.1 255.255.255.0
ipv6 address 3001:ABCD:ABCD:C4::1/64
no shut
exit
int vlan 16
ip address 192.168.26.1 255.255.255.0
ipv6 address 3001:ABCD:ABCD:C8::1/64
no shut
exit
int e0/2
no switchport
ip address 192.168.41.1 255.255.255.252
ipv6 address 3001:ABCD:ABCD:41::1/112
exit
router ospf 1
router-id 4.4.4.4
network 192.168.20.1 0.0.0.0 area 1
network 192.168.22.1 0.0.0.0 area 1
network 192.168.24.1 0.0.0.0 area 1
network 192.168.26.1 0.0.0.0 area 1
network 192.168.41.1 0.0.0.0 area 1
passive-interface vlan 13
passive-interface vlan 14
passive-interface vlan 15
passive-interface vlan 16
exit
ipv6 router ospf 1
router-id 4.4.4.4
passive-interface vlan 13
passive-interface vlan 14
passive-interface vlan 15
passive-interface vlan 16
exit
int vlan 13
ipv6 ospf 1 area 0
exit
int vlan 14
ipv6 ospf 1 area 0
exit
int vlan 15
ipv6 ospf 1 area 0
exit
int vlan 16
ipv6 ospf 1 area 0
exit
int e0/2
ipv6 ospf 1 area 0
exit
ip dhcp excluded-address 192.168.20.1 192.168.20.10
ip dhcp excluded-address 192.168.22.1 192.168.22.10
ip dhcp excluded-address 192.168.24.1 192.168.24.10
ip dhcp excluded-address 192.168.26.1 192.168.26.10
ip dhcp pool VLAN13
network 192.168.20.0 255.255.255.0
default-router 192.168.20.1
exit
ip dhcp pool VLAN14
network 192.168.22.0 255.255.255.0
default-router 192.168.22.1
exit
ip dhcp pool VLAN15
network 192.168.24.0 255.255.255.0
default-router 192.168.24.1
exit
ip dhcp pool VLAN16
network 192.168.26.0 255.255.255.0
default-router 192.168.26.1
exit
int e0/2
ip opsf hello-interval 20
ip ospf dead-interval 80
ipv6 opsf hello-interval 20
ipv6 ospf dead-interval 80
exit
end
copy running-config unix:
```
## R1
```
enable
config t
int e0/2
ip address 192.168.41.2 255.255.255.252
no shut
exit
router ospf 1
router-id 1.1.1.1
network 192.168.41.2 0.0.0.0 area 1
network 192.168.21.1 0.0.0.0 area 0
exit
ipv6 unicast-routing ospf 1
router-id 1.1.1.1
exit
int e0/2
ipv6 ospf 1 area 1
exit
int e0/0
ipv6 ospf 1 area 0
exit
int range e0/0,e0/2
ip opsf hello-interval 20
ip ospf dead-interval 80
ipv6 opsf hello-interval 20
ipv6 ospf dead-interval 80
ip ospf network point-to-point
exit
end
copy running-config unix:
```
# R2
```
enable
config t
ipv6 unicast-routing
router ospf 1
router-id 2.2.2.2
network 192.168.21.2 0.0.0.0 area 0
default-information originate
exit
ip route 0.0.0.0 0.0.0.0 200.0.0.1
ipv6 router ospf 1
router-id 2.2.2.2
default-information originate
exit
int e0/0
ipv6 ospf 1 area 0
exit
ipv6 route ::/0 3001:ABCD:ABCD:200::1
int e0/0
ip opsf hello-interval 20
ip ospf dead-interval 80
ipv6 opsf hello-interval 20
ipv6 ospf dead-interval 80
ip ospf network point-to-point
exit
end
copy running-config unix:
```
## ISP
```
enable
config t
ipv6 unicast-routing
ip route 0.0.0.0 0.0.0.0 200.0.0.2
ipv6 route ::/0 3001:ABCD:ABCD:200::1
end
copy running-config unix:
```
# Extras
## Truco de Feria
Mover Root del SW4 al SW2 y luego otra vez al SW4
### SW2
```
enable
config t
spanning-tree mst 0 root primary
exit
```
### SW4
```
enable
config t
spanning-tree mst 0 root primary
exit
```