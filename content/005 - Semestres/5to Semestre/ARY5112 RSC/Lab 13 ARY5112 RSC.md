# Info
## Imagen Topologia
![500](https://slink.proxylivy.work/image/5e636488-cf9b-4f79-8368-38ba985ea960.png)
## Sobre
Implementacion de una Red con Alta Disponibilidada
## Requerimientos
- Crear las VLANs en todos los switches de la topología.
- Asociar las interfaces que están en uso a la VLAN correspondiente. Interfaces sobrantes deberán estar apagadas.
- Implementar seguridad de puerto dinámica.
- Implementar agregado de enlaces, según protocolo propuesto. Un extremo debe negociar y otro escuchar.
- En las interfaces lógicas resultantes, deberá permitir el paso solo de las VLANs de Datos y desactivar la negociación automática.
- Implementar PVST+ Rápido en todos los dispositivos de capa 2 de la red.
- En las interfaces de acceso implementar mecanismos de estabilización de STP.
- Administrar los switches mediante VLAN de Administración, según direccionamiento propuesto.
- Realizar enrutamiento intervlan en equipos adecuados.
- Implementar HSRPv2 en todos los equipos, donde el tráfico IPv4 deberá preferir MLS2 y el tráfico IPv6 deberá preferir MLS3. Utilizar parámetros a elección.
- Modificar los temporizadores de HSRP a elección.
- Implementar IP SLA a nivel IPv4, donde deberá monitorear la IP 8.8.8.8, en caso de caída todo el tráfico IPv4, HSRP deberá utilizar equipo de respaldo.
- Implementar EIGRP según AS solicitado. Procure configurar interfaces pasivas.
- Sumarice las rutas en equipos MPLS para que sean enviados a R1 a nivel IPv4/IPv6.
- Router R1 deberá proveer de direccionamiento dinámico a todos los equipos finales.
- Permitir salida hacia Internet a nivel IPv4/IPv6.
- Implementar MP-BGP a nivel IPv4/IPv6. Las loopbacks deben actuar como prefijos dentro del protocolo de enrutamiento.
- Redistribuir protocolo IGP y EGP. A nivel IPv4 será mediante mapas de rutas y para IPv6 de manera directa.
- Comprobar conectividad completa en la topología.
## Tabla Sumarizacion 
IP-network 1: `10.0.21.0/24`
IP-network 2: `10.0.31.0/24`
IP-network 3: `10.0.41.0/24`
IP-network 4: `10.0.51.0/24`

| 1er <br>Octeto | 2do <br>Octeto | 128 | 64  | Corte | 32  | 16  | 8   | 4   | 2   | 1   | 4to <br>Octeto |
| -------------- | -------------- | --- | --- | ----- | --- | --- | --- | --- | --- | --- | -------------- |
| 10             | 0              | 0   | 0   | /     | 0   | 1   | 0   | 1   | 0   | 1   | 0              |
| 10             | 0              | 0   | 0   | /     | 0   | 1   | 1   | 1   | 1   | 1   | 0              |
| 10             | 0              | 0   | 0   | /     | 1   | 0   | 1   | 0   | 0   | 0   | 0              |
| 10             | 0              | 0   | 0   | /     | 1   | 1   | 0   | 0   | 1   | 1   | 0              |

IP Sumarizada: `10.0.0.0/18`
## IPV6
0-9

| A   | 10  |
| --- | --- |
| B   | 11  |
| C   | 12  |
| D   | 13  |
| E   | 14  |
| F   | 15  |

| Network IPv6          | Diferencia | 8   | 4   | 2   | 1   | \|  | 8   | 4   | 2   | 1   | \|  | 8   | 4   | 2   | 1   | \|  | 8   | Corte | 4   | 2   | 1   | \|  |
| --------------------- | ---------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | ----- | --- | --- | --- | --- |
| 3001:ABCD:ABCD:A::/64 | 000A       | 0   | 0   | 0   | 0   | \|  | 0   | 0   | 0   | 0   | \|  | 0   | 0   | 0   | 0   | \|  | 1   | /     | 0   | 1   | 0   | \|  |
| 3001:ABCD:ABCD:B::/64 | 000B       | 0   | 0   | 0   | 0   | \|  | 0   | 0   | 0   | 0   | \|  | 0   | 0   | 0   | 0   | \|  | 1   | /     | 0   | 1   | 1   | \|  |
| 3001:ABCD:ABCD:C::/64 | 000C       | 0   | 0   | 0   | 0   | \|  | 0   | 0   | 0   | 0   | \|  | 0   | 0   | 0   | 0   | \|  | 1   | /     | 1   | 0   | 0   | \|  |
| 3001:ABCD:ABCD:D::/64 | 000D       | 0   | 0   | 0   | 0   | \|  | 0   | 0   | 0   | 0   | \|  | 0   | 0   | 0   | 0   | \|  | 1   | /     | 1   | 0   | 1   | \|  |

La red Sumarizada es: `3001:ABCD:ABCD:8::/61`
# Configuracion
## SWC
```
enable
config t
int range e0/2-3
duplex half
exit
vlan 21-25
vtp mode transparent
int range e0/2-3
switchport mode access
switchport port-security
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int e0/2
switchport access vlan 21
exit
int e0/3
switchport access vlan 22
exit
int range e0/0-1,e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 21-25
switchport nonegotiate
exit
int range e1/0-1
channel-group 1 mode desirable
exit
int range e0/0-1
channel-group 4 mode auto
exit
int range e1/2-3
channel-group 5 mode on
exit
int vlan 25
ip address 10.0.61.14 255.255.255.0
no shut
exit
ip default-gateway 10.0.61.1
end
copy running-config unix:
```
## SWD
```
enable
config t
int range e0/2-3
duplex half
exit
vlan 21-25
vtp mode transparent
int range e0/2-3
switchport mode access
switchport port-security
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
spanning-tree portfast
exit
int e0/2
switchport access vlan 23
exit
int e0/3
switchport access vlan 24
exit
int range e0/0-1,e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 21-25
switchport nonegotiate
exit
int range e1/0-1
channel-group 1 mode auto
exit
int range e0/0-1
channel-group 2 mode passive
exit
int range e1/2-3
channel-group 3 mode on
exit
int vlan 25
ip address 10.0.61.28 255.255.255.0
no shut
exit
ip default-gateway 10.0.61.1
end
copy running-config unix:
```
## SWB
```
enable
config t
vlan 21-25
vtp mode transparent
int range e0/0-1,e0/3,e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 21-25
switchport nonegotiate
exit
int range e1/2-3
channel-group 3 mode on
exit
int range e0/0-1
channel-group 4 mode desirable
exit
int range e1/0-1
channel-group 6 mode passive
exit
int vlan 25
ip address 10.0.61.21 255.255.255.0
no shut
exit
ip default-gateway 10.0.61.1
end
copy running-config unix:
```
## SWA
```
enable
config t
vlan 21-25
vtp mode transparent
int range e0/0-1,e0/3,e1/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 21-25
switchport nonegotiate
exit
int range e0/0-1
channel-group 2 mode active
exit
int range e1/2-3
channel-group 5 mode on
exit
int range e1/0-1
channel-group 6 mode active
exit
int vlan 25
ip address 10.0.61.7 255.255.255.0
no shut
exit
ip default-gateway 10.0.61.1
end
copy running-config unix:
```
## MLS2
```
enable
config t
ip routing
ipv6 unicast-routing
int e0/1
duplex half
exit
vlan 21-25
vtp mode transparent
int e0/3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 21-25
switchport nonegotiate
interface vlan 21
ip address 10.0.21.2 255.255.255.0
ipv6 address 3001:ABCD:ABCD:A::2/64
no shut
exit
interface vlan 22
ip address 10.0.31.2 255.255.255.0
ipv6 address 3001:ABCD:ABCD:B::2/64
no shut
exit
interface vlan 23
ip address 10.0.41.2 255.255.255.0
ipv6 address 3001:ABCD:ABCD:C::2/64
no shut
exit
interface vlan 24
ip address 10.0.51.2 255.255.255.0
ipv6 address 3001:ABCD:ABCD:D::2/64
no shut
exit
int vlan 21
standby version 2
standby 21 ip 10.0.21.1
standby 21 priority 120
standby 21 preempt
standby 21 timers msec 200 msec 800
standby 211 ipv6 3001:ABCD:ABCD:A::1/64
standby 211 priority 90
standby 211 preempt
standby 211 timers msec 200 msec 800
exit
int vlan 22
standby version 2
standby 22 ip 10.0.31.1
standby 22 priority 120
standby 22 preempt
standby 22 timers msec 200 msec 800
standby 221 ipv6 3001:ABCD:ABCD:B::1/64
standby 221 priority 90
standby 221 preempt
standby 221 timers msec 200 msec 800
exit
int vlan 23
standby version 2
standby 23 ip 10.0.41.1
standby 23 priority 120
standby 23 preempt
standby 23 timers msec 200 msec 800
standby 231 ipv6 3001:ABCD:ABCD:C::1/64
standby 231 priority 90
standby 231 preempt
standby 231 timers msec 200 msec 800
exit
int vlan 24
standby version 2
standby 24 ip 10.0.51.1
standby 24 priority 120
standby 24 preempt
standby 24 timers msec 200 msec 800
standby 241 ipv6 3001:ABCD:ABCD:D::1/64
standby 241 priority 90
standby 241 preempt
standby 241 timers msec 200 msec 800
exit
ip sla 1
icmp-echo 8.8.8.8
frecuency 5
ip sla schedule 1 start-time now life forever
track 1 ip sla 1 reachability
delay down 5 up 3
exit
int vlan 21
standby 21 track 1 decrement 31
exit
int vlan 22
standby 22 track 1 decrement 31
exit
int vlan 23
standby 23 track 1 decrement 31
exit
int vlan 24
standby 24 track 1 decrement 31
exit
router eigrp 123
network 10.0.21.2 0.0.0.0
network 10.0.31.2 0.0.0.0
network 10.0.31.2 0.0.0.0
network 10.0.51.2 0.0.0.0
network 192.168.12.2 0.0.0.0
passive-interface vlan 21
passive-interface vlan 22
passive-interface vlan 23
passive-interface vlan 24
exit
int e0/1
no switchport
ip address 192.168.12.2 255.255.255.0
ipv6 address 3001:ABCD:ABCD:12::2/64
exit
ipv6 router eigrp 123
passive-interface vlan 21
passive-interface vlan 22
passive-interface vlan 23
passive-interface vlan 24
exit
int vlan 21
ipv6 eigrp 123
exit
int vlan 22
ipv6 eigrp 123
exit
int vlan 23
ipv6 eigrp 123
exit
int vlan 24
ipv6 eigrp 123
exit
int e0/1
ipv6 eigrp 123
exit
int e0/1
ip summary-address eigrp 123 10.0.0.0 255.255.192.0
ipv6 summary-address eigrp 123 3001:ABCD:ABCD:8::/61
exit
int vlan 21
ip helper-address 192.168.12.1
exit
int vlan 22
ip helper-address 192.168.12.1
exit
int vlan 23
ip helper-address 192.168.12.1
exit
int vlan 24
ip helper-address 192.168.12.1
exit
end
copy running-config unix:
```
## MLS3
```
enable
config t
ip routing
ipv6 unicast-routing
int e0/2
duplex half
vlan 21-25
vtp mode transparent
int e0/3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 21-25
switchport nonegotiate
exit
interface vlan 21
ip address 10.0.21.3 255.255.255.0
ipv6 address 3001:ABCD:ABCD:A::3/64
no shut
exit
interface vlan 22
ip address 10.0.31.3 255.255.255.0
ipv6 address 3001:ABCD:ABCD:B::3/64
no shut
exit
interface vlan 23
ip address 10.0.41.3 255.255.255.0
ipv6 address 3001:ABCD:ABCD:C::3/64
no shut
exit
interface vlan 24
ip address 10.0.51.3 255.255.255.0
ipv6 address 3001:ABCD:ABCD:D::3/64
no shut
exit
int vlan 21
standby version 2
standby 21 ip 10.0.21.1
standby 21 priority 90
standby 21 preempt
standby 21 timers msec 200 msec 800
standby 211 ipv6 3001:ABCD:ABCD:A::1/64
standby 211 priority 120
standby 211 preempt
standby 211 timers msec 200 msec 800
exit
int vlan 22
standby version 2
standby 22 ip 10.0.31.1
standby 22 priority 90
standby 22 preempt
standby 22 timers msec 200 msec 800
standby 221 ipv6 3001:ABCD:ABCD:B::1/64
standby 221 priority 120
standby 221 preempt
standby 221 timers msec 200 msec 800
exit
int vlan 23
standby version 2
standby 23 ip 10.0.41.1
standby 23 priority 90
standby 23 preempt
standby 23 timers msec 200 msec 800
standby 231 ipv6 3001:ABCD:ABCD:C::1/64
standby 231 priority 120
standby 231 preempt
standby 231 timers msec 200 msec 800
exit
int vlan 24
standby version 2
standby 24 ip 10.0.51.1
standby 24 priority 90
standby 24 preempt
standby 24 timers msec 200 msec 800
standby 241 ipv6 3001:ABCD:ABCD:D::1/64
standby 241 priority 120
standby 241 preempt
standby 241 timers msec 200 msec 800
exit
router eigrp 123
network 10.0.21.3 0.0.0.0
network 10.0.31.3 0.0.0.0
network 10.0.41.3 0.0.0.0
network 10.0.51.3 0.0.0.0
network 192.168.13.3 0.0.0.0
passive-interface vlan 21
passive-interface vlan 22
passive-interface vlan 23
passive-interface vlan 24
exit
int e0/2
no switchport
ip address 192.168.113.3 255.255.255.0
ipv6 address 3001:ABCD:ABCD:13::3/64
exit
ipv6 router eigrp 123
passive-interface vlan 21
passive-interface vlan 22
passive-interface vlan 23
passive-interface vlan 24
exit
int vlan 21
ipv6 eigrp 123
exit
int vlan 22
ipv6 eigrp 123
exit
int vlan 23
ipv6 eigrp 123
exit
int vlan 24
ipv6 eigrp 123
exit
int e0/2
ipv6 eigrp 123
exit
int e0/2
ip summary-address eigrp 123 10.0.0.0 255.255.192.0
ipv6 summary-address eigrp 123 3001:ABCD:ABCD:8::/61
exit
int vlan 21
ip helper-address 192.168.13.1
exit
int vlan 22
ip helper-address 192.168.13.1
exit
int vlan 23
ip helper-address 192.168.13.1
exit
int vlan 24
ip helper-address 192.168.13.1
exit
end
copy running-config unix:
```
## R1
```
enable
config t
ipv6 unicast-routing
router eigrp 123
network 192.168.12.1 0.0.0.0
network 192.168.13.1 0.0.0.0
exit
ipv6 router eigrp 123
exit
int range e0/1-2
ipv6 eigrp 123
exit
ip dhcp excluded-address 10.0.21.1 10.0.21.4
ip dhcp excluded-address 10.0.31.1 10.0.31.4
ip dhcp excluded-address 10.0.41.1 10.0.41.4
ip dhcp excluded-address 10.0.51.1 10.0.51.4
ip dhcp pool VLAN21
network 10.0.21.0 255.255.255.0
default-router 10.0.21.1
exit
ip dhcp pool VLAN22
network 10.0.31.0 255.255.255.0
default-router 10.0.31.1
exit
ip dhcp pool VLAN23
network 10.0.41.0 255.255.255.0
default-router 10.0.41.1
exit
ip dhcp pool VLAN24
network 10.0.51.0 255.255.255.0
default-router 10.0.51.1
exit
ip route 0.0.0.0 0.0.0.0 209.50.0.2
ipv6 route ::/0 3001:ABCD:ABCD:209::2
router eigrp 123
redistribute static
exit
ipv6 router eigrp 123
redistribute static
exit
router bgp 65000
address-family ipv4
neighbor 209.50.0.2 remote-as 100
exit
address-family ipv6
neighbor 3001:ABCD:ABCD:209::2 remote-as 100
exit
ipv6 router eigrp 123
redistribute bgp 65000 metric 10000 100 255 1 1500
exit
router bgp 65000
address-family ipv6
redistribute eigrp 123 metric 20
exit
!# Prefix List EIGRP -> BGP
ip prefix-list RUTA-EMPRESA permit 10.0.0.0/18
ip prefix-list RUTA-EMPRESA permit 192.168.12.0/24
ip prefix-list RUTA-EMPRESA permit 192.168.13.0/24
route-map BGP permit
match ip address prefix-list RUTA-EMPRESA
set metric 50
exit
router bgp 65000
address-family ipv4
redistribute eigrp 123 route-map BGP
exit
!# Prefix List BGP -> EIGRP
ip prefix-list RUTAS-BGP permit 209.50.0.0/30
ip prefix-list RUTAS-BGP permit 219.50.0.0/30
ip prefix-list RUTAS-BGP permit 8.8.8.8/32
ip prefix-list RUTAS-BGP permit 10.0.71.1/32
route-map EIGRP permit
match ip address prefix-list RUTAS-BGP
set metric 10000 100 255 1 1500
exit
router eigrp 123
redistribute bgp 65000 route-map EIGRP
exit
end
copy running-config unix:
```
## ISP
```
enable
config t
ipv6 unicast-routing
!# Se chispoteo
int e0/3
no ipv6 address
ipv6 address 3001:ABCD:ABCD:209::2/112
int e0/2
!# Fin del chispoteo
no shut
exit
router bgp 100
address-family ipv4
neighbor 209.50.0.1 remote-as 65000
neighbor 219.50.0.1 remote-as 64800
network 8.8.8.8 mask 255.255.255.255
exit
address-family ipv6
neighbor 3001:ABCD:ABCD:209:1 remote-as 65000
neighbor 3001:ABCD:ABCD:219:1 remote-as 64800
network 3001:ABCD:ABCD:8::8/128
exit
end
copy running-config unix:
```
## BR
```
enable
config t
ipv6 unicast-routing
router bgp 64800
address-family ipv4
neighbor 219.50.0.2 remote-as 100
network 10.0.71.1 mask 255.255.255.255
exit
address-family ipv6
neighbor 3001:ABCD:ABCD:219::2 remote-as 100
network 3001:ABCD:ABCD:71::1/128
exit
end
copy running-config unix:
```