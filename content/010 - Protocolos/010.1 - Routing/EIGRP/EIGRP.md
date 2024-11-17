# Info
**E**nhanced **I**nterior **G**ateway **R**outing **P**rotocol, es un protocolo semi-propiertario(Info en [[#Extras#Open Source|#Extras]]) de Cisco, su funcionamiento se basa en AS(Autonomous System) y comparten informacion de topologias y vecinos, Entre AS no se generan adyacencia de vecinos.

## Datos
- Algoritmo calculo de Ruta: **Vector Distancia**, basado en el rumor ([Bellman-Ford](https://en.wikipedia.org/wiki/Bellman%E2%80%93Ford_algorithm))
- Motor de calculo de Rutas: **Dual** (**D**iffusing **U**pdate **Al**gorithm)
- Identificador de encabezado IP: 88
- Valores [[020 - Conceptos/020.3 - Fundamentos/Tabla Distancia Administrativa|Tabla Distancia Administrativa]]
	- EIGRP Interno -> 90
	- EIGRP Externo -> 170
- **Temporizadores** por Defecto
	- Alta Velocidad (Enlaces Ethernet)
		- Hello: 5 Sec
		- Hold: 15 Sec (3 x Hello)
	- Baja Velocidad (Enlaces Seriales o Frame Relay)
		- Hello: 60 Sec
		- Hold: 180 Sec (3 x Hello)

## Terminologia de Rutas EIGRP
- Metric (Metrica): Distancia local hasta el router vecino
- FD (Feasible Distance o Distancia Factible)
	- La metrica total desde el router local hasta el destino final
- RD o AD (Reported Distance o Distancia Reportada) o (Advertise Distance)
	- La metrica reportada por un vecino hasta el destino final
- FC (Feasibility Condition o Condicion de Factibilidad)
	- Requisito para ser **FS**, "$\text{RD or AD} < \text{FD}$", no acepta rutas con bucles de enrutamiento
- FS (Feasible Successor o Sucesor Factible)
	- Rutas que cumplen con **FC**, se mantienen como rutas de respaldo, cuando falla **SR**, se envia una **FS**
- SR (Successor Route o Ruta Sucesora)
	- Rutas ordenadas segun metrica en RIB, selecciona la del costo mas bajo con DUAL y es la ruta para alcanzar el destino (Next-Hop).

## Tipos de Mensaje IPv4
- (1) Hello: Descubre, Mantiene y Despide Vecinos
- (2) Request: Busca Informacion sobre Topologias
- (3) Update: Modificacion de Topologias
- (4) Query: Busca Caminos alternativos (Ruta Activa) se demora 3 minutos, aunque se pone ansioso y dura 90 segundos
- (5) Reply: Enviar respuesta sobre mensaje Query

## Tipos de Mensaje IPv6

| Packet | Fuente           | Destino          | Proposito                                           |
| ------ | ---------------- | ---------------- | --------------------------------------------------- |
| Hello  | Link-Local<br>IP | FF02::A          | Descubrir y Mantener Vecinos                        |
| ACK    | Link-Local<br>IP | Link-Local<br>IP | ACK de Updates                                      |
| Query  | Link-Local<br>IP | FF02::A          | Pide informacion de Rutas en un cambio de topologia |
| Reply  | Link-Local<br>IP | Link-Local<br>IP | Responde un Query                                   |
| Update | Link-Local<br>IP | Link-Local<br>IP | Forma Adjacencia                                    |
| Update | Link-Local<br>IP | FF02::A          | Cambios de Topologia                                |

## Parametros K
Parte del calculo de metrica en EIGRP, deben estar activados los mismos para generar adyacencia entre routers del AS
**Defecto K1+K3**
- K1 - BW (Bandwidth): 
- K2 - Load: 
- K3 - DLY (Delay):
- K4 - Reliability: 
- K5 - MTU(\*): No es una metrica usada por EIGRP. [Blog Explica](http://web.archive.org/web/20240229142953/http://blog.capaocho.net/2011/06/el-misterioso-caso-de-eigrp-y-la-mtu.html)

## Valores de Interfaz
Valores Referenciales que son parte del calculo de metrica en EIGRP, pueden variar entre routers del AS
- TOS (Opcional) -  Type of Service: Prioriza Servicios, no se usa
- BW - Bandwidth: Capacidad Maxima teorica del enlace, Medido en Kbps
- DLY - Delay: Tiempo de transito en la interfaz, Medido en Decenas de Microsegundos
- Conf - Reliability: SLA/Fiabilidad de la interfaz cada 30 minutos (0-255)
- Load: Utilizacion del enlace en 5 minutos (0-255)
- MTU: Tamaño maximo de transmision de cada trama, medido en Bytes

## Calculo de Metrica
Se calculan para generar un valor de metrica unico, calculado desde Cisco IOL
- **K* State**: Habilitado = 1, Deshabilitado = 0
- Calculo de Valores
	- **BW Value**
	- Formula: $\dfrac{10^7}{\text{BW in Kbps}}$
	- **Load Value**
	- Formula: $\dfrac{\text{K2 State}\times\text{BW Value}}{256-\text{IOL Load Value}}$
	- **Delay Value**
	- Formula: $\dfrac{\text{IOL Delay Value}}{10}$
	- **Reliability Value
	- Formula: $\dfrac{\text{K5 State}}{\text{K4 state}+\text{IOL Reliability Value}}$

- Calculo K1+K3 (Default) $$256\times((\text{K1 State}\times\text{BW Value})+(\text{K3 State}\times\text{Delay Value}))$$
- Calculo K1+K2+K3+K4+K5 $$\text{Metrica}=256\times[(\text{K1 State}\times\text{BW Value})+(\dfrac{\text{K2 State}\times\text{BW Value}}{256-\text{Load Value}})+(\text{K3 State}\times\text{Delay Value})]\times\dfrac{\text{K5 State}}{\text{K4 State}+\text{Reliability Value}}$$
## Redistribucion
En la [[010 - Protocolos/010.1 - Routing/Redistribucion|Redistribucion]] de rutas, las rutas que se incluyan a [[010 - Protocolos/010.1 - Routing/Redistribucion#EIGRP IPv4|EIGRP]] deben incluir las metricas de la interfaz, para que funcione correctamente.

## Router STUB
Diferente al significado en [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv2|OSPFv2]]
Se configura en los ultimos routers de la topologia, para no seguir enviando mensajes, ahorrando ancho de banda

## Balanceo de Carga
Vease [[#Terminologia de Rutas EIGRP]]
Las rutas que pasen **FC** se convierten en **FS**, la mejor ruta es la **SR**, para habilitar este balanceo con las rutas sobrantes se modifica el valor `maximum-path` (Por defecto el valor es 4) (Valor maximo 16)

Dentro de `eigrp topology` saldran los valores
- FS (Primer Valor)
- RD (Segundo Valor)

Variance permite multiplicar el valor **RD** de la ruta **SR** para que se acerque el maximo posible a su propio **FD**, permitiendo que las rutas **FS** participen del balanceo de carga.
Su calculo es $\dfrac{\text{FD de SR}}{\text{FD del FS mas alto}}$ y se aproxima el valor resultado al entero siguiente, ese sera el valor de `multiplicador de variance`

![Seleccion de rutas para RIB|1000](https://slink.proxylivy.work/image/1b4f8ac5-f751-4e56-8008-b118afd182da.png)

# Configuracion
## EIGRP IPv4
### Network
```
router eigrp [AS-number]
network [ip] [dec-mask]
```
### Router-ID
No necesita este valor para funcionar
```
router eigrp [AS-number]
eigrp router-id [router-id]
```
### Interfaces Pasivas
```
router eigrp [AS-number]
passive-interface [int S/S/P]
```
### Sumarizacion de Redes
Apoyo: [[020 - Conceptos/020.3 - Fundamentos/Sumarizacion de Redes|Sumarizacion de Redes]]
- No se envian rutas sumarizadas hasta que haya adyacencia
- Prefiere compartir rutas sumarizadas en vez de especificadas
- Interfaz `Null0` ficticia, tiene un [[020 - Conceptos/020.3 - Fundamentos/Tabla Distancia Administrativa|AD]] 5 para RIB, si una ruta que deberia estar sumarizada coincide con esta, se descarta, asi previene loops de enrutamiento
```
int [int S/S/P]
ip summary-address eigrp [AS-number] [IPv4-network/dec-mask]
```
### Modificar Temporizadores
Pueden funcionar con tiempos disparejos entre routers
Tiempos por Defecto en [[#Datos]]
```
int [int S/S/P]
ip hello-interval eigrp [AS-number] [hello-interval-sec]
ip hold-time eigrp [AS-number] [hold-time-sec]
```
### Autenticacion
Se configura en la vista de configuracion general `R1 (config)#`, ambas interfaces deben estar autenticadas para generar adyacencia
```
key chain [llavero]
key 1
key-string [password]
exit
exit
!
int [int S/S/P]
ip authentication mode eigrp [AS-number] md5
ip authentication key-chain eigrp [AS-number] [llavero]
```
### Modificar Estado Parametros K
Informacion en [[#Parametros K]], Debe ser los mismos en los routers del mismo AS
- **1** = Habilitado
- **0** = Deshabilitado
```
router eigrp [AS]
metric weight [TOS] [BW] [DLY] [LOAD] [MTU]
```
### Modificar Valores Interfaz
Informacion en [[#Valores de Interfaz]]
Se modifican los valores
#### Modificar BW
```
int [int S/S/P]
bandwidth [BW in Kbps]
```
#### Modificar DLY
```
int [int S/S/P]
delay [DLY en decenas de microsegundos]
```
#### Modificar Load
Modifica el tiempo para verificar la estabilidad del enlace
```
int [int S/S/P]
load-interval [sec]
```
#### Modificar MTU
```
int [int S/S/P]
mtu [MTU in bytes]
```
### Balanceo de Carga
Solo se hace en el router que hace el balanceo de carga, en los demas no es un problema
#### Modificar Variance
$\text{variance value} = \text{FD}\times\text{valor multiplicador de varianza}$
```
router eigrp [AS-number]
variance [valor multiplicador de varianza]
```
#### Modificar Interfaces Maximas
Default: 4
Desactivado: 1
Maximo: 16
```
router eigrp [AS-number]
maximum-path [maximum-path value]
```
### Router STUB
```
router eigrp [AS-number]
eigrp stub
```
## EIGRP IPv6
La configuracion va en la interfaz
### Network
Necesita habilitar el routing IPv6 con `ipv6 unicast-routing`
```
ipv6 router eigrp [AS-number]
!# Pasiva a los dispositivos Finales
passive-interface [int S/S/P]
!# Aplicar a la interfaz que comunica EIGRP
interface [int S/S/P]
ipv6 eigrp [AS-number]
```
### Router-ID
No es necesario para adyacencia, se puede usar estructuras IPv4 para identificar
```
ipv6 router eigrp [AS-number]
eigrp router-id [router-id]
```
### Sumarizacion IPv6
```
Interface [int S/S/P]
ipv6 summary-address eigrp [AS-number] [IPv6-network/Prefix]
```
### Modificar Temporizadores IPv6
```
ipv6 hello-interval eigrp [AS-number] [hello-interval-sec]
ipv6 hold-time eigrp [AS-number] [hold-interval-sec]
```
### Balanceo de Carga IPv6
$\text{variance value} = \text{FD}\times\text{valor multiplicador de varianza}$
```
router eigrp [AS-number]
variance [Valor Mulitplicador de varianza]
```
# Visualizacion
## Running-config Corto
Ver configuracion general de EIGRP
```
show run | section router eigrp
```
## Ver Interfaces
Muestra Interfaces Activas y Configuradas que participan de EIGRP, No mostrara interfaces **Apagadas** o **Pasivas**
```
show ip eigrp interfaces
show ipv6 eigrp interfaces
```
## Verificar Protocolos y Parametros
Verifica N° AS y Parametros K
```
show ip protocols
show ipv6 protocols
```
## Ver Tabla Vecindad
Muestra la lista de Vecinos EIGRP, su estado y tiempo de espera
```
show ip eigrp neighbor
show ipv6 eigrp neighbor
```
## Ver Tabla Topologica
Muestra las rutas aprendidas y redistribuidas, tanto disposibles como en espera
```
show ip eigrp topology all-links
show ipv6 eigrp topology all-links
```
## Verificar Tabla Rutas
Muestra las rutas en RIB, util para ver que rutas estan siendo usadas
```
show ip route eigrp
show ipv6 route eigrp
```
## Ver Redistribucion
Ver informacion sobre protocolos que comparten rutas hacia EIGRP
```
show ip eigrp redistribution
show ipv6 eigrp redistribution
```
## Ver tabla de Metricas
Verifica las metricas asociadas con las rutas EIGRP
```
show ip eigrp metrics
```
## Ver Temporizadores
```
show ip eigrp interface detail | s Hello-interval
```
# Troubleshooting
Relacionados a la Relacion de Vecindad
- Interfaces
	- Inactivas o Down: No envia ninguna informacion
		- `show ip int brief`: Debes ver que las interfaces se vean como "up" y "up"
	- Pasivas: No permite el envio ni recepcion de Paquetes Hello, la interfaz no se anuncia
		- `show ip protocols`
		- `show ip eigrp interfaces`: Muestra las interfaces activas (No muestra las pasivas)
		- `show ip eigrp interfaces detail`: Muestra informacion mas detallada sobre la interfaz
- Basadas en [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv4|IPv4]]
	- Enrutamiento Incorrecto: No encontrara `network` para vecindad
		- `show ip eigrp interfaces`
		- `show ip int brief`
		- `show ip eigrp neighbors`
		- `show ip eigrp neighbors detail`
		- `show running-config | section router eigrp`
	- Diferentes Subredes: Si los paquetes vienen de otra subred, son ignorados
	- [[020 - Conceptos/020.1 - Administracion/ACL|ACL]]: Puede estar denegando paquetes a la direccion multicast 224.0.0.10
		- `show ip int [int S/S/P]`
		- `show access-list [access-list]`
- AS
	- Inconsistencia Numero AS: Entre AS no se generan relaciones de vecindad
		- `show ip protocols`
	- Parametros K: Deben coincidir entre vecinos para formar adyacencia
		- `show ip protocols | include K`
	- Autenticacion: El ID de clave y llavero deben coincidir y ser validos
		- `show run | section key chain`
		- `show key chain`
		- `show ip eigrp interfaces detail [int S/S/P)]`
	- Temporizadores: Pueden variar, pero "$\text{Dead Time} < 3 \times \text{Hello Time}$"
		- `show ip eigrp interfaces`
		- `show ip eigrp interface detail | s Hello-interval`

# Extras
EIGRP Envia saludos a travez de la direccion multicast a travez de todas las interfaces que no sean pasivas

## Limites
El limite maximo de routers en todos los Autonomous System(AS) conectados es de `255` dentro de EIGRP, Cada router es un AS en un sistema, si hay 2 sistemas, son independientes y deben redistribuirse, es capaz de hacer balanceo de carga desigual, es un protocolo unico

Funciona en 32 bits
## Open Source
Mayo 2016, IETF envio el [RFC 7868](https://datatracker.ietf.org/doc/html/rfc7868), en el cual desarrollan junto a cisco, un modelo EIGRP Abierto
Este Incluye:
- Mensajes Basicos de EIGRP: Hello, Update, Query, Reply y ACK para establecer y mantener vecindad e intercambiar informacion
- Algoritmo DUAL: Permite la seleccion de rutas y evita bucles de enrutamiento
- Calculo de Metricas: Parametros K, como BW, DLY, Conf, Load, MTU
- Estructura de Paquetes

No Incluye:
- Funciones Avanzadas de EIGRP Nombrado
- Optimizacion de Metricas por parte de CISCO
- Compatibilidad y Configuracion detallada de modo nombrado

## Direccion Muticast
Para enviar saludos en las interfaces dentro de EIGRP
- IPv4: 224.0.0.10
- IPv6: FF02::A

## VS otros Protocolos
El mejor en su categoria, una mejora a [[010 - Protocolos/010.1 - Routing/RIP|RIP]] o IGRP
Debido a sus caracteristicas:
- Permite 255 Saltos
- Convergencia Rapida
- Balanceo de carga con carga desigual (Rutas sucesoras factibles)
- Mecanismos de recuperacion de informacion perdida
