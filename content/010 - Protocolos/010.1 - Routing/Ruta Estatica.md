# Info


# Configuracion
## Estructura
```
ip route [ip-destino] [mascara-destino] [ip-next-hop]
```
## Ruta Estica
```
ip route 0.0.0.0 0.0.0.0 [ip_salto]
```
## Ruta Estatica Flotante
```
ip route 0.0.0.0 0.0.0.0 [ip_salto] [ad-value]
```
## Ruta Estatica Doblemente Especificada
Encontrar interfaz de la ip especificada
```
ip route [ip-salida] [ip-next-hop] [int-salida] [ad-value]
```

## Anunciar en Protocolos
Se debe hacer en el router de borde que contiene los saltos
### RIP
Ve [[010 - Protocolos/010.1 - Routing/RIP|RIP]]
```
!# IPv4
router rip
default-information originate
```
### OSPF
Ve [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv2|OSPFv2]]
```
!# IPv4
router osfp [proceso]
default-information originate
exit
!# IPv6
ipv6 router ospf [proceso]
default-information originate include-connected
```
### EIGRP
EIGRP no tiene el comando `default-information originate` por lo que comparte todas las rutas con el `[AS]` mediante `redistribute static`
Ve [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP|EIGRP]]
```
!# IPv4
router eigrp [AS]
redistribute static
exit
!# IPv6
router eigrp [AS]
redistribute static include-connected
```

# Visualizacion
`show running-config | include ip route` -> Ver rutas IPv4
`show running-config | include ipv6 route` -> Ver rutas IPv6
`show ip route`
`show ip route [ip]` -> Ver informacion detallada ruta, donde va y donde la aprendio
