# Info
**V**irtual **R**outer **R**edundancy **P**rotocol es un protocolo electoral no propietario, el cual asigna 1 ip compartida a un grupo de routers de una red LAN. Permite [[020 - Conceptos/020.2 - Seguridad/Tecnicas de Redundancia|Tecnicas de Redundancia]], su funcionamiento es parecido a [[010 - Protocolos/010.2 - Switching/HSRP|HSRP]]

## Datos
- VRRP VRID: 0-255
- Prioridad VRRP: 1-255, Default 100
- Timers
	- Hello: 1 sec
	- Dead: 3 sec
- Multicast: `224.0.0.18`
- Tracking: Objetos (Track) | No es compatible con [[010 - Protocolos/010.2 - Switching/HSRP#IP SLA IPv4|IP SLA]]
- RFC: No soporta autenticacion, Cisco: Añade cifrado md5 en los anuncios | [Info](https://www.rfc-editor.org/rfc/rfc9568#name-security-considerations)
- Versiones
	- v2: ~~deprecated~~
	- v3 (Se usa): Funciona en entornos de varios proveedores

## Terminologia
- Router VRRP: Esta ejecutando el protocolo
- Router Virtual: Objeto Abstracto manejado por VRRP, actua como router compartido y gateway para dispositivos finales
- VRID (Virtual Router Identifier): In valor integral (1-255) identificando la instancia del router virtual en la LAN
- Virtual Router MAC Address: Direccion [[020 - Conceptos/020.3 - Fundamentos/MAC|MAC]] Multicast usada para enviar anuncios VRRP sobre "*VRID*"
- Router Activo: Asume la responsabilidad del funcionamiento
- Router(s) Backup: Asume la responsabilidad de ser router activo si el router activo falla

# Configuracion
## Habilitar VRRPv3
```
fhrp version vrrp v3
```
## Asignar IPv4
> `vrrp-group` puede ser el mismo que `vlan-id`
> Puedes configurar `ip address [ip] [dec-mask]`
```
int vlan [vlan-id]
vrrp [vrrp-group] ip [ip-address]
vrrp [vrrp-group] ip [ip-address] secondary
```
## Asignar prioridad VRRP
```
int vlan [vlan-id]
vrrp [vrrp-group] priority [level]
```
## Cambiar tiempo Hello
> Tiempo Default en [[#Datos]]
```
int vlan [vlan-id]
vrrp [vrrp-group] timers advertise [msec] interval??
```
## Autenticacion
`password` se cifrara en MD5
```
int vlan [vlan-id]
vrrp [vrrp-group] authentication [password]
```
## Track
> Valor minimo de `decrement-value`: $(\text[proridad\ alta] - \text[prioridad\ baja]) + 1$
> `int [int S/S/P]` es el "link ascendente" o el router que conecta el nucleo de la red
```
track [track-number] int [int S/S/P] line-protocol
delay down [sec-down] up [sec-up]
exit
int vlan [vlan-id]
vrrp [vrrp-group] track [object-number] [decrement-value]
```

## AF VRRP IPv4
```
fhrp version vrrp v3
interface vlan [vlan-id]
vrrp [vrrp-group] address-family ipv4
address [virtual-ip]
priority [ipv4-priority-value]
preempt
```

## AF VRRP IPv6
```
fhrp version vrrp v3
interface vlan [vlan-id]
vrrp [vrrp-group] address-family ipv6
address FE80::10 primary
address [ipv6/prefix]
priority [ipv6-priority-value]
preempt
```

# Visualizacion
`show vrrp brief`: Ver configuracion general, con la interfaz, grupo, prioridad, tiempo, estado preempt, estado rol, ip activa y de grupo
`show vrrp int vlan [vlan-id]`: Ver temporizadores

# Troubleshooting
VRRP Solo detecta un fallo del dispositivo o la ruta que usan los paquetes de saludo. Pero un enlace ascendente no lo detecta, generando un enrutamiento suboptimo.

# Extra
Standard Original: [RFC 2338](https://datatracker.ietf.org/doc/html/rfc2338)
Standard Actual: [RFC9568](https://www.rfc-editor.org/rfc/rfc9568)


## Formato MAC
VRRPv3 tiene un rango de MAC virtuales desde `00-00-5E-00-01-00` hasta `00-00-5E-00-01-FF`, lo que permite 255 routers IPv4 o IPv6.

Ejemplo VRRPv3 IPv4 Grupo 10
`0000.5E00.010A`
- `0000.5E`: 6 digitos HEX (24 bits), Organizationally unique identifier (OUI Mac Adress - Block Large) (IANA)
- `00.01`: 4 digitos HEX (16 bits), VRRPv3 ID
- `0A`: 2 digitos HEX (8 bits), Grupo VRRPv3

Ejemplo VRRPv3 IPv6 Grupo 10
`0000.5E00.020A`
- `0000.5E`: 6 digitos HEX (24 bits), Organizationally unique identifier (OUI Mac Adress - Block Large) (IANA)
- `00.02`: 4 digitos HEX (16 bits), VRRPv3 ID
- `0A`: 2 digitos HEX (8 bits), Grupo VRRPv3