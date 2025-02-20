Una herramienta para registrar logs a travez de la red, su estructura es Servidor/Cliente
## Caracteristicas
- Usa el puerto 514 UDP
- [RFC 5424](https://www.rfc-editor.org/rfc/rfc5424)
## Capacidades
- Recopilar informacion de registro de seguimiento y solucion de problemas
- Seleccionar el tipo de informacion de registro que se captura
- Especificar los distintos tipos de mensajes syslog capturados


# Tabla Mensajes de Syslog

| Level | Keyword      | Descripcion                  | Definicion  | Ejemplo                          |
| ----- | ------------ | ---------------------------- | ----------- | -------------------------------- |
| 0     | Emergencias  | Sistema Inutilizable         | LOG_EMERG   | Sistema CISCO IOS no pudo cargar |
| 1     | Alertas      | Accion Inmediata             | LOG_ALERT   | La temperatura es muy alta       |
| 2     | Critico      | Condiciones Criticas         | LOG_CRIT    | No se puede asignar memoria      |
| 3     | Errores      | Condiciones Errores          | LOG_ERR     | Tamaño memoria no valido         |
| 4     | Advertencia  | Condicion Advertencia        | LOG_WARNING | La encriptacion Fallo            |
| 5     | Notificacion | Normal pero Necesita Algo    | LOG_NOTICE  | Interfaz Enciende/Apaga          |
| 6     | Informacion  | Solo Mensajes de Informacion | LOG_INFO    | Paquete denegado por ACL         |
| 7     | Debugging    | Mensajes de Debug            | LOG_DEBUG   | Tipo de paquete invalido         |

# Configuracion
## Syslog en Router(Cliente)
```
logging host [server-ip]
logging trap
logging source-interface [int S/S/P]
logging on
```
