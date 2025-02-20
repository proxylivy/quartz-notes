# Info
## Imagen Topologia
![1000](https://slink.proxylivy.work/image/19b3536c-6e0b-4421-ace7-066856518b32.png)
## Sobre

## Instrucciones Generales
- El examen transversal de la asignatura SEGURIDAD EN REDES CORPORATIVAS (CCNA SECURITY) CSY5112 corresponderá a la entrega de un encargo sin presentación.
- Este encargo se elabora de forma individual. Cualquier intento de copia o plagio implicará nota 1.0 para todos los estudiantes involucrados.
- El ET deberá ser encargado a los alumnos en la semana 17. La evaluación deberá ser entregada por los estudiantes el día y hora asignado para el examen transversal.
- Es obligatorio que aparezca el nombre completo del estudiante y su correo electrónico en la sección “User Profile” del archivo Cisco Packet Tracer. No se revisarán exámenes que no cuenten con esta información.
- Los estudiantes deberán configurar un archivo de Cisco Packet Tracer, según los requerimientos. Este archivo debe ser proporcionado por el docente a través de AVA, y los estudiantes a través de la misma actividad respectiva deberán realizar el envío en la fecha y hora correspondiente. El nombre del archivo de tener el formato apellido-nombre.pka.

## Caso de Estudio
Una empresa, que tiene su casa matriz en Santiago y una sucursal en Viña del Mar, debido a las nuevas leyes y normativas nacionales, tiene que, obligatoriamente, mejorar la seguridad de su red y del acceso a sus activos críticos, de tal manera de proteger los datos de clientes y usuarios. Los riesgos son muchos, como ataques de acceso a información sensible y crítica, infección por ransomware, ataques de denegación de servicios distribuido, etc.
Como experto en redes y con amplios conocimientos en seguridad de la información, le han contactado para este proyecto. Lo siguiente son los requerimientos del cliente y la topología de su red.

## Direccionamiento

| Dispositivos      | Interfaz | Direccion IP  | Subred          |
| ----------------- | -------- | ------------- | --------------- |
| Edge-Santiago     | G0/0.15  | 172.15.15.1   | 255.255.255.0   |
| =                 | G0/0.25  | 172.25.25.1   | 255.255.255.0   |
| =                 | G0/1     | 200.2.7.6     | 255.255.255.248 |
| =                 | G0/2     | 172.16.0.1    | 255.255.255.240 |
| ISP               | G0/1     | 200.2.7.1     | 255.255.255.248 |
| =                 | G0/0     | 177.44.56.1   | 255.255.255.248 |
| FW-ASA            | E0/0     | 177.44.56.6   | 255.255.255.248 |
| =                 | E0/1     | 192.168.23.1  | 255.255.255.0   |
| =                 | E0/2     | 10.20.30.1    | 255.255.255.224 |
| Server AAA-Syslog | F0       | 172.16.0.14   | 255.255.255.240 |
| SW1               | VLAN15   | 172.15.15.254 | 255.255.255.0   |

## Requerimientos
### Ítem 1: Verificar direccionamiento y conectividad, si hay errores o problemas de conectividad, los debe resolver.
### Ítem 2: Protección de acceso al dispositivo.
En Router Edge-Santiago, se ha solicitado configurar las medidas de protección de acceso a los dispositivos de red la red de Santiago. Todas las contraseñas deben ser ciscocisco y protegidas usando MD5. A continuación, se señalan los requerimientos a realizar:
1. Obliga, técnicamente, a que todas las contraseñas sean de un largo mínimo de 8 caracteres y que aparezcan cifradas en el running. | [[010 - Protocolos/010.3 - Comunicaciones/010.3.1 - AAA/AAA|AAA]]
3. La contraseña de usuario privilegiado debe ser protegida.
4. Al acceder los usuarios se le debe desplegar un banner de advertencia que diga `# ACCESO RESTRINGIDO#`. | [[020 - Conceptos/020.1 - Administracion/Configuracion Inicial de Dispositivos#Generar MOTD|MOTD]]
5. Usa una técnica para evitar los ataques por fuerza bruta, la que debe bloquear por 45 segundos a quien se equivoque 3 veces en sus credenciales de acceso en 60 segundos.
6. Actualmente, el acceso remoto es vulnerable a ataques de privacidad. Para evitar esto, usa un protocolo seguro con un módulo RSA de 2048 bits. [[010 - Protocolos/010.3 - Comunicaciones/SSH|SSH]] Permita 2 intentos, y si hay inactividad en el login durante 30 segundos se debe cerrar la sesión. El acceso debe ser habilitado solamente para este protocolo. Usa dominio cisco.com, versión 2 del protocolo. El usuario debe ser admin con privilegios nivel 15 con contraseña protegida **ciscocisco**. [[020 - Conceptos/020.2 - Seguridad/Nivel de Privilegio|Nivel de Privilegio]]
7. Todos los accesos exitosos y fallidos deben quedar registrados en el servidor [[010 - Protocolos/010.3 - Comunicaciones/Syslog|Syslog]] remoto.
### Ítem 3: Control de acceso a dispositivos.
Se ha solicitado configurar medidas de control de acceso a dispositivos de la red, según los siguientes requerimientos:
1. En Edge-Santiago, establezca un mecanismo de roles y crea una cuenta con el nombre de administrador que podrá usar todos los comandos show, el comando configure y configure terminal. También deberás crear una cuenta con el nombre de soporte que podrá hacer ping, usar todos los comandos show y traceroute. Las claves serán ciscocisco.
2. En switch de sucursal Santiago, proteja el acceso al modo privilegiado con una contraseña protegida ciscocisco, cree el usuario admin con contraseña ciscocisco, habilite ssh versión 2, para esto use el dominio duoc.cl, 4 reintentos con un time-out de 30 segundos, aplique a todas las líneas vty el acceso con el usuario local y solamente permita SSH. Todas las interfaces de acceso deben estar asociadas a las VLANs correspondientes, en las cuales debe habilitar seguridad de puerto, permita sólo 2 MAC aprendidas de manera automática y en caso de violación de seguridad debe incrementar el contador de violaciones y generar un log. Todos los logs deben ir al servidor de logs. Todas las contraseñas que están en claro, se deben cifrar.
### Item 4: Control de Acceso por AAA y Monitoreo.
Realizar los siguientes requerimientos en Edge-Santiago:
1. El acceso remoto debe ser autenticado remotamente (debe ser default) a través del servidor RADIUS empresarial y si éste no está disponible, debe hacerlo a través de los usuarios de la base de datos local. La configuración de RADIUS debe llamarse REMOTAAA, use el puerto 1812 y el secreto entre cliente y servidor es ciscocisco. Los usuarios radius deben ser los que están configurados en el servidor AAA con sus respectivas contraseñas. El nombre del cliente RADIUS debe ser el hostname del router y la IP de la interfaz LAN SERVER.
2. Permite SSH a Edge-Santiago sólo desde la red de la VLAN 15 de sucursal Santiago y desde la red LAN de Viña (INSIDE). Usa la acl 1 y aplica a las líneas VTY.
3. Todos los logs deben ser enviados al servidor syslog.
4. Todos los dispositivos de red de Santiago se deben sincronizar la hora con el servidor NTP 172.16.0.14.
### ÍTEM 5: Protección perimetral.
Se te ha solicitado configurar los controles de acceso desde el exterior usando un firewall corporativo ASA. Los requerimientos son los siguientes:
1. Configura los parámetros básicos del firewall, nombre FW-VINA, dominio duoc.cl, la contraseña para todo es ciscocisco, de consola y enable.
2. En el firewall ASA, define cada interfaz con el nombre inside (100), outside (0) y dmz (50) y el nivel de seguridad indicado entre los paréntesis y configura la IP que aparece en la tabla de direccionamiento. Interfaz vlan1 con nombre inside, asociada a la interfaz e0/1, nivel de seguridad 100, la interfaz vlan 2, con nombre outside, nivel de seguridad 0, asociada a la interfaz e0/0, la interfaz vlan 3, con nombre dmz, con nivel de seguridad 50, asociada a la interfaz e0/2.
3. Configura una ruta por defecto en el firewall, con la IP de siguiente salto.
4. Este firewall es el servidor DHCP para la red inside, entre el rango de la .100 a la .130, con DNS 8.8.4.4. Los dispositivos de la zona DMZ deben tener IP manual.
5. El firewall debe realizar la traducción PAT de la red inside a la interfaz outside. El object debe llamarse INSIDE y debe traducir toda la red inside.
6. La autenticación debe ser AAA LOCAL, usando el usuario admin y contraseña ciscocisco.
7. Habilita SSH sólo para los equipos de la red interna, con un timeout de 5 minutos.
8. El firewall debe traducir la ip del servidor web de la dmz a la IP 177.44.56.5, de manera estática. El object debe llamarse static.
9. Debe permitir icmp y www a la IP pública del servidor (la asociada al NAT estático) desde cualquier origen hacia la IP nateada del servidor web (DMZ), la lista de acceso debe llamarse OUT, aplica a la interfaz outside.
10. Crea un class-map con nombre inspection_default que haga match con default-inspection-traffic. Cree un global_policy que llame al class-map y que inspeccione icmp, http y dns. Aplique el service-policy de manera global.
### ÍTEM 6: Configuración de VPN Site-to-Site.
Protección del tráfico interesante. La configuración se debe realizar entre Edge-Santiago y el ISP, recuerde activar la licencia de seguridad en ambos routers:
1. Identifica el tráfico interesante con la lista de acceso 100. Este es el tráfico que va desde la red de la VLAN 15 a la IP de la interfaz loopback 1 del router ISP y viceversa.
2. Configura la política ISAKMP fase 1 de IKE, en donde se debe usar el cifrado AES 256, con autenticación pre compartida, y el grupo Diffie-Hellman 5. La key isakmp, entre los pares debe ser ciscocisco.
3. Configura la política Ipsec fase 2 de IKE. Cree la transform-set Ipsec con nombre TRANS, usando esp-aes y esp-sha-hmac. Cree el crypto map con nombre VPN-MAP, en donde debes configurar el peer, aplicar el transform-set y coincidir con el tráfico interesante.
4. Aplica el crypto map a la interfaz correcta.
### ÍTEM 7: Detección y prevención de intrusos.
Realizar los requerimientos que se mencionan a continuación:
1. Habilita el IOS IPS en el router Edge_Santiago. Recuerde que debe estar habilitada la licencia de seguridad.
2. Crea el directorio ipsdir, y configura la ubicación de almacenamiento de firmas IPS.
3. Crea una regla IPS con nombre iosips. Defina la localización en la flash:.
4. Configure para usar la categoría de firmas. Para esto, retíralas todas y agrega la categoría ios_ips basic. Salga y confirme cambios
5. Aplica la regla IPS a la interfaz de la red del SERVER.
6. Cambia el evento de acción de una firma. Retira la firma 2004, ID 0, en status habilítala, en engine el evento debe ser que produzca alertas, sal y confirma los cambios.
# Configuracion
## Dispositivos Finales
DNS: 8.8.4.4
### PC-2
- IP: DHCP
### PC-3
- IP: DHCP
### PC-0
- IP: DHCP
### PC-1
- IP: DHCP
### Server Web
- Configurado
### Server AAA-Syslog
- Configurado
#### NTP
- Encendido
#### Syslog
- Encendido
#### AAA
- Necesita ser Encendido
- Puerto: 1812
- Client Name: REMOTAAA
- Secret: ciscocisco
- Client IP: 172.16.0.14

- User/Password: Edge-Santiago/ciscocisco 
- User/Password: SW-SANTIAGO/ciscocisco

## Troubleshoting
### Edge-Santiago
Salto a la ip de isp esta incorrecta
```
no ip route 0.0.0.0 0.0.0.0 200.7.7.1
ip route 0.0.0.0 0.0.0.0 200.2.7.1
```
G0/2 hacia server esta Mal la mascara
```
int g0/2
no ip address 172.16.0.1 255.255.255.0
ip address 172.16.0.1 255.255.255.240
no shut
exit
```
G0/1 no tiene configurada ip
```
int g0/1
ip address 200.2.7.6
```
G0/0 Puerta apagada
```
int g0/0
no shut
exit
```
### ISP
Creacion de Loopback 1 para VPN
```
int loopback 1
ip address 8.8.8.8 255.255.255.255
exit
```
## Firewall ASA
Nombre incorrecto
```
hostname FW-VINA
```
Domain-Name Incorrecto
```
no domain-name asa.cl
domain-name duoc.cl
```
Viene configuracion telnet timeout, ssh es el por defecto por seguridad
```
no telnet timeout 5
```
## PC2 y PC3 tienen IP equivocadas
Al configurar DHCP y activarlo, quedan en el segmento correct
## SW-Santiago
```
enable
config t
hostname SW-Santiago
service password-encryption
vlan 15
exit
vlan 25
exit
!# Modo privilegiado
enable secret ciscocisco
!# Usuario Admin
username admin privilege 15 secret ciscocisco
!# SSH
ip domain-name duoc.cl
crypto key generate rsa
2048
line vty 0 15
transport input ssh
login local
exit
ip ssh version 2
ip ssh authentication-re 4
ip ssh time-out 30
!
!# Desactiva modo acceso en todas las demas vlan
int range f0/1-24
no switchport mode access
no switchport access vlan 15
no switchport access vlan 25
no spanning-tree portfast
exit
!# Acceso VLAN
int range f0/6,f0/20
switchport mode access
switchport port-security
switchport port-security mac-address sticky
switchport port-security maximum 2
switchport port-security violation restrict
no shut
exit
int f0/6
switchport access vlan 15
exit
int f0/20
switchport access vlan 25
exit
!# Troncales VLAN
int g0/1
switchport mode trunk
switchport trunk allowed vlan 15,25
no shut
exit
!# Syslog
logging host 172.16.0.14
logging trap
logging on
!# NTP
ntp server 172.16.0.14
```
## SW-Viña
```
enable
config t
hostname SW-VINA
service password-encryption
!# Modo Privilegio
enable secret ciscocisco
!# Acceso VLAN
int range f0/8,f0/16
switchport mode access
switchport port-security
switchport port-security mac-address sticky
switchport port-security maximum 2
switchport port-security violation restrict
no shut
exit
!# Troncales VLAN
int g0/1
switchport mode trunk
no shut
exit
!# Syslog
logging host 172.16.0.14
logging trap
logging on
!# NTP
ntp server 172.16.0.14
```
## Activar Licencia Edge-Santiago
```
enable
config t
license boot module c2900 technology-package securityk9
yes
do wr
exit
reload
```
## Router Edge-Santiago
```
enable
clock set 13:00:00 JUL 10 2024
config t
!# Troubleshooting
!# 1. Mascara server Mala
int g0/2
no ip address 172.16.0.1 255.255.255.0
ip address 172.16.0.1 255.255.255.240
no shut
exit
!
!# 2. Puerta g0/0 Apagada
int g0/0
no shut
exit
!
!# Salto ip a ISP Malo
no ip route 0.0.0.0 0.0.0.0 200.7.7.1
ip route 0.0.0.0 0.0.0.0 200.2.7.1
!
!# Habilitar AAA
aaa new-model
!# Min Length
service password-encryption
security passwords min-length 8
login block-for 45 attempts 3 within 60
!# Modo Privilegiado
enable secret ciscocisco
!# Banner
banner motd # ACCESO RESTRINGIDO#
!# Usuario Admin con privilegio
username admin privilege 15 secret ciscocisco
!
!# SSH
ip domain-name cisco.com
crypto key generate rsa
2048
yes
2048
line vty 0 15
transport input ssh
ip ssh version 2
ip ssh authentication-retries 2
ip ssh time-out 30
!# Syslog
logging host 172.16.0.14
logging trap
logging on
!# NTP
ntp server 172.16.0.14
!# ITEM 3
!# Usuario Administrador con todos los comandos show
username administrador privilege 14 secret ciscocisco
privilege exec level 14 configure
privilege exec level 14 configure terminal
!
!# Usuario Soporte con todos los comandos show, ping y traceroute
username soporte privilege 13 secret ciscocisco
privilege exec level 13 show
privilege exec level 13 ping
privilege exec level 13 traceroute
!
privilege exec level 13 enable
privilege exec level 13 no
!
!# AAA
!# RADIUS Y LOCAL / Configuracion RADIUS: REMOTAAA / Puerto: 1812 / Clave: ciscocisco
!
!
!
!# ITEM 6: VPN Site-to-Site
crypto isakmp policy 1
authentication pre-share
encryption aes 256
hash sha
group 5
lifetime 86400
exit
crypto isakmp key ciscocisco address 200.2.7.1
crypto ipsec transform-set TRANS esp-aes 256 esp-sha-hmac
ip access-list extended 100
permit ip 172.15.15.0 255.255.255.0 host 8.8.8.8
exit
crypto map VPN-MAP 1 ipsec-isakmp
match address 100
set transform-set TRANS
set peer 200.2.7.1
exit
int g0/1
crypto map VPN-MAP
exit
```
## Router Edge-Santiago IPS
usualmente se escribe esta parte a mano para evitar errores
```
!# Item 7: IPS
mkdir ipsdir
ip ips config location flash:ipsdir
ip ips name iosips
ip ips signature-category
category all
retired true
exit
category ios_ips basic
retired false
exit
exit
!# Enter
!
int g0/2
ip ips iosips in
exit
!# DUDA, las demas interfaces van con out??
!
ip ips signature-definition
signature 2004 0
status
retired false
enabled true
exit
engine
event-action produce-alert
event-action deny-packet-inline
exit
exit
exit
!# Confirm
```
## Activar Licencia ISP
```
enable
config t
license boot module c2900 technology-package securityk9
yes
do wr
exit
reload
```
## Router ISP
```
enable
config t
!# Troubleshooting Creacion loopback 1
int loopback 1
ip address 8.8.8.8 255.255.255.255
exit
!# Syslog
logging host 172.16.0.14
logging trap
logging on
!# NTP
ntp server 172.16.0.14
!# ITEM 6: VPN Site-to-Site
crypto isakmp policy 1
authentication pre-share
encryption aes 256
hash sha
group 5
lifetime 86400
exit
crypto isakmp key ciscocisco address 200.2.7.6
crypto ipsec transform-set TRANS esp-aes 256 esp-sha-hmac
ip access-list extended 100
permit ip host 8.8.8.8 172.15.15.0 255.255.255.0
exit
crypto map VPN-MAP 1 ipsec-isakmp
match address 100
set transform-set TRANS
set peer 200.2.7.6
exit
int g0/1
crypto map VPN-MAP
exit
```
## FW-ASA
`enable` y luego "Enter"
Parte Manual
```
config t
!# Troubleshooting
hostname FW-VINA
no domain-name asa.cl
domain-name duoc.cl
no telnet timeout 5
crypto key generate rsa modulus 1024
```

```
config t
!# ITEM 5: FW-ASA
clock set 13:00:00 10 JUL 2024
hostname FW-VINA
enable password ciscocisco
username admin password ciscocisco
no dhcp address 192.168.1.5-192.168.1.36 inside
int vlan 1
no ip address 192.168.1.1 255.255.255.0
exit
int vlan 2
no nameif OUTSIDE
exit
int vlan 3
nameif DMZ
security-level 50
ip address 10.20.30.1 255.255.255.224
no forward interface vlan 1
exit
int vlan 1
nameif INSIDE
security-level 100
ip address 192.168.23.1 255.255.255.0
exit
int vlan 2
nameif OUTSIDE
security-level 0
ip address 177.44.56.6 255.255.255.248
exit
int e0/1
switchport access vlan 1
exit
int e0/0
switchport access vlan 2
exit
int e0/2
switchport access vlan 3
exit
route OUTSIDE 0.0.0.0 0.0.0.0 177.44.56.1
dhcpd address 192.168.23.100-192.168.23.130 inside
dhcpd dns 8.8.4.4 int inside
dhcpd enable inside
!# NAT PAT Inside -> Outside
object network INTERNET
subnet 192.168.23.0 255.255.255.0
nat (INSIDE,OUTSIDE) dynamic interface
!
!# MPF PING
class-map inspection_default
match default-inspection-traffic
exit
policy-map global_policy
class inspection_default
inspect icmp
inspect http
inspect dns
exit
service-policy global_policy global
!
!# NAT ESTATICO PC-DMZ -> OUTSIDE
object network static
host 10.20.30.30
nat (DMZ,OUTSIDE) static 177.44.56.1
!
access-list static permit ip any host 10.20.30.30
access-group static in interface outside
!# SSH
aaa authentication ssh console LOCAL
ssh 192.168.23.0 255.255.255.0 inside
ssh timeout 5
```