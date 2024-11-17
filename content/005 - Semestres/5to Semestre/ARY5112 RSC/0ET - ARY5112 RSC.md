# Info
Ejecucion Practica tipo Prueba Practica
Fecha: 09-07-2024
Duracion: 4 Horas
## Imagen Topologia
![](https://slink.proxylivy.work/image/aebbc1c0-45ab-4710-ba18-f215efa7bf98.jpg)
## Sobre
## Instrucciones Generales
La ejecución práctica tiene un 100% de ponderación del puntaje total de este Examen.
- El tiempo para desarrollar el examen práctico es de 5 módulos.
- Los equipos de trabajo son por grupos de 4 integrantes
- La ejecución del examen se realizará en semana 18.
- El simulador de red a usar para este examen será el IOU WEB.
- Los grupos de alumnos deberán cargar en la máquina virtual IUO WEB el archivo correspondiente al
examen y disponible en el anexo “Examen_ARY5112_IOU.gz”
- Inter-vlan en los Routers
- No hay IPv6
- Cuidado con iBGP y eBGP
- Modo Transpent en los switchs
- No aplica la red 11.0.X.0/28
## Contexto
La empresa TucsNet ha decidido ampliar sus redes integrando varios dispositivos nuevos en sus sucursales, los cuales y de acuerdo con lo solicitado deberán ser configurados para permitir una conectividad completa de extremo a extremo
## Requerimientos
NOTA: el símbolo X deberá reemplazarse por el número del grupo asignado al grupo de alumno 
**Somos el Grupo 9**

1) Asignar las direcciones IP en las interfaces de los distintos equipos de red y según diagrama de topología.
2) Deshabilitar DTP en los enlaces troncales de los Switches (capa 3 y capa 2) y permitir solo las VLANs indicadas.
3) Crear las VLAN en los equipos de red y según diagrama de topología, crea una VLAN 5X0 que será la VLAN nativa para toda la red corporativa.
4) Configurar manualmente MST en DLS1, DLS2 y ALS1:
- Región: CCNP2
- Revisión 1
- Instancia 1: VLAN 1X0 y 2X0 y 5X0
- Instancia 2: VLAN 3X0 y 4X0
5) Configurar los 2 EtherChannels de capa 2 según indicado en el diagrama de topología.
6) Configurar el EtherChannel de capa 3 según indicado en el diagrama de topología
7) Configurar Enrutamiento Intervlan según IP solicitada. Además de configurar HSRP en los routers R1 y R3:
- Configure la versión más reciente del protocolo HSRP.
- Configure la dirección IP de la puerta de enlace virtual predeterminada que será la 3er IP de acuerdo al diagrama de topología.
- Designe el router activo para el grupo HSRP, en este caso R1 será el router activo para las VLANs 1X0 y 4X0, el R3 será router activo para las VLANs 2X0 y 3X0
8) Configurar el enrutamiento OSPF Multiarea según indicado en topología
9) Configurar el enrutamiento EIGRP según indicado en topología
10) Configurar la redistribución de rutas según los siguientes criterios, solo debe utilizar un route-map por protocolo de enrutamiento, hacia EIGRP el route-map se deberá llamar “Para-EIGRP” y hacia BGP el route-map se deberá llamar “Para-BGP”
	- Filtrar las redes EIGRP 192.168.128.0/24, 192.168.130.0/24, 192.168.132.0/24 y 192.168.192.0/24, 192.168.194.0/24 y 192.168.196.0/24 utilizando prefix-lits de manera que estas redes no se encuentren en el dominio OSPF.
	- Filtrar las redes OSPF 192.168.50.0/24 y 192.168.60.0/24, en donde no deben ser propagadas al resto de los routers.
10) Configurar eBGP mediante función MP-BGP según el diagrama de topología.
11) Configuración iBG mediante función MP-BGP P con IP de siguiente salto.
12) Aplicar la ruta sumarizada en BGP ASN 65500 solo para las redes del OSPF área 25
13) Aplicar en R1 y R3 el atributo propietario de Cisco donde el tráfico de salida sea por R2
14) Aplicar en R6 y R7 el mismo atributo del punto 13 para dejar el tráfico simétrico hacia R8.
# Configuracion
## Dispositivos Finales
## PC-VLAN190
```
enable
config t
ip route 0.0.0.0 0.0.0.0 192.168.11.3
int e2/1
ip address 192.168.11.10 255.255.255.0
no shut
exit
end
copy running-config unix:
```
## PC-VLAN290
```
enable
config t
ip route 0.0.0.0 0.0.0.0 192.168.12.3
int e0/0
ip address 192.168.12.20 255.255.255.0
no shut
exit
end
copy running-config unix:
```
## PC-VLAN390
```
enable
config t
ip route 0.0.0.0 0.0.0.0 192.168.13.3
int e0/1
ip address 192.168.13.30 255.255.255.0
no shut
exit
end
copy running-config unix:
```
## PC-VLAN490
```
enable
config t
ip route 0.0.0.0 0.0.0.0 192.168.14.3
int e2/1
ip address 192.168.14.40 255.255.255.0
no shut
exit
end
copy running-config unix:
```
## ALS1
```
enable
config t
vlan 190
exit
vlan 290
exit
vlan 390
exit
vlan 490
exit
vlan 590
exit
vtp mode transparent
int e0/0
duplex half
exit
int e0/1
duplex half
exit
int e0/0
switchport mode access
switchport access vlan 290
switchport nonegotiate
no shut
exit
int e0/1
switchport mode access
switchport access vlan 390
switchport nonegotiate
no shut
exit
int range e1/1-3,e2/0
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 190,290,390,490
switchport trunk native vlan 590
switchport nonegotiate
no shut
exit
int range e1/1-2
channel-group 2 mode passive
exit
int range e1/2,e2/0
channel-group 3 mode on
exit
spanning-tree mode mst
spanning-tree mst configuration
name CCNP2
revision 1
instance 1 vlan 190,290,590
instance 2 vlan 390,490
exit
end
copy running-config unix:
```
## DLS1
```
enable
config t
vlan 190
exit
vlan 290
exit
vlan 390
exit
vlan 490
exit
vlan 590
exit
vtp mode transparent
int e0/0
half duplex
exit
int e2/1
half duplex
exit
int e2/1
switchport mode access
switchport access vlan 190
switchport nonegotiate
no shut
exit
int range e0/1-3,e1/0
no switchport
exit
int range e1/1-2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 190,290,390,490
switchport trunk native vlan 590
switchport nonegotiate
no shut
exit
int range e1/1-2
channel-group 2 mode active
exit
int range e0/1-3,e1/0
channel-group 1 mode auto
exit
spanning-tree mode mst
spanning-tree mst configuration
name CCNP2
revision 1
instance 1 vlan 190,290,590
instance 2 vlan 390,490
exit
int po1
ip address 12.12.9.1 255.255.255.240
exit
router ospf 1
router-id 11.11.11.11
network 12.12.9.0 255.255.255.240 area 0
exit
end
copy running-config unix:
```
## DLS2
```
enable
config t
vlan 190
exit
vlan 290
exit
vlan 390
exit
vlan 490
exit
vlan 590
exit
vtp mode transparent
int e0/0
duplex half
exit
int e2/1
duplex half
exit
int e2/1
switchport mode access
switchport access vlan 490
switchport nonegotiate
no shut
exit
int range e0/1-3,e1/0
no switchport
no shut
exit
int range e1/3,e2/0
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 190,290,390,490
switchport trunk native vlan 590
switchport nonegotiate
no shut
exit
int range e1/3,e2/0
channel-group 3 mode on
exit
int range e0/1-3,e1/0
channel-group 1 mode desirable
exit
spanning-tree mode mst
spanning-tree mst configuration
name CCNP2
revision 1
instance 1 vlan 190,290,590
instance 2 vlan 390,490
exit
int po1
ip address 12.12.9.2 255.255.255.240
exit
router ospf 1
router-id 12.12.12.12
network 12.12.9.0 255.255.255.240 area 0
exit
end
copy running-config unix:
```
## R1
```
enable
config t
int loopback 1
ip address 192.168.10.1 255.255.255.0
exit
int loopback 2
ip address 192.168.20.1 255.255.255.0
exit
int loopback 3
ip address 192.168.30.1 255.255.255.0
exit
int loopback 4
ip address 192.168.40.1 255.255.255.0
exit
!# IP
int e0/2
ip address 209.60.20.1 255.255.255.252
exit
int s1/0
ip address 12.0.9.1 255.255.255.252
exit
int e0/0
no shut
exit
int e0/0.190
encapsulation dot1q 190
ip address 192.168.11.1 255.255.255.0
no shut
exit
int e0/0.290
encapsulation dot1q 290
ip address 192.168.12.1 255.255.255.0
no shut
exit
int e0/0.390
encapsulation dot1q 390
ip address 192.168.13.1 255.255.255.0
no shut
exit
!# HSRP
int e0/0.190
standby version 2
standby 190 ip 192.168.11.3
standby 190 preempt
standby 190 priority 120
exit
int e0/0.290
standby version 2
standby 290 ip 192.168.12.3
standby 290 preempt
standby 290 priority 90
exit
int e0/0.390
standby version 2
standby 390 ip 192.168.13.3
standby 390 preempt
standby 390 priority 90
exit
int e0/0.490
standby version 2
standby 490 ip 192.168.14.3
standby 490 preempt
standby 490 priority 120
exit
!# OSPF
router ospf 1
router-id 1.1.1.1
network 12.0.9.0 255.255.255.252 area 0
network 192.168.11.0 255.255.255.0 area 0
network 192.168.12.0 255.255.255.0 area 0
network 192.168.13.0 255.255.255.0 area 0
network 192.168.14.0 255.255.255.0 area 0
network 192.168.10.1 255.255.255.0 area 25
network 192.168.20.1 255.255.255.0 area 25
network 192.168.30.1 255.255.255.0 area 25
network 192.168.40.1 255.255.255.0 area 25
passive-interface e0/0.190
passive-interface e0/0.290
passive-interface e0/0.390
passive-interface e0/0.490
passive-interface loopback 1
passive-interface loopback 2
passive-interface loopback 3
passive-interface loopback 4
exit
!# MP-BGP
router bgp 65500
bgp router-id 1.1.1.1
address-family ipv4
neighbor 12.0.9.2 remote-as 65500
neighbor 12.0.9.2 update-source loopback 1
neighbor 209.60.20.2 remote-as 100
network 192.168.10.1 mask 255.255.255.0
network 192.168.20.1 mask 255.255.255.0
network 192.168.30.1 mask 255.255.255.0
network 192.168.40.1 mask 255.255.255.0
network 192.168.11.0 mask 255.255.255.0
network 192.168.12.0 mask 255.255.255.0
network 192.168.13.0 mask 255.255.255.0
network 192.168.14.0 mask 255.255.255.0
exit
end
copy running-config unix:
```
## R3
```
enable
config t
int loopback 1
ip address 192.168.50.1 255.255.255.0
exit
int loopback 2
ip address 192.168.60.1 255.255.255.0
exit
int loopback 3
ip address 192.168.70.1 255.255.255.0
exit
int loopback 4
ip address 192.168.80.1 255.255.255.0
exit
!# IP
int e0/1
ip address 209.50.10.1 255.255.255.252
no shut
exit
int s1/0
ip address 12.0.9.2 255.255.255.252
exit
int e0/2
ip address 209.90.50.1 255.255.255.252
no shut
exit
!# Intervlan
int e0/0
no shut
exit
int e0/0.190
encapsulation dot1q 190
ip address 192.168.11.2 255.255.255.0
no shut
exit
int e0/0.290
encapsulation dot1q 290
ip address 192.168.12.2 255.255.255.0
no shut
exit
int e0/0.390
encapsulation dot1q 390
ip address 192.168.13.2 255.255.255.0
no shut
exit
!# HSRP
int e0/0.190
standby version 2
standby 190 ip 192.168.11.3
standby 190 preempt
standby 190 priority 90
exit
int e0/0.290
standby version 2
standby 290 ip 192.168.12.3
standby 290 preempt
standby 290 priority 120
exit
int e0/0.390
standby version 2
standby 390 ip 192.168.13.3
standby 390 preempt
standby 390 priority 120
exit
int e0/0.490
standby version 2
standby 490 ip 192.168.14.3
standby 490 preempt
standby 490 priority 90
exit
!# OSPF
router ospf 1
router-id 3.3.3.3
network 12.0.9.0 255.255.255.252 area 0
network 192.168.11.0 255.255.255.0 area 0
network 192.168.12.0 255.255.255.0 area 0
network 192.168.13.0 255.255.255.0 area 0
network 192.168.14.0 255.255.255.0 area 0
network 192.168.50.0 255.255.255.0 area 50
network 192.168.60.0 255.255.255.0 area 50
network 192.168.70.0 255.255.255.0 area 50
network 192.168.80.0 255.255.255.0 area 50
passive-interface e0/0.190
passive-interface e0/0.290
passive-interface e0/0.390
passive-interface e0/0.490
passive-interface loopback 1
passive-interface loopback 2
passive-interface loopback 3
passive-interface loopback 4
exit
!# MP-BGP
router bgp 65500
bgp router-id 3.3.3.3
address-family ipv4
neighbor 12.0.9.1 remote-as 65500
neighbor 12.0.9.1 update-source loopback 1
neighbor 209.50.10.2 remote-as 100
neighbor 209.90.50.2 remote-as 300
network 192.168.10.0 mask 255.255.255.0
network 192.168.20.0 mask 255.255.255.0
network 192.168.30.0 mask 255.255.255.0
network 192.168.40.0 mask 255.255.255.0
network 192.168.11.0 mask 255.255.255.0
network 192.168.12.0 mask 255.255.255.0
network 192.168.13.0 mask 255.255.255.0
network 192.168.14.0 mask 255.255.255.0
exit
end
copy running-config unix:
```
## R2
```
enable
config t
int loopback 0
ip address 8.8.8.8 255.255.255.255
exit
!# IP
int e0/2
ip address 209.60.20.2 255.255.255.252
no shut
exit
int e0/1
ip address 209.50.10.2 255.255.255.252
no shut
exit
int e0/0
ip address 209.70.30.1 255.255.255.252
exit
router bgp 100
bgp router-id 2.2.2.2
address-family ipv4
neighbor 209.70.30.2 remote-as 100
neighbor 209.70.30.2 update-source loopback 0
neighbor 209.60.20.1 remote-as 65500
neighbor 209.50.10.1 remote-as 65500
network 8.8.8.8 mask 255.255.255.255
exit
end
copy running-config unix:
```
## R4
```
enable
config t
int loopback 0
ip address 9.9.9.9 255.255.255.255
exit
int e0/0
ip address 209.70.30.2 255.255.255.252
no shut
exit
int e0/1
ip address 209.80.40.2 255.255.255.252
no shut
exit
router bgp 100
bgp router-id 4.4.4.4
address-family ipv4
neighbor 209.70.30.1 remote-as 100
neighbor 209.70.30.1 update-source loopback 0
neighbor 209.80.40.1 remote-as 64400
network 9.9.9.9 mask 255.255.255.255
exit
end
copy running-config unix:
```
## R5
Nota: La conexion a R4 es por la E0/1 y no la e0/2
```
enable
config t
int e0/1
ip address 209.80.40.1 255.255.255.252
no shut
exit
int s1/0
ip address 172.16.10.1 255.255.255.252
no shut
exit
int s1/1
ip address 172.16.30.1 255.255.255.252
no shut
exit
router eigrp 123
eigrp router-id 5.5.5.5
network 172.16.10.0 255.255.255.252
network 172.16.30.0 255.255.255.252
exit
router bgp 64400
bgp router-id 5.5.5.5
address-family ipv4
neighbor 172.16.30.2 remote-as 64400
neighbor 172.16.10.2 remote-as 64400
neighbor 209.80.40.2 remote-as 100
exit
exit
end
copy running-config unix:
```
## R6
```
enable
config t
int loopback 1
ip address 192.168.128.1 255.255.255.0
exit
int loopback 2
ip address 192.168.129.1 255.255.255.0
exit
int loopback 3
ip address 192.168.130.1 255.255.255.0
exit
int loopback 4
ip address 192.168.131.1 255.255.255.0
exit
int loopback 5
ip address 192.168.132.1 255.255.255.0
exit
int s1/0
ip address 172.16.10.2 255.255.255.252
no shut
exit
int s1/2
ip address 172.16.20.2 255.255.255.252
no shut
exit
router eigrp 123
eigrp router-id 6.6.6.6
network 172.16.10.0 255.255.255.252
network 172.16.20.0 255.255.255.252
exit
router bgp 64400
bgp router-id 6.6.6.6
neighbor 172.16.10.1 remote-as 64400
neighbor 172.16.20.1 remote-as 64400
neighbor 172.16.20.1 update-source loopback 1
neighbor 209.100.60.2 remote-as 300
network 192.168.128.0 mask 255.255.255.0
network 192.168.129.0 mask 255.255.255.0
network 192.168.130.0 mask 255.255.255.0
network 192.168.131.0 mask 255.255.255.0
network 192.168.132.0 mask 255.255.255.0
exit
end
copy running-config unix:
```
## R7
```
enable
config t
int loopback 6
ip address 192.168.192.1 255.255.255.0
exit
int loopback 7
ip address 192.168.193.1 255.255.255.0
exit
int loopback 8
ip address 192.168.194.1 255.255.255.0
exit
int loopback 9
ip address 192.168.195.1 255.255.255.0
exit
int loopback 10
ip address 192.168.196.1 255.255.255.0
exit
int s1/1
ip address 172.16.30.2 255.255.255.252
no shut
exit
int s1/2
ip address 172.16.20.1 255.255.255.252
no shut
exit
int e0/0
ip address 209.110.70.1 255.255.255.252
no shut
exit
router eigrp 123
eigrp router-id 7.7.7.7
network 172.16.30.0 255.255.255.252
network 172.16.20.0 255.255.255.252
exit
router bgp 64400
bgp router-id 7.7.7.7
neighbor 172.16.30.1 remote-as 64400
neighbor 172.16.20.2 remote-as 64400
neighbor 172.16.20.2 update-source loopback 6
neighbor 209.110.70.2 remote-as 300
network 192.168.192.0 mask 255.255.255.0
network 192.168.193.0 mask 255.255.255.0
network 192.168.194.0 mask 255.255.255.0
network 192.168.195.0 mask 255.255.255.0
network 192.168.196.0 mask 255.255.255.0
exit
end
copy running-config unix:
```
## R8
```
enable
config t
int loopback 0
ip address 7.7.7.7 255.255.255.255
exit
int e0/2
ip address 209.90.50.2 255.255.255.252
no shut
exit
int e0/1
ip address 209.100.60.2 255.255.255.252
no shut
exit
int e0/0
ip address 209.110.70.2 255.255.255.252
no shut
exit
!# MP-BGP
router bgp 300
bgp router-id 8.8.8.8
address-family ipv4
neighbor 209.90.50.1 remote-as 65500
neighbor 209.100.60.1 remote-as 64400
neighbor 209.110.70.1 remote-as 64400
network 7.7.7.7 mask 255.255.255.255
exit
end
copy running-config unix:
```
