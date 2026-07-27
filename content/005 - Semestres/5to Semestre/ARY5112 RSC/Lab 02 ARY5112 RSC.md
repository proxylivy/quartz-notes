# Info
## Imagen Topologia
![500](https://slink.proxylivy.work/image/5ca3c0b3-3646-4a03-ad34-ebfb79751ffd.png)
## Imagen Anexo A
![500](https://slink.proxylivy.work/image/086b6df6-f332-45a9-8812-dfff0be59eb3.png)
## Imagen Anexo B
![500](https://slink.proxylivy.work/image/9962c14d-f4ff-4441-a25a-43f0341c8473.png)
## Sobre
Implementacion [[010 - Protocolos/010.2 - Switching/spanning-tree/802.1D (STP)|802.1D (STP)]] en Redes de Datos
## Requerimientos
- [[010 - Protocolos/010.2 - Switching/VLAN|VLAN]] -> Implementar microsegmentación de dominios de broadcast en equipos adecuados.
- Asignar [[020 - Conceptos/020.3 - Fundamentos/Interfaz|Interfaces]] a [[010 - Protocolos/010.2 - Switching/VLAN|VLAN]] correspondiente.
- Habilitar [[010 - Protocolos/010.2 - Switching/Seguridad de Puerto|Seguridad de Puerto]] Dinamica.
- Apagar [[020 - Conceptos/020.3 - Fundamentos/Interfaz|Interfaces]] de [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]] que no estén en uso.
- Implementar [[010 - Protocolos/010.2 - Switching/VLAN#Troncal|Troncales]], permitiendo solo el paso de [[010 - Protocolos/010.2 - Switching/VLAN|VLAN]] creadas en la topología.
- Administrar los SW mediante [[010 - Protocolos/010.2 - Switching/VLAN#Vlan Administrativa|VLAN Administrativa]]. Utilice IP propuestas.
- [[020 - Conceptos/020.1 - Administracion/Enrutamiento Inter-Vlan|Enrutamiento Inter-Vlan]] -> Realizar enrutamiento intervlan en IPv4/IPv6 mediante subinterfaces.
- NOTA: RPVST+ esta por defecto en [[020 - Conceptos/020.4 - Dispositivos de Red/IOU WEB|IOU WEB]] -> Implementar RPVST+ en los equipos.
- Habilitar [[010 - Protocolos/010.2 - Switching/spanning-tree/Mecanismos de estabilizacion|Mecanismos de estabilizacion]] de [[010 - Protocolos/010.2 - Switching/spanning-tree/802.1D (STP)|802.1D (STP)]] según sea necesario.
- Optimizar RPVST+ en base a topologías propuestas (Anexo A y Anexo B)
- Habilitar [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv2|OSPFv2]] con area 0, configurar interfaces pasivas según corresponda.
- Permitir salida hacia Internet en IPv4/IPv6. Considerar al enlace E0/1 como enlace principal.
- Comprobar conectividad completa en la topología a nivel de IPv4/IPv6.
# Configuracion
## SWA
```
enable
config t
int range e0/2-3
duplex half
exit
vlan 110,120,130,140,200
exit
vtp mode transparent
int e0/2
switchport mode access
switchport access vlan 110
switchport port-security
spanning-tree bdpuguard enable
spanning-tree bdpufilter enable
spanning-tree portfast
exit
int e0/3
switchport mode access
switchport access vlan 120
switchport port-security
spanning-tree bdpuguard enable
spanning-tree bdpufilter enable
spanning-tree portfast
exit
int e1/0,e0/1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 110,120,130,140,200
switchport nonegotiate
exit
int range e0/0,e1/1-3
shutdown
exit
int vlan 200
ip address 10.10.200.5 255.255.255.0
no shut
exit
ip default-gateway 10.10.200.1
spanning-tree vlan 110,130 root primary
end
copy running-config unix:
```
## SWB
```
enable
config t
int range e0/2-3
duplex half
exit
vlan 110,120,130,140,200
exit
vtp mode transparent
int e0/2
switchport mode access
switchport access vlan 130
switchport port-security
spanning-tree bdpuguard enable
spanning-tree bdpufilter enable
spanning-tree portfast
exit
int e0/3
switchport mode access
switchport access vlan 140
switchport port-security
spanning-tree bdpuguard enable
spanning-tree bdpufilter enable
spanning-tree portfast
int range e0/1,1/1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 110,120,130,140,200
switchport nonegotiate
exit
int range e0/0,e1/0,e1/2-3
shutdown
exit
int vlan 200
ip address 10.10.200.4 255.255.255.0
no shut
exit
ip default-gateway 10.10.200.1
copy running-config unix:
```
## SWC
```
enable
config t
int e1/2
duplex half
exit
vlan 110,120,130,140,200
exit
vtp mode transparent
int range e0/1,e1/0
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 110,120,130,140,200
switchport nonegotiate
exit
int range e0/0,1/0,e1/2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 110,120,130,140,200
switchport nonegotiate
exit
int range e0/2-3,e1/1,e1/3
shutdown
exit
int vlan 200
ip address 10.10.200.2 255.255.255.0
no shut
exit
ip default-gateway 10.10.200.1
spanning-tree vlan 120,140 root primary
end
copy running-config unix:
```
## SWD
```
enable
config t
hostname SWD
int e0/3
duplex half
vlan 110,120,130,140,200
exit
vtp mode transparent
int e0/3
switchport mode access
switchport access vlan 200
switchport port-security
spanning-tree bdpuguard enable
spanning-tree bdpufilter enable
spanning-tree portfast
exit
int range e0/0,e0/1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 110,120,130,140,200
switchport nonegotiate
exit
int range e0/1-2,e1/0,e1/2,e1/2-3
shutdown
exit
int vlan 200
ip address 10.10.200.3 255.255.255.0
no shut
exit
ip default-gateway 10.10.200.1
int e0/0
spanning-tree vlan 110,130 cost 200000000
exit
end
copy running-config unix:
```
## RA
La vlan 200(Administrativa) no se enruta
```
enable
config t
ipv6 unicast-routing
int e1/2
no shut
exit
int e1/2.110
encapsulation dot1q 110
ip address 10.10.110.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:A::1/64
exit
int e1/2.120
encapsulation dot1q 120
ip address 10.10.120.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:B::1/64
exit
int e1/2.110
encapsulation dot1q 130
ip address 10.10.130.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:D::1/64
exit
router ospf 1
router-id 1.1.1.1
network 192.168.11.0 255.255.255.0 area 0
network 10.10.110.0 255.255.255.0 area 0
network 10.10.120.0 255.255.255.0 area 0
network 10.10.130.0 255.255.255.0 area 0
network 10.10.140.0 255.255.255.0 area 0
passive-interface e1/2.110
passive-interface e1/2.120
passive-interface e1/2.130
passive-interface e1/2.140
ipv6 router ospf 1
router-id 1.1.1.1
passive-interface 1/2.110
passive-interface 1/2.120
passive-interface 1/2.130
passive-interface 1/2.140
exit
int e1/2.110
ipv6 osfp 1 area 0
exit
int e1/2.120
ipv6 osfp 1 area 0
exit
int e1/2.130
ipv6 osfp 1 area 0
exit
int e1/2.140
ipv6 osfp 1 area 0
exit
int e0/0
ipv6 osfp 1 area 0
exit
end
copy running-config unix:
```
## Borde
```
enable
config t
ipv6 unicast-routing
router ospf 1
router-id 2.2.2.2
network 192.168.11.0 255.255.255.0
default-information originate
exit
ip route 0.0.0.0 0.0.0.0 200.0.0.2
ip route 0.0.0.0 0.0.0.0 201.0.0.2 10
ipv6 router osfp 1
router-id 2.2.2.2
default-information originate
exit
int 0/0
ipv6 ospf 1 area 0
exit
ipv6 route ::/0 3001:ACAD:ACAD:200::2
ipv6 route ::/0 3001:ACAD:ACAD:201::2 20
exit
end
copy running-config unix:
```
## ISP
```
enable
config t
ipv6 unicast-routing
ip route 0.0.0.0 0.0.0.0 200.0.0.1
ip route 0.0.0.0 0.0.0.0 201.0.0.1 10
ipv6 route ::/0 3001:ACAD:ACAD:200::1
ipv6 route ::/0 3001:ACAD:ACAD:201::1 20
end
copy running-config unix:
```
# Visualizacion

# Extras
Para poder saber que puertas no estan siendo utilizadas por un switch, ejecuta
`show vlan brief` y las que sean del segmento vlan 1, son las que no estan asignadas a nada :D