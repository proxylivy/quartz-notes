# Info
Se usa entre [[020 - Conceptos/020.3 - Fundamentos/Modelo OSI#Capa 2|Capa 2]] y [[020 - Conceptos/020.3 - Fundamentos/Modelo OSI#Capa 3|Capa 3]], permite ver la conexion entre [[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]]

## Estructura
- Types
	- 0 -> Echo Reply
	- 3 -> See codes
		- C0 -> Network Unreachable
		- C1 -> Host Unreachable
		- C2 -> Protocol Unreachable
		- C3 -> Port Unreachable
	- 5 -> Redirect
	- 8 -> Echo Request

## Configuracion

## Eliminar ICMP (Ping)

Hacia Internet
```
access-list 112 permit icmp any any echo-replay
access-list 112 permit icmp any any source-quench
access-list 112 permit icmp any any unreachable
access-list 112 deny icmp any any
access-list 112 permit ip any any
```

Red Local
```
access-list 114 permit icmp 192.168.1.0 0.0.0.255 any echo
access-list 114 permit icmp 192.168.1.0 0.0.0.255 any parameter-problem
access-list 114 permit icmp 192.168.1.0 0.0.0.255 any packet-too-big
access-list 114 permit icmp 192.168.1.0 0.0.0.255 any source-quench
access-list 114 deny icmp any any
access-list 114 permit ip any any
```