## Que es?
Debido a que las contraseñas son debiles e inseguras, se creo un protocolo para verificar la informacion de login con un [[020 - Conceptos/020.4 - Dispositivos de Red/Servidor|Servidor]]
## Componenetes
- Autenticacion(Authentication): Los administradores deben demostrar quienes son
- Autorizacion(Authorization): Limites de servicios permitidos a los usuarios
- Auditar(Accounting): recolecta y reporta datos como inicio y termino de conexiones con fecha y hora del comando ingresado, util para resolver problemas
## Metodos de Autenticacion
- Modo de Caracteres: `login`, `exec` y `enable`, Administracion remota, puertos console, vty, aux y tty 
- Modo de Paquetes: `ppp` y `network`, Acceso remoto, Puertos Dial-up y VPN
- Autenticacion Local: Tambien conocidad como `autenticacion Autocontenida` Se utiliza el [[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]] para la verificacion en la base de datos, es la misma para [[020 - Conceptos/020.2 - Seguridad/Administracion Segura#CLI Basada en roles|CLI Basada en Roles]]
- Autenticacion AAA Basada en Servidor: El [[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]] hace conexion con un [[020 - Conceptos/020.4 - Dispositivos de Red/Servidor|Servidor]] Externo, con RADIUS o TACACS+
## Tabla Diferencia
Nota 1: TACACS+ es una mejora de Cisco a TACACS, a pesar del nombre, son incompatibles, soportado desde 10.3 de IOS
Nota 2: Radius tiene la mejor seguridad de contraseña pero lo demas lo envia en texto plano, usa los puertos en UDP 1645 o 1812 para la autenticacion con autorizacion y el 1646 o 1813 para la auditoria.

|                    | Tacacs+                           | Radius                                                                     |
| ------------------ | --------------------------------- | -------------------------------------------------------------------------- |
| Funcionalidad      | Modular                           | Combina Autenticacion con Autorizacion<br>Separa Auditoria, menos flexible |
| Estandar           | Mayormente Soportado<br>por cisco | Estandar Abierto/RFC                                                       |
| UDP/TCP?           | TCP                               | UDP                                                                        |
| CHAP               | Desafio Bidireccional             | Desafio Unidireccional                                                     |
| Soporte Protocolo  | Soporte Multiprotocolo            | No ARA, No NetBEUI                                                         |
| Confidencialidad   | Paquete Cifrado 100%              | Contraseña Cifrada                                                         |
| Personalizacion    | Provee autorizacion de comandos   | No tiene opciones para autorizar                                           |
| Registro Auditoria | Limitado                          | Extensivo                                                                  |
Se puede integrar Tacacs+ y Radius con ACS en Windows Server, sus caracteristicas son
- Arquitectura distribuida en gran escala
- Interfaz grafica para clientes
- Autenticacion a travez de AD y LDAP
- Syslog
- Administracion flexible de dispositivos


# Configuracion
## AAA Local
Nota: `local` no es sensible a mayuscula y minusculas y `local-case` si lo es
```
username [username] secret [password]
aaa new-model
aaa authentication login default local-case
aaa local authentication attempts max-fail 10
```
## AAA Nombrado en Telnet ACL
Nota: Apoyate de esta [[020 - Conceptos/020.1 - Administracion/ACL#Protege Acceso Telnet|ACL]]
```
aaa new-model
aaa authentication login [username-telnet] local-case
line vty 0 1
login authentication [username-telnet]
```
## Tacacs+
```
aaa new-model
tacacs-server host [IP-Tacacs-Server] key [password]
aaa authentication login default group tacacs+ local-case
```
## Radius en IOU-WEB
Nota: El comando de `radius-server` se configura 2 veces debido a que esta pronto ~~deprecated~~
```
aaa new-model
radius-server host [IP-Radius-Server] auth-port 1812 acct-port 1813 key [password]
aaa authentication login default group radius local-case
```
## Ejemplo Autorizacion Tacacs+
Nota: Asegurarse que al menos un usuario tiene derechos de acceso sin restricciones
```
username [admin] secret [password]
aaa new-model
aaa authentication login default group tacacs+
aaa authorization exec default group tacacs+
aaa authorization network default group tacacs+
```