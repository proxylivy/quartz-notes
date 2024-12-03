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
> - `network-id`, `authentication` es el mismo en HUB Y SPOKE
```
!
!# mGRE
!
int tunnel [tunnel-number]
 tunnel mode gre multipoint
 tunnel source [int S/S/P] 
 tunnel key [key-number]
 ip address [ip-source] [dec-mask]
!
!# NHRP
!
 ip nhrp network-id 1
 ip nhrp authentication [password]
 ip nhrp map multicast dynamic
 ip nhrp redirect
!
!# Ajustes Finos
!
 bandwidth 4000
 ip mtu 1400
 ip tcp adjust-mss 1360
 exit
```

### Asegurar IPSEC
> Vaya Vaya Vaya...
```
crypto isakmp policy 90
 hash sha384
 encryption aes 256
 group 14
 authentication pre-share
 exit
crypto isakmp key [password] address 0.0.0.0
crypto ipsec transform-set [set-name] esp-aes 256 esp-sha384-hmac
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
!
!# Tunnel GRE IP
!
int tunnel [tunnel-number]
 tunnel mode gre multipoint
 tunnel source loopback 0
 tunnel key [key-number]
 ip address [ip-tunnel] [dec-mask]
!
!# NHRP
!
 ip nhrp network-id 1
 ip nhrp authentication [password]
 ip nhrp map multicast [ip-tunnel-hub]
 ip nhrp map [ip-tunnel-hub] [ip-normal-hub]
 ip ngrp nhs [ip-tunnel-hub]
 ip nhrp shortcut
!
!# Ajustes Finos
!
 ip mtu 1400
 ip tcp adjust-mss 1360
```

# Visualizacion
- `sh int tunnel [tunnel-number]`: Configuracion Tunnel
- `sh dmvpn detail`: Estado de enlace DMVPN
- `sh ip nhrp detail`: Detalles NHRP

# Extra
- NHRP Definido en [RFC2332](https://datatracker.ietf.org/doc/html/rfc2332)