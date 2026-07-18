# Info
## Caracteristicas
- Formato de paquete
- Direccion Multicast (224.0.0.5 y 224.0.0.6)

## Requerimientos para vecindad
- Router-ID unicos entre los dispositivos de todo el dominio OSPF
- Interfaces comparten una subred en comun, ospf envia saludos a travez del gateway
- Las MTU (Unidad Maxima de transmision) debe coincididr en las interfaces, OSPF no permite fragmentacion
- El Router-ID debe coincider con el segmento(network)
- El DR debe coincidir en el segmento
- Los temporizadores de saludo y tiempo muerto en OSPF deben coincidir
- El tipo de autenticacion y las credenciales deben coincidir en el segmento
- Los indicadores de tipo de area deben coincidir con el segmento (Stub, NSSA)


# Aplicacion

Cisco: [[030 - Plataformas/Cisco IOS/OSPFv2|OSPFv2]]