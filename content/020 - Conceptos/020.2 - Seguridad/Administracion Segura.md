# Seguridad
- Fisica
- Sistema Operativo/Seguridad de Archivos de Configuracion
- Hardening

Enfoques
- Un solo router: implementado en Susursales o SOHO(Small Office Home Office)
- Defensa Profunda: Un Router y Un [[020 - Conceptos/020.4 - Dispositivos de Red/Firewall|Firewall]]
- Enfoque DMZ: Un Router y Firewall, hay una zona protegida con mas filtros

Acceso Administrativo Seguro
- Restriccion Acceso a Dispositivos
- Registros y Administracion de Accesos
- Autenticacion del Acceso
- Autorizar las acciones

# Nivel De Privilegios
## Distribucion
Nivel 0-1: Modo Usuario (*Router>*), Privilegios basicos al usuario del modo EXEC
Nivel 2-14: Modos Modificados
Nivel 15: Modo Privilegiado (*Router#*), Incluye todos los comandos de nivel enable
## Palabras Clave
- Interface: Modo Comandos dentro de Interfaces
- Router: Modo Protocolos de Enrutamiento
- Configure: Modo Configuracion Global
- Exec: Modo Privilegiado
## Limites
- No hay control de acceso a int e int logicas, puertos y ranuras especificas en un router
- Los comandos Heredables pueden generar problemas
- Los comandos superiores no estaran disponibles para los inferiores
- Asignar un comando espefico activara toda la rama (show ip route) -> (show \*)
# CLI Basada en roles
Disponible desde 12.3(11)T del IOS
Acceso mucho mas granular que con el nivel de privilegios
## Beneficios
- Seguridad: Define Grupos de comandos
- Disponibilidad: Imposibilita ejecuciones mal intencionadas de no autorizados
- Eficiencia: Solo veran comanodos aplicables a lo que tengan acceso
## Roles Basados en Vistas
- Vista de Root: Crea, elimina y Configura Vistas, Tiene privilegios nivel 15 no es igual a usuario nivel 15
- Vista CLI: Comandos vistos desde una CLI
- Supervista:  2 o mas vistas CLI, los admin pueden definir que comandos son aceptados y visibles

## Configuracion Resistente
- `secure boot-image` -> Asegura la imagen actual IOS, funciona mejor cuando la configuracion esta ejecutada para almacenamientos externos de conexion ATA
- `secure boot-config` -> Toma una instantanea que esta oculta y no puede ser visto o eliminado directamente desde el prompt
- `show secure bootset` -> Verifica si hay alguna imagen o configuracion guardada
- `no service password-recovery` -> Peligroso, es un comando oculto, que desabilita el acceso al modo ROMMON



# Configuracion
## Contraseña Segura y limite tiempo
```
security passwords min-length 10
service password-encryption
line vty 0 4
exec-timeout 3 30
line console 0
exec-timeout 3 30
```
## Habilitar Usuario Contraseña Secreta
Nota: secret 9 significa
```
R1 Config# enable secret 9 cisco12345
R1 Config# enable secret 9
```
## Configuracion Base de Datos Local
```
username [name] password [password]
username [name] secret [password]
line con 0
login local
```
## Habilitar Fail2Ban
```
login block-for 15 attempts 5 within 60
```
## Logeo Basado en IP
```
ip access-list standard [acl_name]
remark [Comment]
permit [ip]
exit
login quiet-mode access-class [acl_name]
```
## Delay por cada inicio de sesion
```
login delay [seconds]
login on-success log
login on-failure log
```
## Proteccion Acceso Remoto
```
ip domain-name [domain_name]
crypto key generate rsa general-keys modulus 1024
ip ssh version 2
username [username] algorith-type scrypt secret [password]
line vty 0 4
login local
transport input ssh
exit
ip ssh time-out 60
ip ssh authentication-retries 2
```
## Nivel de Privilegios
Nota: Los niveles superiores tendran los comandos de los niveles inferiores
### Nivel 1
```
username [username1] privilege 1 secret [password1]
```
### Nivel 5
```
privilege exec level 5 ping
enable secret level 5 [password2]
username [username2] privielege 5 secret [password2]
```
### Nivel 10
```
privilege exec level 10 reload
enable secret level 10 [password3]
username [username3] privilege 10 secret [password3]
```
### Nivel 15
```
username [username4] privilege 15 secret [password4]
```

## Ejemplo Configuracion de Vistas
```
aaa new-model
enable view
!
parser view [View_Name1]
secret [passwpord1]
commands exec include show
exit
!
parser view [View_Name2]
secret [password2]
commands exec include ping
exit
!
parser view [View_Name3]
secret [password3]
commands exec include reload
exit
```
## Desabilita SNMP
```
no snmp-server
```
# Visualizar
`show ip ssh` -> Revisa el protocolo SSH

## Verificacion de Vistas
`enable view [View_Name]` -> Acceder a la vista con \[Password\]
`?` -> Ver comandos Disponibles
`show run` -> Ver Configuracion
`show parser view` -> Ver parser Activo
`show parser view all` -> (root) Ver todos los parser view
