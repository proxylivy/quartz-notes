# Info
## Caracteristicas
- Formato de paquete
- Direccion Multicast (224.0.0.5 y 224.0.0.6)

## Requerimientos para vecindad
- Router-ID unicos entre los dispositivos de todo el dominio OSPF
- Interfaces comparten una subred en comun, ospf envia saludos a travez del gateway
- Las MTU (Unidad Maxima de transmision) debe coincididr en las interfaces, OSPF no permite fragmentacion
- El Router-ID debe coincider con el segmento(network)
- El DR debe coincidir en el segmento
- Los temporizadores de saludo y tiempo muerto en OSPF deben coincidir
- El tipo de autenticacion y las credenciales deben coincidir en el segmento
- Los indicadores de tipo de area deben coincidir con el segmento (Stub, NSSA)

# Configuracion
## Configuracion Basica
### Habilitar OSPF con Redes
```
router ospf [proceso]
network [ip-network] [wilcard or dec-mask] area [area-id]
```

### Configuracion Router-ID
```
router ospf [proceso]
router-id [router-id]
```

### Interfaces Pasivas
```
router ospf [proceso]
passive-interface [int S/S/P]
```

## Configuracion Avanzada
### Ajuste de Costos de Interfaz
### Autenticacion
Debe estar en la interfaz de cada router de area en OSPF
```
int [int S/S/P]
ip ospf message-digest-key 1 md5 [password]
ip ospf authentication message-digest
```
### Sumarizacion de Rutas
Se hace en los ABR
- `area-id` va la del mismo lado donde estan las redes sumarizadas
Difiere con `ipv6` con el comando `ipv6 router ospf [ospf]`
```
router ospf [proceso]
area [area-id] range [IPv4-Sumarizada] [dec-mask-sumarizada]
```
### Configuracion de Areas STUB
Las rutas OE2/OE1 se ven como 0.0.0.0/0
```
ip router ospf [proceso]
area [area-id] stub
```
### Not-Advertise
Permite filtrar rutas sin usar [[020 - Conceptos/020.1 - Administracion/ACL|ACL]] o [[020 - Conceptos/020.1 - Administracion/Prefix-List|Prefix-List]] o [[020 - Conceptos/020.1 - Administracion/Route-Map|Route-Map]]
```
router ospf [proceso]
area [area-id] range [ip-network] [dec-mask] not-advertise
```

## Optimizacion
### Temporizadores
Tiempos por defecto en [[010 - Protocolos/010.1 - Routing/OSPF/OSPF#Tipos de Redes|OSPF]]
```
int [int S/S/P]
ip ospf hello-interval [sec]
ip ospf dead-interval [sec]
```


# Visualizacion
`show ip protocols`
`show ip route ospf`
`show ip ospf`
`show ip ospf int` -> OSPF desde las interfaces
`show ip ospf int [int S/S/P]`
`show ip ospf interface brief` -> OSPF desde las interfaces de forma resumida
`show ip ospf neighbor`
`show ip ospf database`
`show ip route ospf | begin Gateway` -> Mostrar los gateway de los protocolos al enrutar
`debug ip ospf adj`
`debug ip ospf events`
`show ip ospf int [int S/S/P] | include Timer`

# Troubleshooting
## Problemas de Vecindad
- Interfaz
	- Inactiva: La interfaz debe estar UP/UP
	- Pasiva: No envia ni recibe saludos por esa interfaz
		- `show ip protocols`
	- No habilitada OSPF: No enviara paquetes de saludos ni forma adyacencia
		- `show ip int brief`
		- `show ip protocols`
- Temporizadores Coincidentes: Hello y Dead Time deben ser los mismos entre routers
	- Tiempos por defecto en [[010 - Protocolos/010.1 - Routing/OSPF/OSPF#Tipos de Redes|OSPF]]
	- `show ip ospf int [int S/S/P]`
- N° Area Coincidentes: Los dos extremos de un enlace deben coincidir en area
	- `show ip ospf int [int S/S/P]`
	- `show ip ospf int brief`
- Tipo de Area Coincidente: Todos los routers de un area deben elegir el mismo tipo (Normal, Stub o NSSA(Totally Stub))
- Diferentes Subredes
- Inconsistencia Autenticacion: Ambos extremos deben tener el mismo tipo de autenticacion y llave (Se puede habilitar por interfaz o todas las interfaces de un area)
	- Permite 3 tipos de autenticacion
		- 0: Null / No hay
		- 1: Texto Plano
		- 2: MD5
	- `show ip ospf int [int S/S/P]`
- ACL: Puede denegar paquetes de descubrimiento a travez de broadcast (224.0.0.5)
- Inconsistencia MTU: Debe ser la misma entre vecinos o quedara atascado en el estado "ExStart/Exchange"
	- `show run int [int S/S/P]`
- Router-ID Duplicados: Deben ser diferentes entre routers
	- Recibe el mensaje de error "`OSPF-3-DUP_RTID_NBR: OSPF detected duplicate router-id`"
	- `show ip protocols`


### Interfaces no habilitadas con OSPF
`show ip ospf int brief` y `show ip protocols`
### Inconsistencia en temporizadores
`show ip ospf int [int S/S/P]`
### Inconsistencia N° Area
`show ip ospf int [int S/S/P]` y `show ip ospf int brief`
### Inconsistencias Int Pasivas
`show ip protocols`
### Inconsistencia Autenticacion
`show ip ospf int [int S/S/P]`
### Inconsistencia MTU
`show run int [int S/S/P]`
### Router ID Duplicados
`show ip protocols`

# Extra
## Configuracion Avanzada
### Modificar Prioridad desde interfaz
Permite Modificar la eleccion de DR y BDR, ospf necesita reiniciar su proceso para funcionar `clear ip ospf process`
```
int [int S/S/P]
ip ospf priority [priority]
```
### Cambio de Ancho de Banda
```
int [int S/S/P]
bandwidth [bandwidth-value]
```
### Cambio de Referencia de Costos
```
router ospf [proceso]
auto-cost reference-bandwidth [reference-value]
```
### Modificar Costo de interfaz
```
int [int S/S/P]
ip ospf cost [cost-value]
```


## Totally Stub
TSTUB → LSA Tipo 3,4 y 5 se convirte en ruta por defecto (OIA/OE2/OE1 → 0.0.0.0/0)

### Area Totally Stub
Las rutas OIA/OE2/OE1 se ven como 0.0.0.0/0
Difiere con `ipv6` con el comando `ipv6 router ospf [ospf]`
```
ip router ospf [proceso]
area [area-id] stub no-summary
```
## OSPF Virtual Links
No se ve esta configuracion
Configuracion Excepcional, no es final, como un parche, la mejor solucion seria la reestructuracion de red
Unir una area separada a la 0
Dentro de los ABR se configura, poniendo el Router-ID(Por eso es importante tener el Router-ID configurado)
### Configuracion Virtual-Link
Esta configuracion se hace en ambos ABR, con el Router-ID del otro extremo
```
router ospf [proceso]
area [area-id] virtual-link [router-id]
```


## Cambiar red de Broadcast a p2p
NOTA: Hacer esto desde ambos Routers
```
int [int S/S/P]
ip ospf network point-to-point
```

