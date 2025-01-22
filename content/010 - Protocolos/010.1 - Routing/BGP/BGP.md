# Info
**B**order **G**ateway **P**rotocol es el protocolo de enrutamiento principal en internet para intercambiar informacion de enrutamiento entre *AS*. 

## Datos
- Algoritmo de calculo: **Vector Ruta**
- Tipo de Protocolo: [[020 - Conceptos/020.3 - Fundamentos/EGP|EGP]]
- Puerto: TCP 179
- Valores [[020 - Conceptos/020.3 - Fundamentos/Tabla Distancia Administrativa|Tabla Distancia Administrativa]]
	- eBGP: 20
	- iBGP: 200
- TTL
	- eBGP: 1 (puede modificarse con `ebgp-multihop`)
	- iBGP: 255
- Versiones
	- [[010 - Protocolos/010.1 - Routing/BGP/BGP-3|BGP-3]]: Solo [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv4|IPv4]]
	- [[010 - Protocolos/010.1 - Routing/BGP/BGP-4|BGP-4]]: Usa AF, [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv4|IPv4]] y [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv6|IPv6]]
- Temporizadores
	- Keepalive: 60s
	- Holdtime: 180s (Se configura el valor mas bajo entre vecinos)

## Terminologia
- AS (Autonomous System): Conjunto de routers configurados en comun
- ASN (Autonomous System Number): Identificador Unico, Rango privado especificado por la IANA en [RFC6996](https://www.rfc-editor.org/rfc/rfc6996) y [Listado Oficial de Uso](https://www.iana.org/assignments/as-numbers/as-numbers.xhtml)
	- Rango Privado - 2 bytes(64.512 - 65.534)
	- Rango Privado - 4 bytes(4.200.000.000 - 4.294.967.294)
- AF (Adress-Family)
- R-ID (BGP Router-ID): identifica un router dentro del AS, usualmente su IPv4

## Estados BGP
> [!NOTE]- Funcionamiento BGP
> Fuente: [Wikipedia - BGP Adjacency Status](https://upload.wikimedia.org/wikipedia/commons/a/a8/BGP_FSM.svg)
>```mermaid
>graph TD
>    Idle <--> Connect
>    Connect --> OpenSent
>    Connect <--> Active
>    Active <--> OpenSent
>    Active --> Idle
>    OpenSent --> Idle
>    OpenSent --> OpenConfirm
>    OpenConfirm --> Idle
>    OpenConfirm --> Established
>    Established --> Idle
>```

1. **Idle**: Estado Inicial
2. **Connect**: Esperando que se complete la conexion TCP
3. **Active**: Conexion TCP no establecida, intentara nuevamente
4. **OpenSent**: Mensaje Open Enviado, esperando ACK
5. **OpenConfirm**: Envia ACK, esperando mensaje keepalive o Updates
6. **Established**: Sesion Establecida, comienza envio de mensajes Updates
7. **Updates**: Intercambio de actualizaciones de topologia BGP

## Mensajes BGP
- **Open**: Iniciado despues de establecer conexion TCP para sesion BGP
- **Keepalive** (60s): Envia ACK para mantener la sesion activa
- **Update**: Envia Anuncios y retiros de rutas
- **Notification**: Enviado cuando se detecta un error, utilizado para terminar una sesion (codigo y datos)

## Atributos Generales

| Atributo         | Categoria                | Descripcion                                                      |
| ---------------- | ------------------------ | ---------------------------------------------------------------- |
| AS-Path          | Well-Know, Mandatory     | Lista de AS enrutados                                            |
| Next-Hop         | Well-Know, Mandatory     | Siguiente Salto                                                  |
| Origin           | Well-Know, Mandatory     | Codigo origen de la ruta<br>Orden: (i)IGP, (e)EGP, (?)Incompleto |
| Local-Preference | Well-Know, Discretionary | Preferencia Local (↑)<br>Default: 100                            |
| MED              | Optional, Non-Transitive | Metrica para elegir desde multiples salidas<br>Default: 0        |
| Weight           | Optional, Local          | Propietario de Cisco<br>Se prefiere el valor mas alto            |

## Atributos Extra

| Atributo         | Categoria                   | Descripcion                                                                 |
| ---------------- | --------------------------- | --------------------------------------------------------------------------- |
| Aggregator       | Optional,<br>Transitive<br> | ID y AS del router para sumarizacion<br>No se usa para seleccionar camino   |
| Atomic aggregate | Well-Know,<br>discretionary | Al sumarizar nombra los AS<br>No se usa para seleccionar camino             |
| Clusted ID       | Optional,<br>nontransitive  | Identifica un cluster, Route Reflector<br>No se usa para seleccionar camino |
| Community        | Optional,<br>transitive     | Etiqueta de Prefijos<br>No se usa para seleccionar camino                   |
| Originator ID    | Optional,<br>nontransitive  | Identifica al Router Reflector<br>No se usa para seleccionar camino         |
## Proceso de Seleccion de rutas
1. *Weight* (↑): Propietario de Cisco
2. *Local-Preference* (↑)
3. Rutas originadas localmente (`network` o `aggregate-address`)
4. *AS-Path* mas corto
5. *Origin* mas corto (IGP < EGP < Incompleto)
6. *Med* (↓)
7. eBGP > iBGP
8. Vecino IGP mas cercano
9. Ruta mas antigua eBGP
10. Vecino BGP Router-ID (↓)
	- En caso de [[#RR]] se sustituye con *Originator ID*
11. Vecino IP (↓)

## Regla del Horizonte Dividido iBGP
Split Horizon, es un metodo para que en sesiones iBGP, las rutas aprendidas por un vecino no se propagan para evitar loops de enrutamiento, **RR** (Router Reflector) configurado en el router con mas vecinos para poder controlar y progagar rutas entre vecinos.

Debido a la regla del Horizonte Dividido (Split Horizon), las rtas aprendidas de un vecino iBGP no se propagan, se configura RR en el router que mas vecinos iBGP tenga directamente conectados.

Ejemplo de configuracion en [[010 - Protocolos/010.1 - Routing/BGP/BGP-3#Router Reflector iBGP|BGP-3]]

## Multihop eBGP
El TTL en una comunicacion eBGP es de uno, por lo que solo puede llegar al siguiente Router, debera hacerlo a travez de saltos intermedios. Multihop permite aumentar el valor de TTL al paquete BGP, si no se especifica un valor, este sera del maximo (255). Ejemplo de configuracion en [[010 - Protocolos/010.1 - Routing/BGP/BGP-3#MultiHop eBGP|BGP-3]]

# Verificacion General
- `sh ip bgp`: Tabla enrutamiento de BGP con vecinos y rutas
	- (\*) son rutas validas 
	- (>) indican mejor ruta
	- (i) para anuncios iBGP (e) para anuncios eBGP
	- **s** son rutas que se suprimen (Porque hay rutas resumen)
	- **d** son rutas penalizadas por no ser confiables
	- **h** son rutas historicas, por lo que posiblemente ya no existan
	- **r** son rutas con fallos y no pasaran a la [[999 - Archivado/RIB|RIB]]
	- **S** son rutas viciadas, que continen inconvenientes
- `sh ip bgp summary`: Tabla enrutamiento de BGP resumida
- `sh ip bgp neighbors`: Vecinos BGP
- `sh ip bgp neighbor [ip] received-routes`: Ve si esa ruta fue aceptada por un vecino
- `sh ip bgp neighbor [ip] advertised-routes`: Ve las rutas posibles a esa ip a travez de vecinos
- `sh ip protocols`: Funcionamiento y Configuracion BGP
- `sh run | s router bgp`: Configuracion BGP en el router
- `sh tcp brief all`: Muestra todas las conexiones TCP (Sesiones BGP)
- `sh bgp ipv4 unicast`: Tabla BGP IPv4 Interna
- `sh bgp ipv4 unicast summary`: Resumen BGP, Numero AS Local, adyancencia, prefix
- `sh bgp ipv4 unicast neighbor [ip]`: Informacion especifica de un vecino BGP
- `sh bgp ipv4 unicast neighbor | inc ^BGP neighbor |Local port|Foreign port`: mostrar vecinos y puertos usados
- `sh bgp ipv4 unicast neighor | inc BGP neigbor|TTL`: Ver TTL configurado

# Troubleshooting General
> [!NOTE] Nota
> BGP no funciona sobre rutas por defecto | No [[010 - Protocolos/010.1 - Routing/Ruta Estatica|Ruta Estatica]]

- Numero de AS Incorrecto
	- `show ip protocols`
- Configuracion de Vecinos Incorrecta / Inalcanzable (state/pfxRcd no debe estar en 0 o idle o active)
	- `show ip int brief`: IP Incorrecta o Superpuesta
	- `show ip route`
	- `show ip bgp`
	- `show bgp ipv4 unicast summary`
	- `ping [ip-neighbor]`: Para tener adyacencia deben conocerse
- ACL Bloqueando el puerto comunicacion BGP
	- `show access-lists`
- Interfaces Inactivas (Pasivas)
	- `show ip protocols`
- [[010 - Protocolos/010.1 - Routing/Redistribucion|Redistribucion]] incorrecta
	- `show ip route`
	- `show ip bgp sum`
- Desajuste en los temporizadores | Info en [[#Datos]]]
	- `show bgp timers`
- Estado Debug
	- `debug ip bgp`: Comportamiento BGP
	- `debug ip bgp updates`: Actualizaciones BGP

# Extra
El comando `network` permite que una ruta de la RIB, vaya a la tabla de BGP.

Los ASN se pueden usar para enrutar pero no para internet (enviado por `as-path` de la tabla BGP), crearia problemas, se puede detener con el comando `neighbor [ip] remove-private`

- Permite filtrar rutas con [[020 - Conceptos/020.1 - Administracion/ACL|ACL]] y [[020 - Conceptos/020.1 - Administracion/Prefix-List|Prefix-List]] para poder gestionar el trafico con [[020 - Conceptos/020.1 - Administracion/Route-Map|Route-Map]] y [[020 - Conceptos/020.1 - Administracion/PBR|PBR]]
- La [[020 - Conceptos/020.3 - Fundamentos/Sumarizacion de Redes|Sumarizacion]] puede ser sin o con supresion de redes
- Permite ser integrados en uso de [[010 - Protocolos/010.3 - Comunicaciones/VPN|VPN]] con [[010 - Protocolos/010.1 - Routing/MPLS|MPLS]]

## Estandares
- BGP-3
	- Definido en [RFC1267](https://www.rfc-editor.org/rfc/rfc1267)
- BGP-4
	- Definido en [RFC4271](https://www.rfc-editor.org/rfc/rfc4271)
	- BGP Operations and Security | [RFC 7454](https://www.rfc-editor.org/rfc/rfc7454)
	- BGP SEC | [RFC8205](https://www.rfc-editor.org/rfc/rfc8205)