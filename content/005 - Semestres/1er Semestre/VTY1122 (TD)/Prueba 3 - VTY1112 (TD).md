# Info
**Topologia**
![](https://slink.proxylivy.work/image/c7242f80-8d04-469b-8568-1d7030e163d9.png)

**Contexto Cliente**
El minimarket “Doña Juanita” visto en experiencias pasadas, presentó una caída en sus ventas del 20% durante el 2018, El Gerente General Benjamín Toledo, ha contratado sus servicios como consultor TIC para que presente una solución tecnológica que permita revertir esta situación, logrando mejorar las ventas y experiencias del cliente.
Dentro de las soluciones realizadas por usted fue la implementación de dispositivos finales y de IOT que operaban bajo direccionamiento IPv4/IPv6, lo cual operaron por un tiempo determinado (en imagen de documento entregado por docente). El Gerente detectó que muchos equipos de IOT se desconectaban debido al estado del cableado instalado que correspondía a UTP 5e, por lo cual ahora se recableará utilizando cableado UTP Categoría 6, además de solicitar que los equipos IOT estén conectados mediante conexión inalámbrica, utilizando para esto la solución de autenticación más robusta que ofrecen los routers inalámbricos adquiridos por este minimarket.
Por tal motivo, su tarea como consultor TIC, será el recableado de la red, según lo especificado por el cliente, la configuración de equipos IOT, mediante conexión inalámbrica y mecanismos de autenticación seguros, además de la asignación de IPv4/IPv6 en equipos apropiados.
Para esta labor, dispone de un total de 120 minutos.
Se muestra a continuación, solución realizada anteriormente en minimarket “Doña Juanita”

1. Conectorizacion de Dispositivos de Red
	- Realizar la interconexión de dispositivos de red, utilizando cable UTP categoría 6, según la información señala en las siguientes tablas:

| Dispositivo    | Interfaz | Conecta A         | Interfaz A |
| -------------- | -------- | ----------------- | ---------- |
| Router Central | G0/1     | SW-Izquierda-1    | G0/1       |
| ==             | G0/2     | SWA               | G0/2       |
| ==             | G0/0     | SW-Derecha-1      | G0/1       |
| ---            |          |                   |            |
| SW-Izquierda-1 | F0/1     | Wireless-1        | 0/0        |
| SW-Derecha-1   | F0/1     | Wireless-2        | 0/0        |
| ---            |          |                   |            |
| SWA            | F0/24    | HTTP-DNS          | F0         |
| ==             | F0/16    | Servidor-IOT      | F0         |
| ==             | F0/12    | FTP               | F0         |
| ==             | F0/10    | Administrador-IOT | F0         |

2. Asignacion Direccionamiento IPv4
	- Realizar asignación de direccionamiento IPv4 ha equipos finales y servidores, según la información que se proporciona a continuación:
	- Comprobar que los equipos tengan conectividad entre ellos, a nivel de IPv4.

| Dispositivo       | Direccion IPv4                      | Subred | Default-Gateway | DNS-Server  |
| ----------------- | ----------------------------------- | ------ | --------------- | ----------- |
| HTTP-DNS          | 11000000.10101000.00110010.00010100 | /27    | .1              | IP HTTP-DNS |
| SERVIDOR-IOT      | 11000000.10101000.00110010.00001010 | /27    | .1              | IP HTTP-DNS |
| FTP               | 11000000.10101000.00110010.00001111 | /27    | .1              | IP HTTP-DNS |
| ADMINISTRADOR-IOT | 11000000.10101000.00110010.00000101 | /27    | .1              | IP HTTP-DNS |

3. Asignacion Direccionamiento IPv6
	- Realizar asignación de direccionamiento IPv6 de la forma más resumida a equipos finales y servidores, según la información que se proporciona a continuación:
	- Comprobar que los equipos tengan conectividad entre ellos, a nivel de IPv6.

| Dispositivo       | Direccionamiento IPv6                    | Prefijo | Link-Local | Default-Gateway | DNS-Server  |
| ----------------- | ---------------------------------------- | ------- | ---------- | --------------- | ----------- |
| HTTP-DNS          | 2019:ACAD:ACAD:0003:0000:0000:0000:AAAA  | /64     | FE80::1    | .1              | IP HTTP-DNS |
| Servidor-IOT      | 2019:ACAD:ACAD:0003:ABCD:4567:0000:0001  | /64     | FE80::2    | .1              | IP HTTP-DNS |
| FTP               | 2019:ACAD:ACAD:0003:0000:0000:0000:BBBB  | /64     | FE80::3    | .1              | IP HTTP-DNS |
| Administrador-IOT | 2019:ACAD:ACAD:0003:AAAA:BBBB:CCCC:FFFFE | /64     | FE80::4    | .1              | IP HTTP-DNS |

4. Configuracion Equipos Inalambricos
	- Se ha solicitado configurar los routers inalámbricos adquiridos por la empresa, para lo cual deberá utilizar el siguiente esquema de direccionamiento en la opción apropiada para esto.


| Dispositivo | Direccion IPv4                      | Subred | Default-Gateway | DNS-Server  |
| ----------- | ----------------------------------- | ------ | --------------- | ----------- |
| Wireless-1  | 11000000.10101000.00001010.00000010 | /25    | .1              | IP HTTP-DNS |
| Wireless-2  | 11000000.10101000.00011001.00001010 | /24    | .1              | IP HTTP-DNS |

Tambien Deberá configurar los DHCP de los routers inalámbricos, para que estos equipos puedan proporcionar direccionamiento a los equipos IOT de
forma automática. Para lo cual deberá utilizar la siguiente información proporcionada.

| Dispositivo | Direccion IPv4                      | Subred | IP inicial | Maximo IP |
| ----------- | ----------------------------------- | ------ | ---------- | --------- |
| Wireless-1  | 11000000.10101000.00011110.00000001 | /24    | IP 100     | 50        |
| Wireless-2  | 11000000.10101000.00111100.00000001 | /24    | IP 50      | 25        |

5. Configuracion Equipos de IOT
	- Configurar el servidor de IOT, para lo cual deberá utilizar el usuario "*transformacion*" y la password "*digital*"
	- Configurar los equipos de IOT de la red del minimarket, para lo cual, debe establecer el modo de conexión correspondiente, utilizando como credenciales de acceso, "*transformacion*" como usuario y "*digital*" como contraseña.
	- La comprobación de los equipos de IOT podrá validarla una vez que haya realizado la configuración de equipos inalámbricos.

6. Configuracion Seguridad Inalambrica
	- Se ha solicitado proteger la conexión entre el router inalámbrico y los equipos de IOT, para lo cual deberá configurar los siguientes parámetros que se encuentran en la tabla de los equipos inalámbricos.
	- En los equipos de IOT, realizar las configuraciones necesarias, para que estos dispositivos puedan conectarse de manera inalámbrica al router inalámbrico más cercano.
	- Para aquellos dispositivos de IOT que no dispongan de tarjeta de red inalámbrica, deberá hacer la incorporación de la misma, en la sección "**I/O Config**" para el Adaptador de Red 2, deberá elegir el adaptador "**PT-IOT-NM-1W-AC**".
	- Comprobar la conexión inalámbrica que se forma entre los equipos IOT y router inalámbrico.

| Dispositivo | SSID  | Autenticacion | Contraseña     |
| ----------- | ----- | ------------- | -------------- |
| Wireless-1  | IOT-1 | WPA2-PSK      | transformacion |
| Wireless-2  | IOT-2 | WPA2-PSK      | transformacion |

7. Comprobacion funcionamiento Red Minimarket
	- Comprobar la conectividad de los equipos IOT. Estos equipos debido a los equipos de red que se están utilizando deberán tener conectividad completa entre los equipos IOT, hacia los servidores de la empresa, y hacia el ISP que se encuentra ubicado en Internet, que da conectividad a la red de la empresa del minimarket “Doña Juanita”
	- Comprobar que el equipo ADMINISTRADOR-IOT, pueda administrar los dispositivos de forma remota a través del navegador web.
	- Comprobar que los servidores puedan llegar al servidor web `www.transformaciondigital.cl`

