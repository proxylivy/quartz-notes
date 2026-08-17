# Info

Anexos Disponibles en [Copyparty - EX 2 - AA2](https://copyparty.proxylivy.work/Duoc/Conectividad%20y%20Redes/2do%20Semestre/VTY2112%20(CE)/Actividades/EX%202/AA2/)
- [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv4|IPv4]]
- [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv6|IPv6]]

Objetivo: Comprenda el concepto de enrutamiento de los routers y el uso de Gateway predeterminado en redes que operan a nivel IPv4/IPv6.

Contexto: Se tiene una topología de red, conformada con un routers y 3 switches, lo que permite en esta situación interconectar 3 redes LAN, cada una con diferentes servicios que deben permitir su funcionamiento desde cualquier red a nivel IPv4/IPv6.

## Calculo Direccionamiento

**Calculo VLSM Red IPv4**

- Calcular VLSM para la dirección de red 172.16.128.0/18, según cantidad de hosts que se encuentran señalado en la topología. Asignar IPv4 a todos los equipos finales de la topología.

**Calculo Subredes IPv6**

- Calcular las subredes IPv6 según lo solicitado para a cada red en la topología, realizando la asignación de IPv6 en todos los equipos finales de la topología.

## Configuracion Router y Switch

**Configuracion Router**

1. Asignar el nombre según lo propuesto en la topología.
2. Configurar clave de protección a la CLI del router
	- La clave será `acceso`
3. Configurar clave de protección a modo privilegiado
	- La clave será `duoc.uc`
4. Colocar el mensaje de advertencia `#SOLO ACCESO PERSONAL AUTORIZADO#`
5. Todas las contraseñas deben estar cifradas.
6. Configurar SSH, donde el equipo debe pertenecer al dominio `www.duoc.cl`. Utilizar llave criptográfica de 2048 bits. Además, en las conexiones remotas solo debe permitirse conexiones entrantes SSH. La cantidad simultánea de conexiones remotas será de 3 sesiones.
7. Asignar direccionamiento IPv4/IPv6 en las interfaces según la dirección solicitada en la topología.
8. Colocar descripción en las interfaces con el formato “CONEXIÓN A RED X”. La “X” reemplaza el nombre de la red señalada en la topología.
9. Probar desde los PC que tengan conectividad completa a nivel IPv4/IPv6.

**Configuracion de Switches**

1. Asignar el nombre según lo propuesto en la topología.
2. Configurar clave de protección a la CLI del router.
	- La clave será `acceso`
3. Configurar clave de protección a modo privilegiado
	- La clave será `duoc.uc`
4. Colocar el mensaje de advertencia `#SOLO ACCESO PERSONAL AUTORIZADO#`
5. Todas las contraseñas deben estar cifradas.
6. Implementar Telnet, con la password remoto. Permitir solo un máximo de 2 conexiones simultaneas.
7. Asignar IPv4 en la SVI 1 del switch, según la dirección IPv4 solicitada.
8. Asignar Gateway predeterminado en el switch a nivel IPv4.

## Servicios en Redes IPv4/IPv6

**Servicios HTTP/DNS**

1. Habilitar servicio HTTP/HTTPS.
	- Edita `index.html`, donde el titulo deberá decir “`EXPERIENCIA 7 CONECTIVIDAD ESENCIAL`”.
2. En servidor DNS, permitir la traducción de la página web `www.conectividad.cl` que estará cargado en servidor web. Esta traducción debe estar a nivel IPv4/IPv6.
3. Procurar que todos los equipos finales a nivel IPv4/IPv6 tengan DNS.
4. Probar desde todos los PC si pueden acceder a la página web en IPv4/IPv6.

**Implementacion Servicio TFTP**

1. Deberá guardar el archivo de configuración de ejecución del router en el archivo de ejecución de inicio.
2. Deberá respaldar la configuración de inicio del router en el servidor TFTP a nivel IPv4. Colocar como nombre `RESPALDO-INICIO-RA`
3. Deberá respaldar la configuración de ejecución del router en el servidor TFTP a nivel IPv6. Colocar como nombre `RESPALDO-EJECUCION-RA`
4. Comprobar en el servidor que ambos respaldos se hayan efectuado, revisando el servidor TFTP.

**Implementacion Servicio FTP**

1. Crear en el servidor FTP una cuenta que permita solo Escribir, Leer y Enlistar archivos. Utilizar como usuario ftp y password conectividad
2. Desde cualquier PC de la topología el acceso el acceso al servidor FTP a través de la ventana Command Prompt.
