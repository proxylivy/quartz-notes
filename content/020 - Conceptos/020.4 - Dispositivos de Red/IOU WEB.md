Para las clases de `Seguridad en Redes` la configuracion de Red se mantiene en `Adaptador solo anfitrion`, El modo promiscuo en "`Permitir Todo`" y el firewall abajo

Para poder copiar las configuraciones de cada Router se usa el comando `copy running-config unix:`

Los [[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]] usa planos de control para el intercambio de informacion de protocolos, y los paquetes de datos se remiten en el plano de datos (interfaz microcodificada), estos se encargan de:
- Gestion de datos internos y control para reenvio de paquetes
- Informacion de enrutamiento y control para capa 2 y 3
- Informacion de plano de datos (estadisticas de trafico)
- Manejo de paquetes de datos para procesar rutas

# Aplicaciones
## TFTP64
Un programa desarrollado por Ph. Jounin
el cual nos permite conectarnos a:
- TFTP Server
- TFTP Client
- DHCP Server
- Syslog Server
- Log Viewer

Para la configuracion
Primero, ponle en la pestaña que necesitas
El directorio es: `C:\ProgramFiles\Tftp64`
Server interface: Virtualbox Host-Only Ethernet Adapter

## WinRadius
Se ejecuta como administrador
En la barra superior, Settings>System
y en las configuraciones, cambiamos `RADIUS IP` por `192.168.56.1`

Luego se pueden crear usuarios en Operation>Add User

| Value           | Value  |
| --------------- | ------ |
| User Name       | user01 |
| Password        | cisco  |
| Group           | N/A    |
| Calling Address | N/A    |
## Mecanismo de Conmutacion
Tambien conocido como Conmutacion de Software o ruta lenta, usa CPU en [[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]] para la conmutacion de paquetes, se ejecuta en el proceso **int_put** en tablas CAM y TCAM

Las tramas de un [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]] las manda a CAM

## FIB
Sincronizada con la tabla de enrutamiento y contiene la IP ded siguiente salto de cada destino en una red.

## Tabla de Adyacencia
Contiene la IP y MAC de siguiente salto conectadas directamente, ademas de las MAC de salida se sincroniza con tabla ARP y otras tablas de protocolos capa 2.

TO-DO: Representacion de CEF, pasar a Mermaid
![500](https://slink.proxylivy.work/image/e8cd033e-ea41-4eff-b861-ce18106acac3.png)

## CAM
**C**ontent **A**ddressable **M**emory o "Memoria de Contenido Direccionable", 

dentro de un [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]] se guarda una "Tabla de Direcciones MAC", las cuales identifican el puerto del switch y las 

Para guiarse usa Tabla de Direcciones MAC, las cuales identifican el puerto del Switch y las VLAN asociadas, ayuda a reducir la inundacion unicast desconocida


!
!
!
Escribe de como funciona cam y de que se encarga realmente
!
!
!

## TCAM
Temporal+CAM, se usa para seguridad como ACL, o prioridad como QoS

![500](https://slink.proxylivy.work/image/00a5b080-a866-4972-85e0-bd5f95623a95.png)

Extra: [Info en Produndidad - Cisco Community](https://community.cisco.com/t5/documentos-routing-y-switching/uso-de-memoria-tcam-y-cam-en-los-routers-y-switchs/ta-p/4536017)

## CEF
**C**isco **E**xpress **F**orwarding, propietario de cisco, es una tabla FIB

# Extra
## Configurar interfaz Duplex (Para que no moleste, se usa half)
```
SW Config# interface [int]
SW Config-if# duplex [half/full]
```
## Permitir ver los cambios en la consola
se configura en el modo Configuracion general (`Config#`), que este desactivado no es un error
```
logging console
```
## Consejos Acelera Terminal
- Desabilita la resolucion DNS para acelerar la CLI
```
no ip domain-lookup
```
- Output completo comando (No apretar espacio para ver mas)
```
terminal lenght 0
```