# Info
**Co**ntrol **P**lane **P**olicing es una caracteristica de QoS para dispositivos con Plano de Control ([[020 - Conceptos/020.3 - Fundamentos/SDN|SDN]]) del [[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]] para proteger la CPU de trafico excesivo o malicioso, filtrando el contenido que quiere controlar, lo clasifica en varias clases segun sus caracteristicas y por ultimo limita las tasas para cada tipo de trafico basado en QoS.

> [!WARNING] Cuidado
> Permitir reglas de trafico puede generar trafico indeseado desde otros origenes

Para configurar se usan [[020 - Conceptos/020.1 - Administracion/ACL|ACL]] que filtran el trafico a travez de reglas "*permit*", y 
# Configuracion




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