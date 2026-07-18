# Info
## Diferencias con OSPFv2
Vease [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv2|OSPFv2]]
- Uso de direcciones Link-Local para adyacencias
- Multiples instancias por interfaz
- Cambios en el formato del paquete

## Caracteristicas
- Soporte para multiples familias de direcciones
- Mas Tipos de LSA
- Ya no hay prefijo IP en los encabezados
- Inundacion de LSA: determina el alcance
- El formato se ejecuta directamente sobre IPv6
- ID de Router: Identifica a los vecinos
- Ya no hay autenticacion y se usa IPsec
- Adyacencia Vecinas a travez de direccionamiento local
- Varias Instancias que incluyen su propio ID para formar adyacencia

## Tipos de LSA

| Tipo de LSA | Nombre            | Descripcion                                                                                                                                                                                     |
| ----------- | ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0x2001      | Router            | Todos los routers<br>LSA con el estado y el costo                                                                                                                                               |
| 0x2002      | Network           | Router Borde de Area<br>LSA para anunciar los routers unidos al resignado, incluyendose                                                                                                         |
| 0x2003      | Interarea Prefix  | Router Borde de Area<br>LSA para describir rutas a prefijos IPv6 de otras areas                                                                                                                 |
| 0x2004      | Interarea Router  | ASBR<br>LSA para anunciar la direccion del ASBR en otras areas                                                                                                                                  |
| 0x2005      | AS external       | ASBR<br>LSA para anunciar rutas por defecto o rutas aprendidas de otros protocolos                                                                                                              |
| 0x2007      | NSSA              | ASBR<br>                                                                                                                                                                                        |
| 0x2008      | Link              | LSA para mapear todo los prefijos de direcciones de unidifusion global<br>asociada a desde una interfaz a la ip de la interfaz local del router<br>Se comparte solo en vecinos del mismo enlace |
| 0x2009      | Intra-area Prefix | LSA para anunciar prefijos IPv6 que estan asociado a un router,stub o segmento                                                                                                                  |

# Aplicacion

Cisco: [[030 - Plataformas/Cisco IOS/OSPFv3|OSPFv3]]
