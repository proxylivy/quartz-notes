Es un Servicio para sincronizar fecha y hora de manera exacta, Sigue el modelo Servidor/Cliente
Hay Servidores [Publicos](https://www.ntppool.org/es/) o Privados
## Caracteristicas
- Usa el puerto 123 UDP
- [RFC 1305](https://www.rfc-editor.org/rfc/rfc1305)
## Seguridad
- Esquema de restriccion basado en [[020 - Conceptos/020.1 - Administracion/ACL|ACL]]
- Autenticacion Cifrada (Version >=3 NTP)

# Configuracion
## Hora Manual
```
clock set [hr:mn:sc] Month Day Year
```
## Declarar Master y Cliente
### Router Master (10.0.0.1)
```
ntp master 1
```
### Router Slave
```
ntp server 10.0.0.1
```

## Autenticacion NTP
Nota: Se hace en todos los dispositivos
```
ntp authenticate
ntp authentication-key 1 md5 [password]
ntp trusted-key 1
```
# Verificacion
`show clock` -> Revisar Hora
`show ntp status` -> Revisa sincronizacion NTP
`show ntp associations detail` -> Revisa toda la informacion de ntp
`show ntp associations detail | include [IP-NTP-Server]` -> Ver solo la configuracion del peer NTP