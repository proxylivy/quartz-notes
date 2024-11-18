**V**irtual **R**outing and **F**orwarding es una tecnologia que permite la coexistencia de multiples instancias de tablas de enrutamiento en un mismo router, lo que permite manetener separadas las rutas de diferentes clientes o redes

`VRF-Lite` es la implementacion de VRF sin la necesidad de [[999 - Archivado/MPLS|MPLS]], Se utiliza para crear instancias de enrutamiento separadas en [[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]] sin la complejidad de MPLS, permitiendo la segmentacion de redes en entornos empresariales o para soportar multiples conexiones [[010 - Protocolos/010.3 - Comunicaciones/VPN|VPN]]

# Sintaxis Configuracion
## VRF-Lite
```
ip vrf [vrf-name]
 exit
int [S/S/P]
 ip vrf forwarding [vrf-name]
 ip address [ip-host] [dec-mask]
 clock rate [rate-speed]
 no shut
 exit
```

# Ejemplos Configuracion
## VRF-LITE
```
ip vrf VRF-A
 exit
ip vrf VRF-B
 exit
int s0/0/0
 ip vrf forwarding VRF-A
 ip address 10.10.1.1 255.255.255.252
 clock rate 2000000
 no shut
 exit
int s0/0/1
 ip vrf forwarding VRF-A
 ip address 10.20.2.1 255.255.255.252
 no shut
 exit
int s0/1/0
 ip vrf forwarding VRF-B
 ip address 10.30.3.1 255.255.255.252
 clock rate 2000000
 no shut
```

# Visualizar
- `show ip route | b Gateway`: Ver tabla de enrutamiento global
- `show ip route vrf [vrf-name] | b Gateway`: Rutas de VRF
- `show ip vrf int`: Ver interfaces asociadas con VRF
- `ping vrf [vrf-name] [ip-host]`: Hace ping desde el contexto de una VRF


# Troubleshooting
El orden de creacion es muy importante, primero se crea la regla VRF, luego se aplica a la interfaz, y despues va la ip, configurar la ip primero hara que exista fuera del dominio de VRF