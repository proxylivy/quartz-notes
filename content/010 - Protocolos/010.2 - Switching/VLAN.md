# Info
Segmentacion Logica mediante dominios de broadcast, no se pueden comunicar entre diferentes vlan.
Se puede aplicar [[010 - Protocolos/010.2 - Switching/Seguridad de Puerto|Seguridad de Puerto]]

Ultima Revision Estandar: [802.1Q-2022](https://ieeexplore.ieee.org/document/10004498)
## Campos
Total: 32 Bits
- Destination MAC
- Source MAC
- TPID (0x8100) -> 16 bits
- PCP -> 3 bits
- DEI -> 1 bit
- VLAN ID -> 12 bits
- Destination IP
- Source IP
- Payload

## Tipos de Enlace
### Acceso
se asigna un puerto a una [[010 - Protocolos/010.2 - Switching/VLAN|VLAN]] especifica y se envian paquetes en base a estos, las etiquetas 802.1Q no se incluyen.
### Troncales
Pueden transportar multiples VLAN entre Switch, los datos recibidos, examina los encabezados, se asocia a la VLAN, se eliminan los encabezados, y se reenvia al siguiente puerto segun MAC


# Configuracion
## Crear Vlans
```
vlan [vlan-id]
name [vlan-name]
exit
vtp mode transparent
```
## Access
```
int [int S/S/P]
switchport mode access
switchport access vlan [vlan-number]
```
## Troncal
> Se le puede desactivar [[010 - Protocolos/010.2 - Switching/DTP#Desactivar DTP|DTP]]
```
interface [int S/S/P]
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan [vlan1,vlan2...]
```
## Nativa (Trunk)
```
interface [int S/S/P]
switchport trunk native vlan [vlan]
```
## Apagar interfaces sin uso
Se ve mejor cuando configuras enlaces de acceso y troncales y las que se mantengan en vlan 0, se apagan
```
enable
show vlan brief
conf terminal
interface range [int S/S/P-P]
shut
exit
```
## Vlan Administrativa
IP sobre capa 2 para administrar [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]] via [[010 - Protocolos/010.3 - Comunicaciones/SSH|SSH]] o Telnet, tambien se usa para interfaces de VLAN
```
int vlan [vlan-id]
ip address [ip] [dec-mask]
no shut
exit
ip default-gateway [ip-default-gateway]
```

# Visualizacion
`show vlan` -> Muestra toda la informacion las vlans
`show vlan brief` -> Resumen relacion vlan -> Puerto
`show vlan summary` -> Hace un resumen sobre la creacion de vlans
`show vlan id [number]` -> Muestra info por id
`show vlan name [name]` -> Muestra info por nombre
`show interface trunk` -> Resumen vlan -> Interfaz Troncal
`show interfaces int [S/S/P] switchport` -> Ver informacion detallada de la interfaz
`show running-config | begin interface [int S/S/P]` -> Ver configuracion de puerto
`show mac address-table dynamic` -> Resumen Relacion VLAN -> MAC

## Troubleshooting
- Diferente tipo de encapsulacion (802.1q vs ISL)
- Diferente VLAN Nativa entre dispositivos
- Un enlace troncal no envia las suficientes vlans
- Un enlace acceso esta mal configurado