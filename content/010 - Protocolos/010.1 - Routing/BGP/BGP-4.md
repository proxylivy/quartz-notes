# Info
**M**ulti **P**rotocol - **B**order **G**ateway **P**rotocol es la siguiente version de [[010 - Protocolos/010.1 - Routing/BGP/BGP-3|BGP-3]], soportando IPv4/IPv6 Unicast/Multicast, Permite usar expresiones regulares(^$).
## Datos
- Agrega el Atributo NLRI(OPT), es ignorado en sistemas viejos
- Usa Adress-Family
- Prefijos se pueden compartir en sesiones TCP IPv4 o IPv6

# Configuracion
> Nota-1: ASN y Vecinos se configuran fuera de la configuracion AF, luego se **activa** dentro de la configuracion AF
> Nota-2: La configuracion de AF es general, cambia IPv4 o IPv6 segun corresponda
> Nota-3: Recuerda activar `ipv6 unicast-routing`
## AF iBGP
> Nota: IPv4 compatible con [[010 - Protocolos/010.1 - Routing/BGP/BGP-3#Configuracion|BGP-3]], agrega AF
```
router bgp [BGP-ASN]
bgp router-id [R-ID]
neighbor [ipvx-neighbor] remote-as [remote-BGP-ASN]
neighbor [ipvx-neighbor] update-source loopback [self-loopback-number]
address-family [ipvx]
neighbor [ipvx-neighbor] activate
network [ipvx-network/prefix]
```

### AF iBGP Next-Hop-Self
> Router de borde que reemplaza el valor next-hop de las rutas con su propia IP antes de anunciar a los vecinos iBGP, se configura en el router que anuncia la ruta
```
router bgp [BGP-ASN]
address-family [ipvx]
neighbor [ip-neighbor] next-hop-self
```
### AF iBGP Next-Hop-Self via Route-map
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
router bgp [AS-ASN]
bgp router-id [R-ID]
neighbor [ipvx] remote-as [remote-AS-ASN]
address-family [ipvx]
neighbor [ipvx] remote-as [remote-AS-ASN] activate
```

## Manipular Atributos BGP
> Manipulacion IPv4 en [[010 - Protocolos/010.1 - Routing/BGP/BGP-3#Manipulacion Atributos de BGP|BGP-3]]
### Weight IPv6
- Atributo **iBGP**
- Le da preferencia a una ruta cuando hay multiples rutas de salida.
```
!# Crea Route-map
route-map [map-name] permit
set weight [weight-value]
exit
!# Aplica route-map a vecino BGP
router bgp [AS]
address-family ipv6
neighbor [ipv6-loopback-neighbor] route-map [map-name] {in|out}
!# Limpia tabla BGP
do clear bgp ipv6 unicast * soft
```
### AS-PATH IPv6
- Atributo **eBGP**
- Envenena una rutas para preferir otra
```
route-map [map-name] permit
set as-path prepend [AS-Local] [AS-Local]
exit
router bgp [AS]
address-family ipv6
neighbor [ipv6-neighbor] route-map [map-name] {in|out}
!# Limpia tabla BGP
do clear bgp ipv6 unicast * soft
```
### MED IPv6
Atributo **iBGP-eBGP**
**M**ultiple **E**xit **D**iscriminator
```
route-map [route-map-name] permit
set metric [metric-value]
exit
router bgp [AS]
address-family ipv6
neighbor [ipv6-neighbor] route-map [route-map-name] {in | out}
!# Limpia tabla BGP
do clear bgp ipv6 unicast * soft
```

## Sumarizar Redes
Sobre [[020 - Conceptos/020.3 - Fundamentos/Sumarizacion de Redes|Sumarizacion]]
```
router bgp [AS]
address-family ipv6
aggregate-address [ipv6/prefix] summary-only
```

# Visualizacion
`show bgp ipv6 unicast`: Verifica contenido tabla BGP con rutas validas y Adyacencia Vecinos
`show bgp ipv6 unicast summary`: Verifica resumen contenido tabla BGP
`show bgp ipv6 unicast nexthop`
`show run | section router bgp`

# Troubleshooting
`clear bgp ipv6 unicast soft`

# Extra
Funciona para VPN normalitas (MPLS) no Site-to-Site
se usa en MPLS VPN para intercambiar "Etiquetas VPN"
dentro de address-family no hay tabuleo
Nota caso, bgp prefijo es network, ojo

Regla `IN`: Rutas que llegan
Regla `OUT`: Rutas que manda

- Conocido como MP-BGP
- Definido en [RFC4271](https://www.rfc-editor.org/rfc/rfc4271)
- BGP Operations and Security | [RFC 7454](https://www.rfc-editor.org/rfc/rfc7454)
- BGP SEC | [RFC8205](https://www.rfc-editor.org/rfc/rfc8205)