# Info
[VyOS](https://vyos.net/) es un proyecto completamente Open-Source y de grado empresarial para plataformas de routing, incluyendo su [Documentacion](https://docs.vyos.io).

# Config Test
## Lab 1
### VyOS1
```
VyOS1
=====
configure
set system host-name vyos1
set interfaces ethernet eth1 address 198.51.100.1/24
set interfaces loopback lo address 192.0.2.1/32

set protocols ospf area 0 network '198.51.100.0/24'
set protocols ospf area 0 network '192.0.2.1/32'
set protocols ospf interface eth1
set protocols ospf redistribute connected
```

### VyOS2
```
VyOS2
=====
configure
set system host-name vyos2
set interfaces ethernet eth1 address 198.51.100.2/24
set interfaces loopback lo address 192.0.2.2/32

set protocols ospf area 0 network '198.51.100.0/24'
set protocols ospf area 0 network '192.0.2.2/32'
set protocols ospf interface eth1
set protocols ospf redistribute connected
```

### VyOS3
```
VyOS3
=====
configure
set system host-name vyos3
set interfaces ethernet eth1 address 198.51.100.3/24
set interfaces loopback lo address 192.0.2.3/32

set protocols ospf area 0 network '198.51.100.0/24'
set protocols ospf area 0 network '192.0.2.3/32'
set protocols ospf interface eth1
set protocols ospf redistribute connected
```


## Lab 2
### VyOS3
```
VyOS3
=====
set protocols ospf default-information originate
set interfaces ethernet eth2 address 203.0.113.1/24
set protocols ospf area 0 network '203.0.113.0/24'
```
### VyOS4
```
VyOS4
=====
configure
set system host-name vyos4
set interfaces ethernet eth0 address 203.0.113.2/24
```

### VyOS3
```
VyOS3
=====
configure
set protocols bgp system-as '15954'
set protocols bgp neighbor 203.0.113.2 address-family ipv4-unicast
set protocols bgp neighbor 203.0.113.2 remote-as 100
set protocols bgp address-family ipv4-unicast import
set protocols bgp parameters router-id '203.0.113.1'


set protocols bgp address-family ipv4-unicast network 192.168.150.0/24


set policy route-map Todo rule 1 action 'permit'
set protocols bgp neighbor 203.0.113.2 address-family ipv4-unicast route-map export 'Todo'
set protocols bgp neighbor 203.0.113.2 address-family ipv4-unicast route-map import 'Todo'
```

### VyOS4
```
VyOS4
=====
configure
set protocols bgp system-as 100

set protocols bgp neighbor 203.0.113.1 address-family ipv4-unicast
set protocols bgp neighbor 203.0.113.1 remote-as 15954
set protocols bgp address-family ipv4-unicast import
set protocols bgp parameters router-id 203.0.113.2

set policy route-map Todo rule 1 action 'permit'
set protocols bgp neighbor 203.0.113.1 address-family ipv4-unicast route-map export 'Todo'
set protocols bgp neighbor 203.0.113.1 address-family ipv4-unicast route-map import 'Todo'
```