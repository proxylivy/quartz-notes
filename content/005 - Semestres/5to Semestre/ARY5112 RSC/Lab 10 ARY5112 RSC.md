# Info
## ERRATA
En la sucursal 2 hay una ip de topologia duplicada, el enlace entre R7 y R8 sera a travez de la `172.16.78.0/24` y no la `68`, y en R8, no tiene creada la loopback
## Imagen Topologia
![500](https://slink.proxylivy.work/image/0d66a9a8-eed2-4018-bbbd-96c7183db748.png)
## Sobre
Implementacion de Atributos BGP
## Requerimientos
- [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv2|OSPFv2]] -> Habilitar IGP mediante protocolo de estado de enlace. Utilizar formato router-id "X.X.X.X", donde "X" sea reemplazado por el número del router. Configurar interfaces pasivas según sea necesario.
- Router R2 no debe ver la loopback de R3.
- Router R7 no debe ver la loopback de R8
- Garantizar conectividad a red externa. Los routers de borde deben recibir IPv4 por DHCP de parte de los ISP.
- Implementar BGP según los AS solicitados. Utilizar formato router-id "X.X.X.X" donde "X" sea reemplazado por el número del router.
- A nivel de iBGP realizar realización de vecindad mediante loopback. Para el caso de los eBGP, realizar relación con IP de siguiente salto. La configuración es malla completa.
- Permitir que los router iBGP tenga next-hop valido.
- Implementar solución para que a nivel de BGP, los routers R3 y R6 utilicen a ISP-A como equipo principal mediante este protocolo de enrutamiento.
- Implementar solución que permite alterar el valor de métrica de ruta BGP a elección, dejándolo con un valor de 50.
- Sumarizar las loopback que participan en BGP.
- Realizar la redistribución entre protocolo IGP y EGP mediante mapas de rutas.
- Comprobar conectividad completa entre los PC.
## Tabla Sumarizacion ISP-A
L4:40.40.40.1/24
L44:40.40.50.1/24

| 1er Octeto | 2do Octeto | 128 | 64  | 32  | /   | 16  | 8   | 4   | 2   | 1   |
| ---------- | ---------- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 40         | 40         | 0   | 0   | 1   | /   | 0   | 1   | 0   | 0   | 0   |
| 40         | 40         | 0   | 0   | 1   | /   | 1   | 0   | 0   | 1   | 0   |
Red Sumarizada: `40.40.32.0/19`
## Tabla Sumarizacion 
IP 1: 50.50.50.1/24
IP 2: 50.50.80.1/24

| 1er <br>Octeto | 2do <br>Octeto | 128 | /   | 64  | 32  | 16  | 8   | 4   | 2   | 1   | 4to <br>Octeto |
| -------------- | -------------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | -------------- |
| 50             | 50             | 0   | /   | 0   | 1   | 1   | 0   | 0   | 1   | 0   | 1              |
| 50             | 50             | 0   | /   | 1   | 0   | 1   | 0   | 0   | 0   | 0   | 1              |
IP Sumarizada: `50.50.0.0/17`
# Configuracion
## R1
```
enable
config t
router ospf 1
router-id 1.1.1.1
network 1.1.1.1 0.0.0.0 area 0
network 192.168.10.1 0.0.0.0 area 0
network 192.168.12.1 0.0.0.0 area 0
network 192.168.13.1 0.0.0.0 area 0
passive-interface loopback 1
passive-interface e0/0
exit
router bgp 65001
bgp router-id 1.1.1.1
neighbor 3.3.3.3 remote-as 65001
neighbor 3.3.3.3 update-source loopback 1
neighbor 2.2.2.2 remote-as 65001
neighbor 2.2.2.2 update-source loopback 1
neighbor 192.168.10.0 255.255.255.0
network 1.1.1.1 mask 255.255.255.255

```
## R2
```
enable
config t
router ospf 1
router-id 2.2.2.2
network 192.168.12.2 0.0.0.0 area 0
network 192.168.23.2 0.0.0.0 area 0
network 2.2.2.2 area 0
passive-interface loopback 2
exit
!# Prefix list para que R2 no vea el loopback de R3
ip prefix-list FILTRO-1 deny 3.3.3.3/32
ip prefix-list FILTRO-1 permit 0.0.0.0 /0 le 32
router ospf 1
distribute-list prefix FILTRO-1 in serial 1/0
distribute-list prefix FILTRO-1 in serial 1/2
exit
!# Nota, aqui en vez de la loopback, debido a que esta bloqueada con el filtro de prefix-list, pone el salto de ip al siguiente router.
router bgp 65001
bgp router-id 2.2.2.2
neighbor 1.1.1.1 remote-as 65001
neighbor 1.1.1.1 update-source loopback 2
neighbor 192.168.23.3 remote-as 65001
network 2.2.2.2 mask 255.255.255.255
exit
```
## R3
```
enable
config t
int range e0/0-1
ip address dhcp
no shut
exit
router ospf 1
router-id 3.3.3.3
network 3.3.3.3 0.0.0.0 area 0
network 192.168.13.3 0.0.0.0 area 0
network 192.168.23.3 0.0.0.0 area 0
passive-interface loopback 3
default-information originate
exit
ip route 0.0.0.0 0.0.0.0 209.50.0.1
ip route 0.0.0.0 0.0.0.0 219.50.0.1
exit
router bgp 65001
bgp router-id 3.3.3.3
neighbor 1.1.1.1 remote-as 65001
neighbor 1.1.1.1 update-source loopback 3
neighbor 192.168.23.2 remote-as 65001
neighbor 209.50.0.1 remote-as 400
neighbor 219.50.0.1 remote-as 500
network 3.3.3.3 mask 255.255.255.255
neighbor 1.1.1.1 next-hop-self
neighbor 192.168.23.2 next-hop-self
exit
route-map ATRIBUTO-1 permit
set as-path prepend 65001 65001
exit
router bgp 65001
neighbor 219.50.0.1 route-map ATRIBUTO-1 in
do clear ip bgp * soft
exit
ip prefix-list RUTAS-EMPRESA permit 192.168.10.0/24
ip prefix-list RUTAS-EMPRESA permit 192.168.13.0/24
ip prefix-list RUTAS-EMPRESA permit 192.168.12.0/24
ip prefix-list RUTAS-EMPRESA permit 192.168.23.0/24
ip prefix-list RUTAS-EMPRESA permit 1.1.1.1/32
ip prefix-list RUTAS-EMPRESA permit 2.2.2.2/32
ip prefix-list RUTAS-EMPRESA permit 6.6.6.6/32
ip prefix-list RUTAS-EMPRESA permit 7.7.7.7/32
ip prefix-list RUTAS-EMPRESA permit 172.16.67.0/24
ip prefix-list RUTAS-EMPRESA permit 172.16.68.0/24
ip prefix-list RUTAS-EMPRESA permit 172.16.78.0/24
ip prefix-list RUTAS-EMPRESA permit 172.16.80.0/24
route-map BGP permit
match ip address prefix-list RUTAS-EMPRESA
set metric 100
exit
router bgp 65001
redistribute ospf 1 route-map BGP
exit
ip prefix-list RUTAS-BGP permit 40.40.40.32.0/19
ip prefix-list RUTAS-BGP permit 50.50.0.0/17
ip prefix-list RUTAS-BGP permit 209.50.0.0/30
ip prefix-list RUTAS-BGP permit 209.60.0.0/30
ip prefix-list RUTAS-BGP permit 219.50.0.0/30
ip prefix-list RUTAS-BGP permit 219.60.0.0/30
route-map OSPF permit
match ip address prefix-list RUTAS-BGP
set metric 500
set metric-type type-1
exit
router ospf 1
redistribute bgp 65001 subnets route-map OSPF
exit
```
## R6
```
enable
config t
int range e0/0,e0/3
ip address dhcp
no shut
exit
router osfp 1
router-id 6.6.6.6
network 6.6.6.6 0.0.0.0 area 0
network 172.16.68.6 0.0.0.0 area 0
network 172.16.67.6 0.0.0.0 area 0
passive-interface loopback 6
default-information originate
exit
ip route 0.0.0.0 0.0.0.0 209.60.0.1
ip route 0.0.0.0 0.0.0.0 219.60.0.1
router bgp 65002
bgp router-id 6.6.6.6
neighbor 209.60.0.1 remote-as 400
neighbor 219.60.0.1 remote-as 500
neighbor 7.7.7.7 remote-as 65002
neighbor 7.7.7.7 update-source loopback 6
neighbor 8.8.8.8 remote-as 65002
neighbor 8.8.8.8 update-source loopback 6
network 6.6.6.6 mask 255.255.255.255
neighbor 7.7.7.7 next-hop-self
neighbor 8.8.8.8 next-hop-self
exit
route-map ATRIBUTO-1 permit
set as-path prepend 65002 65002
exit
router bgp 65002
neighbor 219.60.0.1 route-map ATRIBUTO-1 in
do clear ip bgp * soft
ip prefix-list RUTAS-EMPRESA permit 192.168.10.0/24
ip prefix-list RUTAS-EMPRESA permit 192.168.13.0/24
ip prefix-list RUTAS-EMPRESA permit 192.168.12.0/24
ip prefix-list RUTAS-EMPRESA permit 192.168.23.0/24
ip prefix-list RUTAS-EMPRESA permit 1.1.1.1/32
ip prefix-list RUTAS-EMPRESA permit 2.2.2.2/32
ip prefix-list RUTAS-EMPRESA permit 6.6.6.6/32
ip prefix-list RUTAS-EMPRESA permit 7.7.7.7/32
ip prefix-list RUTAS-EMPRESA permit 172.16.67.0/24
ip prefix-list RUTAS-EMPRESA permit 172.16.68.0/24
ip prefix-list RUTAS-EMPRESA permit 172.16.78.0/24
ip prefix-list RUTAS-EMPRESA permit 172.16.80.0/24
route-map BGP permit
match ip address prefix-list RUTAS-EMPRESA
set metric 100
exit
router bgp 65002
redistribute ospf 1 route-map BGP
exit
ip prefix-list RUTAS-BGP permit 40.40.40.32.0/19
ip prefix-list RUTAS-BGP permit 50.50.0.0/17
ip prefix-list RUTAS-BGP permit 209.50.0.0/30
ip prefix-list RUTAS-BGP permit 209.60.0.0/30
ip prefix-list RUTAS-BGP permit 219.50.0.0/30
ip prefix-list RUTAS-BGP permit 219.60.0.0/30
route-map OSPF permit
match ip address prefix-list RUTAS-BGP
set metric 500
set metric-type type-1
exit
router ospf 1
redistribute bgp 65002 subnets route-map OSPF
exit
```
## R7
```
enable
config t
int serial 1/2
ip address 172.16.78.7 255.255.255.0
exit
router ospf 1
router-id 7.7.7.7
network 7.7.7.7 0.0.0.0 area 0
network 172.16.78.7 0.0.0.0 area 0
network 172.16.67.7 0.0.0.0 area 0
passive-interface loopback 7
exit
ip prefix-list FILTRO-2 deny 8.8.8.8/32
ip prefix.list FILTRO-2 permit 0.0.0.0/0 le 32
router ospf 1
distribute-list prefix FILTRO-2 in seria 1/1
distribute-list prefix FILTRO-2 in seria 1/2
exit
router bgp 65002
bgp router-id 7.7.7.7
neighbor 6.6.6.6 remote-as 65002
neighbor 6.6.6.6 update-source loopback 7
neighbor 172.16.78.8 remote-as 65002
network 7.7.7.7 mask 255.255.255.255
```
## R8
```
enable
config t
int s1/2
ip address 172.16.78.8 255.255.255.0
exit
int loopback 8
ip address 8.8.8.8 255.255.255.255
exit
router ospf 1
router-id 8.8.8.8
network 8.8.8.8 0.0.0.0 area 0
network 172.16.80.8 0.0.0.0 area 0
network 172.16.68.8 0.0.0.0 area 0
network 172.16.78.8 0.0.0.0 area 0
passive-interface loopback 8
passive-interface e0/0
exit
router bgp 65002
bgp router-id 8.8.8.8
neighbor 6.6.6.6 remote-as 65002
neighbor 6.6.6.6 update-source loopback 8
neighbor 172.16.78.7 remote-as 65002
network 8.8.8.8 mask 255.255.255.255
network 172.16.80.0 mask 255.255.255.0

```
## ISP-A
```
enable
config t
!# DHCP
ip dhcp pool R3
network 209.50.0.0 255.255.255.252
default-router 209.50.0.1
exit
ip dhcp pool R6
network 209.60.0.0 255.255.255.252
default-router 209.60.0.1
exit
!# BGP
router bgp 400
bgp router-id 10.10.10.10
neighbor 209.50.0.2 remote-as 65001
neighbot 209.60.0.2 remote-as 65001
neighbor 222.0.0.1 remote-as 500
network 40.40.40.0 mask 255.255.255.0
network 40.40.50.0 mask 255.255.255.0
exit
route-map ATRIBUTO-2 permit
set metric 50
neighbor 222.0.0.1 route-map ATRIBUTO-2 out
do clear ip bgp * soft
aggregate-address 40.40.32.0 255.255.224.0 summary-only
```
## ISP-B
```
enable
config t
ip dhcp pool R3
network 219.50.0.0 255.255.255.252
default-router 219.50.0.1
exit
ip dhcp pool R6
network 219.60.0.0 255.255.255.252
default-router 219.60.0.1
exit
router bgp 500
bgp router-id 20.20.20.20
neighbor 219.50.0.2 remote-as 65001
neighbor 209.50.0.2 remote-as 65002
neighbor 222.0.0.2 remote-as 400
network 50.50.50.0 mask 255.255.255.0
network 50.50.80.0 mask 255.255.255.0
aggregate-address 50.50.0.0 255.255.128.0 summary-only
```
