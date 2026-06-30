# Que es?
Un firewall Bloquea y maneja paquetes entre areas dentro de varias redes, haciendo cumplir politicas de acceso, evitando el trafico indeseable.
## Propiedades
- Resientes a ataques
- Todo el trafico fluje por el en un unico punto de transito
- Hace cumplir la politica de control de acceso
## Beneficios
- Evita la exposicion del host, recursos y aplicaciones
- Evita la explotacion de fallas de protocolo
- Bloquea datos maliciosos de servidores y clientes
- Redue la complejidad de administracion de seguridad
## Limitaciones
- Usualmente no protege ataques internos de la misma red
- Su mal configuracion puede degradar la red
- Un firewall no asegura la seguridad de los datos
- Un usuario puede buscar formas de "Saltarlo", como "tunelizar" o ocultar el trafico
- El rendimiento de la red puede ser peor
## Tipos
- Firewall Filtrado de Paquetes: Filtra las Capa 3 y 4
	- Desventajas
		- Posible Suplantacion de IP
		- No filtran correctamente Paquetes Fragmentados
		- No filtra dinamicamente los servicios, algunos necesitan amplia gama de puertos
- Firewall de Estado: Monitorea el estado de las conexiones
	- Beneficios
		- Filtrado mas Fuerte
		- Mejor rendimiento
		- defiende Spoofing y Ataques DoS
		- Mejor LOG
	- Limitaciones
		- No inspecciona capa Aplicacion
		- No trackea bien Protocolos Stateless
		- Dificultades con Puertos Dinamicos
		- No soporta Autenticacion
- Firewall de Aplicacion de Gateway: Filtra las capas 3,4,5 y 7, 

## Zona DMZ
Zona Demilitarizada, usualmente es la que se comunica con la red publica, y a pesar de estar dentro de la red privada la comunicacion es bloqueada, debe ser protegida.
## Zonas ZPF
Firewall Basado en Zonas, el cual crea zonas para separar redes privadas en aun mas pequeñas, se pueden comunicar dispositivos de la misma zona pero no interactuar con otras zonas
Beneficios
- No depende de las ACL
- Bloquea por defecto excepto permiso explicito
Acciones
- Inspect: Inspecciona Paquetes
- Drop: Deniega, tiene opcion de log
- Pass: Permite, no rastrea el estado o sesion del trafico

Capas de Defensa
Como una cebolla, la mala implementacion, crea capas alcachofa
1. Seguridad del nucleo: Aplica Politicas de red, protegiendo contra malware y anomalias
2. Seguridad del Perimetro: Protege las fronteras de las zonas
3. Seguridad de Comunicacion: Protege la Informacion
4. Seguridad de extremo: Se asegura del cumplimiento de politicas de identidad y seguridad
5. Plan de Recuperacion: Almacenamiento externo al sitio y arquitectura

Buenas Practicas con Firewall
- Ubicar un Firewall en las fronteras de las zonas
- No depender solamente de estos
- Denegar todo el trafico por defecto y habilitar solo el necesario


Ejemplos:
- PFSense
- ASA (Cisco)
- FortiOS (Fortinet)
# Configuracion
## Activar Firewall (No se ve AUN)
```
ip inspect name [name] [protocol]
ip access-list extended [name]
permit tcp [ip] [wildcard] [destination] eq [port]
deny ip any any
```

## Zona ZPF
```
license boot module c1900 technology-package securityk9
copy running-config unix:
zone security [name-zone-1] #Privada
exit
zone security [name-zone-2] #Publica
class-map type inspect match-any [name-class-map]
match protocol [http|https|dns|...]
policy-map type inspect [name-policy-map]
class type inspect [??]
inspect
zone-pair security [name-zone-pair] source [name-zone-1] destination [name-zone-2]
service-policy type inspect [name-policy-map]
int [int S/S/P]
zone-member security [name-zone-1|name-zone-2]
exit
```
### Alternativo: 
Permitir Class-map con access list
```
ip access-list extended [acl-name]
permit ip [IPv4] [Wildcard]
class-map type inspect match-all [name-class-map]
match access-group [acl-name]
```

# Visualizacion
`show policy-map type inspect zone-pair sessions` -> # Comprobar Funcionamiento de ZPF

# Extras
Los peores ataques son llamados APT(Advanced Persistent Thread)