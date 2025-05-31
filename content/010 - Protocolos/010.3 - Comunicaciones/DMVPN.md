# Info
**D**ynamic **M**ulti-Point **V**irtual **P**rivate **N**etwork, no es un protocolo sino una tecnica para crear VPN de forma dinamica

Estas se logra usando 3 tecnologias
- [[010 - Protocolos/010.1 - Routing/Tunel GRE|Tunel GRE]] Mejorado -> mGRE: Permite conexiones Multipunto, IP Unicast, Multicast y Broadcast
- [[010 - Protocolos/010.1 - Routing/IPSEC|IPSEC]]
- NHRP: Configuracion Hub/Spoke, el cual es un servidor para que los Spoke puedan encontrar rutas a multiples Spoke bajo demanda

## Fases
Fase 1: Se configura NHRP y mGRE para crear la topologia Spoke-to-Hub de DMVPN desde el HUB

Fase 2: Se configura mGRE para los Spoke

Fase 3: Se establece la coneccion NHRP entre spoke

# Configuracion
## Hub
### Configuracion Basica
> Revisa que tenga conexion hacia los Spoke
```
hostname [hostname-name]
no ip domain lookup
int [S/S/P]
 ip address [ip] [dec-mask]
 exit
```

### Configura el Tunnel
> - `network-id` es el mismo en HUB y SPOKE
```
int tunnel [tunnel-number]
 ip address [ip-tunnel] [dec-mask]
 tunnel source [int S/S/P] 
 tunnel mode gre multipoint
 ip nhrp network-id 1
 ip nhrp map multicast dynamic
```

### Implementa Protocolo IGP
> Se configura la IP de Loopback y del tunnel
```
router eigrp [AS]
 network [ip-network] [dec-mask]
!
!# Desactivar Regla del Horizonte Dividido
!
int tunnel [tunnel-number]
 no ip split-horizon eigrp [AS]
```

### Configurar IPSEC
```
crypto isakmp policy [policy-number]
 encryption [encryption-type]
 group [DH-number]
 lifetime [life-number]
 exit
crypto isakmp key [password] address 0.0.0.0
crypto ipsec transform-set [set-name] [encryption-type2] [encryption-type3]
 mode transport
 exit
crypto ipsec profile [profile-name]
 set transform-set [set-name]
 exit
int tunnel [tunnel-number]
 tunnel protection ipsec profile [profile-name]
 exit
```

## Spoke
### Configuracion Basica
> Revisa que tenga conexion hacia el HUB
```
hostname [hostname-name]
no ip domain lookup
int [S/S/P]
 ip address [ip] [dec-mask]
 no shut
 exit
int loopback [lookback-number]
 ip address [ip] [dec-mask]
 no shut
 exit
```

### Configurar Tunnel
> `network-id` es el mismo en HUB Y SPOKE
```
int tunnel [tunnel-number]
 ip address [ip-tunnel] [dec-mask]
 tunnel source [int S/S/P]
 tunnel mode gre multipoint
 ip nhrp network-id 1
 ip ngrp nhs [ip-tunnel-hub]
 ip nhrp map [ip-tunnel-hub] [ip-public-hub]
 ip nhrp map multicast [ip-public-hub]
```

### Implementa Protocolo IGP
> Se configura la IP de Loopback y la del tunnel
```
router eigrp [AS]
network [ip-network] [dec-mask]
```

### Configurar IPSEC
```
crypto isakmp policy [policy-number]
 encryption [encryption-type]
 group [DH-number]
 lifetime [life-number]
 exit
crypto isakmp key [password] address 0.0.0.0
crypto ipsec transform-set [set-name] [encryption-type2] [encryption-type3]
 mode transport
 exit
crypto ipsec profile [profile-name]
 set transform-set [set-name]
 exit
int tunnel [tunnel-number]
 tunnel protection ipsec profile [profile-name]
 exit
```

# Visualizacion
- `sh int tunnel [tunnel-number]`: Configuracion Tunnel
- `sh dmvpn`
- `sh dmvpn detail`: Estado de enlace DMVPN
- `sh ip route`: 
- `sh ip nhrp detail`: Detalles NHRP
- `sh crypto ipsec sa`

# Extra
- NHRP Definido en [RFC2332](https://datatracker.ietf.org/doc/html/rfc2332)

Se puede ajustar las interfaces de forma fina dentro de tunel
```
int tunnel [tunnel-number]
 ip nhrp shortcut
 bandwidth 4000
 ip mtu 1400
 ip tcp adjust-mss 1360
 exit
```

Se puede autenticar el tunnel
```
int tunnel [tunnel-number]
 tunnel key [key-number]
 ip nhrp authentication [password]
```