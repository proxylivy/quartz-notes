# Info
**H**ot **S**tandby **R**outer **P**rotocol, es un protocolo propietario de cisco para lograr Alta [[020 - Conceptos/020.2 - Seguridad/Tecnicas de Redundancia|Tecnicas de Redundancia]], los routers necesitan adyacencia L2 para intercambiar saludos. Se usa la v2.

## Datos
- Compatibilidad
	- v1: IPv4
	- v2: IPv4/IPv6, permite autenticacion MD5, no es retrocompatible, funciona en milisegundos
- Tracking: Interfaces(IP SLA) o Objetos
- Timers
	- Hello: 3 sec (ms)
	- Dead: 3 x Hello = 10 sec (ms)
- UDP `1985`
- Prioridad defecto: `100`
- Direccion Multicast
	- v1: `224.0.0.2`
	- v2: `224.0.0.102` o `FF02::66`

Utiliza una IP Virtual y [[020 - Conceptos/020.3 - Fundamentos/MAC|MAC]] virtual para representar un router virtual, este tiene una IP unica que comparten los routers como gateway para los dispositivos.
El numero de grupo HSRP deriva en la MAC, y la MAC en el vinculo IPv6 Virtual Local.
Un solo Router queda activo y envia anuncios a los demas routers pasivos del grupo, cuando el router activo se cae, el Standby con mas prioridad se levanta.
Se puede lograr Balanceo de Carga entre vlan modificando la priodidad a un valor superior a `100` y en el otro lado menor a `100`

## Tipos de Router HSRP
- Virtual: Una IP y MAC virtuales en comun, uno por grupo HSRP 
- Activo: Es el router que funciona y procesa el trafico de la red
- Standby: Monitorea el estado del grupo HSRP, y cuando falla el `Activo` o se cumplen reglas preconfiguradas, toma el rol de `activo`
- Otros: Solo puede haber un router en modo `activo` y otro en `standby`, los demas estan en modo inicial hasta que `standby` pase a `activo`

## Estados HSRP
- Inicial: HSRP no correra, pasa cuando cambia una configuracion o una interfaz se levanta
- Escucha: El router conoce la IP virtual, pero no es ni activo ni pasivo, espera un saludo de esos routers
- Hablar: El router manda y recibe saludos, y participa de la eleccion de router activo o standby
- Standby: El router es el candidato a siguiente router "Activo" y envia saludos hello, solo hay un router en este estado
- Activo: Es el router que recibe la informacion de la IP/MAC Virtual, envia saludos hello, solo hay un router en este estado


## Versiones
- v1 (Vieja y ~~Deprecated~~), limite de grupo de 255
- v2: permite numeros de grupo hasta 4095, no es retrocompatible

Su eleccion se hace con la **Prioridad** mas alta
`[group-number]` sigue a `[vlan-id]` para evitar confusiones
`[decrement-value]` es el valor que queda cuando necesite hacer failover, y su valor minimo es:
$(\text[proridad\ alta] - \text[prioridad\ baja]) + 1$

[[#IP SLA IPv4]] monitorea un enlace y cambiar la configuracion en caso de fallo, su configuracion es el equipo con mayor prioridad para IPv4

[[#Track Enlace Ascendente IPv6]] monitorea un enlace en IPv6, su configuracion es en el equipo con mayor prioridad para IPv6
# Configuracion
## HSRP IPv4
Nota: `[ip-virtual-gateway]` se puede ver desde dispositivos finales con `show running-config | section ip route`
```
int vlan [vlan-id]
standby version 2
standby [group-ipv4-number] ip [ip-virtual-gateway]
standby [group-ipv4-number] priority [ipv4-priority-value]
standby [group-ipv4-number] preempt
```
## Modificar Temporizadores
Los `[hello-time-sec]` y `[dead-time-sec]` pueden ser los de [[010 - Protocolos/010.1 - Routing/VRRP#Timers por defecto|VRRP]]
```
!# SEC
int [S/S/P]
standby version 2
standby [group-ipv4-number] timers [hello-time-sec] [dead-time-sec]
!# MSEC
standby [group-ipv4-number] timers msec [hello-time-sec] msec [dead-time-sec]
```
## IP SLA IPv4
```
ip sla [sla-number]
icmp-echo [ip-ckeck]
frecuency [sec]
exit
ip sla schedule [sla-number] start-time now life forefer
track [track-number] ip sla [sla-number] reachability
delay down [sec-down] up [sec-up]
exit
!# Aplicar en la interfaz
int vlan [vlan-id]
standby [group-number] track [track-number] decrement [decrement-value]
```
## HSRP IPv6
Ve `[ipv6-virtual-gateway/prefix]` de los pc finales con `show running-config | section ipv6 route`
```
int vlan [vlan-id]
standby version 2
standby [group-ipv6-number] ipv6 [ipv6-virtual-gateway/prefix]
standby [group-ipv6-number] priority [ipv6-priority-value]
standby [group-ipv6-number] preempt
standby [group-ipv6-number] timers msec [hello-time-msec] msec [dead-time-msec]
exit
```
## Track Enlace Ascendente IPv6
```
track [track-number] int [int S/S/P] line-protocol
delay down [sec-down] up [sec-up]
exit
int vlan [vlan-id]
standby [group-ipv6-number] track [track-number] decrement [decrement-value]
```
## Autenticacion HSRP
Switch config??
Auth: MD5 o Texto Plano
```
standby group authentication string
standby group authentication md5 key-string [0 | 7] string
key chain [chain-name]
key [key-number]
key-string {0 | 7} [string]
exit
int [int S/S/P]
standby group authentication md5 key-chain [chain-name]
```
# Visualizacion
`show ip arp` -> Ver estado de HSRP
`show standby`
`show standby brief`: Ver interfaz en HSRP, numero de grupo, prioridad, y si es "preempt"
`show standby [int S/S/P]`: Ver MAC Virtual de grupo, temporizadores, prioridad standby, prioridad del equipo local
`show standby int vlan [vlan-id]`

# Extras
Modo Geek: HSRP tiene 2 versiones (v1 y v2), la vw es compatible con las versiones de Cisco IOS 12.2 (46) SE.

`show standby` permite ver la mac de grupo, la cual se basa en los 3 digitos del grupo que se crea

## MAC Virtual Group Selection

HSRPv1 tiene un rango de MAC virtuales desde `0000.0C07.AC00` a `0000.0C07.ACFF` para IPv4, usa 2 digitos hexadecimales para representar el grupo
Ejemplo HSRPv1 Grupo 10
`0000.0c07.ac0a`
- `0000.0c`: Organizationally unique identifier (OUI Mac Adress - Block Large (Cisco IPv4)
- `07.ac`: HSRPv1 ID
- `0a`: Grupo HSRP 10 en Hex

HSRPv2 tiene un rango de MAC virtuales desde `0000.0C9F.F000` a `0000.0C9F.FFFF` para IPv4 y desde `0005.73A0.0000` a `0005.73A0.0FFF` para IPv6, usa 3 digitos hexadecimales para representar el grupo
Ejemplo HSRPv2 Grupo 10
`0005.73A0.000A`
- `0005.73`: 6 digitos HEX (24 bits), Organizationally unique identifier (OUI Mac Adress - Block Large) (Cisco IPv6)
- `A0.00`: 4 digitos HEX (16 bits), HSRPv2 ID
- `0A`: 2 digitos HEX (8 bits), Grupo HSRPv2