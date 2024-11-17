```
auto bridge
iface bridge
    bridge-ports swp1 swp2 swp3
    bridge-vlan-aware yes
    bridge-stp on
    bridge-vid 100 200
    bridge-pvid 1

auto swp1
iface swp1
    bridge-access 100
```
