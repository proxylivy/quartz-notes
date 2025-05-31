22: TCP | SSH

UDP | syslog
UDP | snmptrap
UPD | domain
TCP | smtp
TCP | ftp

## ICMP
## Eliminar ICMP (Ping)
### Hacia Internet
```
access-list 112 permit icmp any any echo-replay
access-list 112 permit icmp any any source-quench
access-list 112 permit icmp any any unreachable
access-list 112 deny icmp any any
access-list 112 permit ip any any
```
### Red Local
```
access-list 114 permit icmp 192.168.1.0 0.0.0.255 any echo
access-list 114 permit icmp 192.168.1.0 0.0.0.255 any parameter-problem
access-list 114 permit icmp 192.168.1.0 0.0.0.255 any packet-too-big
access-list 114 permit icmp 192.168.1.0 0.0.0.255 any source-quench
access-list 114 deny icmp any any
access-list 114 permit ip any any
```