# Info

El termino "Servidor" puede definirse desde dos perspectivas
- HW: Sistema fisico o virtual conectado a una red que ejecuta un [[020 - Conceptos/020.6 - Sistemas/OS|OS]] y proporciona uno o mas servicios a otros dispositivos
- SW: Programa que atiende solicitudes de clientes y proporciona determinada funcionalidad, normalmente mediante una red.

El termino describe un rol/funcion/nombre, no sus especificaciones ni potencia, no tiene que ser necesariamente un "`HPE DL380 G10`" sino que puede ser un PC, una laptop, un SBC o un VM, incluso un contenedor.

Ejemplo de servicios
- [[010 - Protocolos/010.3 - Comunicaciones/DHCP|DHCP]]
- [[010 - Protocolos/010.3 - Comunicaciones/DNS|DNS]]
- [[010 - Protocolos/010.3 - Comunicaciones/SSH|SSH]]
- [[010 - Protocolos/010.3 - Comunicaciones/NTP|NTP]]
- [[010 - Protocolos/010.3 - Comunicaciones/010.3.1 - AAA/Radius|Radius]] | [[010 - Protocolos/010.3 - Comunicaciones/010.3.1 - AAA/Tacacs+|Tacacs+]]

Ademas de existir segun su enfoque
- Archivos - NAS (Network Attached Storage) | SAN (Storage Area Network)
- Web
- Correo
- Base de Datos (SQL, NoSQL)
- Autenticacion
- Virtualizacion

Un servidor es un dispositivo que da un servicio
- FTP
- TFTP
- etc

Se puede utilizar [[020 - Conceptos/020.6 - Sistemas/Linux|Linux]] YEAH

Un conjunto de servidores en un espacio especifico para ello, se considera un [[020 - Conceptos/020.3 - Fundamentos/Data Center|Data Center]]