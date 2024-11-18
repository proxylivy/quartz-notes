# Info
**Co**ntrol **P**lane **P**olicing (Politica de vigilancia del plano de control) es un conjunto de reglas o politicas para proteger la CPU de trafico excesivo o malicioso en un [[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]]

> [!WARNING] Cuidado
> - Permitir reglas de trafico puede generar trafico indeseado desde otros origenes
> - Los protocolos de gestion como [[010 - Protocolos/010.3 - Comunicaciones/SSH|SSH]] o [[010 - Protocolos/010.3 - Comunicaciones/010.3.2 - Diag y Control/SNMP|SNMP]] deben ser manejados controlarse cuidadosamente
> - Se recomienda limitar el acceso a rangos especificos de IP que residan dentro del **NOC** (Network Operation Center)

## Funcionamiento
Se utilizan [[020 - Conceptos/020.1 - Administracion/ACL#ACL Extendida Nombrada|ACL Extendida Nombrada]] para identificar el trafico interesante mediante reglas "*permit*", luego lo clasifica mediante [[020 - Conceptos/020.1 - Administracion/Class-Map|Class-Map]] segun el tipo de trafico y se configura una [[020 - Conceptos/020.1 - Administracion/Policy-Map|Policy-Map]] para poder aplicar una tasa de velocidad.

# Sintaxis Configuracion
## ACL CoPP
```
ip access-list extended [acl-name]
 permit [protocol] [source-ip] [source-wildcard] [dest-ip] [dest-wildcard] [port-operator] [port-number]
```

## Class-map CoPP
```
class-map match-all [class-name]
 match access-group name [acl-name]
```

# Ejemplos
## ACL PING
```
ip access-list extended ACL-CoPP-ICMP
 permit icmp any any echo-reply
 permit icmp any any ttl-exceeded
 permit icmp any any unreachable
 permit icmp any any echo
 permit udp any any range 33434 33463 ttl eq 1
```
## ACL IPsec
```
ip access-list extended ACL-CoPP-IPsec
 permit esp any any
 permit gre any any
 permit udp any eq isakmp any eq isakmp
 permit udp any any eq non500-isakmp
 permit udp any eq non500-isakmp any
!
ip access-list extended ACL-CoPP-Initialize
 permit udp any eq bootps any eq bootpc
!
ip access-list extended ACL-CoPP-Management
 permit udp any any eq ntp any
 permit udp any any eq snmp
 permit tcp any any eq 22
 permit tcp any eq 22 any established
!
ip access-list extended ACL-CoPP-Routing
 permit tcp any eq bgp any established
 permit eigrp any host 224.0.0.10
 permit ospf any host 224.0.0.5
 permit ospf any host 224.0.0.6
 permit pim any host 224.0.0.13
 permit igmp any any
```

## Class-map
```
class-map match-all CLASS-CoPP-ICMP
 match access-group name ACL-CoPP-ICMP
```
