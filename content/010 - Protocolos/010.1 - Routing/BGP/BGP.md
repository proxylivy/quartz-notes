# Info
**B**order **G**ateway **P**rotocol es el protocolo de enrutamiento principal en internet para intercambiar informacion de enrutamiento entre AS.

## Datos
- Algoritmo calculo de Ruta: **Vector Ruta**
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
- AS: Autonomous System: Conjunto de routers configurados en comun
- ASN: Autonomous System Number
	- Rango Privado - 2 bytes(64.512 - 65.534)
	- Rango Privado - 4 bytes(4.200.000.000 - 4.294.967.294)
- AF: Adress-Family
- R-ID: BGP Router-ID, identifica un router dentro del AS, usualmente su IPv4

## Estados BGP
Ayuda: [Wikipedia - BGP Adjacency Status](https://upload.wikimedia.org/wikipedia/commons/a/a8/BGP_FSM.svg)
1. Idle: Estado Inicial
2. Connect: Esperando completar conexion TCP
3. Active: Conexion TCP no establecida, reintentar
4. OpenSent: Mensaje Open Enviado, esperando ACK
5. OpenConfirm: Envia ACK, esperando mensaje keepalive o Updates
6. Established: Sesion Establecida, comienza envio de mensajes Updates
7. Updates: Intercambio de actualizaciones de topologia BGP

## Mensajes BGP
- Open: Iniciado despues de establecer conexion TCP para sesion BGP
- Keepalive (60s): Envia ACK a los open para mantener la sesion activa
- Update: Envia Anuncios y retiros de rutas
- Notification: Envio de Errores y cierre de sesion con codigo y datos
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

# Verificacion
- `sh ip bgp`: tabla enrutamiento BGP
- `sh ip bgp summ`: Resumen estado vecinos BGP

# Troubleshooting
- Numero de AS Incorrecto
- Configuracion de Vecinos Incorrecta
- ACL Bloqueando el puerto BGP
- Interfaces Pasivas mal configuradas
- Capa 3 con errores
	- IP incorrecta o superpuesta
	- Vecino no alcanzable
- Alcanze de Vecino a travez de [[010 - Protocolos/010.1 - Routing/Ruta Estatica|Ruta Estatica]]
	- BGP no funcioan sobre rutas por defecto
- TTL Expirado
- Autenticacion Incorrecta
	- MD5
- Community Peers mal configurados
- Desajuste en los temporizadores
- [[010 - Protocolos/010.1 - Routing/Redistribucion|Redistribucion]] incorrecta

# Extra
El comando `network` permite que una ruta de la RIB, vaya a la tabla de BGP.

Los ASN se pueden usar para enrutar pero no para internet (enviado por `as-path` de la tabla BGP), crearia problemas, se puede detener con el comando `neighbor [ip] remove-private`


- Rutas iBGP no se prograpan por la regla del horizonte dividido | [[#Router Reflector]]
- Permite filtrar rutas con [[010 - Protocolos/010.1 - Routing/Route-Map|Route-Map]], [[020 - Conceptos/020.1 - Administracion/Prefix-List|Prefix-List]] y [[999 - Archivado/PBR|PBR]]
- La [[020 - Conceptos/020.3 - Fundamentos/Sumarizacion de Redes|Sumarizacion]] puede ser sin o con supresion de redes
