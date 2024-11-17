## Imagen Topologia
![500](https://slink.proxylivy.work/image/3512110b-c2ed-44a5-94d2-7633bd488da9.png)
## Notas
Tienes que abrir TFTP y configurarlo para mostrar el syslog

## R2
```
enable 
config t
router ospf 1
router-id 2.2.2.2
network 192.168.12.0 255.255.255.0 area 12
network 192.168.23.0 255.255.255.0 area 0
exit
clock set [Hora:Min:Sec] [Dia] [Month] [Año]
show clock
logging on
logging host 192.168.56.1
logging trap 5
logging source-interface s1/0
int loopback 5

```

## R3
```
enable
config t
router ospf 1
router-id 3.3.3.3
network 192.168.23.0 255.255.255.0 area 0
network 192.168.33.3 255.255.255.255 area 0
passive-interface loopback 3
exit
int tunnel 0
ipv6 address 3001:ACAD:ACAD:13::3/112
tunnel source s1/1
tunnel destination 192.168.12.1
tunnel mode ipv6ip
keepalive 5
end
ping 3001:ACAD:ACAD:13::3
ping 3001:ACAD:ACAD:13::1
config t
ipv6 unicast-routing
ipv6 router ospf 1
router-id 3.3.3.3
passive-interface loopback 3
exit
interface range loopback 3, tunnel 0
ipv6 ospf 1 area 0
exit
aaa new-model
parser view VISTA_USER_SOPORTE
secret cisco8
commands exec include show version
commands exec include show ip route
commands exec include configure terminal
commands configure include hostname
commands configure include interface s1/0
commands interface include ip address
commands interface include shutdown
commands interface include no
exit
enable secret perita
ntp server 192.168.23.2
logging on
logging host 192.168.56.1
logging trap 5
logging source-interface s1/1
int loopback 7
exit
```
### Probar Vista en 
```
enable view VISTA_USER_SOPORTE
password: cisco8
show parser view
?
show ?
end
enable view
password: perita
show parser view
```

## R1
```
enable
config t
router ospf 1
router-id 1.1.1.1
network 192.168.12.0 255.255.255.0 area 12
network 192.168.56.0 255.255.255.0 area 12
passive-interface e0/1
exit
int tunnel 0
ipv6 address 3001:ACAD:ACAD:13::1/64
tunnel source s1/0
tunnel destination 192.168.23.3
tunnel mode ipv6ip
keepalive 5
exit
ipv6 unicast-routing
ipv6 router ospf 1
router-id 1.1.1.1
passive-interface e0/1
exit
interface range e0/1, tunnel 0
ipv6 ospf 1 area 0
exit
security password min-length 6
line console 0
password perita
exec-timeout 2 0
exit
username user01 password cisco1
username user02 secret cisco2
login block-for 60 attempts 2 within 30
login on-success log every 2
login on-failure log every 2
enable secret modoshorizo
username user_exec_5 privilege 5 secret cisco5
privilege exec level 5 show versin
privilege exec level 5 configure terminal
privilege configure level 5 hostname
line console 0
login local
exit
ntp server 192.168.12.2
logging on
logging host 192.168.56.2
logging trap 5
logging source-interface e0/1
int loopback 4
exit
```
### Verificacion de usuarios en R1 
```
!
username: user_exec_5
password: cisco5
show privilege
configure terminal
end
exit
!
username: user02
pass: cisco02
enable
password: modoshorizo
show privilege
```

## SW1
```
enable
config t
int range e0/1-2
duplex half
```