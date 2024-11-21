# Info
Route-map permite manipular perimetros, atributos, rutas, tipos de paquetes, etc. Se editan preferente de manera individual

El valor de tag aumenta de 10 en 10

> [!NOTE] Nota
> Regla `IN`: Rutas que llegan
> Regla `OUT`: Rutas que manda

## Usos Comunes de Route-Map
- Filtrado de rutas durante la [[010 - Protocolos/010.1 - Routing/Redistribucion|Redistribucion]]: Luego de Filtrar con [[020 - Conceptos/020.1 - Administracion/Prefix-List|Prefix-List]], se manipulan metricas
- Enrutamiento Basado en Politica (PBR): Se hace a nivel de interfaz y se usa para que el administrador eliga la ruta en vez del enrutador basado en coindicir direcciones de origen y destino, tipos de protocolo y aplicaciones para el usuario final, entre otros
- BGP: Se usa en las politicas de BGP, controlando las sesiones y modificar los atributos de [[010 - Protocolos/010.1 - Routing/BGP/BGP|BGP]]

# Configuracion
## Route-map en EIGRP - OSFP
Uso de [[020 - Conceptos/020.1 - Administracion/Prefix-List|Prefix-List]]
```
route-map [map-tag] {permit|deny} [seq-number]
match ip address prefix-list [list-name]
set metric [metric-value]
set metric-type type-1
```

# Visualizacion
`show route-map`