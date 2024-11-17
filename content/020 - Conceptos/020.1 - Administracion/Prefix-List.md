 
Solo Filtra pero se complementa con [[010 - Protocolos/010.1 - Routing/Route-Map|Route-Map]], y [[010 - Protocolos/010.1 - Routing/Redistribucion|Redistribucion]]. A diferencia de las [[020 - Conceptos/020.1 - Administracion/ACL|ACL]], que usan Wildcard y crea rutas para sumarizar, las prefix-list son mas exactas y flexibles, gracias al uso de los parametros `ge`(>) y `le`(<), que permiten `{permit|deny}` prefijos.

El valor de tag aumenta de 5 en 5
# Configuracion
## Prefix-list IPv4
```
ip prefix-list [list-name-1] deny [ip-network/cidr-mask]
ip prefix-list [list-name-1] permit 0.0.0.0/0 le 32
```
## Prefix-list IPv6
```
ipv6 prefix-list [list-name-2] deny 3001:ABCD:ABCD:10::/64
ipv6 prefix-list [list-name-2] permit ::/0 le 128
```
# Visualizar
`show ip prefix-list`
`show ipv6 prefix-list`
`show route-map`
`show ip route ospf`
`show ip route eigrp`

# Extra
EIGRP es el unico que puede filtrar a la salida, los demas son “IN”