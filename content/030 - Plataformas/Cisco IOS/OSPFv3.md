
Material Teorico
- [[010 - Protocolos/010.1 - Routing/OSPF/OSPF|OSPF]]
- [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv2|OSPFv2]]
- [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv3|OSPFv3]]

# Configuracion
## Configuracion Basica
### Habilitar OSPFv3 en el router
Recuerda activar `ipv6 unicast-routing` para que funcione
```
ipv6 router ospf [id-proceso]
```
### Router-ID
Puede ser el mismo valor que se usa en IPv4
```
ipv6 router ospf [id-proceso]
router-id [router-id]
```
### Interfaces Pasivas
```
ipv6 router osfp [proceso]
passive-interface [int S/S/P]
```
### Aplicar enrutamiento interfaz
```
int [S/S/P]
ipv6 ospf [id-proceso] area [area-number]
```

## Configuracion Avanzada
### Sumarizacion de Rutas
`area number range` -> La tabla se vera mas pequeña
### Area Stub
```
ipv6 router ospf [proceso]
area [area-id] stub
```
### Area Totally Stub
```
ipv6 router ospf [proceso]
area [area-id] stub no-summary
```

## Optimizacion
## Modificar Tiempos de OSPF
Tiempos por defecto en [[010 - Protocolos/010.1 - Routing/OSPF/OSPF#Tipos de Redes|OSPF]]
```
ipv6 ospf hello-interval [hello-sec]
ipv6 ospf dead-interval [dead-sec]
```

# Visualizacion
- `show ipv6 protocols` -> Ver estado protocolo
- `show ipv6 route ospf` -> Ver enrutamiento OSPF
- `show ipv6 route ospf | begin Application` -> Ver las rutas en ipv6 ingresadas
- `show ipv6 ospf neighbor` -> Adjacencias con estado
- `show ipv6 ospf interface brief` -> Resumen del estado de las interfaces
- `show ospfv3 ipv6 neighbor` -> Ver los vecinos
- `show ospfv3 interface brief` -> Ver las interfaces conectadas en local


# Extras
Las diferencias se encuentran en el [RFC 5340](https://www.rfc-editor.org/rfc/rfc5340)
## Modificar Costo OSPFv3
```
int [int S/S/P]
bandwidth
ipv6 ospf cost [cost-number]
```
## Sumarizacion OSPFv3 ???
```
router ospfv3 1
address-family ipv6 unicast
area 0 range 2001:db8:0:0::/65
```
## Virtual-Link OSPFv3
Esta configuracion se hace en ambos ABR, con el Router-ID del otro extremo
```
ipv6 router ospf [proceso]
area [area-id] virtual-link [router-id]
```