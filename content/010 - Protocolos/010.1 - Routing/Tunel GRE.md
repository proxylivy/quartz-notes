# Qué es?
GRE (Generic Routing Encapsulation) es un protocolo de tunneling definido por el [RFC1702](https://www.rfc-editor.org/rfc/rfc1702) y [RFC2784](https://www.rfc-editor.org/rfc/rfc2784), fue desarrolado originalmente por Cisco, soporta encapsulacion de multiples protocolos dentro de un tunel viajando a travez de [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv4|IPv4]]. Para su funcionamiento se agrega un encabezado GRE al paquete y al tunel. Soporta tunel IP multicast. los tuneles GRE son sin estado por lo que no mantienen informacion de disponibilidad del extremo del tunel.
Protocolo 47 (ID para Sniffer).
Deben tener conectividad completa, incluso las loopback deben verse con algun protocolo de enrutamiento
# Configuracion
## Tunel GRE 4 over 4
> Nota: Sin `keepalive` se genera un agujero negro de enrutamiento, una especie de loop
```
int tunnel [tunnel-number]
ip address [ip-source] [dec-mask]
tunnel source [int S/S/P]
tunnel destination [ip-destination]
tunnel mode gre ip
keepalive [keepalive-number]
no shut
```
## Tunel GRE 6 over 4
```
int tunnel [tunnel]
no ip address
ipv6 address [ipv6-source/prefix]
tunnel source [int S/S/P]
tunnel destination [ip-destination]
tunnel mode ipv6ip
keepalive [keepalive-number]
no shut
```

# Visualizacion
`show int tunnel [tunnel-number]` 
`show ip int brief`
`show ipv6 int brief`
`show run | section interface Tunnel`