Estos paquetes se usan para comunicarse entre blablabla
## Hello
Descubrir y mantener vecinos (neighbors).
## DBD
Database Description, Resumen del contenido en una base de datos, se envian cuando hay adyacencia, describen el contenido de LSDB
## LSA
Anuncios de estado de enlace que contienen el estado y costo del enlace
Tipos de LSA
- Tipo 1 (Routing Link) (O)
	- Sincroniza las rutas de LSDB en una sola area de OSPF
- Tipo 2 (Network Link) (O)
	- Sirve para identificar al router DR y otros sepan que es el
	- Solo existe en conexiones multiacceso, no se genera en conexiones seriales ni se mantiene info en LSDB
- Tipo 3 (Summary) (OIA)
	- Routers ABR comunican rutas de un area a otra transformando los LSA de tipo 1 -> tipo 3, solo cuando estan contiguas al area 0 (Excepto con Virtual Link)
- Tipo 4 (Summary ASBR) 
	- OE1
		- Metrica varia y se suma con cada paso
		- Rutas Externas sumarizadas de otro protocolo
	- OE2
		- Metrica estatica y exacta
		- Rutas Externas sumarizadas de otro protocolo
- Tipo 5 (AS Exteral) (OE1/OE2)
	- Rutas Externas


LSA Que no vemos
- LSA Tipo 6 (Multicast OSPF)
- LSA Tipo 7 (Not-so-stubby area): No se trabaja porque es de EIGRP (NSSA)
- LSA Tipo 8 (External Atribute LSA for BGP)
### Rutas por Areas
- O: OSPF Intra-Area
- OIA: OSPF Inter-Area
- OE2/OE1: OSPF Externo
## LSK
(Link-State Acknowledgment): Confirma que recibio un LSU

## LSDB
Link-State Database o Base de datos de estado de enlace
Almacena [[010 - Protocolos/010.1 - Routing/OSPF/Paquetes OSPF#LSA|LSA]] de forma local y mantiene una copia con los demas routers de OSPF, creando una topologia(mapa) sin loops a travez del camino mas corto.

## LSR
(Link-State Request): Pide una actualizacion del contenido de LSDB

## LSU
(Link-State Update): Le envian LSA explicitos a LSR para actualizar el contenido de LSDB