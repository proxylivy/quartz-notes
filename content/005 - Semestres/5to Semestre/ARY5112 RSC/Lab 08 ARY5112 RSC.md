# Info
## Imagen Topologia
![500](https://slink.proxylivy.work/image/82603b01-6037-42fe-830b-e7f8e57de367.png)
## Sobre
Implementacion de [[020 - Conceptos/020.1 - Administracion/Prefix-List|Prefix-List]] y [[020 - Conceptos/020.1 - Administracion/Route-Map|Route-Map]]
## Requerimientos
- Habilitar [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP|EIGRP]] según AS propuesto y [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv2|OSPFv2]] de forma clásica. Dejar interfaces pasivas donde sea necesario.
- Considerar para R4, que las loopback en IPv4 deben ser enrutadas para EIGRP y para IPv6 enrutadas en OSPF.
- Deberá realizar redistribución entre protocolos de enrutamiento a nivel IPv4 mediante route-maps, pero deberán existir las siguientes consideraciones:
- **A.** Las rutas que provienen de EIGRP hacia OSPF deberán tener una métrica variable en su resultado final, pero al momento de pasar por el router de borde le deberá añadir una métrica de 60.
- **B**. Las rutas que provienen OSPF hacia EIGRP deberán ser redistribuida, obteniendo los parámetros K de interfaz correspondiente.
- La redistribución entre OSPF y EIGRP para IPv6 será de forma directa.
- Loopback 2 no debe ser vista por el router R1 a nivel IPv4.
- Loopback de R5 debe ser vista solo por el router R5 a nivel de IPv4.
- Router R6 no debe ver las loopback 5 ni loopback 555 a nivel de IPv6.
- Router R4 deberá permitir solo rutas a nivel IPv4 con prefijos entre el /24 y el /26.
- Comprobar conectividad completa en la topología.
# Configuracion
## R1
```
enable
config t
router ospf 1
router-id 1.1.1.1
network 172.16.10.1 0.0.0.0 area 2
network 172.16.12.1 0.0.0.0 area 2
network 172.16.13.1 0.0.0.0 area 2
passive-interface e0/0
ipv6 unicast-routing
ipv6 router ospf 1
router-id 1.1.1.1
passive-interface 0/0
exit
int e0/0
ipv6 ospf 1 area 2
exit
int s1/1
ipv6 ospf 1 area 2
exit
int s1/2
ipv6 ospf 1 area 2
exit
```
## R2
```
enable
config t
router ospf 1
router-id 2.2.2.2
network 20.20.20.1 0.0.0.0 area 2
network 20.20.21.1 0.0.0.0 area 2
network 20.20.22.1 0.0.0.0 area 2
network 172.16.12.2 0.0.0.0 area 2
network 172.16.23.2 0.0.0.0 area 2
passive-interface loopback 2
passive-interface loopback 22
passive-interface loopback 222
exit
ipv6 unicast-routing
ipv6 router ospf 1
router-id 2.2.2.2
passive-interface loopback 2
passive-interface loopback 22
passive-interface loopback 222
exit
int range loopback 2, loopback 22, loopback 222
ipv6 ospf 1 area 2
exit
int s1/1
ipv6 ospf 1 area 2
exit
int s1/0
ipv6 ospf 1 area 2
exit
!# Hacer que OSPF Respete las Mascaras
int range loopback 2, loopback 22, loopback 22
ip ospf network point-to-point
ipv6 ospf network point-to-point
exit
```
## R3
```
enable
config 
router ospf 1
router-id 3.3.3.3
network 172.16.13.3 0.0.0.0 area 2
network 172.16.23.3 0.0.0.0 area 2
network 172.16.34.3 0.0.0.0 area 0
exit
ipv6 unicast-routing
ipv6 router ospf 1
router-id 3.3.3.3
exit
int s1/0
ipv6 ospf 1 area 2
exit
int s1/1
ipv6 ospf 1 area 0
exit
```
## R4
la interfaz de ospf se revisa con `show int [int S/S/P]`
```
enable
config
router ospf 1
router-id 4.4.4.4
network 172.16.34.4 0.0.0.0 area 0
exit
router eigrp 1
network 172.16.46.4 0.0.0.0
network 172.16.45.4 0.0.0.0
network 40.40.40.1 0.0.0.0
network 40.40.41.1 0.0.0.0
network 40.40.42.1 0.0.0.0
passive-interface loopback 4
passive-interface loopback 44
passive-interface loopback 444
exit
ipv6 unicast-routing
ipv6 router ospf 1
router-id 4.4.4.4
passive-interface loopback 4
passive-interface loopback 44
passive-interface loopback 444
exit
int range loopback 4, loopback 44, loopback 444
ipv6 ospf 1 area 0
exit
int s1/1
ipv6 ospf 1 area 0
exit
ipv6 router eigrp 1
exit
int s1/2
ipv6 eigrp 1
exit
int s1/3
ipv6 eigrp 1
exit
!# Rutas IPv6 EIGRP -> OSPF
ipv6 router ospf 1
redistribute eigrp 1 include-connected
exit
!# RUTAS IPv6 OSPF -> EIGRP
ipv6 router eigrp 1
redistribute ospf 1 metric 1544 2000 255 1 1500 include-connected
exit
int range loopback 4, loopback 44, loopback 444
ip ospf network point-to-point
ipv6 ospf network point-to-point
!# RUTAS IPv4 EIGRP -> OSPF
ip prefix-list RUTAS-EIGRP permit 50.50.50.0/18
ip prefix-list RUTAS-EIGRP permit 50.50.51.0/21
ip prefix-list RUTAS-EIGRP permit 50.50.52.0/24
ip prefix-list RUTAS-EIGRP permit 40.40.40.0/25
ip prefix-list RUTAS-EIGRP permit 40.40.41.0/26
ip prefix-list RUTAS-EIGRP permit 40.40.42.0/27
ip prefix-list RUTAS-EIGRP permit 172.16.60.0/24
ip prefix-list RUTAS-EIGRP permit 172.16.45.0/24
ip prefix-list RUTAS-EIGRP permit 172.16.46.0/24
ip prefix-list RUTAS-EIGRP permit 172.16.56.0/24
route-map OSPF permit
match ip address prefix-list RUTAS-EIGRP
set metric-type type-1
set metric 60
exit
router ospf 1
redistribute eigrp 1 subnets route-map OSPF
exit
!# RUTAS IPv4 OSPF -> EIGRP
ip prefix-list RUTAS-OSPF permit 172.16.10.0/24
ip prefix-list RUTAS-OSFP permit 172.16.12.0/24
ip prefix-list RUTAS-OSFP permit 172.16.13.0/24
ip prefix-list RUTAS-OSFP permit 172.16.23.0/24
ip prefix-list RUTAS-OSFP permit 172.16.34.0/24
ip prefix-list RUTAS-OSFP permit 20.20.20.0/24 
ip prefix-list RUTAS-OSFP permit 20.20.21.0/26
ip prefix-list RUTAS-OSFP permit 20.20.22.0/28
route-map EIGRP permit
match ip address prefix-list RUTAS-OSPF
set metric 1544 2000 255 1 1500
exit
router eigrp 1
redistribute ospf 1 route-map EIGRP
exit
ip prefix-list FILTRO-4 permit 0.0.0.0/0 ge 24 le 26
router eigrp 1
distribute-list prefix FILTRO-4 in serial 1/2
distribute-list prefix FILTRO-4 in serial 1/3
exit
router ospf 1
distribute-list prefix FILTRO-4 in serial 1/1
```
## R5
```
enable
config t
router eigrp 1
network 172.16.45.5 0.0.0.0
network 172.16.56.5 0.0.0.0
network 50.50.50.1 0.0.0.0
network 50.50.51.1 0.0.0.0
network 50.50.52.1 0.0.0.0
passive-interface loopback 5
passive-interface loopback 55
passive-interface loopback 555
exit
ipv6 unicast-routing
ipv6 router eigrp 1
passive-interface loopback 5
passive-interface loopback 55
passive-interface loopback 555
exit
int range loopback 5, loopback 55, loopback 555
ipv6 eigrp 1
exit
int s1/0
ipv6 eigrp 1
exit
int s1/2
ipv6 eigrp 1
exit
!# Bloquear rutas de Loopback a los demas routers
ip prefix-list FILTRO-2 deny 50.50.50.0.0/18
ip prefix-list FILTRO-2 permit 0.0.0.0/0 le 32
router eigrp 1
distribute list prefix FILTRO-2 out serial 1/0
distribute list prefix FILTRO-2 out serial 1/2
exit
!# Bloquear rutas de Loopback en R6 IPv6
ipv6 prefix-list FILTRO-3 deny 3001:ABCD:ABCD:51::64
ipv6 prefix-list FILTRO-3 deny 3001:ABCD:ABCD:53::64
ipv6 prefix-list FILTRO-3 permit ::/0 le 128
ipv6 router eigrp 1
distribute-list prefix-list FILTRO-3 in 1/0
distribute-list prefix-list FILTRO-3 in 1/3
exit
```
## R6
```
enable
config t
router eigrp 1
network 172.16.60.6 0.0.0.0
network 172.16.46.6 0.0.0.0
network 172.16.56.6 0.0.0.0
passive-interface e0/0
exit
ipv6 unicast-routing
ipv6 router eigrp 1
passive-interface e0/0
exit
int e0/0
ipv6 eigrp 1
exit
int s1/0
ipv6 eigrp 1
exit
int s1/3
ipv6 eigrp 1
```