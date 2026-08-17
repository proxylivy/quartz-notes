# Info

Anexos disponibles en [Copyparty - EX 2 - AA3](https://copyparty.proxylivy.work/Duoc/Conectividad%20y%20Redes/2do%20Semestre/VTY2112%20(CE)/Actividades/EX%202/AA3/)

# Parte 1

Objetivos: Comprenda el concepto de enrutamiento estático en redes LAN que operan con direccionamiento en IPv4/IPv6.

Contexto: Se tiene una topología de red, conformada por tres routers, que permiten conformar dos redes WAN, y dos switches, que permiten la creación de 3 redes LAN. Le han pedido realizar la configuración básica de los equipos, configuración de servicios HTTP/DNS y el funcionamiento de dispositivos de IoT, mediante enrutamiento estático para redes que operan en IPv4/IPv6.

## Calculo Direccionamiento

**Calculo VLSM IPv4**

- Calcular VLSM para la dirección de red 192.168.0.0/16, según cantidad de hosts que se encuentran señalado en la topología. Asignar IPv4 a todos los equipos finales de la topología.

**Calculo Subredes IPv6**

- Calcular las subredes IPv6 a partir de la dirección de red 2019:AAAA:BBBB::/48 según lo solicitado para a cada red en la topología, realizando la asignación de IPv6 en todos los equipos finales de la topología.

## Configuracion Basica en Routers y Switches

**Configuracion en Router**

1. Asignar el nombre según lo propuesto en la topología
2. Configurar clave de protección a la CLI del router
	- La clave será `consola`
3. Configurar clave de protección a modo privilegiado
	- La clave será `duoc.uc`
4. Colocar el mensaje de advertencia `#SE PERMITE EL ACCESO AL ESPECIALISTA DE RED#`
5. Todas las contraseñas deben estar cifradas
6. Configurar SSH, donde el equipo debe pertenecer al dominio `www.duoc.cl`. Utilizar llave criptográfica de 1024 bits. Además, en las conexiones remotas solo debe permitirse conexiones entrantes SSH. La cantidad simultánea de conexiones remotas será de 2 sesiones
7. Asignar direccionamiento IPv4/IPv6 en las interfaces según la dirección solicitada en la topología
8. Probar desde los PC que tengan conectividad completa a nivel IPv4/IPv6

**Configuraciones en Switches**

1. Asignar el nombre según lo propuesto en la topología
2. Configurar clave de protección a la CLI del router
	- La clave será `consola`
3. Configurar clave de protección a modo privilegiado
	- La clave será `duoc.uc`
4. Colocar el mensaje de advertencia `#SE PERMITE EL ACCESO AL ESPECIALISTA DE RED#`
5. Todas las contraseñas deben estar cifradas
6. Implementar Telnet, con la password telnet. Permitir solo un máximo de 2 conexiones simultáneas.
7. Asignar IPv4 en la SVI 1 del switch, según la dirección IPv4 solicitada
8. Asignar Gateway predeterminado en el switch a nivel IPv4

## Enrutamiento Estatico IPv4/IPv6

**Configuracion Rutas Estaticas IPv4**

1. Implementar rutas estáticas con IPv4 de siguiente salto.
2. Comprobar que las rutas estáticas se vean reflejadas en la tabla de enrutamiento de los routers.
3. Comprobar conectividad desde el PC hacia el servidor HTTP/DNS y Equipos de IoT.

**Configuracion Rutas Estaticas IPv6**

1. Implementar rutas estáticas con IPv6 de siguiente salto.
2. Comprobar que las rutas estáticas se vean reflejadas en la tabla de enrutamiento de los routers.
3. Comprobar conectividad desde el PC hacia el servidor HTTP/DNS y Equipos de IoT.

## Servicios en Redes IPv4/IPv6

**Servicios HTTP/DNS**

1. Habilitar servicio HTTP/HTTPS. 
	- Edita index.html, donde el titulo deberá decir “`EXPERIENCIA 8-1 CONECTIVIDAD ESENCIAL`”.
2. En servidor DNS, permitir la traducción de la página web `www.conectividad.cl` que estará cargado en servidor web. Esta traducción debe estar a nivel IPv4/IPv6.
3. Procurar que todos los equipos finales correspondientes a nivel IPv4/IPv6 tengan DNS.
4. Probar desde todos los PC si pueden acceder a la página web en IPv4/IPv6.

**Servicios de IoT**

1. Configurar el servidor de IoT, creando una cuenta.
	- El nombre de la cuenta será `conectividad` y la password `esencial`
2. Configurar los equipos de IoT, permitiendo que los equipos puedan conectarse hacia el servidor.
3. Desde PC de RED-A comprobar que los equipos de IoT puedan ser manipulados de forma remota, todo esto mediante al enrutamiento estático implementado anteriormente.

# Parte 2

Objetivo: Implementación de enrutamiento estático en redes que operan con IPv4/IPv6

Escenario: En este laboratorio, en donde se tiene una topología de red estándar para realizar varias experiencias de la red, deberás realizar la configuración básica de routers y switch, el cálculo de VLSM para IPv4, el cálculo de subredes de IPv6, implementar enrutamiento estático IPv4/IPv6 y realizar el respaldo de las configuraciones de los dispositivos.

**Material**
- 3 Routers
- 2 Switches
- 2 PCs Finales
- 7 Cables de Red UTP CAT6a
- 1 Cable Consola Rollout 8P8C (RJ45) a USB
	- Alternativo: 1 Cable Consola Cisco 8P8C (RJ45) a DB9 + Adaptador DB9 a USB

**Topologia**

![](https://slink.proxylivy.work/image/ffd68416-c0d5-48b1-9fdc-237cb9bf37f3.png)

En un computador con Windows, necesitas
- Driver CH341 (Chip Barato): [Github - DecaturMakers/CH340_drivers](https://github.com/DecaturMakers/CH340_drivers-Linux-Mac-Windows)
- Saber que dentro de "Administrador de Dispositivos" buscar como se llama el serial, deberia aparecer como "COMn" (Donde la N es el numero de la conexion)

En Linux, los drivers estan dentro del Kernel, y deberia reconocerlo como `/dev/ttyUSB0` (Va incrementando 1 en 1 por cada conexion Serial USB)

Para ambos se utiliza [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/)

## Configuracion Basica de Routers y Switches

1. Asignar nombre al equipo, según el equipo asignado, según la topología oficializada.
2. Colocar un mensaje de advertencia `#SOLO ACCESO PERSONAL AUTORIZADO#`
3. Asignar clave cifrada que protege el acceso al modo privilegiado. La clave será `duoc.uc`
4. Asignar clave que protege acceso a la CLI del dispositivo. La clave será `acceso`
5. Configurar Telnet para permitir un máximo de 2 conexiones remotas entrantes al dispositivo. La password será `remoto`
6. Emita el comando `show running-config` y verifique todas las configuraciones anteriormente escrita. Notará que para el comando enable secret la contraseña aparecerá cifrada.

## Calculo de Direccionamiento

1. Realizar calculo de VLSM a partir de la dirección de red IPv4 10.0.0.0/16, según la información proporcionada a continuación:

| Subred | Hosts |
| ------ | ----- |
| LAN-1  | 500   |
| LAN-2  | 600   |
| LAN-3  | 120   |
| WAN-1  | 6     |
| WAN-2  | 4     |

2. Asignar IPv4 a todas las interfaces activas de los routers, interfaces VLAN de los switches y equipos finales.
3. Para los switches configurar default-gateway correspondiente.
4. Realizar calculo de subredes a partir de la dirección de red IPv6 2019:FFFF:EEEE::/48, según la información proporcionada a continuación


| Red   | Subred |
| ----- | ------ |
| LAN-1 | 12     |
| LAN-2 | 15     |
| LAN-3 | 18     |
| WAN-1 | AAA    |
| WAN-2 | BBB    |

5. Asignar IPv6 a todas las interfaces activas de los routers y equipos finales.

## Enrutamiento Estatico IPv4/IPv6

1. Implementar enrutamiento estático IPv4 con next-hop en todos los routers de la topología.
2. Comprobar la existencia de las rutas estáticas IPv4 configuradas, emitiendo el comando `show ip route`
3. Implementar enrutamiento estático IPv6 con next-hop en todos los routers de la topología.
4. Comprobar la existencia de las rutas estáticas IPv6 configuradas, emitiendo el comando `show ipv6 route`
5. Comprobar conectividad completa desde los PC hacia todas las subredes de la topología a nivel IPv4/IPv6. Si no hay conectividad completa, emitir comando `show running-config` en los dispositivos para revisar si no existe alguna configuración errónea o faltante. Es importante para esta experiencia lograr la conectividad completa.

## Respaldo de configuracion en los Routers y Switches

1. Teniendo conectividad completa en la topología, deberán guardar la configuración en todos los routers y switches de la topología, para este efecto, ir al modo privilegiado, y emitir el comando `wr`
2. En el PC de la red LAN-1, abrir el programa [TFTPD64](https://github.com/PJO2/tftpd64).
3. En el escritorio, crear una carpeta llamada Experiencia 8-2
4. En el programa TFTP64, hacer clic en Browse y buscar la carpeta anteriormente creada, acá se guardarán los archivos de configuración de los dispositivos.
5. En Server Interfaces del mismo programa, asociarlo a la tarjeta de red del computador. Esta deberá mostrar la IPv4 que asignaron según VLSM calculado en pasos anteriores.
6. En todos los routers y switches de la topología ir al modo privilegiado.
7. Emitir el comando copy running-config tftp:
8. El equipo le solicitará que digite la IP, acá deberán colocar la IPv4 del PC de la red LAN-1
9. Luego en el nombre, utilizar la siguiente estructura `R1-GRUPOX` ó `SWA-GRUPOX`. (La “X” se reemplazará por el número del POD que estén situados)

