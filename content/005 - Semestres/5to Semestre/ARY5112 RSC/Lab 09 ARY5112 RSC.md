# Info
## Imagen Topologia
![700](https://slink.proxylivy.work/image/c7d420b5-f80c-4998-9128-d1b464eaeed0.png)
## Sobre
Implementacion de [[010 - Protocolos/010.1 - Routing/BGP/BGP|BGP]]
## Requerimientos
- Habilitar [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP|EIGRP]] con AS propuesto en ambas sucursales. Dejar interfaces pasivas según corresponda
- Garantizar conectividad a red externa. Los routers borde deben recibir [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv4|IPv4]] por [[010 - Protocolos/010.3 - Comunicaciones/DHCP|DHCP]] por parte del ISP.
- Filtre loopback 3 y 77 de manera que los routers que tienen esa loopback aparezca dentro de la RIB.
- Habilitar [[010 - Protocolos/010.1 - Routing/BGP/BGP|BGP]], utilizar bgp router-id con formato X.X.X.X, donde "X" represente número de router.
- Las relaciones de vecindad a nivel de iBGP será mediante loopback y para eBGP será mediante next-hop.
- Las loopback deben participar en [[010 - Protocolos/010.1 - Routing/BGP/BGP|BGP]] . Para ISP, implemente loopback y agréguela dentro del protocolo EGP.
- Permitir que los router iBGP tengan next-hop-self de manera alcanzables.
- [[010 - Protocolos/010.1 - Routing/BGP/BGP#Redistribuir rutas a EIGRP|Redistrubuir]] BGP en protocolo IGP, mediante route-map.
- Compruebe conectividad completa en la topología.
# Configuracion
## R1
```
enable
config t
int loopback 1
ip address 10.10.10.1 255.255.255.0
exit
router eigrp 10
network 192.168.10.1 0.0.0.0
network 192.168.12.1 0.0.0.0
network 192.168.13.1 0.0.0.0
network 10.10.10.1 0.0.0.0
passive-interface e0/0
passive-interface loopback 1
router bgp 65000
bgp router-id 1.1.1.1
neighbor 20.20.20.1 remote-as 65000
neighbor 20.20.20.1 update-source loopback 1
neighbor 30.30.31.1 remote-as 65000
neighbor 30.30.31.1 update-source loopback 1
network 10.10.10.0 mask 255.255.255.0
network 192.168.10.0 mask 255.255.255.0
exit
end
copy running-config unix:
```
## R2
```
enable
config t
int s1/0
ip address 192.168.12.2 255.255.255.0
exit
int s1/2
ip address 192.168.23.2 255.255.255.0
exit
int loopback 2
ip address 20.20.20.1 255.255.255.0
exit
router eigrp 10
network 192.168.12.2 0.0.0.0
network 192.168.23.2 0.0.0.0
network 20.20.20.1 0.0.0.0
passive-interface loopback 2
redistribute static
exit
ip route 0.0.0.0 0.0.0.0 209.50.0.1
router bgp 65000
bgp router-id 2.2.2.2
neighbor 10.10.10.1 remote-as 65000
neighbor 10.10.10.1 update-source loopback 2
neighbor 30.30.31.1 remote-as 65000
neighbor 30.30.31.1 update-source loopback 2
neighbor 209.50.0.1 remote-as 200
network 20.20.20.0 mask 255.255.255.0
!# Next Hop Self
neighbor 10.10.10.1 next-hop-self
neighbor 30.30.31.1 next-hop-self
exit
!# BGP -> EIGRP
ip prefix-list RUTAS-BGP permit 209.50.0.0/30
ip prefix-list RUTAS-BGP permit 8.8.8.8/32
route-map EIGRP permit
match ip address prefix-list RUTAS-BGP
set metric 10000 100 255 1 1500
exit
router eigrp 10
redistribute bgp 65000 route-map EIGRP
exit
!# EIGRP -> BGP
!# TODAS LAS RUTAS CON TODO
ip prefix-list EMPRESA permit 192.168.10.0/24
ip prefix-list EMPRESA permit 192.168.12.0/24
ip prefix-list EMPRESA permit 192.168.13.0/24
ip prefix-list EMPRESA permit 192.168.23.0/24
ip prefix-list EMPRESA permit 10.10.10.0/24
ip prefix-list EMPRESA permit 20.20.20.0/24
ip prefix-list EMPRESA permit 30.30.31.0/24
ip prefix-list EMPRESA permit 172.16.56.0/24
ip prefix-list EMPRESA permit 172.16.57.0/24
ip prefix-list EMPRESA permit 172.16.67.0/24
ip prefix-list EMPRESA permit 172.16.60.0/24
ip prefix-list EMPRESA permit 50.50.50.0/24
ip prefix-list EMPRESA permit 60.60.60.0/24
ip prefix-list EMPRESA permit 70.70.70.0/24
route-map BGP permit
match ip address prefix-list EMPRESA
set metric 1
exit
router bgp 65000
redistribute eigrp 10 route-map BGP
exit
end
copy running-config unix:
```
## R3
```
enable
config t
router eigrp 10
network 192.168.13.3 0.0.0.0
network 192.168.23.3 0.0.0.0
network 30.30.30.1 0.0.0.0
network 30.30.31.1 0.0.0.0
passive-interface loopback 3
passive-interface loopback 33
exit
ip prefix-list FILTRO-1 deny 30.30.30.0/24
ip prefix-list FILTRO-1 permit 0.0.0.0/0 le 32
router eigrp 10
distribute-list prefix FILTRO-1 out serial 1/1
distribute-list prefix FILTRO-1 out serial 1/2
exit
router bgp 65000
bgp router-id 3.3.3.3
neighbor 10.10.10.1 remote-as 65000
neighbor 10.10.10.1 update-source loopback 33
neighbor 20.20.20.1 remote-as 65000
neighbor 20.20.20.1 update-source loopback 33
network 30.30.30.0 mask 255.255.255.0
network 30.30.31.0 mask 255.255.255.0
exit
end
copy running-config unix:
```
## R5
```
enable
config t
int loopback 5
ip address 50.50.50.1 255.255.255.0
exit
router eigrp 20
network 172.16.56.5 0.0.0.0
network 172.16.57.5 0.0.0.0
network 50.50.50.1 0.0.0.0
passive-interface loopback 5
redistribute static
exit
ip route 0.0.0.0 0.0.0.0 219.50.0.1
int e0/1
ip address dhcp
no shut
exit
router bgp 65100
bgp router-id 5.5.5.5
neighbor 60.60.60.1 remote-as 65100
neighbor 60.60.60.1 update-source loopback 5
neighbor 70.70.70.1 remote-as 65100
neighbor 70.70.70.1 update-source loopback 5
neighbor 219.50.0.1 remote-as 200
network 50.50.50.0 mask 255.255.255.0
!# Next Hop Self
neighbor 60.60.60.1 next-hop-self
neighbor 70.70.70.1 next-hop-self
!# BGP -> EIGRP
ip prefix-list RUTAS-BGP permit 209.50.0.0/30
ip prefix-list RUTAS-BGP permit 8.8.8.8/32
route-map EIGRP permit
match ip address prefix-list RUTAS-BGP
set metric 10000 100 255 1 1500
exit
router eigrp 20
redistribute bgp 65100 route-map EIGRP
exit
!# EIGRP -> BGP
!# TODAS LAS RUTAS CON TODO
ip prefix-list EMPRESA permit 192.168.10.0/24
ip prefix-list EMPRESA permit 192.168.12.0/24
ip prefix-list EMPRESA permit 192.168.13.0/24
ip prefix-list EMPRESA permit 192.168.23.0/24
ip prefix-list EMPRESA permit 10.10.10.0/24
ip prefix-list EMPRESA permit 20.20.20.0/24
ip prefix-list EMPRESA permit 30.30.31.0/24
ip prefix-list EMPRESA permit 172.16.56.0/24
ip prefix-list EMPRESA permit 172.16.57.0/24
ip prefix-list EMPRESA permit 172.16.67.0/24
ip prefix-list EMPRESA permit 172.16.60.0/24
ip prefix-list EMPRESA permit 50.50.50.0/24
ip prefix-list EMPRESA permit 60.60.60.0/24
ip prefix-list EMPRESA permit 70.70.70.0/24
route-map BGP permit
match ip address prefix-list EMPRESA
set metric 1
exit
router bgp 65100
redistribute eigrp 20 route-map BGP
exit
end
copy running-config unix:
```
## R6
```
enable
config t
int loopback 6
ip address 60.60.60.1 255.255.255.0
exit
router eigrp 20
network 172.16.67.6 0.0.0.0
network 172.16.60.6 0.0.0.0
network 60.60.60.1 0.0.0.0
passive-interface loopback 6
passive-interface e0/0
exit
router bgp 65100
bgp router-id 6.6.6.6
neighbor 50.50.50.1 remote-as 65100
neighbor 50.50.50.1 update-source loopback 6
neighbor 70.70.70.1 remote-as 65100
neighbor 70.70.70.1 update-source loopback 6
network 60.60.60.0 mask 255.255.255.0
network 172.16.60.0 mask 255.255.255.0
exit
end
copy running-config unix:
```
## R7
```
enable
config t
router eigrp 20
network 172.16.57.7 0.0.0.0
network 172.16.67.7 0.0.0.0
network 70.70.70.1 0.0.0.0
network 70.70.71.1 0.0.0.0
passive-interface loopback 7
passive-interface loopback 77
exit
!# Bloquea la loopback que quiere bloquear
ip prefix-list FILTRO-2 deny 70.70.71.0/24
!# Permite todas las demas IP
ip prefix-list FILTRO-2 permit 0.0.0.0/0 le 32
router eigrp 20
!# Se configura en las interfaces a otros routers
distribute-list prefix FILTRO-1 out serial 1/1
distribute-list prefix FILTRO-1 out serial 1/2
exit
router bgp 65100
bgp router-id 7.7.7.7
neighbor 50.50.50.1 remote-as 65100
neighbor 50.50.50.1 update-source loopback 7
neighbor 60.60.60.1 remote-as 65100
neighbor 60.60.60.1 update-source loopback 7
network 70.70.70.0 mask 255.255.255.0
network 70.70.71.0 mask 255.255.255.0
exit
end
copy running-config unix:
```
## ISP
```
enable
config t
ip dhcp pool R2
network 209.50.0.0 255.255.255.252
default-router 209.50.0.1
exit
ip dhcp pool R5
network 219.50.0.0 255.255.255.252
default-router 219.50.0.1
exit
int loopback 0
ip address 8.8.8.8 255.255.255.255
exit
router bgp 200
bgp router-id 10.10.10.10
neighbor 209.50.0.2 remote-as 65000
neighbor 219.50.0.2 remote-as 65100
exit
end
copy running-config unix:
```