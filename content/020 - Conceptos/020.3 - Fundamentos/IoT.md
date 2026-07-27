# Info
**I**nternet **o**f **T**hings es la conexion de una variedad de sensores, actuadores y dispositivos ligeros a travez de internet, permitiendo conectar objetos que antes no lo estaban (como puertas, ampolletas, ventanas, etc.)

Esto habilita la [[020 - Conceptos/020.1 - Administracion/Automatizacion en Redes|Automatizacion en Redes]] y de procesos, ejecutandolos en el momento preciso y de forma adecuada, ademas de la generacion de redes P2P, M2P o M2M, utiles para el seguimiento de recursos, la optimizacion de opearaciones y la recoleccion de datos de sensores

Su base son las conexiones [[020 - Conceptos/020.3 - Fundamentos/WLAN|WLAN]], mediante protocolos de comunicacion tales como:
- Basados en IEEE 802.15
- Bluetooth
- ZigBee
- MQTT
- Thread
- RPL
- NFC
- LoWPAN

Las conexiones en red usualemente se hacen mediante Gateways que intercambian informacion, se agregan a la infraestructura de [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]] y [[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]]

## Modelo de Mitigacion de Amenazas
- Identificar Objetivos de Seguridad
	- Identidad
	- Financiero
	- Reputacion
	- Privacidad y Regulacion
	- Garantias de Disponibilidad
	- Seguridad
- Documentar Arquitectura de Sistema IoT
	- Componentes del Sistema en las diferentes capas de aplicacion, comunicacion y dispositivo
	- Flujo de datos entre componentes y capas
	- Tecnologias, protocolos y estandares que se hayan utilizado
- Descomponer el sistema
	- Analizar componentes Individuales y sus caracteristicas que podrian impactar
	- Crear un perfil de seguridad para identificar vulnerabilidades de diseño o implementacion
	- Identifica redes confiables y no confiables
	- Identifica el flujo de datos entre dispositivos, la red de comunicaciones y las aplicaciones
	- Identifica los datos confidenciales dentro del sistema
	- Documenta el perfil de seguridad para hacer validaciones y autorizacion
- Identifica y Clasifica las amenazas
	- Manten la base de datos sobre Amenazas y Vulnerabilidades actualizadas
	- Conoce tu superficie de ataque y clasifica las amenazas
- Tecnicas de mitigacion
	- Luego de clasificar, determina tecnicas de mitigacion para cada amenaza