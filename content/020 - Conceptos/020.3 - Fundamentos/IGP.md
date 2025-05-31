# Info
**I**nterior **G**ateway **P**rotocol, es un conjunto de protocolos dentro de un AS(Autonomous System).

## Vector-Distancia
Cada nodo solo tiene informacion sobre los vecinos y sincronizan las tablas de enrutamiento que usan en cada ciclo, usa el algoritmo de [Bellman-Ford](https://es.wikipedia.org/wiki/Algoritmo_de_Bellman-Ford). 
Son mas sencillos de usar, pero son mas lentos y adecuados para redes pequeñas

### Tipos
- [[010 - Protocolos/010.1 - Routing/RIP|RIP]] (v1, v2, ng): UDP
- IGRP: ~~Deprecated~~ TCP
- [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP|EIGRP]]: TCP

## Enlace-Estado
Cada Nodo posee informacion sobre toda la topologia de red, las rutas finales y saltos son los mejores posibles entre nodos, se comparte la informacion para contruir mapa de conectividad
Calcula rutas en poco tiempo y con un trafico minimo en la red

### Tipos
- [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv2|OSPFv2]] y [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv3|OSPFv3]]
- [[010 - Protocolos/010.1 - Routing/IS-IS|IS-IS]]