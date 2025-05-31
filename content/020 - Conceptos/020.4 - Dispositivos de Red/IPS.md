# Que es?
Intrusion Prevention System o Sistema de Prevencion de Intrusos detecta trafico malicioso y puede detener ataques en tiempo real, pasando todo el trafico por el, funciona solo en capa 3 y 4, permite trabajar con Protocolos [[010 - Protocolos/010.3 - Comunicaciones/Syslog|Syslog]] o Secure Device Event Exchange (SDEE), el cual guarda mensajes en una consola administrativa de red, tiene 2 tipos "Basado en Host" o "Basado en Red"
## Comparacion
Nota: a [[020 - Conceptos/020.4 - Dispositivos de Red/IDS#Comparacion|IDS]]
Ventajas
- Detiene paquetes malignos
- usa tecnicas de normalizacion de flujo (Vease. Reset Kill)

Deventajas
- La sobrecarga afecta la red
- Se debe tener una politica de seguridad bien definida
## Tipos de Alarma
- Falso Positivo: No se realiza Ataque pero da alarma (Alarma)
- Falso Negativo: Se realiza Ataque pero no detecto nada (Alarma)
- Positivo Real: Se realiza Ataque y detecto Ataque (Ideal)
- Negativo Real: No se realiza Ataque ni alarma (Ideal)


IPS basados en host o HIPS
Se instalan en el usuario final, analizando los paquetes entrantes y salientes, o procesos del sistema ante ataques

IPS basado en red o NIPS
Los [[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]] ISR o [[020 - Conceptos/020.4 - Dispositivos de Red/Firewall|Firewall]] ASA, modulos de red Catalyst 6500 o IPS dedicados monitorean el correcto uso de firmas en la red o comportamientos extraños en la red

Ventajas
- No es visible en la red
- Puede ver la magnitud de un ataque
- No depende de ningun OS
- Bajo nivel de uso en la red
Desventajas
- No puede revisar trafico cifrado
- No puede determinar cuando un ataque fue exitoso

## Eleccion Solucion:
- Cantidad de trafico de red: Que tanta informacion pasa
- Topología de la red: Se diseña en base a que tenga un IDS/IPS/Firewall
- Presupuesto de Seguridad: Dinero para ser sustentable
- Personal Disponible para administrar IPS: Personas que sepan para configurarlo
## Firmas
Identifican de forma unica gusanos, virus o anomalias de protocolos, si coincide una firma con un flujo de datos, se registra el evento y/o se envia una alarma.
Estas firmas pueden ser:
Atomicas: Unicas, son faciles de identificar, es llevado a cabo rapido y eficiente
Compuestas: conocidas tambien como "Firmas con Estado", Requieren varios datos, su duracion se llama "Horizonte de eventos".
Las alertas que generen estas firmas son "Atomicas" o "Resumidas" Pero significa lo mismo

## Tabla Acciones IPS

| Categoria                      | Alerta                                                     | Descripcion                                                                                                                                                                                                                                                   |
| ------------------------------ | ---------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Generar Alerta                 | Producir alerta                                            | Se registra una alerta                                                                                                                                                                                                                                        |
| =                              | Producir alerta detallada                                  | Incluye transcripcion del paquete                                                                                                                                                                                                                             |
| Registro de la actividad       | Registrar Paquetes atacantes                               | Registra IP atacante y alerta                                                                                                                                                                                                                                 |
| =                              | Registrar Paquetes Pares                                   | Registra IP atacante, victima y alerta                                                                                                                                                                                                                        |
| =                              | Registrar Paquetes victima                                 | Registra IP victima y alerta                                                                                                                                                                                                                                  |
| Descartar o prevenir actividad | Denegar Atacante en linea                                  | - Terrmina paquete actual y futuros por un tiempo<br>- Mantiene lista de atacantes denegados<br>- Entradas pueden borrar a mano o por tiempo<br>- El temporizador puede extenderse si atacan otra vez<br>- Si se llena la lista de atacantes, se deniega todo |
| =                              | Denegar conexion en linea                                  | Termina paquete actual y futuros del flujo TCP                                                                                                                                                                                                                |
| =                              | Denegar Paquete en linea                                   | Termina un paquete                                                                                                                                                                                                                                            |
| Reinicio de una conexion TCP   | Reinicia conexion TCP                                      | "TCP reset" para tomar control de TCP y terminarlo                                                                                                                                                                                                            |
| Bloqueo de actividad futura    | Solicitar bloqueo de conexion                              | le pide al atacante que bloquee la conexion                                                                                                                                                                                                                   |
| =                              | Solicitar bloqueo de host                                  | le pide al atacante que pare                                                                                                                                                                                                                                  |
| =                              | Solicitar [[003 - Protocolos/Protocolos de Red/SNMP|SNMP]] trap | sapea al sensor sobre el atacante                                                                                                                                                                                                                             |
# Configuracion
## Configuracion General
Nota: Usa [[010 - Protocolos/010.3 - Comunicaciones/Syslog|Syslog]] para logging
Nota2: In|Out como un flujo, out es hacia donde va la regla
Nota3: Signatures funciona asi
- `signature 2004 0` bloqueara `ICMP Echo Request`
- `signature 2000 0` bloqueara `ICMP Echo Replies`
```
!# Activa la Licencia
license boot module c1900 technology-package securityk9
do wr
end
reload
!# Enter "Yes"
!# Confirm
!# Wait
!# enter
enable
!
!# Crea Directorio y configura ubicacion de firmas
mkdir [dir]
ip ips config location flash:[dir]
!
!# Crea regla ips
ip ips name [ips-rule-name]
!
!# Activa Logging Atomico
clock set [hr:mn:sec] [day] [month] [year]
logging on
logging host [ip-syslog-server]
service timetamps log datetime msec
!
!# Carga y Uso de fimas ios basic
ip ips signature-category
category all
retired true
exit
category ios_ips basic
retired false
!
!# Aplicar Regla IPS
int [int S/S/P]
ip ips [ips-rule-name] {in|out}
!
!# Modifica el funcionamiento de firmas IPS
ip ips signature-definition
signature 2004 0
status
retired false
enable true
exit
engine
event-action produce-alert
event-action Deny-Packet-Inline
```
## Habilitar Servidor sdee
Nota1: No se usa en el ramo 
Nota2: Debes tener un servidor HTTP Habilitado
```
ip http server
ip ips notify sdee
```
# Verificacion
`show ip ips all` -> Comprueba funcionamiento del IPS
`show ip ips configuration` -> Verificar Configuracion
`show ip ips interfaces` -> Ver interfaces entrantes y salientes
`show ip ips statistics` -> Ver estadisticas del ips

# Extras
Un [[020 - Conceptos/020.4 - Dispositivos de Red/Firewall|Firewall]] de Aplicacion en Gateway sustituye al IPS/IDS, debido a que funciona 
Tienen una version mas ligera llamada [[020 - Conceptos/020.4 - Dispositivos de Red/IDS|IDS]]

El port mirroring es la copia de datos para analizar luego
SPAN o Analizador de Puerto con Switches usa la funcion anterior y es conectado a un IDS o IPS
- Trafico de ingreso: entra a un switch
- Trafico de egreso: sale de un switch
- Puerto (SPAN) de origen: puerto que supervisa funcion SPAN
- Puerto (SPAN) de destino: Supervisa puertos de origen, creando un "Puerto de Supervision"
- Sesion SPAN: Asociacion de un puerto de destino con alguno de sesion
- Origen VLAN: VLAN que se supervisa para el analisis de trafico