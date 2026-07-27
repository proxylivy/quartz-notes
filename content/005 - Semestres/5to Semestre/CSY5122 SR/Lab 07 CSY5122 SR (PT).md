# Topologia
![500](https://slink.proxylivy.work/image/25e60633-6163-4963-ba65-244c388b0793.png)
R3
```
enable
config t
router ospf 20
router-id 3.3.3.3
network 172.16.32.0 255.255.255.0 area 0
default-information originate
exit
int s0/0/0
ip ospf message-digest-key 1 md5 modoshorizo
ip ospf authentication message-digest
ip ospf hello-interval 5
ip ospf dead-interval 20
exit
!
ip route 0.0.0.0 0.0.0.0 208.50.0.2
!
ip access-list standard INTERNET
permit 172.16.12.0 0.0.0.255
exit
ip nat inside source list INTERNET interface s0/0/1 overload
int s0/0/0
ip nat inside
exit
int s0/0/1
ip nat outside
exit
```

R2
```
enable
config t
router ospf 20
router-id 2.2.2.2
network 172.16.21.0 255.255.255.0 area 0
network 172.16.32.0 255.255.255.0 area 0
exit
int s0/0/0
ip ospf message-digest-key 1 md5 modoshorizo
ip ospf authentication message-digest
ip ospf hello-interval 5
ip ospf dead-interval 20
exit
int s0/0/1
ip ospf message-digest-key 1 md5 modoshorizo
ip ospf authentication message-digest
ip ospf hello-interval 5
ip ospf dead-interval 20
exit
```

R1
```
enable
config t
router ospf 20
router-id 1.1.1.1
network 172.16.21.0 255.255.255.0 area 0
network 172.16.11.0 255.255.255.0 area 1
exit
int s0/0/1
ip ospf message-digest-key 1 md5 modoshorizo
ip ospf authentication message-digest
ip ospf hello-interval 5
ip ospf dead-interval 20
exit
int s0/0/0
ip ospf message-digest-key 1 md5 modoshorizo
ip ospf authentication message-digest
ip ospf hello-interval 5
ip ospf dead-interval 20
exit
```

IPS
```
enable
config t
router ospf 20
router-id 4.4.4.4
network 172.16.11.0 255.255.255.0 area 1
network 172.16.12.0 255.255.255.0 area 1
passive-interface g0/0
exit
int s0/0/0
ip ospf message-digest-key 1 md5 modoshorizo
ip ospf authentication message-digest
ip ospf hello-interval 5
ip ospf dead-interval 20
exit
license boot module c1900 technology-package securityk9
!# WRITE "YES"
do wr
end
reload
!# WAIT
enable
mkdir DIRECTORIO-IPS
config t
ip ips config location flash:DIRECTORIO-IPS
ip ips name REGLA-IPS
clock set 17:50:00 24 APRIL 2024
logging on
logging host 172.16.12.20
service timestamps log datetime msec
ip ips signature-category
category all
retired true
exit
category ios_ips basic
retired false
exit
exit
!# enter
enable
config t
int g0/0
ip ips REGLA-IPS out
exit
ip ips signature-definition
signature 2004 0
status
retired false
enable true
exit
engine
event-action produce-alert
event-action deny-packet-inline
exit
exit
!# Confirm
```