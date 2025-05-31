Marco de Arquitectura de seguridad, diseñar soluciones seguras propietario de CISCO
Los lugares a Asegurar se llaman "PIN"
- Sucursales
- Redes Campus
- Data Center
- Equipos de Borde
- Nube
- Redes de Area Extendida (WAN)

Dominios seguros, Conceptos de seguridad para evaluar cada PIN:
- Administracion
- Inteligencia de Seguridad
- Conformidad
- Segmentacion
- Defensa contra Amenazas

El enfoque de seguridad abarca
- Antes: Se conoce todos los activos a proteger y identifica posibles amenazas
	- AMP (Advanced Malware Protection)
	- NGFW
	- Network Access Control
- Durante: Se definen acciones en caso de ataques, analisis de amenazas y respuesta a incidentes es comun verlo aqui
	- NGIPS
	- Web Security with AMP
	- Email Security with AMP
- Despues: Capacidad de detecta, contener y remediar un ataque, las lecciones que se aprender se incorporan a la retroalimentacion para futuras amenazas.
	- AMP
	- Analisis de Comportamiento
	- Analisis de Seguridad

## Threat Grid
Analiza archivos estaticos y dinamicos, esta disposible en fisico y en la nube
- Analisis de comoportamiento con "Talos"
- Cargar archivos sospechosos en Sandbox con "Glovebox" para interactuar de forma segura y observar el comportamiento
## AMP
Advance Malware Protection, es una solucion de analisis y proteccion de malware, con un analisis continuo
Ejemplos
	- AMP for Email
	- AMP for network Firewall & IPS
	- AMP for Web
	- AMP for Meraki MX
	- [[#Umbrella]]
	- AMP for Endpoints

Tiene un enfoque de seguridad
- Antes: Talos y Threat Grid protege amezadas nuevas y conocidas
- Durante: Determina los archivos limpios y los inseguros, ademas de identificar la amenazas
- Despues: Proporciona retrospeccion, IoC(Indicadores de Compromiso), Deteccion de infreacciones, seguimiento, analisis y remediaciones.
## Umbrella
Antes conocido como OpenDNS, bloquea el internet malicioso(Dominios, IP, URL) antes de que se establezca la conexion o se descargue un archivo, su uso es en la nube, y tiene una red global con uso de Anycast DNS, lo que permite 100% de tiempo de actividad al permitir un failover entre servidores.
Conforma:
- DNS-layer security
- Secure web gateway
- Cloud-delivered firewall
- Cloud access security broker (CASB)
- Interactive threat intel