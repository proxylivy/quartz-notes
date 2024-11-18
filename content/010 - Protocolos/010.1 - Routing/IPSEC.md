# Info
IP Secure, fue creado con el desarrollo de IPv6 en mente, luego fue adaptado a IPv4 y es usado desde la Capa 3 hacia arriba protegiendo al implementar estandares abiertos, usa Confidencialidad de datos e integridad de datos usando [[020 - Conceptos/020.2 - Seguridad/Criptografia|Criptografia]]
## Modos de IPSEC

Ambos usan Modo IKE para transportar informacion
- Transporte: No encripta "Header IP Original", se especifica, usado en DMVPN
- Tunel: Encripta "Header IP Original" y agrega un nuevo "Header IP"

- Transporte: Se inserta despues de la cabecera IP, la cabecera no esta encriptada pero las capas de transporte y superiores si lo estan
- Tunel: La cabecera IP esta encriptada, genera una nueva cabecera IP que contiene la IP de los extremos IPSEC([[010 - Protocolos/010.3 - Comunicaciones/VPN|VPN]], [[020 - Conceptos/020.4 - Dispositivos de Red/Firewall|Firewall]] o [[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]]).

## Protocolos de IPSEC
- IKE(Internet Key Exchange): Confidencialidad de datos
	- Intercambio de la negociacion de llaves de autenticacion y claves de forma segura utilizadas para los algoritmos de encriptacion simetrica dentro de [[010 - Protocolos/010.3 - Comunicaciones/VPN|VPN]]
	- Usan HMAC para chequeo de la autenticacion e integridad
- ESP(Encapsulating Security Payload): 
	- Confidencialidad e Integridad de datos
	- autentica el origen de los datos y es anti-replay(Opcional) asegurando que los datos no han sido modificados ni suplantados 
	- Es el unico que permite cifrar los datos usando DES, TDES y AES.
	- Usan [[020 - Conceptos/020.2 - Seguridad/Criptografia#HMAC o KHMAC|HMAC]] para chequeo de la autenticacion e integridad
- AH(Authentication Header): 
	- Integridad de Datos sin Confidencialidad
	- Asegura que los datos no hayan sido modificados o interferidos,

## IKE en profundidad
### Protocolos IKE:
- ISAKMP (Internet Security Association and Key Management Protocol): Define como establecer, negociar, modificar y borrar las SA.
- Oakley: Utiliza el algoritmo DH (Diffie-Hellman) para gestionar el intercambio de clases en SA
### Fases IKE
Fase 1: Una SA bidireccional entre 2 peers
Fase 2: Se usan SA unidireccionales entre peers usando los parametros de la fase 1
Fase 3: Opcional, ???