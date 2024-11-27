# Info
**O**pen **S**ystem **I**nterconection o **I**nterconexion de **S**istemas **A**biertos) es un modelo de referencia ISO/IEC 7498-1:1994, el cual consiste en la separacion de 7 capas de abstraccion, en el que se provee bases para cordinar otros estandares de interconexion, no es una arquitectura de red
## Tabla Comparativa

| Standard TCP/IP model | OSI model      | Equivalent TCP/IP model | PDU                 |
| --------------------- | -------------- | ----------------------- | ------------------- |
| 4- Aplication         | 7- Application | 5- Application          | Data                |
| "                     | 6- Prentation  | "                       | "                   |
| "                     | 5- Session     | "                       | "                   |
| 3- Host-to-Host       | 4- Transport   | 4- Transport            | Segmento, Datagrama |
| 2- Internet           | 3- Network     | 3- Network              | Paquete             |
| 1- Network Access     | 2- Data Link   | 2- Data Link            | Frame               |
| "                     | 1- Physical    | 1- Physical             | Bit                 |
# Capas
## Capa 1: Fisica
Responsable de la transmision de datos raw entre dispostivos, infraestructura basica donde se arma todo, se comunican bits(`1/0`) a travez de un medio fisico

Ejemplos:
- Señales de Radio
- Señales Opticas
- Enchufes
- Conectores (IEEE 802.3ab: Gigabit Ethernet)
- Cableado

Problemas Comunes:
- Desconexiones Fisicas o defectuosas
- Cables Dañados o Orden incorrecto
- Dispositivos Apagados o incorrectos
- Interferencia Electromagneticas

Pasos Verificacion
1. Verifica que todo este correctamente conectado y los cables no esten dañados
2. Comprobar que los dispositivos esten encendidos y operativos, observa los indicadores LED
3. Revisa en entorno fisico y que este limpio de interferencias

Troubleshooting
- Estado de Interfaces
	- `show int status`
- Detalle de interfaz
	- `show int [int S/S/P]`
- Revisa conteo de errores
	- `show int [int S/S/P] counters errors`
- Desabilita la resolucion DNS para acelerar la CLI
	- `no ip domain-lookup`


## Capa 2: Enlace de Datos
Permite la comunicacion entre nodos conectados directamente como [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]] para enlaces fisicos y AP para Wi-fi, usa [[020 - Conceptos/020.3 - Fundamentos/MAC|MAC]] para controlar el permiso para el envio de datos.

Estandares y Protocolos:
- [[010 - Protocolos/010.9 - Estandares/IEEE 802/IEEE 802.1X|IEEE 802.1X]]: Autenticacion de Red
- IEEE 802.1Q: [[010 - Protocolos/010.2 - Switching/VLAN|VLAN]]
- IEEE 802.3: Ethernet
- IEEE 802.15.4: Zigbee (Low Rate Wireless Personal Area Network)
- PPP (Point-to-Point Protocol)
	- Open Standard (Multi-Vendor), enlaces seriales punto a punto
	- Permite autenticacion via PAP y CHAP 
- HDLC (High-Level Data Link Control)
	- Closed Standard by Cisco, 
	- Permite QoS, Autenticacion via PAP y CHAP, Compresion
- Flow Control: Negociacion entre velocidades automaticas.
- ARP
- [[020 - Conceptos/020.3 - Fundamentos/MAC|MAC]]
- Autenticacion
	- PAP
	- CHAP

Problemas Comunes
- Asignacion incorrecta de VLANs
- Puertos en estado err-disable
- Configuraciones de trunk mal configuradas
- Problemas con STP
- Errores en Etherchannel

Pasos de Verificacion
1. Comprobar asignacion de VLANs
	- Asegurate que esten creadas
	- Puertos de acceso con la vlan correspondiente
2. Compobar Enlaces troncales
	- Asegurate de la encapsulacion (802.1Q) este activa
	- Verifica que los troncales permitan el paso de las VLANs necesarias
3. Estado de los puertos
	- Revisa los puertos con error `err-disable` y determina la causa
4. STP
	- Asegurate que todos los roles esten bien asignados y no hayan bloqueos indebidos
	- Revisa las prioridades de los dispositivos
5. Etherchannel
	- Comprueba que los parametros Etherchannel coinciden en ambos extremos (modo, protocolo y configuraciones)

Troubleshooting
- Ver VLANS
	- `show vlan brief`
- Ver Puertos Troncales
	- `show int trunk`
- Ver Estado de puertos
	- `show int status`
	- `show port-security`
- Ver STP
	- `show spanning-tree`
- Ver EtherChannel
	- `show etherchannel summary`

## Capa 3: Red
Envia paquetes entre nodos de diferentes redes(conjunto de nodos), en el que cada nodo tiene una direccion para poder comunicarse, la funcion permite estar en esta capa, no el protocolo en si.
La capa 3 de divide en 3 subcapas, las cuales permiten diseñar un sistema de datagrama unificado para las comunicaciones.
- 3a: Subnetwork Access
- 3b: Subnetwork Dependent Convergence
- 3c: Subnetwork Independent Convergence

Protocolos:
- [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv4|IPv4]]
- [[020 - Conceptos/020.3 - Fundamentos/EGP|EGP]]
- [[020 - Conceptos/020.3 - Fundamentos/IGP|IGP]]
- [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP|EIGRP]]
- [[010 - Protocolos/010.3 - Comunicaciones/010.3.2 - Diag y Control/ICMP|ICMP]]
- [[010 - Protocolos/010.1 - Routing/IPSEC|IPSEC]] (AH,ESP)
- [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv2|OSPFv2]] y [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv3|OSPFv3]]
- [[010 - Protocolos/010.1 - Routing/Tunel GRE|Tunel GRE]]

Problemas Comunes
- Direcciones IP Duplicadas o incorrectas
- Falta de rutas en la tabla de enrutamiento
- Protocolos de enrutamiento dinamico mal configurados
- ACLs Permitiendo y/o Bloqueando trafico incorrectamente
- Problemas con NAT y acceso a internet

Pasos de Verificacion
1. Verifica la configuracion IP, debe estar correcta segun topologia y activada
2. Comprobar la tabla de enrutamiento, debe tener rutas de origen y destino y que se vean las rutas por defecto
3. Verificar protocolos de enrutamiento, como OSPF, EIGRP o BGP, deben estar bien configurados y con sus vecinos
4. Revisar ACLs y Filtros
5. Verifica NAT/PAT, con sus reglas de enrutamiento

Troubleshooting
- Ver IP
	- `sh ip int brief`
- Ver tabla
	- `sh ip route`
- Verificar Protocolos de enrutamiento
	- General
		- `show ip protocols`
		- `show ipv6 protocols`
	- OSPF
		- `show ip ospf neighbor`
		- `show ip ospf interface`
	- EIGRP
		- `show ip eigrp neighbor`
		- `show ip eigrp topology`
	- BGP
		- `show ip bgp summary`
- ACLs
	- `show access-lists`
- NAT
	- `show ip nat translations`
	- `show ip nat statistics`

## Capa 4: Transporte
Entrega metodos para enviar informacion desde un host a otro, esta informacion debe ser repartida en trozos mas pequeños (MTU)

Ejemplos:
- TCP
- UDP
- QUIC
- SSL
- Socks
- [[010 - Protocolos/010.3 - Comunicaciones/VPN|VPN]]
- PPPoE (Point-to-Point Protocol Over Ethernet)
- Wireguard

Problemas Comunes
- Puertos UDP/TCP Bloqueados
- Sesiones Interrumpidas
- Problemas de estabilidad

Pasos de Verificacion
1. Verifica ACLs molestando, sobretodo si bloquea puertos especificos
2. Comprueba la estabilidad tanto de TCP como UDP con ping y traceroute o acceder a paginas web
3. Verifica la transmision de errores

Troubleshooting
- Ver estadisticas de transporte
	- `show tcp brief`
- Ver puertos abiertos
	- `sh control-plane host open-ports`: Posiblemente no soportado por 15.2D o 15.4.1T
- Ver ACL Aplicadas
	- `sh access-lists`

## Capa 5: Sesion
Crea, establece, controla, suspende, reincia y termina las conexiones entre computadores.

Ejemplo:
- NetBIOS
- SOCKS
- RTP

Problemas Comunes
- Fallos con la sesiones
- Problemas de autenticacion
- Sesiones caidas

Pasos de Verificacion
1. Verificar Autenticacion y Permisos, con credenciales y usuarios correctos
2. Verificar Configuraciones de Sesion, revisando los limites y restricciones
3. Verificar Estado de las sesiones activas

Troubleshooting
- Mostrar sesiones activas
	- `show users`
- Verificar AAA
	- `show aaa sesions`
- Logs
	- `show logging`

## Capa 6: Presentacion
Establece formato de datos y la traduccion de datos en el contexto de redes

Ejemplo:
- Codificacion y Decodificacion: base64 | ASCII | Unicode (UTF-8)
- Lenguajes de Representacion de datos: XML | JSON | YAML
- Cifrado y Descifrado: PGP | TLS | AES | RSA | SHA
- Compresion y Descompresion: MPEG | JPEG | PNG | MP3 
- MIME (Multipurpose Internet Mail Extensions)

## Capa 7: Aplicacion
Se encarga de interactuar con el usuario

Ejemplos:
- HTTP/1.1 | HTTP/2 | HTTP/3
- [[010 - Protocolos/010.3 - Comunicaciones/NTP|NTP]]
- [[010 - Protocolos/010.3 - Comunicaciones/SSH|SSH]] | Telnet
- [[010 - Protocolos/010.3 - Comunicaciones/010.3.2 - Diag y Control/SNMP|SNMP]]
- POP3 | IMAP | SMTP
- SSL
- DNS
- FTP | TFTP | SFTP
- DHCP
- MQTT (Message Queuing Telemetry Transport)
- [[010 - Protocolos/010.1 - Routing/IPSEC|IPSEC]] (ISAKMP)

## Capas Multiples
Hay protocolos que afectan mas de una capa, o que viven entre ellas, como los protocolos orientados a la administracion o seguridad (Confidencialidad, Integridad y Disponibilidad).
- Servicio de Seguridad en Telecomunicaciones (ITU-T X.800 recomendation)
- CMIP (Common Management Information Protocol)
- CMIS (Common Management Information Service)
- [[010 - Protocolos/010.1 - Routing/MPLS|MPLS]] | ATM | X.25 | Frame Relay: Se les dice "Capa 2.5"
- IEEE 802.11 Wi-fi (BSSID): Mac & Modulacion Inalambrica PHY: Se les dice "Capa 2.5"

## Capas de abstraccion como Stack de Protocolos
En la practica existen 3 capas que no se juntan
- Media
- Transporte
- Aplicacion

Estos para comunicarse, deben transformar y comunicarse entre ellos, estos son
- Media-Transporte: Que informacion y que dispositivo de Hardware se asocia
	- Ejemplo: protocolo [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv4|IPv4]] communmente se le dice TCP/IP
- Transporte-Aplicacion: Que aplicacion y donde la envia 
	- Ejemplo: Navegador web se comunica por TCP/IP
