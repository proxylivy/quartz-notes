# Info
## Imagen Topologia
![500](https://slink.proxylivy.work/image/94f3de99-b2e8-4754-ba4f-e412b7427c9a.png)
## Sobre
Reforzar Implementacion Capa 2
## Requerimientos:
- [[010 - Protocolos/010.2 - Switching/VLAN|VLAN]] -> Realizar microsegmentación de dominios de broadcasts en equipos adecuados.
- [[010 - Protocolos/010.2 - Switching/Seguridad de Puerto|Seguridad de Puerto]] -> Habilitar seguridad de puerto dinámica.
- [[010 - Protocolos/010.3 - Comunicaciones/SSH|SSH]] -> Permitir administrar los switches mediante SSHv2. Utilizar parámetros a elección y ocupar segmento IP propuesto para dicha VLAN de Administración.
- [[010 - Protocolos/010.2 - Switching/VLAN#Vlan Administrativa|VLAN Administrativa]] -> La administración de los switches será a través de la VLAN30.
- [[010 - Protocolos/010.1 - Routing/RIP#RIPv2|RIP]] -> Habilitar protocolo enrutamiento RIPv2, configurando interfaces pasivas según sea necesario.
- [[020 - Conceptos/020.3 - Fundamentos/Sumarizacion de Redes|Sumarizacion de Redes]] -> Router RA debe anunciar las rutas de las VLAN de forma sumarizada hacia el router RB.
- [[010 - Protocolos/010.3 - Comunicaciones/DHCP|DHCP]] -> Router RB debe recibir direccionamiento dinámico por DHCP por parte del router ISP.
- Generar salida hacia redes externas.
- [[010 - Protocolos/010.1 - Routing/NAT|NAT]] -> Todo el tráfico generado por las redes LAN debe salir traducido por IP pública otorgado por el router ISP hacia el router RB.
- Comprobar que exista conectividad completa en la topología.
# Configuracion
## SWA
```
enable
config t
vlan 10,20,30
vtp mode transparent
int e0/3
switchport mode access
switchport access vlan 10
switchport port-security
exit
int range e0/0-1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30
int vlan 30
ip address 192.168.30.100 255.255.255.0
no shut
exit
ip default-gateway 192.168.30.1
username SWA secret perita
ip domain-name www.duoc.cl
crypto key generate rsa
1024
line vty 0 1
transport input ssh
login local
exit
ip ssh version 2
ip ssh authentication-retries 3
ip ssh time-out 45
exit
exit
copy running-config unix:
```
## SWB
```
enable
config t
vlan 10,20,30
vtp mode transparent
int e0/3
switchport mode access
switchport access vlan 20
switchport port-security
exit
int range e0/0,e0/2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30
exit
int vlan 30
ip address 192.168.30.50 255.255.255.0
no shut
exit
ip default-gateway 192.168.30.1
username SWA secret perita
ip domain-name www.duoc.cl
crypto key generate rsa
1024
line vty 0 1
transport input ssh
login local
exit
ip ssh version 2
ip ssh authentication-retries 3
ip ssh time-out 45
end
copy running-config unix:
```
## SWC
```
enable
config t
vlan 10,20,30
vtp mode transparent
int e0/3
switchport mode access
switchport access vlan 30
switchport port-security
exit
int range e0/0-2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30
exit
int vlan 30
ip address 192.168.30.150 255.255.255.0
no shut
exit
ip default-gateway 192.168.30.1
username SWA secret perita
ip domain-name www.duoc.cl
crypto key generate rsa
1024
line vty 0 1
transport input ssh
login local
exit
ip ssh version 2
ip ssh authentication-retries 3
ip ssh time-out 45
end
copy running-config unix:
```
## RA
```
enable
config t
int e0/0
no shut
exit
int e0/0.10
encapsulation dot1q 10
ip address 192.168.10.1 255.255.255.0
exit
int e0/0.20
encapsulation dot1q 20
ip address 192.168.20.1 255.255.255.0
exit
int e0/0.30
encapsulation dot1q 30
ip address 192.168.30.1 255.255.255.0
exit
router rip
version 2
no auto-summary
network 192.168.10.0
network 192.168.20.0
network 192.168.30.0
network 192.168.100.0
passive-interface e0/0.10
passive-interface e0/0.20
passive-interface e0/0.30
exit
end
copy running-config unix:
```
## RB
```
enable
config t
router rip
version 2
no auto-summary
network 192.168.100.0
default-information originate
int e0/0
ip address dhcp
no shut
exit
ip route 0.0.0.0 0.0.0.0 200.0.0.1
ip access-list standard INTERNET
permit 192.168.10.0 0.0.0.255
permit 192.168.20.0 0.0.0.255
permit 192.168.30.0 0.0.0.255
exit
ip nat inside source list INTERNET interface e0/0 overload
int e0/0
ip nat outside
exit
int e0/1
ip nat inside
exit
end
copy running-config unix:
```
## ISP
```
enable
config t
ip dhcp pool ISP
network 200.0.0.0 255.255.255.252
default-router 200.0.0.1
exit
end
copy running-config unix:
```
