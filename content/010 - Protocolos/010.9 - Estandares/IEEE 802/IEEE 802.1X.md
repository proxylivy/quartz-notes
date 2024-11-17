Se usa para el control de acceso a la red basado de puertos (PNAC) para autenticar redes LAN y WLAN.
Componentes:
- EAP (Protocolo de Autenticacion Extensible): Definido por varios RFC[^1], proporcionando un transporte encapsulado para autenticar
	- (EAPoL)EAP sobre LAN: protocolo de encapsulacion de capa 2, definido por 802.1X para el transporte de mensajes EAP LAN y WLAN IEEE 802, permitido por defecto y se usa en la verificacion con un servidor RADIUS
	- RADIUS: Protocolo [[010 - Protocolos/010.3 - Comunicaciones/010.3.1 - AAA/AAA|AAA]] utilizado por EAP

[^1]: [RFC 7057: Base de EAP](https://www.rfc-editor.org/rfc/rfc7057) | [RFC 4017: EAP WLAN ](https://www.rfc-editor.org/rfc/rfc4017) | [RFC 9048: EAP-AKA](https://www.rfc-editor.org/rfc/rfc9048) | [RFC 9190: EAP-TLS 1.3](https://www.rfc-editor.org/rfc/rfc9190) | [RFC 9427: EAP Usos TLS](https://www.rfc-editor.org/rfc/rfc9427)

Roles:
- Supplicant: El software de equipo final que comunica y proporciona las credenciales de identidad a travez de EAPoL con el autenticador
- Autenticador: Un dispositivos de acceso a la red (NAD), como un switch o WLC(Wireless LAN Controller), controla el acceso por la autenticacion del usuario final.
- Servidor de Autenticacion: Servidor RADIUS para autenticar el cliente

Pasos Autenticar 802.1X:
1. El autenticador envia tramas de identificacion/solicitud EAP periodicas, a travez de los puertos conectaddos
2. El autenticador permite mensajes EAP entre el solicitante y el servidor de autenticacion
3. Si la autenticacion es exitosa, el servidor devuelve un mesaje de aceptacion de acceso a travez de RADIUS

MAB
Mac Authentication Bypass permite acceder a dispositivos usando la direccion [[020 - Conceptos/020.3 - Fundamentos/MAC|MAC]], Alternativo a 802.1X, cuando se activa RADIUS le pide al switch a escuchar los EAPoL
1. El switch envia solicitud de identidad EAPoL al equipo final cada 30 segundos
2. El switch inicia MAB, permite un paquete para aprender la MAC de origen del equipo final
3. El servidor Radius ve si el dispositivo debe tener acceso a la red

WebAuth
La autenticacion Web se usa para dispositivos finales, Alternativa a 802.1X, WebAuth es para usuarios y verifica con contraseñas, hay 2 tipos de autenticacion
- Web Local
- Web Centralizada con Cisco ISE



# Extras
Estandar IEEE 802.1x
Apoyo [Wikipedia](https://en.wikipedia.org/wiki/IEEE_802.1X)