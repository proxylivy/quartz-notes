# Info

Objetivo: Realizar la configuración básica de routers y switch, utilizando equipamiento físico

**Contexto**

En este laboratorio, en donde se tiene una topología de red estándar para realizar varias experiencias de la red, deberás realizar la configuración básica de routers y switch, aprendiendo a utilizar el cable consola, emulación de terminal y los comandos requeridos para que la red pueda funcionar de forma correcta con las configuraciones realizadas.

**Materiales**
- 3 Routers
- 3 Switches
- 7 Cables de Red UTP CAT6a
- 1 Cable Consola Rollout 8P8C (RJ45) a USB
	- Alternativo: 1 Cable Consola Cisco 8P8C (RJ45) a DB9 + Adaptador DB9 a USB

Topologia
![](https://slink.proxylivy.work/image/f1565d71-78a0-4ded-990f-d1eb7dc8d2b1.png)

En un computador con Windows, necesitas
- Driver CH341 (Chip Barato): [Github - DecaturMakers/CH340_drivers](https://github.com/DecaturMakers/CH340_drivers-Linux-Mac-Windows)
- Saber que dentro de "Administrador de Dispositivos" buscar como se llama el serial, deberia aparecer como "COMn" (Donde la N es el numero de la conexion)

En Linux, los drivers estan dentro del Kernel, y deberia reconocerlo como `/dev/ttyUSB0` (Va incrementando 1 en 1 por cada conexion Serial USB)

Para ambos se utiliza [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/)

**Configuracion**

1. Asignar nombre al equipo, según el equipo asignado, según la topología oficializada.
2. Colocar fecha y hora correcta
	- Verifica con `show clock`
3. Colocar un mensaje de advertencia "`#SOLO ACCESO PERSONAL AUTORIZADO#`"
4. Asignar clave cifrada que protege el acceso al modo privilegiado. La clave será "`duoc.uc`"
5. Asignar clave que protege acceso a la CLI del dispositivo. La clave será "`acceso`"
6. Configurar Telnet para permitir un máximo de 3 conexiones remotas entrantes al dispositivo. La password será "`remoto`"
7. Emita el comando "`show running-config`" y verifique todas las configuraciones anteriormente escrita.
	- Notará que para el comando "`enable secret`" la contraseña aparecerá cifrada.
8. Emitir comando para cifrar todas las contraseñas que se encuentran en texto claro.
	- Luego emitir nuevamente el comando "`show running-config`" y revisar las diferencias con el paso anterior.
9. Bloquee la sesión del router o switch. Ejectuando `exit` hasta que salga la pantalla inicial
10. Notará que el dispositivo muestra el mensaje advertencia, y le solicita una contraseña, para lo cual deberá ingresar primero la clave que protege el acceso a la CLI del dispositivo, y luego la credencial que protege el paso desde el modo usuario al privilegiado.
11. Ingresar a cualquier interfaz del *router* y asignar la IPv4 `192.168.1.1` con máscara de red `255.255.255.0` y la IPv6 `2019:ACAD:ACAD:1::1/64`. Luego encender con el comando `no shutdown`. Luego colocar como descripción “`CONEXIÓN DE RED`”
12. Ingresar a la interfaz vlan `1` del *switch* y asignar la IPv4 `172.16.1.1` con máscara de subred `255.255.255.0` y la IPv6 `2019:ACAD:ACAD:2::1/64`. Luego encender la interfaz con el comando no shutdown. Luego colocar como descripción “`CONEXIÓN DE RED`”
13. Regresar al modo Privilegiado, y emitir el comando PING y la IPv4/IPv6 emitida. Comprobar el PING sea exitoso.
14. Emitir el comando `show version`. Tomar nota de la versión del sistema operativo, fecha de compilación de la IOS, memorias que utiliza el equipo.
15. Emitir el comando `dir flash:` y observar los archivos que tiene almacenado el router o switch utilizado.
16. Realizar la configuración de SSH. Para lo cual deberá crear un dominio el cual será `www.duoc.cl` el usuario será el nombre del dispositivo y la password duoc.ssh. Utilice como llave criptográfica de longitud de 2048 bits. Permitir que el acceso remoto sea solo para conexiones entrantes SSH y que al momento de conectar solicite usuario y contraseña.

