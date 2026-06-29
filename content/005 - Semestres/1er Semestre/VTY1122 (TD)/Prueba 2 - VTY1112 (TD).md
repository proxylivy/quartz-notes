
Esta prueba analiza la conexion entre dispositivos y uso de cableado, conversion IPv4 de Binario a Decimal y aplicacion de IPv6

# Info
**Topologia**
![](https://slink.proxylivy.work/image/20bd1598-9555-4aba-a8cf-994d8efa3dee.png)

**Contexto del Cliente:**
El minimarket “Doña Juanita” visto en la clase anterior, presentó una caída en sus ventas del 20% durante el 2018, El Gerente General Benjamín Toledo, ha contratado sus servicios como consultor TIC para que presente una solución tecnológica que permita revertir esta situación, logrando mejorar las ventas y experiencias del cliente.
Se propusieron soluciones basadas en conectividad e IOT, que podrían ayudar a mejorar las ventas del minimarket, en el cual se ha elegido un prototipo, el cual será implementado y puesto a prueba, según las etapas del Desing Thinking.
Por tal motivo, su labor como consultor TIC, será la implementación de dicho prototipo, el testeo de la solución y análisis global de la solución que se realizará en dependencias del cliente. Para esta actividad dispone de 120 minutos.

1. Conectorizacion de Dispositivos de Red
	- Realizar la interconexión de dispositivos de red, utilizando cableado adecuado para aquello, y realización la conexión según la información proporcionada en las siguientes tablas:

| Dispositivo    | Interfaz | Conecta a         | Interfaz a |
| -------------- | -------- | ----------------- | ---------- |
| Router Central | G0/1     | SW-Izquierda-1    | G0/1       |
| ==             | G0/2     | SWA               | G0/2       |
| ==             | G0/3     | SW-Derecha-1      | G0/1       |
| ---            |          |                   |            |
| SW-Izquierda-1 | G0/2     | SW-Izquierda-2    | G0/2       |
| SW-Derecha-1   | G0/2     | SW-Derecha-2      | G0/2       |
| ---            |          |                   |            |
| SW-Izquierda-2 | F0/3     | IOT1              | F0         |
| ==             | F0/9     | IOT2              | F0         |
| ==             | F0/12    | IOT3              | F0         |
| ==             | F0/6     | IOT4              | F0         |
| ---            |          |                   |            |
| SWA            | F0/24    | HTTP-DNS          | F0         |
| ==             | F0/16    | Servidor-IOT      | F0         |
| ==             | F0/12    | FTP               | F0         |
| ==             | F0/10    | Administrador-IOT | F0         |
| ---            |          |                   |            |
| SW-Derecha-2   | F0/10    | IOT5              | F0         |
| ==             | F0/15    | IOT6              | F0         |
| ==             | F0/20    | IOT7              | F0         |
| ==             | F0/5     | IOT8              | F0         |

2. Asignacion Direccionamiento IPv4
	- Realizar asignación de direccionamiento IPv4 a equipos finales y equipos de IOT, según la información que se proporciona a continuación:
	- Comprobar que los equipos tengan conectividad entre ellos, a nivel de IPv4.


| Dispositivo       | Direccion IPv4 (Binario)            | Mascara de Subred | Default-Gateway | DNS         |
| ----------------- | ----------------------------------- | ----------------- | --------------- | ----------- |
| IOT1              | 11000000.10101000.00001010.00001010 | /25               | .1              | IP HTTP-DNS |
| IOT2              | 11000000.10101000.00001010.01010000 | /25               | .1              | IP HTTP-DNS |
| IOT3              | 11000000.10101000.00001010.01000110 | /25               | .1              | IP HTTP-DNS |
| IOT4              | 11000000.10101000.00001010.01001101 | /25               | .1              | IP HTTP-DNS |
| IOT5              | 11000000.10101000.00011001.00000101 | /26               | .1              | IP HTTP-DNS |
| IOT6              | 11000000.10101000.00011001.00001010 | /26               | .1              | IP HTTP-DNS |
| IOT7              | 11000000.10101000.00011001.00111100 | /26               | .1              | IP HTTP-DNS |
| IOT8              | 11000000.10101000.00011001.00011110 | /26               | .1              | IP HTTP-DNS |
| HTTP-DNS          | 11000000.10101000.00110010.00010100 | /27               | .1              | IP HTTP-DNS |
| Servidor-IOT      | 11000000.10101000.00110010.00001010 | /27               | .1              | IP HTTP-DNS |
| FTP               | 11000000.10101000.00110010.00001111 | /27               | .1              | IP HTTP-DNS |
| Administrador-IOT | 11000000.10101000.00110010.00000101 | /27               | .1              | IP HTTP-DNS |

3. Asignacion Direccionamiento IPv6
	- Realizar asignación de direccionamiento IPv6 de la forma más resumida a equipos finales y equipos de IOT, según la información que se proporciona a continuación:
	- Comprobar que los equipos tengan conectividad entre ellos, a nivel de IPv6.

| Dispositivo       | Direccionamiento IPv6                    | Prefijo | Link-Local | Default-Gateway | DNS-Server    |
| ----------------- | ---------------------------------------- | ------- | ---------- | --------------- | ------------- |
| IOT1              | 2019:ACAD:ACAD:0001:1235:0500:AAAA:0000  | /64     | FE80::A    | ::1             | IPv6 HTTP-DNS |
| IOT2              | 2019:ACAD:ACAD:0001:0000:0000:5555:0888  | /64     | FE80::B    | ::1             | IPv6 HTTP-DNS |
| IOT3              | 2019:ACAD:ACAD:0001:0004:0005:0000:0066  | /64     | FE80::C    | ::1             | IPv6 HTTP-DNS |
| IOT4              | 2019:ACAD:ACAD:0001:0005:0000:0003:0002  | /64     | FE80::D    | ::1             | IPv6 HTTP-DNS |
| IOT5              | 2019:ACAD:ACAD:0009:0000:0000:0000:AAAA  | /64     | FE80::AA   | ::1             | IPv6 HTTP-DNS |
| IOT6              | 2019:ACAD:ACAD:0009:0000:5555:0004:0333  | /64     | FE80::BB   | ::1             | IPv6 HTTP-DNS |
| IOT7              | 2019:ACAD:ACAD:0009:0000:0055:0000:0033  | /64     | FE80::CC   | ::1             | IPv6 HTTP-DNS |
| IOT8              | 2019:ACAD:ACAD:0009:0000:AAAA:BBBB:CCCC  | /64     | FE80::DD   | ::1             | IPv6 HTTP-DNS |
| HTTP-DNS          | 2019:ACAD:ACAD:0003:0000:0000:0000:AAAA  | /64     | FE80::1    | ::1             | IPv6 HTTP-DNS |
| Servidor-IOT      | 2019:ACAD:ACAD:0003:ABCD:4567:0000:0001  | /64     | FE80::2    | ::1             | IPv6 HTTP-DNS |
| FTP               | 2019:ACAD:ACAD:0003:0000:0000:0000:BBBB  | /64     | FE80::3    | ::1             | IPv6 HTTP-DNS |
| Administrador-IOT | 2019:ACAD:ACAD:0003:AAAA:BBBB:CCCC:FFFFE | /64     | FE80::4    | ::1             | IPv6 HTTP-DNS |

4. Revision de Conectividad y Servicios de la empresa
	- Realizar la interconexión de los dispositivos de IOT de la red, para lo cual deberá ingresar a cada dispositivo, y en la sección de configuración correspondiente, deberá indicar el modo de conexión del servidor de IOT, deberá colocar la dirección IPv4 del servidor IOT, y el usuario de la conexión será "*transformacion*" y la password "*digital*"
	- Comprobar desde el PC "ADMINISTRADOR-IOT", que pueda manipular los equipos de IOT y observar su funcionamiento para la red del minimarket.
	- Revisar desde el PC de la red, el acceso a la página web de `www.transformaciondigital.cl` comprobando el funcionamiento de la misma.
5. Reflexion sobre prototipo y testeo
	- En documento anexo, deberá reflexionar y responder sobre el prototipo implementado y las pruebas realizadas para su funcionamiento.

