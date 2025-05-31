# Info
## Diferencias con OSPFv2
Vease [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv2|OSPFv2]]
- Uso de direcciones Link-Local para adyacencias
- Multiples instancias por interfaz
- Cambios en el formato del paquete

## Caracteristicas
- Soporte para multiples familias de direcciones
- Mas Tipos de LSA
- Ya no hay prefijo IP en los encabezados
- Inundacion de LSA: determina el alcance
- El formato se ejecuta directamente sobre IPv6
- ID de Router: Identifica a los vecinos
- Ya no hay autenticacion y se usa IPsec
- Adyacencia Vecinas a travez de direccionamiento local
- Varias Instancias que incluyen su propio ID para formar adyacencia

## Tipos de LSA

| Tipo de LSA | Nombre            | Descripcion                                                                                                                                                                                     |
| ----------- | ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0x2001      | Router            | Todos los routers<br>LSA con el estado y el costo                                                                                                                                               |
| 0x2002      | Network           | Router Borde de Area<br>LSA para anunciar los routers unidos al resignado, incluyendose                                                                                                         |
| 0x2003      | Interarea Prefix  | Router Borde de Area<br>LSA para describir rutas a prefijos IPv6 de otras areas                                                                                                                 |
| 0x2004      | Interarea Router  | ASBR<br>LSA para anunciar la direccion del ASBR en otras areas                                                                                                                                  |
| 0x2005      | AS external       | ASBR<br>LSA para anunciar rutas por defecto o rutas aprendidas de otros protocolos                                                                                                              |
| 0x2007      | NSSA              | ASBR<br>                                                                                                                                                                                        |
| 0x2008      | Link              | LSA para mapear todo los prefijos de direcciones de unidifusion global<br>asociada a desde una interfaz a la ip de la interfaz local del router<br>Se comparte solo en vecinos del mismo enlace |
| 0x2009      | Intra-area Prefix | LSA para anunciar prefijos IPv6 que estan asociado a un router,stub o segmento                                                                                                                  |
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
`show ipv6 protocols` -> Ver estado protocolo
`show ospfv3 ipv6 neighbor` -> Ver los vecinos
`show ospfv3 interface brief` -> Ver las interfaces conectadas en local
`show ipv6 route ospf` -> Ver enrutamiento OSPF
`show ipv6 route ospf | begin Application` -> Ver las rutas en ipv6 ingresadas


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