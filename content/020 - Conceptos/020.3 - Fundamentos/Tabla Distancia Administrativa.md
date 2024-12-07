Permite ver la fuente de preferencia para la eleccion de rutas en la [[999 - Archivado/RIB|RIB]], entre menor el valor, mejor

| Fuente                                                                  | Valor Default |
| ----------------------------------------------------------------------- | ------------- |
| Interfaz Conectada                                                      | 0             |
| [[010 - Protocolos/010.1 - Routing/Ruta Estatica\|Ruta Estatica]]       | 1             |
| Ruta Resumen de [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP\|EIGRP]] | 5             |
| External [[010 - Protocolos/010.1 - Routing/BGP/BGP\|BGP]]                  | 20            |
| [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP\|EIGRP]] Interno         | 90            |
| IGRP(~~deprecated~~)                                                    | 100           |
| [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv2\|OSPFv2]]                | 110           |
| IS-IS                                                                   | 115           |
| [[010 - Protocolos/010.1 - Routing/RIP\|RIP]]                           | 120           |
| [[020 - Conceptos/020.3 - Fundamentos/EGP\|EGP]]                        | 140           |
| ODR                                                                     | 160           |
| [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP\|EIGRP]] Externo         | 170           |
| Internal [[010 - Protocolos/010.1 - Routing/BGP/BGP\|BGP]]                  | 200           |
| Invalido                                                                | 255           |

## Modificar Valor Tabla
### Ruta estatica por Defecto
[[010 - Protocolos/010.1 - Routing/Ruta Estatica|Ruta Estatica]]
```
0.0.0.0 0.0.0.0 [ip-destino] [ad-metric]
```
### EIGRP Clasico Interno
Nota: EIGRP Externo no puede variar
[[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP Numerado|EIGRP Numerado]]
```
distance eigrp 90 [ad-metric]
```
### OSPF
[[010 - Protocolos/010.1 - Routing/OSPF/OSPFv2|OSPFv2]]
```
distance ospf external [ad-metric]
```