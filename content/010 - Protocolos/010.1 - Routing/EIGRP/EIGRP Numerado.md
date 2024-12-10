# Info
Basado en [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP|EIGRP]], Funciona en un rango de 16 Bits (AS 1-65535)

# Configuracion
## Network IPv4
> - Se aplica interfaz Pasiva a dispositivos finales
```
router eigrp [ASN]
 passive-interface [int S/S/P]
 network [ip] [dec-mask]
```
## Network IPv6
> - Necesita habilitar el routing IPv6 con `ipv6 unicast-routing`
> - Se aplica interfaz Pasiva a dispositivos finales
> - Se Aplica EIGRP a la interfaz directamente
```
ipv6 router eigrp [ASN]
 passive-interface [int S/S/P]
 exit
int [int S/S/P]
 ipv6 eigrp [ASN]
```

## Router-ID IPv4
> OPCIONAL
```
router eigrp [ASN]
 eigrp router-id [R-ID]
```
## Router-ID IPv6
> OPCIONAL, se usa IPv4 normalmente para identificar los router facilmente
```
ipv6 router eigrp [ASN]
 eigrp router-id [R-ID]
```

## Sumarizacion de Redes IPv4
> - Apoyo: [[020 - Conceptos/020.3 - Fundamentos/Sumarizacion de Redes|Sumarizacion de Redes]]
> - No se envian rutas sumarizadas hasta que haya adyacencia
> - Prefiere compartir rutas sumarizadas en vez de especificadas
> - Interfaz `Null0` ficticia, tiene un [[020 - Conceptos/020.3 - Fundamentos/Tabla Distancia Administrativa|AD]] 5 para RIB, si una ruta que deberia estar sumarizada coincide con esta, se descarta, asi previene loops de enrutamiento
```
int [int S/S/P]
 ip summary-address eigrp [ASN] [IPv4-network/dec-mask]
```
## Sumarizacion de Redes IPv6
```
int [int S/S/P]
 ipv6 summary-address eigrp [ASN] [IPv6-network/Prefix]
```

## Modificar Temporizadores IPv4
> - Pueden funcionar con tiempos disparejos entre routers
> - Tiempos por Defecto en [[#Datos]]
```
int [int S/S/P]
 ip hello-interval eigrp [ASN] [hello-interval-sec]
 ip hold-time eigrp [ASN] [hold-time-sec]
```
## Modificar Temporizadores IPv6
```
int [int S/S/P]
 ipv6 hello-interval eigrp [ASN] [hello-interval-sec]
 ipv6 hold-time eigrp [ASN] [hold-interval-sec]
```

## Balanceo de Carga IPv4
> - OPCIONAL, Solo se configura en el router objetivo
> - Formula: $\text{variance value} = \text{FD}\times\text{Multiplier-Value}$
```
router eigrp [ASN]
 variance [Multiplier-Value]
```
## Rutas
> - Mas informacion en [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP#Balanceo de Carga|EIGRP]]
> - Default: 4
> - Desactivado: 1
> - Maximo: 16
```
router eigrp [ASN]
 maximum-path [maximum-path-value]
```

## Autenticacion IPv4
> Se debe aplicar a ambas interfaces para generar adyacencia
```
key chain [llavero]
 key 1
  key-string [password]
  exit
 exit
!
int [int S/S/P]
 ip authentication mode eigrp [ASN] md5
 ip authentication key-chain eigrp [ASN] [llavero]
```

## Modificar Estado Parametros K
> - Informacion en [[#Parametros K]], Debe ser los mismos en los routers del mismo AS
> - **1** = Habilitado
> - **0** = Deshabilitado
```
router eigrp [AS]
 metric weight [TOS] [BW] [DLY] [LOAD] [MTU]
```

## Modificar Valores Interfaz
> - Mas informacion en [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP#Valores de Interfaz|EIGRP]]
### BW
> Ancho de Banda del enlace, su configuracion es en `Kbps`
```
int [int S/S/P]
 bandwidth [BW]
```
### DLY
> Latencia del enlace, su configuracion es en `decenas de microsegundos`
```
int [int S/S/P]
 delay [DLY]
```
### Load
> Tiempo para medir estabilidad del enlace
```
int [int S/S/P]
 load-interval [sec]
```
### MTU
> Tamaño Paquete, su configuracion es en `bytes`
```
int [int S/S/P]
 mtu [MTU]
```

## Router STUB
> Se activa en el ultimo router de la topologia, para no continuar la comunicacion con siguientes routers, ahorrando ancho de banda
```
router eigrp [AS-number]
 eigrp stub
```


# Troubleshooting
Relacionados a la Relacion de Vecindad
- Interfaces
	- Inactivas o Pasivas: No envia ninguna informacion o paquetes
		- `sh ip int brief`
		- `sh run | s router eigrp`
		- `sh ip eigrp interfaces`: Muestra las interfaces Activas en IPv4
		- `sh ipv6 eigrp interfaces`: Muestra las interfaces Activas en IPv6
		- `sh ip eigrp interfaces detail`: Informacion detallada de interfaz IPv4
		- `sh ipv6 eigrp interfaces detail`: Informacion detallada de interfaz IPv6
	- Enrutamiento Incorrecto: No encontrara `network` para vecindad o sera diferente subred
		- `sh ip eigrp neighbors`
		- `sh ipv6 eigrp neighbors`
		- `sh ip eigrp neighbors detail`
		- `sh ipv6 eigrp neighbors detail`
		- `sh ip eigrp topology all-links`: Ver rutas aprendidas, redistribuidas posibles y en espera
		- `sh ipv6 eigrp topology all-links`
		- `show ip route eigrp`: Ver rutas en [[999 - Archivado/RIB|RIB]]
		- `sh ipv6 route eigrp`
		- `sh ip eigrp redistribution`: Ver rutas compartidas hacia EIGRP
		- `sh ipv6 eigrp redistribution
	- [[020 - Conceptos/020.1 - Administracion/ACL|ACL]]: Puede estar denegando paquetes a la direccion multicast 224.0.0.10
		- `sh ip int [int S/S/P]`
		- `sh access-list [access-list]`
	- Configuracion Inconsistente entre vecinos
		- ASN, Parametros K: 
			- `sh ip protocols`
			- `sh ipv6 protocols`
			- `sh ip eigrp metrics`: Metricas asociadas a Rutas
	- Autenticacion: El ID de clave y llavero deben coincidir y ser validos
		- `show run | s key chain`
		- `show key chain`
		- `show ip eigrp interfaces detail [int S/S/P)]`
	- Temporizadores: Pueden variar, pero respetar la formula $\text{Dead Time} < 3 \times \text{Hello Time}$
		- `show ip eigrp interfaces`
		- `show ip eigrp interface detail | s Hello-interval`

# Extra
## Open Source
Mayo 2016, IETF envio el [RFC 7868](https://datatracker.ietf.org/doc/html/rfc7868), en el cual desarrollan junto a cisco, un modelo EIGRP Abierto
Este Incluye:
- Mensajes Basicos de EIGRP: Hello, Update, Query, Reply y ACK para establecer y mantener vecindad e intercambiar informacion
- Algoritmo DUAL: Permite la seleccion de rutas y evita bucles de enrutamiento
- Calculo de Metricas: Parametros K, como BW, DLY, Conf, Load, MTU
- Estructura de Paquetes

No Incluye
- Funciones Avanzadas de [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP Nombrado|EIGRP Nombrado]]
- Optimizacion de Metricas por parte de CISCO