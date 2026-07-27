Firewall ASA (**A**daptative **S**ecurity **A**ppliance) statefull(con estado) es un dispositivo de red de Cisco SecureX que mezcla las funciones [[020 - Conceptos/020.4 - Dispositivos de Red/Firewall|Firewall]], [[010 - Protocolos/010.3 - Comunicaciones/VPN|VPN]] y [[020 - Conceptos/020.4 - Dispositivos de Red/IPS|IPS]], el cual es pequeño y se usa en redes LAN, funciona con un simil a las zonas [[020 - Conceptos/020.4 - Dispositivos de Red/Firewall#Zonas ZPF|ZPF]], denegando todo el trafico por defecto y permitiendo el trafico de zonas con un nivel de seguridad mayor a uno menor. el tamaño va a depender de la cantidad de conexiones(IP+Puerto) que tenga que manejar
El filtrado se aplican solo para el trafico de un nivel superior a uno inferior. cuando las interfaces tienen el mismo nivel de seguridad, se revisan en ambos sentidos
Soportan Objetos y Grupos de objetos para configurar por grupo o configuracion y replicarla en diferentes escenarios, cuando un objeto cambia aplica a todos los lugares configurados.
Permite usar [[020 - Conceptos/020.1 - Administracion/ACL#ACL Extendida Nombrada|ACL Extendida Nombrada]] con entradas individuales sin acceder al modo acl
## Niveles de seguridad por defecto:
Trafico nivel de seguridad mas alto a uno mas bajo: **Trafico de Salida** (Revisa)
Trafico nivel de seguridad mas bajo a uno mas alto: **Trafico de Entrada** (Bloquea)
- Inside: 100  
- DMZ: 50
- Outside: 0
## Versiones ASA
Se pueden configurar multiples versiones en diferentes puertos de un firewall asa
- ASA 5505: Funciona como Switch(Transparent)(firewall stealth): Funciona como un [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]], solo requiere ip de administracion. Puede separar diferentes interfaces en redes VLAN
- ASA 5506: Funciona como [[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]], soporta puertos PoE: Requiere una ip, es considerado un salto de router, y puede realizar [[010 - Protocolos/010.1 - Routing/NAT#NAT DINAMICA|NAT Dinamico]], NAT Estatico y [[010 - Protocolos/010.1 - Routing/NAT#PAT|NAT]]
- ASA 5507: Soporta Puertos PoE
## Servicios
- Firewall alto rendimiento(Pequeños Entornos)
- SSL
- [[010 - Protocolos/010.3 - Comunicaciones/VPN|VPN]] w/o [[010 - Protocolos/010.1 - Routing/IPSEC|IPSEC]]
## Ajustes por defecto
- Hostname: `ciscoasa`
- Contraseñas en Blanco
- VLAN1: Red Interior
	- int e0/1-7
	- ip/dec-mask: 192.168.1.1 255.255.255.0
- VLAN2: Red Exterior
	- int e0/0
	- ip/dec-mask: DHCP via ISP
	- Ruta Estatica DHCP via ISP
- Instalado pero desabilitado, Servidor HTTP soporta asistencia ASDM(Adoptive Security Device Manager)
- DHCPD interno activado, rango: 192.16.1.5-192.168.1.36
## Caracteristicas Avanzadas
- Virtualizacion: Se puede dividir en dispositivos mas pequeños
- Alta Disponibilidad: Dos ASA se pueden combinar en una sola configuracion
- Servidor de Seguridad de Identidad: IP sobre Windows Active Directory
- Control de Amenazas y Contencion de Errores: Funciones IPS, pero las funciones avanzadas se pagan aparte a travez de hardware
# Configuracion
## Configuracion Basica
### Cambiar Hora
```
clock set [hr:mn:sc] [day-number] [month] [year]
```
### Hostname, Dominio y Hora
```
hostname [hostname]
```
### Dominio
```
domain-name [domain-name]
```
### Configurar y Cambiar Contraseña 
```
enable password [password]
passwd [password]
```
---
## Ruta por Defecto
Nota: el 99% de los casos, la `[zone-name]` sera `OUTSIDE`
```
route OUTSIDE 0.0.0.0 0.0.0.0 [ip-salto]
```
## DHCPD
Configuracion no compatible con [[010 - Protocolos/010.3 - Comunicaciones/DHCP|DHCP]] directamente
- solo soporta 32 ip
- se recomienda desactivar el por defecto antes de configurarlo otra vez con `no dhcpd address 192.168.1.5-192.168.1.36 inside`
```
dhcpd address [ip-dhcpd-start]-[ip-dhcp-end] INSIDE
dhcpd dns [ip-dns] interface INSIDE
dhcpd enable INSIDE
```
## Zonas
### Zonas Usuales
Nota: Niveles por defecto por `[security-level]`
```
interface vlan [vlan-id]
nameif [zone-name]
security-level [level-number]
ip address [ip] [dec-mask]
!# Aplicar a la Interfaz
int [int S/S/P]
switchport access vlan [vlan-id]
```
### Zona DMZ
Se le agrega esta configuracion ademas de la Zona
Configurar de las primeras para evitar errores con la licencia
```
int vlan [vlan-id]
no forward interface vlan [vlan-id-inside]
nameif DMZ
ip address [ip] [dec-mask]
```
---
## Tipos de NAT
### PAT
Necesita habilitar MPF
```
object network [object-name]
subnet [ip-network] [dec-mask]
nat (inside,outside) dynamic interface
```
### NAT con Sobrecarga
```
object network net
subnet [ip-network] [dec-mask]
nat [INSIDE,OUTSIDE] dynamic inteface
```
### Nat Estatico
Necesita Habilitar Ping
```
object network [object-name]
host [ip-host]
nat (DMZ,OUTSIDE) static [ip-outside-hop]
!
access-list [acl-name] permit ip any host [ip-host]
access-group [acl-group-name] in interface outside
```
### NAT Dinamico Via Objetos
```
!# Public
object network [object-name-public]
range [ip-network] [dec-mask]
!# Inside
object network [object-name-inside]
subnet [ip-network] [dec-mask]
!# Nat
nat (inside,outside) dynamic [object-name-public]
```
---
## MPF
Esta configuracion permite el Ping
```
class-map inspection_default
match default-inspection-traffic
exit
policy-map global_policy
class inspection_default
inspect icmp
exit
service-policy global_policy global
```
## Habilitar Ping
```
access-list ping permit ip any host [ip-host]
access-group ping in int [zone-name]
```
## Servicios
### Telnet ASA
```
username [username] password [password]
aaa authentication telnet console local
telnet [ip-permit-connect] [dec-mask-/32] INSIDE
```
### SSH
```
username [username] password [password]
aaa authenticacion ssh console [LOCAL]
crypto key generate rsa modulus [bit-long-number]
!# y
ssh [ip-network] [dec-mask-network] [zone-name]
ssh [ip-host] [dec-mask-host] [zone-number]
ssh version 2
ssh timeout [sec-timeout]
```
### NTP
```
ntp authenticate
ntp trusted-key [1]
ntp authentication-key [1] md5 [ntp-password]
ntp server [ip-ntp-server]
```
### ASDM HTTP
```
http server enable
http [ip-network-inside] [dec-mask] inside
```
---
# Visualizacion
`show int ip brief`
`show route` → Ver rutas estaticas
`show switch vlan` → Ver la informacion de las VLANs
`show xlate` → Ver traducciones NAT
# Extra
NOTA: BGP se configura cuando hay 2 sucursales.
Modificar MPF(Mecanismo de Politica) que se modifica porque aunque el PAT, los saltos no los va a pescar, ese trafico se debe autorizar, permitiendo los ping