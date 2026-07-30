# Info

Es un interprete del lenguaje `tcl` integrado en Cisco IOS que permite automatizar tareas desde la CLI mediante scripts, siendo uno de los primeros metodos de automatizacion disponibles en equipos Cisco antes de la incorporacion de APIs y soporte de Python para plataformas mas modernas

# Configuracion

Ejemplo para hacer ping a varios IPs
```
tclsh
foreach address {
[ip-address]
[ip-address]
} { ping $address repeat 5 size 1500 }
```
