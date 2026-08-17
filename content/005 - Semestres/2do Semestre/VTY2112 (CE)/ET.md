# Info

Anexos disponibles en [Copyparty - ET](https://copyparty.proxylivy.work/Duoc/Conectividad%20y%20Redes/2do%20Semestre/VTY2112%20(CE)/ET/)

**Caso**

Debido a la contingencia sanitaria, la municipalidad de “Muy Lejano” debe implementar nuevos centros de vacunación, para inocular a sus habitantes contra el Covid-19. La municipalidad ha licitado el servicio para habilitar 2 centros de vacunación.

Su equipo de trabajo se ha adjudicado esta licitación, entre sus bases se ha acordado: la implementación completa de ambos centros de vacunación, mejorar la experiencia de usuario, agregar configuraciones que garanticen la seguridad en el acceso a su infraestructura de red, además de resolver problemas de conectividad. 

Las sucursales son CENTRO_VACUNACION_1 y CENTRO_VACUNACION_2

**Requerimiento**

- Desarrollo Simulación Red
	- Proponer un prototipo de red para la municipalidad de “Muy Lejano”, donde simulará todas las mejoras que permitan a esta empresa disponer de una red escalable, tolerante a fallas y segura.
	- En la simulación de la red deberá implementar las siguientes mejoras.

Configuracion Basica de Router y Switch para ambas sucursales
1. Nombre según lo indicado en la topología (`SUCURSAL_1`; `SUCURSAL_2`; `SWA`; `SWB`; `SWC`; `SWD`)
2. Habilitar autenticación en la línea de consola con la contraseña vacunacion (con letra minúscula).
3. Contraseña MD5 esencial en el modo privilegiado (con letra minúscula).
4. Mensaje de consola “CENTRO_DE_VACUNACION" (con letra mayúscula y comillas).
5. Agregar el dominio `www.vacunacion.cl`
6. Habilitar SSH versión 2.
7. Generar llave de 2048 bit utilizando encriptación RSA.
8. Habilitar líneas VTY para el acceso por medio de SSH (0 a 4 en el caso de los router y 0 a 15 en el caso de los switch).

Direccionamiento [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv4|IPv4]] - [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv6|IPv6]]
1. Según la red 200.100.0.0/21, calcula la máscara de subred de longitud variable IPv4.


| Nombre de Red   | Hosts |
| --------------- | ----- |
| RED_A           | 1000  |
| RED_B           | 500   |
| RED_C           | 200   |
| RED_D           | 100   |
| VLAN_NATIVA_ADM | 50    |

2. Según la red 2021:ACAD:BBBB::/48 , calcula las siguientes Subredes IPv6.


| Nombre Subred | Subred Hexadecimal |
| ------------- | ------------------ |
| RED_A         | A                  |
| RED_B         | B                  |
| RED_C         | C                  |
| RED_D         | D                  |

3. En los router SUCURSAL_1 y SUCURSAL_2 debe configurar las interfaces Giga Ethernet con la primera IP valida del segmento (IPv4 e IPv6)
4. Los dispositivos Finales (PCs, Servidores y Equipos IoT), deben configurarse con el direccionamiento IPv4 e IPv6 que se muestran en la topologia

Implemmentacion de Servicios

1. En la sucursal CENTRO_VACUNACION_1, implemente servicio de DHCPv4, para que asigne dirección IPv4 dinámica a los Equipos IOT, para ello considere 


| Servidor DHCP        | Parametros    |
| -------------------- | ------------- |
| Nombre del Pool      | serverPool    |
| Puerta de Enlace     | 200.100.4.1   |
| DNS Server           | 205.0.0.200   |
| Primera IP Asignable | 200.100.4.10  |
| Mascara de Subred    | 255.255.254.0 |

2. En la sucursal CENTRO_VACUNACION_2, implemente servicio FTP, para ello considere


| SERVIDOR FTP | Parametros |
| ------------ | ---------- |
| Usuario      | vacunacion |
| Clave        | vacunacion |
| Permisos     | Full       |

**Segmentacion de Capa Dos**

1. En los switch SWA, SWB, SWC y SWD configure segmentacion de capa dos segun el siguiente requerimiento


| VLAN | Nombre     |
| ---- | ---------- |
| 99   | NATIVA_ADM |

2. Asigna la VLAN 99 como nativa y administrativa.
3. La interface VLAN 99 se debe configurar en los switch SWA, SWB, SWC y SWD, de acuerdo al siguiente detalle:


| Nombre Switch | Direccion IPv4 |
| ------------- | -------------- |
| SWA           | 2°IP           |
| SWB           | 3°IP           |
| SWC           | 4°IP           |
| SWD           | 5°IP           |

4. En Los Switch debe configurar la puerta de enlace (ip default- gateway) con la primera IP de la VLAN 99.

**Enrutamiento Estatico**

1. En Router SUCURSAL_2 debe habilitar ruta estática por defecto a nivel de IPv4 e Ipv6, con ip del salto siguiente (next-hop), para conectar con redes del Router SUCURSAL_1, correspondiente a la sucursal CENTRO_VACUNACIÓN_1. 
2. En Router SUCURSAL_1 debe habilitar rutas estáticas estándar a nivel de IPv4 e Ipv6, con ip del salto siguiente (next-hop), para conectar con las redes del Router SUCURSAL_2, del CENTRO_VACUNACIÓN_2.

**Resolucion de Problema**

1. El Router SUCURSAL_1 de la sucursal CENTRO_VACUNACION_1 no tiene conectividad hacia Internet, revise la tabla de enrutamiento y corrija las rutas estáticas (IPv4 e IPv6), para lograr conectividad completa hacia la internet.
2. Todos los equipos finales han perdido conectividad hacia la página www.vacunacion.cl (que se encuentra en el clúster INTERNET), resuelva esta falla permitiendo que los equipos finales vuelvan a tener conectividad al sitio web.