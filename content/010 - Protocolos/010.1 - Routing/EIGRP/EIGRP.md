# Info
**E**nhanced **I**nterior **G**ateway **R**outing **P**rotocol, comparte informacion de topologias y vecinos en un mismo AS

## Datos
- Funciona mediante AS (Autonomous System)
- Algoritmo calculo de Ruta: **Vector Distancia**, basado en el rumor y [Bellman-Ford](https://en.wikipedia.org/wiki/Bellman%E2%80%93Ford_algorithm)
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
- **Metric** (Metrica): Distancia local hasta el router vecino
- **FD** (Feasible Distance o Distancia Factible)
	- La metrica total desde el router local hasta el destino final
- **RD o AD** (Reported Distance o Distancia Reportada) o (Advertise Distance)
	- La metrica reportada por un vecino hasta el destino final
- **FC** (Feasibility Condition o Condicion de Factibilidad)
	- Requisito para ser **FS**, "$\text{RD or AD} < \text{FD}$", no acepta rutas con bucles de enrutamiento
- **FS** (Feasible Successor o Sucesor Factible)
	- Rutas que cumplen con **FC**, se mantienen como rutas de respaldo, cuando falla **SR**, se envia una **FS**
- **SR** (Successor Route o Ruta Sucesora)
	- Rutas ordenadas segun metrica en RIB, selecciona la del costo mas bajo con DUAL y es la ruta para alcanzar el destino (Next-Hop).

## Tipos de Mensaje IPv4
1. Hello: Descubre, Mantiene y Despide Vecinos
2. Request: Busca Informacion sobre Topologias
3. Update: Modificacion de Topologias
4. Query: Busca Caminos alternativos (Ruta Activa) se demora 3 minutos, aunque se pone ansioso y dura 90 segundos
5. Reply: Enviar respuesta sobre mensaje Query

## Tipos de Mensaje IPv6

| Packet | Fuente           | Destino          | Proposito                                           |
| ------ | ---------------- | ---------------- | --------------------------------------------------- |
| Hello  | Link-Local<br>IP | FF02::A          | Descubrir y Mantener Vecinos                        |
| ACK    | Link-Local<br>IP | Link-Local<br>IP | ACK de Updates                                      |
| Query  | Link-Local<br>IP | FF02::A          | Pide informacion de Rutas en un cambio de topologia |
| Reply  | Link-Local<br>IP | Link-Local<br>IP | Responde un Query                                   |
| Update | Link-Local<br>IP | Link-Local<br>IP | Forma Adjacencia                                    |
| Update | Link-Local<br>IP | FF02::A          | Cambios de Topologia                                |

## Direcciones Multicast
- IPv4: 224.0.0.10
- IPv6: FF02::A

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
## Balanceo de Carga
Vease [[#Terminologia de Rutas EIGRP]]
Las rutas que pasen **FC** se convierten en **FS**, la mejor ruta es la **SR**, para habilitar este balanceo con las rutas sobrantes se modifica el valor `maximum-path` (Por defecto el valor es 4) (Valor maximo 16)

Dentro de `eigrp topology` saldran los valores
- FS (Primer Valor)
- RD (Segundo Valor)

Variance permite multiplicar el valor **RD** de la ruta **SR** para que se acerque el maximo posible a su propio **FD**, permitiendo que las rutas **FS** participen del balanceo de carga.
Su calculo es $\dfrac{\text{FD de SR}}{\text{FD del FS mas alto}}$ y se aproxima el valor resultado al entero siguiente, ese sera el valor de `multiplicador de variance`

![Seleccion de rutas para RIB|1000](https://slink.proxylivy.work/image/1b4f8ac5-f751-4e56-8008-b118afd182da.png)

# Extras
- EIGRP envia saludos a travez de la direccion multicast a travez de todas las interfaces que no sean pasivas
- Capaz de hacer balanceo de carga desigual, es un protocolo unico

## Limites
- El limite de routers en un AS es de `255`
- Entre sistemas no hay redistribucion automatica, y no genera adyacencia entre vecinos de diferentes AS

## VS otros Protocolos
El mejor en su categoria, una mejora a [[010 - Protocolos/010.1 - Routing/RIP|RIP]] o IGRP

Debido a sus caracteristicas:
- Permite 255 Saltos
- Convergencia Rapida
- Balanceo de carga con carga desigual (Rutas sucesoras factibles)
- Mecanismos de recuperacion de informacion perdida
