La redistribucion es una tecnica que permite que diferentes protocolos "Hablen el mismo idioma" al permitir compartir informacion de rutas y que las usen en las tablas RIB (Routing Information Base), estas rutas que ingresan deben respetar las normas del protocolo donde se injectan.
Se configura en la salida de la interfaz hacia el otro protocolo.

> [!IMPORTANT] Importante
> Su mala aplicacion puede botar el protocolo que se intenta comunicar.

Se evita usar ACL:
- Mascara de subred no puede ser facilmente igualada
- ACL son evaluadas de forma secuencial para cada prefijo IP
- Las listan de control de acceso extendidas pueden ser muy complicadas de configurar

Tipos de redistibucion
- Rutas Estaticas
- Directa entre protocolos
- [[020 - Conceptos/020.1 - Administracion/Prefix-List|Prefix-List]] y [[020 - Conceptos/020.1 - Administracion/Route-Map|Route-Map]]

## BGP
En [[010 - Protocolos/010.1 - Routing/BGP/BGP|BGP]] aliviana el procesamiento de rutas y acelera, tiene 2 tecnicas
- Estatica: Crea [[010 - Protocolos/010.1 - Routing/Ruta Estatica|Ruta Estatica]] a Null0 para el prefijo de red resumen y anuncia el prefijo de direccion de red. La desventaja es que si esas rutas estan caidas, las anuncia de todas formas
- Dinamica: Usa prefijos de red, y las rutas que coincidan ingresan a la tabla BGP, generando el prefijo, el router de origen da el siguiente salto en Null0 como ruta de descarte para evitar loops.

## Metrica Semilla por defecto
Protocolos de Vector Distancia ([[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP|EIGRP]] y [[010 - Protocolos/010.1 - Routing/RIP|RIP]]): Se le asigna metrica infinita(0), por lo que hay que configurar una metrica o no compartira las rutas, excepto cuando se distribuyen rutas estaticas o 2 AS de EIGRP.
[[010 - Protocolos/010.1 - Routing/OSPF/OSPFv2|OSPFv2]]: por defecto son de tipo 2 (E2), con metrica 20, cuando se distribuyen a BGP se le asigna metrica  "1".
[[010 - Protocolos/010.1 - Routing/BGP/BGP|BGP]]: Mantiene las metricas IGP.

## Tecnicas de Redistribucion
- Unica en un sentido(→): Un router, un protocolo a otro
- Unica en Ambos sentidos(⇐ ⇒): Un router, 2 protocolos ambos se comunican
- Redistribucion Multiple en un sentido(→ →): 2 Router, un solo protocolo a otro, es importante la metrica
- Redistribucion Multiple en ambos sentidos(⇐⇐ ⇒⇒): 2 Router, 2 protocolos que se conunican, es importante la metrica

## Tabla
- `list-name`: Nombre de prefix-list (Case Sensitive)
- `list-number`: Numero de prefix-list
- `seq [seq-value]`: un valor de 32 bits, determina el orden al filtrar, si no hay ninguna entrada, se revisa el actual maximo y se le suma 5 (5,10,15...)
- `deny | permit`: Accion tomada cuando calza una regla
- `[network/length]`: Prefijo a hacer math. `network` tiene valor 32 bits, `length` es decimal
- `ge [ge-value]`: El rango para hacer match hacia arriba del `[network/legth-2]`
- `le [le-value]`: El rango para hacer match hacia abajo del `[network/lenght-3]`

# Configuracion
## EIGRP IPv4
[[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP|EIGRP]] ([[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP Numerado|EIGRP Numerado]] y [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP Nombrado|EIGRP Nombrado]])
> [!IMPORTANT] Importante
> Para redistribuir en EIGRP Nombrado, se debe agregar a `topology base` y se usa [[020 - Conceptos/020.1 - Administracion/Route-Map|Route-Map]]
> ```
> router eigrp [AS-name]
>  address-family ipv4 unicast autonomous-system [AS-number]
>   topology base
>    redistribute [protocol] route-map [map-name]
> ```

### Rutas Estaticas EIGRP IPv4
```
router eigrp [AS]
redistribute static
```
### Directa Rutas EIGRP -> EIGRP IPv4
Valores `metric` se ven con `show int [S/S/P]` en la interfaz que va donde el protocolo de las rutas
```
router eigrp [AS1]
redistribute eigrp [AS2] metric [BW] [DLY] [CONF] [Carga] [MTU]
router eigrp [AS2]
redistribute eigrp [AS1] metric [BW] [DLY] [CONF] [Carga] [MTU]
```
### Directa Rutas EIGRP -> OSPFv2 IPv4
```
router ospf [proceso]
redistribute eigrp [AS] subnets
```
### Route-Map Rutas EIGRP -> EIGRP
```
ip prefix-list [prefix-name-eigrp] permit [ip-network/CIDR-mask]
route-map [map-name-eigrp] permit
 match ip address prefix-list [prefix-name-eigrp]
 set metrix [BW] [DLY] [CONF] [CARGA] [MTU]
 exit
router eigrp [AS-local]
 redistribute eigrp [AS-remote] route-map [map-name-eigrp]
```
### Route-Map Rutas EIGRP -> OSPFv2 IPv4
```
ip prefix-list [prefix-name-eigrp] permit [ip-network/CIDR-mask]
route-map [route-map-name-ospf] permit
match ip address prefix-list [prefix-list-name-eigrp]
set metric [metric-value]
set metric-type type-1
exit
router ospf [proceso]
redistribute eigrp [AS] subnets route-map [route-map-name-ospf]
```
### Route-Map Rutas EIGRP -> BGP IPv4
```
ip prefix-list [prefix-name-eigrp] permit [ip-network/CIDR-mask]
!
route-map [route-map-name-bgp] permit
match ip address prefix-list [prefix-list-name-eigrp]
!# Opcional, segun requerimiento
set metric [bgp-metric-value]
exit
!
router bgp [BGP-AS]
redistribute eigrp [EIGRP-AS] route-map [route-map-name-bgp]
```
## EIGRP IPv6
[[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP|EIGRP]]
### Rutas Estaticas EIGRP IPv6
```
ipv6 router eigrp [AS]
redistribute static include-connected
```
### Directa Rutas EIGRP -> EIGRP IPv6
Valores `metric` se ven con `show int [S/S/P]` en la interfaz que va donde el protocolo de las rutas
```
ipv6 router eigrp [AS1]
redistribute eigrp [AS2] metric [BW] [DLY] [CONF] [Carga] [MTU] include-connected
ipv6 router eigrp [AS2]
redistribute eigrp [AS1] metric [BW] [DLY] [CONF] [Carga] [MTU] include-connected
```
### Directa Rutas EIGRP -> OSPFv3 IPv6
```
ipv6 router ospf [proceso]
redistribute eigrp [AS] include-connected
```
### Directa Rutas EIGRP -> MP-BGP IPv6
```
router bgp [BGP-AS]
address-family ipv6
redistribute eigrp [EIGRP-AS] metric [metric-value] included-connected
```
## OSPFv2
[[010 - Protocolos/010.1 - Routing/OSPF/OSPF|OSPF]] ([[010 - Protocolos/010.1 - Routing/OSPF/OSPFv2|OSPFv2]] y [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv3|OSPFv3]])
### Rutas Estaticas OSPFv2 IPv4
Nota: Router ABSR o que conecta con otro protocolo
```
router ospf [proceso]
default-information originate
```
### Directa Rutas OSPFv2 -> EIGRP IPv4
```
router eigrp [AS]
redistribute ospf [proceso] metric [BW] [DLY] [CONF] [CARGA] [MTU]
```
### Directa Rutas OSPFv2 -> RIP
```
router rip
redistribute ospf 1 metric 1
```
### Route-MAP Rutas OSPFv2 -> EIGRP IPv4
Valores `metric` se ven con `show int [S/S/P]` en la interfaz que va donde el protocolo de las rutas
```
ip prefix-list [list-name-OSFP] permit [ip-network-ospf/CIDR-mask]
route-map [map-name-EIGRP] permit
match ip address prefix-list [list-name-OSPF]
set metric [BW] [DLY] [Conf] [Carga] [MTU]
router eigrp [AS]
redistribute ospf [proceso] route-map [map-name-EIGRP]
```
### Route-Map Rutas OSPFv2 -> EIGRP Nombrado
```
ip prefix-list [list-name-OSFP] permit [ip-network-ospf/CIDR-mask]
route-map [map-name-EIGRP]
match ip address prefix-list [list-name-OSPF]
set metric [BW] [DLY] [Conf] [Carga] [MTU]
exit
router eigrp [AS-name]
address-family ipv4 autonomous-system [AS-number]
topology base
redistribute ospf [proceso] route-map [map-name-EIGRP]
```
### Route-MAP Rutas OSPFv2 -> BGP IPv4
```
ip prefix-list [list-name-OSFP] permit [ip-network/CIDR-mask]
route-map [map-name-BGP] permit
match ip address prefix-list [list-name-OSFP]
set metric [metric-value]
router bgp [BGP-AS]
redistribute ospf [proceso] route-map [map-name-BGP]
```

## OSPFv3
[[010 - Protocolos/010.1 - Routing/OSPF/OSPFv3|OSPFv3]]
### Rutas Estaticas OSPFv3 IPv6
```
ipv6 router ospf [proceso]
default-information originate
```
### Directa Rutas OSPFv3 -> RIPng IPv6
```
ipv6 router rip [rip-name]
redistribute ospf 1 include-connected
```
### Directa Rutas OSPFv3 -> EIGRP IPv6
Valores `metric` se ven con `show int [S/S/P]` en la interfaz que va donde el protocolo de las rutas
```
ipv6 router eigrp [AS]
redistribute ospf [proceso] metric [BW] [DLY] [CONF] [CARGA] [MTU] include-connected
```
### Route-Map Rutas OSPFv2 -> EIGRP Nom
```
route-map [map-name-EIGRP]
set metric [BW] [DLY] [Conf] [Carga] [MTU]
exit
router eigrp [AS-name]
address-family ipv6 autonomous-system [AS-number]
topology base
redistribute ospf [proceso] route-map [map-name-EIGRP] include-connected
```
### Directa Rutas OSPFv3 -> MP-BGP
```
router bgp [BGP-AS]
address-family ipv6
redistribute ospf [proceso] metric [metric-value] include-connected
```

## RIP
[[010 - Protocolos/010.1 - Routing/RIP|RIP]]
### Rutas Estaticas RIP IPv4
```
router rip
default-information originate
```
### Directa Rutas RIP -> OSPFv2 IPv4
```
router ospf [proceso]
redistribute rip subnets
```

## RIPng
[[010 - Protocolos/010.1 - Routing/RIP|RIP]]
### Rutas Estaticas RIPng IPv6
```
ipv6 router rip [rip-name]
default-information originate
```
### Directa Rutas RIPng -> OSPFv3 IPv6
```
ipv6 router ospf [proceso]
redistribute rip [rip-name] included-connected
```

## BGP
[[010 - Protocolos/010.1 - Routing/BGP/BGP|BGP]]
### Route-Map Rutas BGP -> EIGRP IPv4
Las metricas se obtienen de `show int [int S/S/P]` hacia ???
```
ip prefix-list [list-name-BGP] permit [ip-network/mask]
route-map [map-name-EIGRP] permit
match ip address prefix-list [list-name-BGP]
set metric [BW] [DLY] [CONF] [Carga] [MTU]
router eigrp [EIGRP-AS]
redistribute bgp [BGP-AS] route-map [map-name-EIGRP]
```
### Route-Map Rutas BGP -> OSPFv2 IPv4
```
ip prefix-list [list-name-BGP] permit [ip-network/CIDR-mask]
route-map [map-name-OSPF] permit
match ip address prefix-list [list-name-BGP]
set metric [metric-value]
set metric-type type-1
router ospf [proceso]
redistribute bgp [BGP-AS] subnets route-map [map-name-OSPF]
```

## MP-BGP
[[010 - Protocolos/010.1 - Routing/BGP/BGP-4|BGP-4]]
### Directa Rutas MP-BGP -> OSPFv3 IPv6
```
ipv6 router ospf [proceso]
redistribute bgp [AS]
```
### Directa Rutas MP-BGP -> EIGRP IPv6
Valores `metric` se ven con `show int [S/S/P]` en la interfaz que va donde el protocolo de las rutas
```
ipv6 router eigrp [eigrp-AS]
redistribute bgp [bgp-AS] metric [BW] [DLY] [CONF] [Carga] [MTU] include-connected
```
