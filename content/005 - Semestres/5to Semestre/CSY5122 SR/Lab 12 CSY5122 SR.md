# Info
## Imagen Topologia
![500](https://slink.proxylivy.work/image/90a10ed6-d737-46c0-9952-9dfb5ef50e7c.png)
## Sobre
Implementacion de VPN Site-to-Site
## Requerimientos
- Habilitar protocolo de enrutamiento estado de enlace. Usar router-id en formato "X.X.X.X" donde la "X" es reemplazada por el número de router.
- Habilitar interfaces pasivas según sea necesario.
- Configurar interfaces multilink, con direccionamiento IPv4/IPv6 propuesto.
- Implementar solución de agregado de enlace según protocolo propuesto.
- Optimizar los temporizadores de protocolo de enrutamiento [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv2#Modificar Tiempos de OSPF|Estado de Enlace]], los cuales deben quedar equivalentes a protocolo [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP#Temporizadores por Defecto|Vector Distancia]], esto es para IPv4/IPv6.
- Router R1, debe asignar direccionamiento IPv4 de forma dinámica a red LAN de VLAN10.
- Implementar seguridad de puerto dinámica.
- Habilitar mecanismos de estabilización de STP, donde interfaces que conectan a equipos finales no deben enviar ni recibir BPDU, además de pasar automáticamente a estado de FWD.
- Habilitar DHCP Snooping, además equipos finales solo pueden solicitar un máximo de 3 direcciones IP.
- Todo el tráfico entre la red 172.16.10.0/24 a 172.16.30.0/24 debe ser cifrado. Utilizar a continuación, los siguientes parámetros solicitados:

| Caractistica              | Posible Conf    | R1        | R3        |
| ------------------------- | --------------- | --------- | --------- |
| Metodo Distribucion Llave | ISAKMP          | ISAKMP 10 | ISAKMP 10 |
| Algoritmo de Cifrado      | DES,3DES o AES  | AES       | AES       |
| Algoritmo Hash            | MD5 o SHA-1     | SHA-1     | SHA-1     |
| Metodo de autenticacion   | PSK o RSA       | Preshare  | Preshare  |
| Intercambio de Llave      | DH 1,2 o 5      | DH5       | DH5       |
| ttl IKE SA                | 86400 sec o mas | 86400     | 86400     |
| Llave ISAKMP              | `[Password]`    | cisco     | cisco     |

| Caracteristica     | R1           | R3           |
| ------------------ | ------------ | ------------ |
| Transform Set      | VPN-SET 10   | VPN-SET 10   |
| Nombre del Peer    | R1           | R3           |
| Red Cifrada        | LAN R1       | LAN R3       |
| Crypto-Name        | VPN-MAP      | VPN-MAP      |
| Establecimiento SA | ipsec-isakmp | ipsec-isakmp |

- Generar eventos Syslog, que permita ser capturados, en servidor TFTP64, correspondiente
# Configuracion
## R1
```
enable
config t
ipv6 unicast-routing
int multilink 1
encapsulation ppp
ip address 192.168.12.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:12::1/64
exit
int s1/0
encapsulation ppp
ppp multilink
ppp multilink group 1
no shut
exit
int s1/2
encapsulation ppp
ppp multilink
ppp multilink group 1
no shut
exit
int 0/0
no shut
exit
int 0/0.10
encapsulation dot1q
ip address 172.16.10.1 255.255.255.0
ipv6 address 3001:ACAD:ACAD:10::1/64
exit
router ospf 1
router-id 1.1.1.1
network 172.16.10.0 255.255.255.0 area 0
network 192.168.12.0 255.255.255.0 area 0
pasive-interface e0/0.10
exit
ipv6 router ospf 1
router-id 1.1.1.1
passive-interface e0/.10
exit
int e0/0.10
ipv6 ospf 1 area 0
exit
int multilink 1
ipv6 ospf 1 area 0
exit
int multilink 1
ip ospf hello-interval 5
ip ospf dead-interval 15
ipv6 ospf hello-interval 5
ipv6 ospf dead-interval 15
exit
ip dhcp excluded-address 172.16.10.1 172.16.10.4
ip dhcp pool VLAN10
network 172.16.10.0 255.255.255.0
default-router 172.16.10.1
exit
!# IKE FASE 1
crypto isakmp policy 10
encryption aes
hash sha
authentication pre-share
group 5
lifetime 86400
exit
crypto isakmp key cisco address 192.168.23.3
!# IKE FASE 2
crypto ipsec transform-set VPN-SET esp-aes 128 esp-sha-hmac
exit
!# Trafico
ip access-list extended PERITA
permit ip 172.16.10.0 0.0.0.255 172.16.30.0 0.0.0.127
exit
!# Crypto map
crypto map VPN-MAP 1 ipsec-isakmp
match address PERITA
set transform-set VPN-SET
set peer 192.168.23.3
exit
int multilink 1
crypto map VPN-MAP
exit
end
copy running-config unix:
```
## R2
```
enable
config t
ipv6 unicast-routing
int multilink 1
encapsulation ppp
ip address 192.168.12.2 255.255.255.0
ipv6 address 3001:ACAD:ACAD:12::2/64
int s1/0
encapsulation ppp
ppp multilink
ppp multilink group 1
no shut
exit
int s1/2
encapsulation ppp
ppp multilink
ppp multilink group 1
no shut
exit
!# Segundo Multilink
int multilink 2
encapsulation ppp
ip address 192.168.23.2 255.255.255.0
ipv6 address 3001:ACAD:ACAD:23::2/64
exit
int s1/1
encapsulation ppp
ppp multilink
ppp multilink group 2
no shut
exit
int s1/3
encapsulation ppp
ppp multilink
ppp multilink group 2
no shut
exit
router ospf 1
router-id 2.2.2.2
network 192.168.12.0 255.255.255.0 area 0
network 192.168.23.0 255.255.255.0 area 0
exit
ipv6 router ospf 1
router-id 2.2.2.2
exit
int multilink 1
ipv6 ospf 1 area 0
exit
int multilink 2
ipv6 ospf 1 area 0
exit
int multilink 1
ip ospf hello-interval 5
ip ospf dead-interval 15
ipv6 ospf hello-interval 5
ipv6 ospf dead-interval 15
exit
int multilink 2
ip ospf hello-interval 5
ip ospf dead-interval 15
ipv6 ospf hello-interval 5
ipv6 ospf dead-interval 15
exit
end
copy running-config unix:
```
## R3
```
enable
config t
ipv6 unicast-routing
int multilink 2
encapsulation ppp
ip address 192.168.23.3 255.255.255.0
ipv6 address 3001:ACAD:ACAD:23::3/64
exit
int s1/1
encapsulation ppp
ppp multilink
ppp multilink group 2
no shut
exit
int s1/3
encapsulation ppp
ppp multilink
ppp multilink group 2
no shut
exit
router ospf 1
router-id 3.3.3.3
network 192.168.23.0 255.255.255.0 area 0
network 172.16.30.0 255.255.255.0 area 0
!# Enruta la red del virtualbox
network 192.168.56.0 255.255.255.0 area 0
!# Pasiva la red del virtualbox
passive-interface e0/0
passive-interface e0/1
exit
ipv6 router ospf 1
router-id 3.3.3.3
passive-interface e0/0
int e0/0
ipv6 ospf 1 area 0
int multilink 2
ipv6 ospf 1 area 0
exit
int multilink 2
ip ospf hello-interval 5
ip ospf dead-interval 15
ipv6 ospf hello-interval 5
ipv6 ospf dead-interval 15
exit
!# IKE FASE 1
crypto isakmp policy 10
encryption aes
hash sha
authentication pre-share
group 5
lifetime 86400
exit
crypto isakmp key cisco address 192.168.12.1
!# IKE FASE 2
crypto ipsec transform-set VPN-SET esp-aes 128 esp-sha-hmac
exit
!# Trafico
ip access-list extended PERITA
permit ip 172.16.30.0 0.0.0.127 172.16.10.0 0.0.0.255
exit
!# Crypto map
crypto map VPN-MAP 1 ipsec-isakmp
match address PERITA
set transform-set VPN-SET
set peer 192.168.12.1
exit
int multilink 2
crypto map VPN-MAP
exit
logging on
logging host 192.168.56.1
logging trap 6
logging source-interface e0/1
int loopback 11
exit
no int loopback 11
end
copy running-config unix:
```
## SWA
```
enable
config t
vlan 10
exit
vtp mode transparent
int range e0/0-2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10
exit
int range e0/1-2
channel-group 1 mode active
exit
ip dhcp snooping
ip dhcp snooping vlan 10
no ip dhcp snooping information option
int e0/0
ip dhcp snooping trust
exit
end
copy running-config unix
```
## SWB
```
enable
config t
vlan 10
exit
vtp mode transparent
int e0/3
switchport mode access
switchport access vlan 10
switchport port-security
spanning-tree bpduguard enable
spanning-tree bpdufiltler enable
spanning-tree portfast
ip dhcp snooping limit rate 3
exit
int range e0/1-2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10
!# Solo en esta version, no acepta DTP
# switchport nonegotiate
exit
int range e0/1-2
channel-group 1 mode passive
exit
ip dhcp snooping
ip dhcp snooping vlan 10
no ip dhcp snooping information option
int port-channel 1
ip dhcp snooping trust
exit
end
copy running-config unix:
```
# Visualizar
## PC-VLAN-10
```
enable
ping 172.16.30.10
ping 172.16.30.10 repeat 100
exit
```
## R1
```
enable
show crypto ipsec sa
```