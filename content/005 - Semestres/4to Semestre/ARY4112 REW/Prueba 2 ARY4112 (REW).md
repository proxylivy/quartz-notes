# Info
## Imagen Topologia
![](https://slink.proxylivy.work/image/5519ed0e-87a8-4571-878b-f166bee3837d.png)
## Requerimientos
- Configurar direccionamiento IP en toda la topología, incluyendo equipos finales como PC y Servidor. (Router CM-STGO crear subinterfaces para las dos vlan 31 y 155).
	- Ejemplo : Interface Giga Ethernet 0/1.31 y Giga ethernet 0/1.155
- Configurar enrutamiento por defecto en router CM-STGO/SUC-VALDIVIA-WORKER, aplicando como parámetro la IP del siguiente salto.
- Configurar en Servidor WEB-CRM HTTP .
- Configurar vlan 31 y 155 en switch correspondiente.
- Configurar ROAS o SobInterfaces para las dos vlan en router CM-STGO.
- Probar conectividad a las IP Publicas de la topología.
- Configurar túnel VPN Site-to-Site entre las redes LAN Gerencia y LAN Sucursal.
- Configurar PC con (IP / Mascara Subred/ Gateway/IP DNS 172.31.1.5) en todos los PC de la topología.
- Considerar nombres de los routers según indicación de topología.
- Considerar para ACL extendida sus nombres NAT y VPN respectivamente.

**PARAMETROS DE CONFIGURACION VPN SITE-to-SITE FASE 1**

| Parametros                 | ROUTER CM-STGO | ROUTER SUC-VALDIVIA |
| -------------------------- | -------------- | ------------------- |
| N° Politica                | 10             | 10                  |
| Metodo Distribucion Claves | ISAKMP         | ISAKMP              |
| Algoritmo Cifrado          | 3des           | 3des                |
| Algoritmo HASH             | MD5            | MD5                 |
| Autenticacion              | PRE-Shared     | PRE-Shared          |
| Intercambio de claves      | Group2         | Group2              |
| Vida Util SA IKE           | 86400          | 86400               |
| ISAKMP Clave               | Ary4112        | Ary4112             |

**PARAMETROS DE CONFIGURACION VPN SITE-to-SITE FASE 2**

| Parametros                                      | ROUTER SM-STGO        | ROUTER SUC-VALDIVIA   |
| ----------------------------------------------- | --------------------- | --------------------- |
| Conjunto de transformaciones<br>(transform-set) | TS-VPN                | TS-VPN                |
| Tipo de encriptacion                            | Esp-3des esp-sha-hmac | Esp-3des esp-sha-hmac |
| Direccion Peer<br>(set peer)                    | 190.78.99.2           | 110.121.23.2          |
| Red a Cifrar<br>(ACL)                           | 10.155.10.0/24        | 192.168.100.0/24      |
| Nombre asignacion Criptografica<br>(cryto map)  | CRYMAP1               | CRYMAP2               |
| Establecimiento de SA                           | 10 / ipsec-isakmp     | 10 / ipsec-isakmp     |

**CONFIGURACION NAT EN TOPOLOGIA**
> Nota: Recordar crear las ACL extendidas correspondientes para filtrar el trafico interesante de la VPN y de PAT correspondiente. (hay que recordar que deben denegar el trafico de la VPN en la ACL del NAT).
- Configurar NAT Estático en Router CM-STGO creando la regla para que la IP Publica 110.121.23.3 apuntar al servidor de la red LAN 172.31.1.5
- Configurar PAT en todos los routers para que los PC puedan llegar a la IP publica en Internet 1.1.1.1 y 8.8.8.8 respectivamente.
- **PROBAR** conectividad tanto de la VPN como del NAT estático y del PAT en todos los equipos.

# Configuracion
## Dispositivos Finales
Servidor
Nota: Habilita HTTP
- IPv4: 172.31.1.5
- Subnet: 255.255.255.248
- DG: 172.31.1.1
- DNS: 172.31.1.5

### PC .100
- IPv4: 10.155.10.100
- Subnet: 255.255.255.0
- DG: 10.155.10.1
- DNS: 172.31.1.5

### PC .50
- IPv4: 192.168.1.50
- Subnet: 255.255.255.0
- DG: 192.168.1.1
- DNS: 172.31.1.5

### PC .120
- IPv4: 192.168.100.120
- Subnet: 255.255.255.0
- DG: 192.168.100.254
- DNS: 172.31.1.5


Switch0
```
enable
config t
!
hostname Switch0
VLAN 31
exit
!
VLAN 155
exit
!
int f0/1
switchport mode access
switchport access vlan 31
no shutdown
exit
!
int f0/2
switchport mode access
switchport access vlan 155
no shutdown
exit
!
int g0/1
switchport mode trunk
switchport trunk allowed vlan 31,155
switchport nonegotiate
no shut
exit
```
SWB
```
enable
config t
hostname Switch1
!
VLAN 31
exit
!
VLAN 155
exit
!
int g0/1
switchport mode trunk
switchport trunk allowed vlan 31,155
switchport nonegotiate
no shut
exit
!
int g0/2
switchport mode access
switchport access
no shutdown
exit
```
## R CM-STGO
```
enable
config t
!
hostname CM-STGO
int g0/1
no shut
exit
!
interface GigabitEthernet0/1.31
encapsulation dot1Q 31
ip address 172.31.1.1 255.255.255.248
no shutdown
exit
!
interface GigabitEthernet0/1.155
encapsulation dot1Q 155
ip address 10.155.10.1 255.255.255.0
no shutdown
exit
!
interface g0/0
ip address 110.121.23.2 255.255.255.248
no shutdown
exit
!
ip access-list extended VPN 
permit ip 10.155.10.0 0.0.0.255 192.168.100.0 0.0.0.255
exit
!
crypto isakmp policy 10
encr 3des
hash md5
authentication pre-share
group 2
lifetime 86400
exit
!
crypto isakmp key Ary4112 address 190.78.99.2 0.0.0.0
crypto ipsec transform-set TS-VPN esp-3des esp-sha-hmac
!
crypto map CRYMAP1 10 ipsec-isakmp 
set peer 190.78.99.2
set transform-set TS-VPN 
match address VPN
exit
!
interface g0/0
crypto map CRYMAP1
!
!
!# ip route 0.0.0.0 0.0.0.0 201.100.100.2 !# No funciona
!# ip route 0.0.0.0 0.0.0.0 190.78.99.2 !# No funciona
```
## SUC-VALDIVIA
```
enable
config t
hostname SUC-VALDIVIA
!
interface g0/2
ip address 190.78.99.2 255.255.255.248
no shutdown
exit
!
interface g0/0
ip address 192.168.100.254 255.255.255.0
no shutdown
exit
!
ip access-list extended VPN 
permit ip 10.155.10.0 0.0.0.255 192.168.100.0 0.0.0.255
exit
!
crypto isakmp policy 10
encr 3des
hash md5
authentication pre-share
group 2
lifetime 86400
exit
!
crypto isakmp key Ary4112 address 110.121.23.2 0.0.0.0
crypto ipsec transform-set TS-VPN esp-3des esp-sha-hmac
!
crypto map CRYMAP1 10 ipsec-isakmp 
set peer 110.121.23.2
set transform-set TS-VPN 
match address VPN
!
interface g0/2
crypto map CRYMAP1
```
## WORKER
```
enable
config t
hostname WORKER
int g0/1
ip add 201.100.100.2 255.255.255.252
no shut
exit
int g0/0
ip add 192.168.1.1 255.255.255.0
no shut
exit
```
