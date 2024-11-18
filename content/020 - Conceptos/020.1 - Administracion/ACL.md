# Info
**A**cess **C**ontrol **L**ist (Lista de Control Acceso), son una tecnica de filtrado de paquetes que funciona en [[020 - Conceptos/020.3 - Fundamentos/Modelo OSI#Capa 3 Red|Capa 3]] y [[020 - Conceptos/020.3 - Fundamentos/Modelo OSI#Capa 4 Transporte|Capa 4]] y permite controlar el trafico que entra y sale de una interfaz, analizando los paquetes entrantes y salientes, identificando diferentes parametros como *Direcciones IP de origen y destino*, *puertos* y *protocolos*, y aplicar reglas para *permitir* o *denegar* segun la regla, una ACL mal configurada puede causar problemas de red y afectar el rendimiento y seguridad de la red. Deben ser definidas de la entrada mas especifica a la menos especifica.

> [!IMPORTANT] Importante
> Las ACL son una tecnica estatica y puede ser evadida si un atacante modifica los parametros de consulta para rodear las reglas establecidas

ACL tienen un uso para
- Identificar redes privadas o publicas que son traducidas por [[010 - Protocolos/010.1 - Routing/NAT#PAT|PAT]]
- Controlar rutas en [[010 - Protocolos/010.1 - Routing/Redistribucion|Redistribucion]]
- Definir que paquetes seran procesador por [[999 - Archivado/PBR|PBR]]
- Determinar que paquetes seran aceptados o rechazados por un [[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]]

El funcionamiento de la ACL se basa en el **Procesamiento Descendente**
1. Las ACL son procesadas en orden secuencial de la parte superior (Valor Menor) a la inferior (Valor Mayor)
2. Se ejecuta inmediatamente cuando coincide con una regla permit o deny y las siguientes no afectan al paquete
3. Se **Deniega Implicitamente** si no hay ninguna regla para aceptar el paquete.

> [!IMPORTANT] Importante
> Una ACL creada no hace nada a menos que se aplique a un servicio, una funcion o una interfaz

## Tabla Tipos de ACL

|             | ACL Estandar    | ACL Extendida                |
| ----------- | --------------- | ---------------------------- |
| **Tipo**    | Origen(in)      | Origen(in) y Salida(out)     |
| **Filtros** | Solo IP source  | IP y Puerto origen y destino |
| **Puertos** | 1-99, 1300-1999 | 100-199, 2000-2699           |
| **Aplicar** | Cerca Destino   | Cerca Origen                 |

## Terminologia
- `any`: Representa todas las IP
- `host`: Especifica una unica IP (Wildcard `0.0.0.0`)
- `network`: Una red entera
- `wildcard`: Especifica un rango de direcciones permitidas
- `protocol`: Ejemplos: `tcp`, `udp`, `ftp`, `ssh`, `http`
- `port-operator`
	- `eq(=)`: Igual a
	- `neq(!=)`: No Igual a
	- `gt(↑)`: Mayor que
	- `lt(↓)`: Menor que
	- `range(↔)`: Rango de puertos

## ACL IPv6
Solo existen ==ACL Extendidas Nombradas== en [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv6|IPv6]] y no puede colisionar el nombre con una regla [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv4|IPv4]], Tiene permit implicito para los mensajes **NS** (**N**eighbor **S**olicitation) y **NA** (**N**eighbor **A**dverisement) que son necesarios para [[010 - Protocolos/010.3 - Comunicaciones/010.3.2 - Diag y Control/ICMP|ICMP]] en IPv6 y asegurar el funcionamiento de **NDP** (**N**eighbor **D**iscovery **P**rotocol), que reemplaza el funcionamiento de ARP(IPv4)

# Sintaxis Configuracion
## ACL Estandar Numerada
> Aplica interfaz solo "`[in]`"
```
access-list [1-99 | 1300-1999] remark ["comentario"]
 access-list [1-99 | 1300-1999] [permit | deny] [source-ip] [wildcard]
```

## ACL Estandar Nombrada
> Aplica interfaz solo "`[in]`"
```
ip access-list standard [acl-name]
 [permit | deny] [source-ip] [wildcard]
```

## ACL Extendido Numerado
> Aplica intefaz `[in | out]`
```
access-list [100-199 | 2000-2699] [permit | deny] [protocol] [source-ip] [source-wildcard] [destination-ip] [destination-wilcard] [port-operator] [port-number]
```

## ACL Extendida Nombrada
> Aplica interfaz `[in | out]`
```
ip access-list extended [acl-name] 
 [permit | deny] [protocol] [source-ip] [source-wilcard] [destination-ip] [destination-wildcard] [port-operator] [port-number]
```

## ACL Extendida Nombrada IPv6
> Aplica interfaz `[in | out]`
```
ipv6 access-list [acl-name]
 [permit | deny] [protocol] [source-ip/prefix] [destination-ip/prefix]
```

## ACL Basada en tiempo
### Define el rango de tiempo
```
time-range [time-acl-name]
 periodic [days] [start-hour] to [end-hour]
```
### Aplica el rango en una ACL Extendida
```
access-list [number-] [permit | deny] [protocol] [source-ip] [destination-ip] time-range [time-acl-name]
```

# Aplicar en Interfaces
## ACL Estandar IPv4
```
int [S/S/P]
 ip access-group [acl-number | acl-name] in
```

## ACL Extendida IPv4
```
int [S/S/P]
 ip access-group [acl-number | acl-name] [in | out]
```

## ACL IPv6
```
int [int S/S/P]
ipv6 traffic-filter [list-name] in
```


# Ejemplos Configuracion
## ACL Estandar Numerada
```
access-list 10 deny 192.168.1.0 0.0.0.255
access-list 10 permit any
!
int e0/0
 ip access-group 10 in
```

## ACL Estandar Nombrada
```
ip access-list standard BLOQUEO_TELNET
 deny host 192.168.1.100
 permit any
exit
!
int e0/0
 ip access-group BLOQUEO_TELNET in
```

## ACL Extendida Numerada
```
access-list 110 permit tcp 192.168.1.0 0.0.0.255 any eq 80
access-list 110 deny ip any any
!
int e0/0
 ip access-group 110 in
```

## ACL Extendida Nombrada
```
ip access-list extended FILTRO_WEB
 permit tcp 192.168.1.0 0.0.0.255 any eq 80
 deny ip any any
exit
!
int e0/0
 ip access-group FILTRO_WEB out
```

## ACL IPv6
```
ipv6 access-list BLOQUEO_HTTPv6
 deny tcp any any eq 80
 permit ipv6 any any
exit
!
int e0/0
 ipv6 traffic-filter BLOQUEO_HTTPv6 in
```

## ACL Basada en tiempo
```
time-range HORARIO_LABORAL
 periodic monday 9:00 to 17:00
!
access-list 101 permit tcp any any eq 80 time-range HORARIO_LABORAL
```

# Visualizacion
- `show access-list`: Mostrar todas las ACL IPv4
- `show access-list`: Mostrar todas las ACL IPv6
- `show access-list [acl-number | acl-name]`: Mostrar una ACL numerada o nombrada en especifico
- `show ipv6 access-list`: Ver ACL IPv6
- `show ip int [int S/S/P]`: Ver ACL aplicada en interfaz IPv4, revisar el flujo "in | out"
- `show ipv6 int [int S/S/P]`: Ver ACL aplicada en interfaz IPv6, revisa el flujo "in | out"
- `show time-range [time-acl-name]`: Ver reglas permitidas por tiempo

# Troubleshooting
- `[acl-name]`
	- Case-Sensitive: Mayuscula != Minuscula, cuidado con el nombre

## IPv6
Los mensajes NA y NS para NDP deben estar permitidos una configuracion como `deny ipv6 any any log` puede romper eso y generar problemas en IPv6