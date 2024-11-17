# Info
## Definicion
**O**pen **S**hort **P**ath **F**irst es un protocolo de enrutamiento de estado de enlace abierto basado en [RFC 2328](https://www.rfc-editor.org/rfc/rfc2328)

## Funcionamiento
envia [[010 - Protocolos/010.1 - Routing/OSPF/Paquetes OSPF#LSA|LSA]] a los vecinos, se almacenan en [[010 - Protocolos/010.1 - Routing/OSPF/Paquetes OSPF#LSDB|LSDB]] y su funcionamiento esta basado en Areas detectando automaticamente los cambios y calcula rutas en poco tiempo y con un trafico minimo en la red

## Datos
- Algoritmo de calculo de Ruta: **Estado de Enlace**
- Motor de calculo de Rutas: **SPF** ([Dijistra](https://es.wikipedia.org/wiki/Algoritmo_de_Dijkstra)) ([Info](https://www.freecodecamp.org/espanol/news/algoritmo-de-la-ruta-mas-corta-de-dijkstra-introduccion-grafica/))
- Identificador de encabezado IP: 89
- Valores [[020 - Conceptos/020.3 - Fundamentos/Tabla Distancia Administrativa|Tabla Distancia Administrativa]]
	- OSPF -> 110
- **Temporizadores** por defecto en [[#Tipos de Redes]]
- Uso de Tablas
	- Vecinos: Otros Routers
	- Topologia: Red Entera
	- Enrutamiento: Rutas Posibles

## Caracteristicas
- Convergencia Rapida
- Escabilidad para redes grandes
- Uso Eficiente del ancho de banda
- Soporte para VLSM y CIDR

### Paquetes OSPF
Nota: LS (Link State)
- [[010 - Protocolos/010.1 - Routing/OSPF/Paquetes OSPF#Hello|Hello]]
- [[010 - Protocolos/010.1 - Routing/OSPF/Paquetes OSPF#DBD|DBD]] (Database Description)
- [[010 - Protocolos/010.1 - Routing/OSPF/Paquetes OSPF#LSDB|LSDB]] (LS Database)
- [[010 - Protocolos/010.1 - Routing/OSPF/Paquetes OSPF#LSA|LSA]] (LS Advertisement)
- [[010 - Protocolos/010.1 - Routing/OSPF/Paquetes OSPF#LSR|LSR]] (LS Request)
- [[010 - Protocolos/010.1 - Routing/OSPF/Paquetes OSPF#LSU|LSU]] (LS Update)
- [[010 - Protocolos/010.1 - Routing/OSPF/Paquetes OSPF#LSK|LSK]] (LS ACK)

## Conceptos Fundamentales
- Router-ID: Identificador Unico de 32 bits para cada router
- Tipos de Routers
	- ABR (Area Border Router o Router de area de borde): se conectan en el area 0 y otra area, son los responsables de anunciar rutas de un area y direccionarlas a otra area diferente, calculan un arbol SPF en cada area que participan.
	- ASBR (Autonomous System Boundary Router): Conecta 2 protocolos de Capa 3
- Roles de OSPF
	- DR (Designated Router)
		- Router principal de redes multiacceso, generan adyacencia con el y sincroniza las rutas de OSPF
	- BDR (Backup Designated Router) 
		- Router de respaldo en caso que DR caiga
	- DROther
		- Otros routers dentro de OSPF que no sean ni DR, ni BDR

## Areas
OSPF opera en dos Niveles, el trafico entre areas siempre pasa por el area 0
- Area 0 (Barebone o Troncal)
- Demas Areas

## Tipos de Redes
- FRMI (Frame Relay Main Interface)
- FRMS (Frame Relay Multipoint Sub-Interface)
- FRP2PS (Frame Relay Point-to-Point Sub-Interface)

| Tipo                             | Default                | DR/BDR en OSPF Hello | Hello | Wait | Dead |
| -------------------------------- | ---------------------- | -------------------- | ----- | ---- | ---- |
| Broadcast                        | En enlaces Ethernet    | SI                   | 10    | 40   | 40   |
| Non-Broadcast                    | FRMI/FRMS              | SI                   | 30    | 120  | 120  |
| Point-to-Point (P2P)             | FRP2PS                 | NO                   | 10    | 40   | 40   |
| Point-to-Point Multipoint (P2MP) | Not enabled as default | NO                   | 30    | 120  | 120  |
| Loopback                         | En loopback            | N/A                  | N/A   | N/A  | N/A  |

## Procesos de Eleccion
### Router-ID
- Router-ID Configurado Manualmente
- IP mas alta de loopback
- IP activa mas alta

### DR-BDR-DROther
NOTA: Si DR se reinicia, perdio su silla, tendra que reinicia proceso ospf.
1. El router-id mas alto
2. Prioridad mas alta (0 baja, 255 alta) 
3. IP Activa mas alta

### Estados Router OSPF
TO-DO: Hace un Dibujo y Explica mejor
- Nota Externa: [Adjacencia Router](https://upload.wikimedia.org/wikipedia/commons/8/8d/OSPF-Adjacency-process.drawio.png) y [Adjacencia Database](https://upload.wikimedia.org/wikipedia/commons/3/36/OSPF-Adjacency-process-Neighbor_state_changes_%28Database_Exchange%29.drawio.png)
- Attempt: No ha recibido informacion pero esta intentando comunicarse, util en topologias NBMA
- Init: Recibio un paquete "Hello" desde otro router, pero la comunicacion bidireccional aun no se establece
- 2-Way: La comunicacion Bi-Direccional se establecio, la eleccion de DR y BDR se elige ahora
- ExStart: Empieza la Adyacencia, identifican el router primario y segundario para la sincronizacion LSDB
- Exchange: Routers intercambian Paquetes DBD/DDP 
- Loading: Paquetes LSR se envian a los vecinos, preguntando por recientes LSAs que hayan descubierto pero no recibido en el Estado Exchange
- Full: Los routers vecinos tienen adyacencia

## Sumarizacion
La [[020 - Conceptos/020.3 - Fundamentos/Sumarizacion de Redes|Sumarizacion de Redes]] en OSPF reduce la cantidad de rutas en la [[999 - Archivado/RIB|RIB]], disminuye la complejidad de la [[010 - Protocolos/010.1 - Routing/OSPF/Paquetes OSPF#LSDB|LSDB]] en cada area.
Los [[010 - Protocolos/010.1 - Routing/OSPF/Paquetes OSPF#LSA|LSA]] tipo 1 y tipo 3 detallados se remplazan con [[010 - Protocolos/010.1 - Routing/OSPF/Paquetes OSPF#LSA|LSA]] Tipo 3 resumidos, ocultando los prefijos especificos y evitar calculos complejos de SPF.

# Extras
- Area Especial Stub:  El area 0 nunca va con STUB, y se debe hacer en todos los routers del area. STUB → LSA Tipo 4 y 5 se convierten en ruta por defecto (OE2/OE1 → 0.0.0.0/0)

- Desventajas de una sola area
	- LSDB Grande: Mas memoria y calculos SPF lentos
	- No hay sumarizacion de rutas

- Documentacion
	- OSPF esta documentado en [RFC 2328](https://datatracker.ietf.org/doc/html/rfc2328)
- Mejores Practicas
	- Area 0 (Backbone) no debe tener mas de 6-7 routers para un rendimiento optimo
	- Utilizar areas permite disminuir el tamaño de [[010 - Protocolos/010.1 - Routing/OSPF/Paquetes OSPF#LSDB|LSDB]]
- Limites
	- El costo maximo de OSPF es de 16 bits (65.535)
	- OSPF no puede hacer balanceo de carga como [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP|EIGRP]]