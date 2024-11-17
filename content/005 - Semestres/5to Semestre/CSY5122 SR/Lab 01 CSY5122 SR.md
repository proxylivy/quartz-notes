# Info
## Imagen Topologia
![1000](https://slink.proxylivy.work/image/e8814739-1334-421a-be07-31a6423e619d.png)
## Sobre
Reforzamiento en Seguridad en Redes
## Requerimientos
- Se encuentra direccionada la topología mediante direccionamiento IPv4/IPv6.
- Habilitar [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv2|OSPFv2]] según área propuesta.
- Deje interfaces pasivas según corresponda en la topología de red.
- Autenticar protocolo de enrutamiento OSPF | [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv2#Autenticacion|Autenticacion]]
- Implemente solución de [[010 - Protocolos/010.1 - Routing/Tunel GRE|Tunel GRE]] 6 over 4.
- Habilitar protocolo de enrutamiento [[010 - Protocolos/010.1 - Routing/RIP#RIPng|RIPng]] donde corresponda, con el nombre de proceso "NetSec"
- Debe existir conectividad mediante IPv6 desde R1 hacia PC.
- Realice alguna mejora de seguridad que permita tener una red estable.
# Configuracion
## R1
```
enable
config t
ipv6 unicast-routing
no ip domain-lookup
router ospf 1
router-id 1.1.1.1
network 192.168.12.0 255.255.255.0 area 0
exit
int s1/0
ip ospf authentication message-digest
ip ospf message-digest-key 1 md5 modoshorizo
exit
int tunnel 0
ipv6 address 3001:ACAD:ACAD:13::1/64
tunnel source s1/0
tunnel destination 192.168.23.3
tunnel mode ipv6ip
keepalive 5
exit
ipv6 router rip NetSec
exit
int range loopback 1, tunnel 0
ipv6 rip NetSec enable
exit
```
## R2
```
enable
config t
no ip domain-lookup
router ospf 1
router-id 2.2.2.2
network 192.168.23.0 255.255.255.0 area 0
network 192.168.12.0 255.255.255.0 area 0
exit
int s1/0
ip ospf authentication message-digest
ip ospf message-digest-key 1 md5 modoshorizo
exit
int s1/1
ip ospf authentication message-digest
ip ospf message-digest-key 1 md5 modoshorizo
exit

```
## R3
```
enable
config t
ipv6 unicast-routing
no ip domain-lookup
router ospf 1
router-id 3.3.3.3
network 192.168.33.0 255.255.255.0 area 0
network 192.168.23.0 255.255.255.0 area 0
passive-interface e0/0
exit
int s1/1
ip ospf authentication message-digest
ip ospf message-digest-key 1 md5 modoshorizo
exit
int tunnel 1
ipv6 address 3001:ACAD:ACAD:13::3/64
tunnel source s1/1
tunnel destination 192.168.12.1
tunnel mode ipv6ip
keepalive 5
exit
ipv6 router rip NetSec
exit
int range e0/0, tunnel 1
ipv6 rip NetSec enable
exit
```