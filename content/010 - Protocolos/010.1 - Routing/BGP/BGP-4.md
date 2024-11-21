# Info
**M**ulti **P**rotocol - **B**order **G**ateway **P**rotocol es la version 4 de [[010 - Protocolos/010.1 - Routing/BGP/BGP|BGP]] y una mejora de [[010 - Protocolos/010.1 - Routing/BGP/BGP-3|BGP-3]].

## Datos
- Soporta [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv4|IPv4]]/[[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv6|IPv6]] Unicast/Multicast
- Permite usar AF y expresiones regulares (e.g. `^$`)
- Agrega el Atributo `NLRI` (opcional), es ignorado por sistemas viejos

# Configuracion
> [!NOTE]- Nota
> La configuracion de `ASN` y `neighbor` se hacen de forma global; la **activacion del vecino** se realiza en una AF especifica.
> Recuerda habilitar `ipv6 unicast-routing`
## AF iBGP
> [!NOTE]- Nota
> La configuracion IPv4 es compatible con [[010 - Protocolos/010.1 - Routing/BGP/BGP-3#Configuracion|BGP-3]], solo que se agrega AF
```
router bgp [BGP-ASN]
 bgp router-id [R-ID]
 neighbor [ipvx-neighbor] remote-as [remote-BGP-ASN]
 neighbor [ipvx-neighbor] update-source loopback [loopback-number]
!
 address-family ipv4
  neighbor [ipv4-neighbor] activate
  network [ipv4-network] mask [dec-mask]
  exit
 address-family ipv6
  neighbor [ipv6-neighbor] activate
  network [ipv6-network/prefix]
```

### AF iBGP Next-Hop-Self
> Modifica el atributo "next-hop" a la direccion del router que anuncia las rutas iBGP, se configura en el router de borde que anuncia las rutas
```
router bgp [BGP-ASN]
 address-family [ipvx]
  neighbor [ip-neighbor] next-hop-self
```
#### AF iBGP Next-Hop-Self via Route-map
```
route-map [map-name] permit [map-value]
 set [ipvx] next-hop [ipvx-next-hop]
 exit
router bgp [BGP-ASN]
 address-family ipv6 unicast
  neighbor [ipvx-neighbor] route-map [map-name] out
```
### AF iBGP Router Reflector
> Para que los vecinos se conoscan entre ellos y sus rutas.
```
router bgp [BGP-ASN]
 address-family [ipvx]
  neighbor [ip-neighbor] route-reflector-client
```
## AF eBGP
```
router bgp [BGP-ASN]
 bgp router-id [R-ID]
 neighbor [ipvx-neighbor] remote-as [remote-BGP-ASN]
!
 address-family ipv4
  neighbor [ipv4-neighbor] activate
  exit
 address-family ipv6
  neighbor [ipv6-neighbor] activate
```

## Manipular Atributos BGP
> [!NOTE] Nota
> Informacion sobre Manipulacion IPv4: Detalles sobre [[010 - Protocolos/010.1 - Routing/BGP/BGP#Proceso de Seleccion de rutas|Proceso de Seleccion de rutas BGP]] | Relacionado [[010 - Protocolos/010.1 - Routing/BGP/BGP-3#Manipulacion Atributos de BGP|Manipulacion de atributos BGP-3]] | Agrega AF a esas configuraciones
### Weight IPv6 (Peso)
- Atributo **iBGP**
- Le da preferencia a una ruta cuando hay multiples rutas de salida.
```
route-map [map-name] permit
 set weight [weight-value]
 exit
!# Aplica route-map a vecino BGP
router bgp [BGP-ASN]
 address-family ipv6
  neighbor [ipv6-loopback-neighbor] route-map [map-name] {in|out}
  exit
!# Limpia tabla BGP
do clear bgp ipv6 unicast * soft
```
### AS-PATH IPv6
- Atributo **eBGP**
- Envenena una rutas para preferir otra
```
route-map [map-name] permit
 set as-path prepend [BGP-ASN-Local] [BGP-ASN-Local]
 exit
router bgp [BGP-ASN]
 address-family ipv6
  neighbor [ipv6-neighbor] route-map [map-name] {in|out}
  exit
!# Limpia tabla BGP
do clear bgp ipv6 unicast * soft
```
### MED IPv6
Atributo **iBGP-eBGP**
**M**ultiple **E**xit **D**iscriminator
```
route-map [map-name] permit
 set metric [metric-value]
 exit
router bgp [BGP-ASN]
 address-family ipv6
  neighbor [ipv6-neighbor] route-map [map-name] {in | out}
  exit
!# Limpia tabla BGP
do clear bgp ipv6 unicast * soft
```

## Sumarizar Redes
> Apoyo sobre [[020 - Conceptos/020.3 - Fundamentos/Sumarizacion de Redes|Sumarizacion de Redes]]
```
router bgp [BGP-ASN]
 address-family ipv6
  aggregate-address [ipv6-sum-network/prefix] summary-only
```

# Visualizacion
- `show bgp ipv6 unicast`: Tabla BGP IPv6 unicast
- `show bgp ipv6 unicast summary`: Tabla BGP IPv6 Resumen
- `show bgp ipv6 unicast nexthop`
- `show run | section router bgp`: Ver configuracion de BGP

# Troubleshooting
- **"Active"** no esta configurado en AF IPv6
	- `show bgp ipv6 unicast`
- Falta limpiar la tabla de los intentos fallidos
	- `clear bgp ipv6 unicast * soft`

# Extra
- En [[999 - Archivado/MPLS|MPLS]] se utiliza para intercambiar etiquetas [[010 - Protocolos/010.3 - Comunicaciones/VPN|VPN]] 
- Dentro de los AF no hay tabuleos

