# Info
**P**oliticas **B**asadas en **R**outing es una tecnica para dirreccionar con reglas las rutas que llegan.

Puede enviar trafico basandose en
- Direccion IP de origen y destino
- Protocolo
- Longitud del prefijo
- [[010 - Protocolos/010.1 - Routing/Route-Map|Route-Map]]
- [[020 - Conceptos/020.1 - Administracion/Prefix-List|Prefix-List]]
- [[020 - Conceptos/020.1 - Administracion/ACL|ACL]]

El contenido se aplica en la interfaz de entrada

# Configuracion
## Aplicar politica de route-map
```
ip policy route-map [router-map-name]
```


# Visualizacion
`show ip policy`
`debug ip policy` -> Mostrar toda la informacion sobre el funcionamiento de PBR