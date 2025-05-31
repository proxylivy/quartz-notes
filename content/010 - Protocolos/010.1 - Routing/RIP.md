# Info
## Datos

Protocolo de [[020 - Conceptos/020.3 - Fundamentos/Modelo OSI#Capa 3|Capa 3]] que funciona en UDP y sin mensajes de configuracion, es de Vector Distancia al igual que [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP|EIGRP]].
RIP no se configura sin `version 2` debido a que es inseguro.

IPv4:
- RIP o RIPv2
IPv6:
- RIPng

Tiene un maximo de metrica de `15`

# Configuracion
## RIPv2
Nota: Las pasivas son las que no se mandan nada, conexiones a equipos finales (como sub-interfaces vlan) y loopback
```
router rip
version 2
no auto-summary
network [ip-network] [dec-mask]
passive-interface [int S/S/P]
```
## RIPng
```
ipv6 unicast-routing
ipv6 router rip [rip-name]
!
int range [int S/S/P]
ipv6 rip [rip-name] enable
```
# Visualizacion
`show ip protocols` -> Muestra el funcionamiento de RIPv2
`show ip route rip` -> Ver tabla de enrutamiento RIP

# Extra
`no ip domain-lookup` -> Desactiva la busqueda de dominio