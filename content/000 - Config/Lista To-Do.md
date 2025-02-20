## Dudas
- Que son los SA y para que sirven dentro de [[010 - Protocolos/010.1 - Routing/IPSEC|IPSEC]] (Security Associations)
- Cual es la Fase 3 Opcional dentro de [[010 - Protocolos/010.1 - Routing/IPSEC|IPSEC]]
- IKE es un tipo de encriptacion Asimetrico o Simetrico | [[010 - Protocolos/010.1 - Routing/IPSEC|IPSEC]]
- Que significan los atributos de [[010 - Protocolos/010.1 - Routing/BGP/BGP|BGP]]
	- Well-know
	- Opcional
	- Transitive
	- Non-Transitive
	- Mandatory

- Sobre los atributos, si no se usan para seleccionar camino, ¿Esta correcto decir que son "Atributo Locales"?
- Los atributos son los mismos entre [[010 - Protocolos/010.1 - Routing/BGP/BGP-4|BGP-4]] con [[010 - Protocolos/010.1 - Routing/BGP/BGP|BGP]]-3?? Aunque solo se le agrega a [[010 - Protocolos/010.1 - Routing/BGP/BGP-4|BGP-4]] el NLRI??
- Como afecta NLRI (Network Layer Reachability Information) a BGP-4([[010 - Protocolos/010.1 - Routing/BGP/BGP-4|BGP-4]])

---
## Vault Obsidian
### Notas
- [ ] [[020 - Conceptos/020.4 - Dispositivos de Red/IOU WEB|IOU WEB]] en la seccion CAM falta definir y explicar el funcionamiento
- [ ] Reescribe [[010 - Protocolos/010.3 - Comunicaciones/010.3.1 - AAA/AAA|AAA]] en [[010 - Protocolos/010.3 - Comunicaciones/010.3.1 - AAA/Radius|Radius]] y [[010 - Protocolos/010.3 - Comunicaciones/010.3.1 - AAA/Tacacs+|Tacacs+]]
- [ ] Linkear Bien los links de Puertos Acceso/Troncales a Puertos
- [ ] Actualizar Packet Tracer a 8.2.2 (Low Priority)
- [ ] Mueve el contenido no neutral de [[010 - Protocolos/010.3 - Comunicaciones/VPN|VPN]] a [[010 - Protocolos/010.1 - Routing/IPSEC|IPSEC]]

### Reescribe
- [ ] Esta entrada de Wikipedia sobre [Suite de Protoclos de Internet](https://en.wikipedia.org/wiki/Template:Internet_protocol_suite) en [[020 - Conceptos/020.3 - Fundamentos/Modelo OSI|Modelo OSI]]
- [ ] Esta entrada de blog sobre [Protocolos IGP](https://disenoredesuptc.blogspot.com/2015/09/protocolos-igp.html)
- [ ] Reescribe en [[010 - Protocolos/010.1 - Routing/MPLS|MPLS]] sobre la eleccion de un paquete en referencia a este [diagrama](http://networkstatic.net/wp-content/uploads/2012/04/flow.jpg) extraido de [aqui](http://networkstatic.net/juniper-and-cisco-comparisons-of-rib-lib-fib-and-lfib-tables/)
- [ ] Extrae la explicacion sobre [Medium - Arquitectura de Docker](https://medium.com/@ravipatel.it/understanding-docker-architecture-a-comprehensive-guide-5ce9129df1a4) y [Github - acastan/Servicios - Repaso Virtualizacion](https://github.com/acastan/Servicios/blob/master/repaso/Ejercicio%200a%20-%20repaso%20Virtualizaci%C3%B3n%20-%20Docker.md) en [[999 - Archivado/Docker|Docker]]

---
## Buscale un lugar a esto
### Comandos Basicos
```
!# Habilitar Comandos de show
enable
!# Habilitar Comandos de configuracion
config t
!# Configurar Nombre
hostname [hostname-name]
!# Generar MOTD
banner motd [motd-banner]
!# Habilitar contraseña Texto Plano
enable password [password]
!# Habilitar Contraseña Cifrada en MD5
enable secret [password]
!# Habilitar servicio de encriptacion??
service password-encryption
```