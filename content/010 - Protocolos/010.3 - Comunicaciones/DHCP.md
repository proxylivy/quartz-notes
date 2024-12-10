# Info
## Datos
- Server: UDP 67
- Cliente: UDP 68

# Configuracion
## Crear DHCP Router
```
ip dhcp excluded-address [ip-start-range] [ip-end-range] 
ip dhcp pool [pool-name]
 network [ip-network] [dec-mask]
 default-router [ip-default-router]
 dns-server [ip-dns]
 domain-name [domain-name]
```
## IP Helper
> `int` -> Dispositivos Finales
```
int [int S/S/P]
 ip helper-address [ip-router-dhcp]
```
## Activar DHCP Cliente
```
interface [int S/S/P]
 ip address dhcp
 no shut
```
## DHCP Snooping
Nota: Configuracion de [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]]
```
ip dhcp snooping
ip dhcp snooping vlan [vlan-id]
no ip dhcp snooping information option
int [int S/S/P]
 ip dhcp snooping trust
```
## Evitar Ataque Hambruna
NOTA: un valor para `number` va de 3-5 
Nota2: Configuracion de [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]]
```
int [int S/S/P]
 ip dhcp snooping limit-rate [number]
```

## IPv6
Nota: Existen 2 estados
- Stateful: Asigna IP y otros parametros
- Stateless: Los dispositivos se configuran mediante SLAAC y el servidor solo proporciona ajustes adicionales
### Crear Pool DHCPv6
```
ipv6 dhcp pool [DHCPv6-pool-name]
 address prefix [ipv6-network/prefix]
 dns-server [ipv6-dns-server]
 domain-name [domain-name]
```
### Stateful DHCPv6
```
int [int S/S/P]
 ipv6 dhcp server [DHCPv6-pool-name]
```
### Stateless DHCPv6
```
int [int S/S/P]
 ipv6 nd managed-config-flag
 ipv6 dhcp server [DHCPv6-pool-name]
```
### Dispositivos Finales IPv6
```
int [int S/S/P]
 ipv6 enable
 ipv6 address DHCP 
 no shut
```
### IP-Helper DHCPv6
```
int [int S/S/P]
 ipv6 dhcp relay destination 3001:ABCD:ABCD:12::2
```
# Visualizacion
`show ip dhcp binding` -> Ver si ha entregado ip por DHCPv4
`show ipv6 dhcp binding` -> Ver si ha entregado ip por DHCPv6
`show ip dhcp snooping`
`show ip dhcp pool`


# Extra
- Estandard Propuesto IPv4: [RFC2131](https://datatracker.ietf.org/doc/html/rfc2131)
- Estandard Propuesto IPv6: [RFC8415](https://www.rfc-editor.org/rfc/rfc8415)