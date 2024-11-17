# Info
## Datos
- Version 3 de [[010 - Protocolos/010.1 - Routing/BGP/BGP|BGP]]
- Soporta IPv4 solamente

# Configuracion
## iBGP
### Neighbors iBGP
Considerados 2 router en el mismo `[AS]`
> La `[self-loopback-number]` es la del mismo router
```
router bgp [BGP-ASN]
bgp router-id [R-ID]
neighbor [ip-neighbor] remote-as [BGP-ASN]
neighbor [ip-neighbor] update-source loopback [self-loopback-number]
```
### Network iBGP
> Consideradas las `[loopback]` y `[ip-network]`
```
network [ip-network] mask [dec-mask]
```
### Next-Hop-Self iBGP
> modifica el atributo "next-hop" a la direccion del router que anuncia las rutas iBGP, se configura en el router de borde que anuncia las rutas
```
router bgp [BGP-ASN]
neighbor [ip-neighbor] next-hop-self
```
### Router Reflector iBGP
> Debido a la regla del Horizonte Dividido (Split Horizon), las rtas aprendidas de un vecino iBGP no se propagan, se configura RR en el router que mas vecinos iBGP tenga directamente conectados.
```
router bgp [BGP-ASN]
neighbor [ip-neighbor] route-reflector-client
```

## EBGP
### Neighbors eBGP
> Routers en diferentes `[AS]`
```
router bgp [BGP-ASN-local]
bgp router-id [R-ID]
neighbor [ip-neighbor] remote-as [BGP-ASN-remote]
```

### MultiHop eBGP
> Router que necesita comunicarse entre otro BGP a travez de un router intermedio. `[Hop-number]` es la cantidad de routers que hay entre las conexiones + 1, con esto sube el ttl del packete BGP, si no se especifica el valor TTL en routers cisco, se configura el valor maximo (255)
```
router bgp [BGP-ASN-local]
neighbor [ip-neighbor-last-hop] remote-as [BGP-ASN-remote]
neighbor [ip-neighbor-last-hop] update-source loopback [self-loopback-number]
neighbor [ip-neighbor-last-hop] ebgp-multihop [hop-number] 
```

## Sumarizacion
### Sumarizacion de Redes con Supresion
> En [[020 - Conceptos/020.3 - Fundamentos/Sumarizacion de Redes|Sumarizacion de Redes]] Como network son con `[loopback]`, esas son las que se sumarizan
```
router bgp [BGP-ASN]
aggregate-address [ip-summarized-network] [dec-mask] summary-only
```
### Sumarizacion de Redes sin Supresion
> Se quita `summary-only` para que existan ambas rutas en el router para elegir
```
router bgp [BGP-ASN]
aggregate-address [ip-summarized-network] [dec-mask]
```
## Manipulacion Atributos de BGP
> NOTA: No se pueden mezclar atributos, solo 1 a la vez
### Atributo Weight
- Atributo iBGP
- Le da preferencia a una ruta cuando hay multiples rutas de salida.
```
route-map [map-name] permit
set weight [weight]
router bgp [BGP-ASN]
neighbor [ip-neighbor] route-map [map-name] {in|out}
```

### Atributo Local-Preference
- Atributo iBGP
```
route-map [map-name] permit
set local preference [preference]
router bgp [BGP-ASN]
neighbor [ip-neighbor] route-map [map-name] {in|out}
```

### Atributo Med
- Atributo iBGP-eBGP
- **M**ultiple **E**xit **D**iscriminator
```
route-map [map-name] permit
set metric [med-number]
exit
router bgp [BGP-ASN]
neighbor [ip-neighbor] route-map [map-name] [in|out]
```
### Atributo AS-Path
- Atributo eBGP
- AS-PATH incluye una lista con todos los caminos entre BGP AS necesarios para encontrar un destino.
- Se usa tanto `[AS-Local]` como pida para envenenar la ruta
```
route-map [map-name] permit
set as-path prepend [AS-Local]
exit
router bgp [BGP-ASN]
neighbor [ip-neighbor] route-map [map-name] [in|out]
```
## Reinicia tabla BGP
> NOTA: Para borrar solo los fallos, `soft` es muy importante, de caso contrario, borra toda la tabla
```
enable
clear ip bgp * soft
```

# Visualizacion
`show ip bgp` -> Tabla BGP con vecinos y redes, lineas (\*) son rutas validas, (>) indican mejor ruta, (i) para anuncios IBGP, otras opciones son
- **s** son rutas que se suprimen (Porque hay rutas resumen)
- **d** son rutas penalizadas por no ser confiables
- **h** son rutas historicas, por lo que posiblemente ya no existan
- **r** son rutas con fallos y no pasaran a la RIB
- **S** son rutas viciadas, con inconvenientes
`show ip protocols`
`show ip bgp summary`: Ver vecinos y redes resumidas
`show ip bgp nexthops`
`show ip bgp neighbor`
`show ip bgp neighbor [ip] received-routes` -> Muestra las rutas recibidad y rechazadas de un vecino
`show ip bgp neighbor [ip] advertised-routes` -> Muestra todas la rutas BGP que anuncian los vecinos
`show running-config | section router bgp`
`show bgp ipv4 unicast`
`show bgp ipv4 unicast [ip]`


# Troubleshooting
## Verificacion eBGP
- BGP Router Identifier: IP que los routers BGP reconocen
- Local AS number: Direccion AS
- BGP Table Version: Numero de tabla BGP local, aumenta con mensjaes updates recibidos
- Main routing table version: Es la ultima version de la base de datos BGP en la RIB
- Neighbor: IP del estado vecino para tener vecindad
- Version(V): Version de BGP ejecutando con los vecinos
- AS: Lista numero Sistema Autonomo
- Message Received (MsgRcvd): Numero de mensajes BGP recibidos por el vecino
- Tb|Ver: La ultima version de la tabla BGP enviada por el vecino
- In queue (InQ): N° Mensajes que esperar ser procesados
- Out queue (OutQ): N° Mensajes que se esperan de un vecino
- Up/Down: Tiempo actual estado BGP
- State: Muestra uno de los [[#Estados]] de BGP

## Debug
`debug ip bgp`: Ver funcionamiento bgp
`debug ip bgp updates`: Ver adjacencias bgp

# Extra
Conocido como BGP
Definido en [RFC1267](https://www.rfc-editor.org/rfc/rfc1267)