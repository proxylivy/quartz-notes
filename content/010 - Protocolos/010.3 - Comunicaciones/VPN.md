# Info
VPN es una red privada que se accede mediante tunneles, esa informacion se protege con [[010 - Protocolos/010.1 - Routing/IPSEC|IPSEC]](protege capa 3 y capa 4, capa 2 se protege con [[010 - Protocolos/010.2 - Switching/Seguridad de Puerto|Seguridad de Puerto]]), Su configuracion se hacen principalmente en Firewalls, pero se puede hacer en Routers o Servidores y su configuracion es igual en ambos puntos con los parametros mas avanzados entre vendors.

En las VPN Site-to-Site, son estaticas y ambos extremos sabes donde van y vienen los mensajes, ejemplos de redes: Frame-Relay, ATM, [[010 - Protocolos/010.1 - Routing/Tunel GRE|Tunel GRE]] y MPLS
## Pasos
1. Configuracion de la politica ISAKMP ([[#IKE Fase 1]])
2. Configuracion de los IPsec transform sets ([[#IKE Fase 2]])
3. Configuracion de la Crypto ACL ([[#Permitir Trafico con ACL Extendida Nombrada|Trafico Interesante]])
4. Configuracion del crypto map ([[#Crypto map]])
5. Aplicacion del crypto map a la interfaz ([[#Aplica interfaz hacia el otro punto (Usualmente ISP)|Funcionar en Interfaz]])

Un atacante no ataca una VPN Directamente

## Tipos
- Acceso Remoto
- SSL
	- Clientless: Navegador sin necesidad de VPN especializado, Ejemplo WebVPN
	- Thin Client: "TCP Port Forwarding", se usa con un complemento de JAVA como servidor Proxy, Ejemplo: Cisco Anyconnect
	- Full Client: Complemento Java maneja tunel entre cliente y gateway vpn ssl, permite el uso de app internas


VPN Clientless
ASDM herramienta basada en JAVA, donde se puede configurar y revisar un firewall basico, no lo configuramos

Preparacion
1. Configurar Interfaz
2. Ponerle IP
3. Y alli se configura

Recordar ponerle no shutdown en las puertas, ya que aunque brillen, estan apagadas
La g0/0 se deja para outside
y la g0/1 para inside
Las vpn point-to-point son fijas, no es la mas comun, la mas comun es la de acceso remoto

# Configuracion
Nota: En Packet tracer se tiene que licenciar
Debe tener conectividad entre ambos puntos para que la VPN funcione, como en un [[010 - Protocolos/010.1 - Routing/Tunel GRE|Tunel GRE]]
## IKE Fase 1
El unico valor que puede cambiar en ambos lados es el `policy-number`
```
!# IKE fase 1
crypto isakmp policy [policy-number]
authentication [auth-type]
encryption [encryption-type]
group [group-number]
hash [hash-type]
lifetime [lifetime-time-sec]
exit
crypto isakmp key [key-password] address [ip-vpn-other-end]
```
## IKE Fase 2
Nota: El modo tunel es el por defecto
```
crypto ipsec transform-set [transform-name] esp-aes [key-number] esp-sha-hmac
exit
```
## Permitir Trafico Interesante con ACL Extendida Nombrada
Apoyo: [[020 - Conceptos/020.1 - Administracion/ACL|ACL]]
Nota: Entre routers, el trafico interesante son las loopback
```
ip access-list extended [list-name]
permit [protocol] [ip-network-source] [wildcard-net-source] [ip-network-destination] [wildcard-net-destination]
```
## Crypto map
Nota: Es un tipo de route-map
```
crypto map [crypto-name] [crypto-number] ipsec-isakmp
match address [list-name]
set peer [ip-vpn-other-end]
set transform-set [transform-name]
exit
```
## Aplica interfaz hacia el otro punto (Usualmente ISP)
```
int [int S/S/P]
crypto map [crypto-name]
exit
```
## Ejemplo EasyVPN
```
aaa new-model
aaa authentication login [ACCESO] local
aaa authorization network [ACCESO] local
username [local-user] password [local-password]
crypto isakmp policy 1
authentication pre-share
encryption aes 256
hash sha
group 2
lifetime 86400
crypto isakmp client configuration group [ACCESO]
key [key-password]
pool [pool-name]
crypto ipsec transform-set [T] esp-aes 256 esp-sha-hmac
crypto dynamic-map [MAPA-D] 1
set transform-set [T]
reverse-route
crypto map [map-name] client authentication list [ACCESO]
crypto map [map-name] isakmp authorization list [ACCESO]
crypto map [map-name] client configuration address respond
crypto map [map-name] [map-number] ipsec-isakmp dynamic [MAPA-D]
ip local pool [pool-name] [ip-start] [ip-end]
int [S/S/P] !# Ruta hacia alla supongo
crypto map [map-name]
```

# Visualizacion
`show crypto ipsec sa` → Ver el trafico que ha sido encriptado y desencriptado
`show crypto isakmp sa` → (Screenshot) Para ver como funciona VPN
`ping [ip-next-endpoint] source loopback [loopback-number]`

# Extra
- En troubleshooting se vera MPLS, el papa de MPLS es Frame-Relay, el cual es lento
- Gestiones en Riesgo Corporativo es el proximo a Seguridad en Redes
- Se pueden hacer "Tuneles Divididos con hairpinning" (No lo veremos)
