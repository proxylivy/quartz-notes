# Info
**B**order **G**ateway **P**rotocol es la version 3 de [[010 - Protocolos/010.1 - Routing/BGP/BGP|BGP]] solo soporta [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv4|IPv4]], la siguiente version es llamada [[010 - Protocolos/010.1 - Routing/BGP/BGP-4|BGP-4]]

## Terminologia
Follow-up de [[010 - Protocolos/010.1 - Routing/BGP/BGP#Terminologia|Terminologia BGP]]
- `[BGP-ASN]`: Numero de AS en BGP
- `[R-ID]`: Router ID especifico para BGP
- `[ip-neighbor]`: IP del vecino
- `[loopback-number]`: Loopback local del router para generar adyacencia

# Configuracion
## iBGP
Al configurar vecinos iBGP, es comun usar loopback, esto permite poder usar enrutamiento [[020 - Conceptos/020.3 - Fundamentos/IGP|IGP]] en caso de que BGP falle al encontrar la ruta.
### Neighbors iBGP
Considerados 2 router en el mismo `[AS]`
> La `[self-loopback-number]` es la del mismo router
```
router bgp [BGP-ASN]
 bgp router-id [R-ID]
 neighbor [ip-neighbor] remote-as [BGP-ASN]
 neighbor [ip-neighbor] update-source loopback [loopback-number]
```
### Network iBGP
> Consideradas las `[loopback]` y `[ip-network]`, estas rutas deben existir en la [[999 - Archivado/RIB|RIB]]
```
router bgp [BGP-ASN]
 network [ip-network] mask [dec-mask]
```
### Next-Hop-Self iBGP
> Modifica el atributo "next-hop" a la direccion del router que anuncia las rutas iBGP, se configura en el router de borde que anuncia las rutas
```
router bgp [BGP-ASN]
 neighbor [ip-neighbor] next-hop-self
```
### Router Reflector iBGP
> Para evitar la regla del horizonte dividido, explicacion en [[010 - Protocolos/010.1 - Routing/BGP/BGP#Regla del Horizonte Dividido iBGP|BGP]]
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
`[Hop-number]` es el valor de AS intermedios + 1, explicacion en [[010 - Protocolos/010.1 - Routing/BGP/BGP#Multihop eBGP|BGP]]
```
router bgp [BGP-ASN-local]
 neighbor [ip-neighbor] remote-as [BGP-ASN-remote]
 neighbor [ip-neighbor] update-source loopback [loopback-number]
 neighbor [ip-neighbor] ebgp-multihop [hop-number] 
```

## Sumarizacion
Relacionado a [[020 - Conceptos/020.3 - Fundamentos/Sumarizacion de Redes|Sumarizacion de Redes]]
### Sumarizacion de Redes con Supresion
> Usualmente usado con network `[loopback]`
```
router bgp [BGP-ASN]
 aggregate-address [ip-summarized-network] [dec-mask] summary-only
```
### Sumarizacion de Redes sin Supresion
> Para rutas que coexistan en ambas tablas de un router
```
router bgp [BGP-ASN]
 aggregate-address [ip-summarized-network] [dec-mask]
```
## Manipulacion Atributos de BGP
Detalles en [[010 - Protocolos/010.1 - Routing/BGP/BGP#Proceso de Seleccion de rutas|Proceso de Seleccion de rutas BGP]]

> [!IMPORTANT] Importante
> No se pueden mezclar atributos, solo 1 a la vez

### Atributo Weight (Peso)
- Atributo iBGP (Propietario de Cisco)
- Le da preferencia a una ruta cuando hay multiples rutas de salida.
```
route-map [map-name] permit
 set weight [weight]
 exit
router bgp [BGP-ASN]
 neighbor [ip-neighbor] route-map [map-name] {in|out}
```

### Atributo Local-Preference
- Atributo iBGP
```
route-map [map-name] permit
 set local preference [preference]
 exit
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
> [!WARNING] Peligro
> Se debe usar `soft` para borrar los datos erroneos, si no se escribe, se borrara toda la tabla de adyancencias BGP
```
enable
clear ip bgp * soft
```

# Troubleshooting
> [!NOTE] Nota
> Visualizacion General en [[010 - Protocolos/010.1 - Routing/BGP/BGP#Verificacion General|BGP]]

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

