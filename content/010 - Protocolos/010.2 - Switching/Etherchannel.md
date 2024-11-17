## Que es??
Protocolo Propietario de Cisco basado en Port-Channel, tecnica switch a switch para agrupar enlaces fisicos en enlaces logicos, se comprueban las consistencias de configuracion y gestiona la adicion o fallas entre dos switchs, la configuracion que se haga despues de crear el etherchannel como velocidad de configuracion, configuracion duplex e informacion de VLAN cambiara a los demas puertos. Comunmente permite 16 enlaces aunque solo 8 pueden estar activos, los demas quedan a la escucha.
Para [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]] capa 3, los puertos conmutado pueden convertirse en puertos enrutados.

Sus ventajas son:
- Configuraciones mas sencillas, al solo configurar un enlace logico en vez de multiples fisicos
- Se puede implementar balanceo de carga (Depende de los modelos)

Versiones:
- LACP -> IEEE 802.3ad
- PAgP -> Cisco
- Estatico -> Sin protocolo

Consideraciones
- Identificar Puertos en ambos switch para evitar problemas de configuracion y conexiones apropiadas
- Cada interfaz debe tener protocolo apropiado (LACP o PAgP)
- Asegurarse que ambos lados funcionen y proporcionen el ancho de banda agregado
- Son son Inter-Compatibles

## LACP
Estandar [IEEE 802.3ad](https://www.ibm.com/docs/es/aix/7.2?topic=protocol-etherchannel-ieee-8023ad-link-aggregation-teaming), negocia paquetes LACP cada 30 segundos, con un dead time de 3 intervalos (90 segundos), "lacp fast" se creo para disminuir drasticamente la velocidad de mensajes, para que anuncie cada segundo y eliminar el enlace en 3 segundos, ademas se puede configurar un minimo y maximo de cables para generar adyacencia.

### Modos
- Activo: Establecer la negociacion
- Pasivo: Escucha un establecimiento 

### Tabla LACP

|         | Active | Passive |
| ------- | ------ | ------- |
| Active  | Si     | Si      |
| Passive | Si     | No      |
## PAgP
Estandar Propietario de Cisco, se intercambian paquetes PAGP, no es compatible con LACP.
### Modos
- Desirable: Habilita PAgP
- Auto: Habilita PAgP solo si el otro dispositivo PAgP es detectado
### Tabla PAgP

|           | Desirable | Auto |
| --------- | --------- | ---- |
| Desirable | Si        | Si   |
| Auto      | Si        | No   |
## Tabla Static Persitance

|     | ON  |
| --- | --- |
| ON  | Si  |
## Balanceo de carga
El trafico se envia a travez de hash, que se ejecuta en los campos de encabezado del paquete, estos campos son:
- dst-ip: Destination IP address
- dst-mac: Destination MAC address
- dst-mixed-ip-port: Destination IP address and destination TCP/UDP port
- dst-port: Destination TCP/UDP port
- src-dst-ip: Source and destination IP addresses
- src-dest-ip-only: Source and destination IP addresses only
- src-dst-mac: Source and destination MAC addresses
- src-dst-mixed-ip-port: Source and destination IP addresses and source and destination TCP/UDP porto
- src-dst-port Source and destination TCP/UDP porto only
- src-ip: Source IP address
- src-mac: Source MAC address
- src-mixed-ip-port: Source IP address and source TCP/UDP port
- src-port: Source TCP/UDP port
## Valores Port-Channel
### LACP
`channel-group [group] mode [active/passive]` -> Crea el etherchannel de LACP
`int port-channel [group]` -> Acceder al port-channel
`switchport trunk encapsulation dot1q` -> Ejemplo, Configurar Encapsulation en los enlaces de Port-Channel dentro de `switchport mode trunk` -> Ejemplo, Dejar enlaces del port-channel como troncales
### Activar lacp fast
`int range [int S/S/P-P]` -> Entrar en las interfaces necesarias
`lacp rate fast` -> Activa la negociacion rapida de lacp
### Minimo enlaces LACP
`interface port-channel [group]`
`port-channel min-links [N° Links]`
### Maximo enlaces LACP
`interface port-channel`
# Configuracion
## Switch Capa 2
Primero configura Enlaces [[010 - Protocolos/010.2 - Switching/VLAN#Troncal|Troncales]]
```
interface range [int S/S/P-P]
switchport mode trunk
switchport trunk encapsulation dot1q
channel-group [group-number] mode [status-mode]
exit
int port-channel [channel-number]
switchport trunk allowed vlan [vlan-id]
```
## Switch Capa 3 Con IP
> Nota1: No configuras trocales, debido a las ip
> Nota2: Los comandos en las interfaces no se heredan como normalmente lo harian
> Nota3: Las interfaces no llevan ip, el portchannel si
```
int range [int S/S/P-P]
no switchport
no ip address
no ipv6 address
channel-group [channel-number] mode [status-mode]
exit
int port-channel [channel-number]
no switchport
ip address [ipv4] [dec-mask]
ipv6 address [ipv6/prefix]
```

## Visualizacion
`show etherchannel summary` -> Resumen de Etherchannel
`show lacp internal` -> Ver configuracion de LACP
`show etherchannel load-balance` -> Verifica La configuracion de load balancer 
`show etherchannel port-channel` -> Vista de los port channel creados
`show etherchannel [port-channel] detail` -> Vista en detalle de port channel

# Troubleshooting
- Incoherencia entre puertos fisicos de un canal
	- Velocidad
	- Modo Transmision
	- Estado de puerto de acceso
	- Paso de Vlan Troncales, Datos y Nativas (Solo L2)
- Incoherencia entre puertos en lados opuestos
	- Diferente protocolo de negociacion
	- Fallos con STP
- Distribucion desigual del trafico
	- Problemas de calculo hash (campos ethernet y cabeceras IP de trama) al envio de la interfaz


# Extras
## Conocido como:
- Port-Channel
- Agregado de enlaces
## Datazo Freak
Cuando hay port-channel creados, las configuraciones siguientes (Como enlaces troncales) se hacen en el port-channel