# Info
[nftables](https://netfilter.org/projects/nftables/) es un remplazo a `{ip,ipv6,arp,eb}tables`, el cual permite filtrar paquetes en linux segun reglas de manera unificada, tiene funciona como [[020 - Conceptos/020.4 - Dispositivos de Red/Firewall|Firewall]] y [[010 - Protocolos/010.1 - Routing/NAT|NAT]].

## Terminologia
- `[tabla]`: Espacio de nombre para agrupar cadenas y reglas
	- `filter`: Tabla predeterminada
	- `nat`: Tabla [[010 - Protocolos/010.1 - Routing/NAT|NAT]]
	- `mangle`: Tabla especial
- `[cadena]`: Reglas para procesar paquetes
	- `input`: paquetes entrantes al sistema local
	- `output`: Paquetes salientes desde sistema local
	- `forward`: Paquetes reenviados a travez del sistema ([[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]])
	- `prerouting`: Paquetes antes de enrutar
	- `postrouting`: Paquetes despues de enrutar
- `[hook]`: Enganche donde se aplica la `[cadena]`
	- `input`, `output`, `forward`, `prerouting`, `postrouting`
- `[interfaz]`: Nombre de la interfaz
	- Ejemplos: `eth0`, `wlan1`, `enp0s2`, `net3`, `wlp4s4`, `l5`, `veth6@if6`, `br-7`
- `[ip/Mask-CIDR]`: Direccion IP Host o Network + [[020 - Conceptos/020.3 - Fundamentos/Mascara|Mascara]] en CIDR
	- Ejemplos: `10.0.0.1/32`, `192.168.1.0/24`, `::1/128`
- `[accion]`: Accion a realizar sobre el paquete
	- `accept`: Aceptar el paquete
	- `drop`: Descartar el paquete
	- `reject`: Rechazar el paquete y notificar al remitente
	- `log`: Registrar informacion del paquete
	- `jump`: Saltar a otra cadena
- `[familia]`: Protocolo de red de [[020 - Conceptos/020.3 - Fundamentos/Modelo OSI#Capa 2 Enlace de Datos|Capa 2]] o [[020 - Conceptos/020.3 - Fundamentos/Modelo OSI#Capa 3 Red|Capa 3]]
	- `ip`: [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv4|IPv4]]
	- `ipv6`: [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv6|IPv6]]
	- `inet`: IPv4 & IPv6
	- `arp`: [[020 - Conceptos/020.3 - Fundamentos/MAC|MAC]]
	- `bridge`: Puentes de Red
	- `netdev`: Dispositivo de red
- `[prioridad]`: Orden en el que se aplican las cadenas
	- `0`: Prioridad Predeterminada
	- `-100`: Alta Prioridad
	- `100`: Baja Prioridad
- `[proto]`: De la [[020 - Conceptos/020.3 - Fundamentos/Modelo OSI#Capa 4 Transporte|Capa 4]] o superior
	- `tcp`
	- `udp`
	- `icmp`: Ver [[010 - Protocolos/010.3 - Comunicaciones/010.3.2 - Diag y Control/ICMP|ICMP]]
	- `icmpv6`
- `[puerto]`: N° Puerto de Comunicacion
	- Ejemplos: `22`(SSH), `53`(DNS), `80`(HTTP), `443`(HTTPS)
`[opciones]`: Parametros adicionales mediante Flags
- `[modificacion]`
	- `-A`: Añade una regla al final de la cadena
	- `-I`: Inserta una regla al inicio o en una posicion especifica
	- `-D`: Elimina una regla de la cadena
	- `-F`: Limpia todas las reglas configuradas
		- `-F [cadena]`: Limpia las reglas de una cadena
	- `-X`: Limpia todas las cadenas configuradas por el usuario
	- `-P`: Establece la politica predeterminada de una cadena
		- `-P [cadena] [accion]`
- `[Configuracion]`
	- `-p`: Modifica el `[protocolo]`
	- `--dport`: Modifica el `[puerto]`
	- `-t`: Modifica la `[tabla]`
	- `-i`: Modifica la `[interfaz]`
	- `-s`: Modifica la `[ip-source/Mask-CIDR]`
	- `-d`: Modifica la `[ip-destination/Mask-CIDR]` o `[dominio-web]`
- `[Muestra]`
	- `-L`: Muestra las reglas de la cadena o tabla
	- `-v`: Muestra informacion detallada
	- `-n`: Muestra la IP y Puerto en formato numerico
	- `--line-numbers`: Muestra las cadenas por numeracion

## Ejemplos de Funcionamiento
> [!NOTE] Nota
> Los valores entre llaves `{}` se puede modificar por cualquier otro valor o quitar segun veas conveniente

## Mostrar el estado
Listando todas las reglas actuales
```
iptables -L [cadena] -n -v --line-numbers
```
Ejemplo
```
iptables -L INPUT -n -v --line-numbers
```

### Agregar una regla
Agregando reglas a una cadena
```
iptables -A [cadena] { -p [proto] --dport [puerto] } -j [accion]
```
Ejemplo
```
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

### Insertar una regla
> NOTA: Puede ser al principio o señando una posicion con `[numero]`
```
iptables -I [cadena] [numero] { -p [proto] --dport [puerto] } -j [accion]
```
Ejemplo
```
iptables -I INPUT 1 -p icmp --dport 53 -j Drop
```

### Eliminar una Regla
Para saber el numero, puedes [[#Mostrar el estado]]
```
iptables -D [cadena] [numero]
```
Ejemplo
```
iptables -D INPUT 3
```

### Limpiar reglas
```
iptables -F
iptables -t [tabla] -F
iptables -X 
```

### Establecer Politicas por defecto
```
iptables -P [cadena] [accion]
```
Ejemplo
```
iptables -P INPUT DROP
```

## Ejemplos de Uso
### Borrar ip privadas en entorno publico
> Relacionado [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv4#Clases de IPv4 Privada|IPv4 Clases Privadas]]
```
iptables -A [cadena] -i [interfaz] -s [ip/mask-CIDR] -j DROP
```
Ejemplo
```
iptables -A INPUT -i eth1 -s 192.168.1.0/24 -j DROP
```

## Bloquear ip publicas en un entorno privado
```
iptables -A [cadena] -p [protocol] -d [ip/mask-CIDR] -j DROP
```
Ejemplo
```
iptables -A OUTPUT -p tcp -d 69.171.224.0/19 -j DROP
```

### Bloquear Dominios Web
```
iptables -A [cadena] -p [protocol] -d [dominio-web] -j DROP
```
Ejemplo
```
iptables -A OUTPUT -p tcp -d www.facebook.com -j DROP
iptables -A OUTPUT -p tcp -d facebook.com -j DROP
```

### Bloquear Ping
```
iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
```

## Configuracion IPtables
### Modificar Estad Servicio
```
service iptables stop
service iptables start
service iptables restart
```

### Guardar reglas
```
service iptables save
iptables-save > /root/reglas.firewall
cat /root/reglas.firewall
```

## Restaurar Reglas
```
iptables-restore < /root/reglas.firewall
```