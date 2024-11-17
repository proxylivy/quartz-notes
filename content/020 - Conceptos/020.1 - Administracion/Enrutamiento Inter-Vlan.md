## Que es??
Diferentes dispositivos en diferentes vlan no se pueden comunicar entre si, en la capa 3, las [[010 - Protocolos/010.2 - Switching/VLAN|VLAN]] se definen en subredes IP, para conectar un switch y router externo es con un enlace troncal para que transporte todas las vlans.

Se puede hacer:
- Switch Multicapa
- Router Externo con enlace troncal (router-on-a-stick)
- Router Externo que pueda separar por interfaz cada VLAN

## Router-on-a-stick
Ventajas:
- Es una solucion factible
- La implementacion es simple, solo necesita una interfaz en un router conectada a un switch
- El flujo de trafico es simple, un solo lugar interconecta VLAN

Desventajas:
- El router es un unico punto de falla
- Solo una ruta puede congestionarse, con router-on-a-stick esta limitado al ancho de banda y se comparte, depende del tamaño de la red, cantidad de trafico VLAN y velocidad de enlaces.
- Se puede introducir latencia, debido a las tramas extras y el router toma decisiones basadas en software

## SVI
Enrutamiento a nivel de Hardware en Switch L3
### Ventajas
- Es mas rapido que router-on-a-stick, porque Hardware interconecta y enruta
- No hay necesidad de configurar un router externo
- No se limita a un enlace, se puede configurar Etherchannel de capa 2 y conseguir mayor ancho de banda
- La latencia es menor

Desventajas
- Son mas caros

## Configuracion
### Sub-Int Router
```
int [int S/S/P]
no shut
exit
int [int S/S/P.SV]
encapsulation dot1q [.Sub]
ip address [ipv4-host] [Dec-Mask]
ipv6 address [ipv6-host/Prefix]
exit
ipv6 unicast-routing
```

### SVI (Switch Virtual Interface)
```
SW1# conf t
SW1 conf# interface vlan [vlan-id]
SW1 conf-if# ip address [ipv4] [Submask]
SW1 conf-if# ipv6 address [ipv6]
SW1 conf-if# no shut
```

### Switch Capa 3
```
conf t
int [int S/S/P]
no switchport
ip address [ipv4] [Submask]
ipv6 address [ipv6]
no shut
```
