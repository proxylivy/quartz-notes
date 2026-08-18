# Info

Anexos disponibles en [Copyparty - Prueba 2](https://copyparty.proxylivy.work/Duoc/Conectividad%20y%20Redes/2do%20Semestre/VTY2112%20(CE)/Pruebas/Prueba%202/)

Objetivo: Configurar solución de networking en función al caso planteando, en donde deberás poner en práctica los conceptos teóricos y prácticos vistos durante la unidad.

**Contexto**

Empresas Doña Juanita S.A, ha sido el renombre que le ha dado su gerente General Benjamín Toledo a su empresa, esto es debido a que su rubro de minimarket y la tienda de óptica han mantenido el crecimiento de ventas sostenido y deseado durante el presente año.   

Este crecimiento ha implicado la apertura de la nueva sucursal “DONDE BENJA” la que debe tener acceso a internet y a la sucursal “DOÑA JUANITA”. Benjamín Toledo le solicita a usted realizar el diseño de direccionamiento IPv4/IPv6, configuración de router y switch, configuración de los dispositivos finales a la red según esquema de direccionamiento IP definido.

Benjamín Toledo ha generado una licitación pública para realizar la mejora de infraestructura, y esta fue adjudicada por la empresa “TODORAPIDO S.A”, debido a que su bajo costo, pero con muy poca trayectoria en el mercado. 

El router ROUTER-BORDE-2 de la sucursal “DONDE BENJA” fue configurado para tener conectividad hacia internet y a la sucursal “DOÑA JUANITA”, sin embargo, la empresa “TODORAPIDO S.A” no logró realizar de manera exitosa la conexión, Benjamín Toledo le solicita solucionar el problema que ha dejado la empresa anterior. 

Debido a la prontitud de que la conectividad y servicios estén implementados, deberá realizar las tareas solicitadas en un tiempo máximo de 180 minutos.

**Configuracion Basica de Router y Switch**

Solo configure los router ROUTER-CENTRAL1, ROUTER-CENTRAL2, SW-CAJAS, SW-BODEGA, SW-SERVIDOR y SW-VENTAS.

1. Nombre según topología.
2. Habilitar autenticación en la línea de consola con la contraseña conectividad.
3. Contraseña esencial al modo privilegiado.
4. Mensaje de consola “DONDE BENJA"
5. Agregar el dominio `duoc.cl`
6. Habilitar ssh versión 2.
7. Generar llave de 2048 bit utilizando encriptación RSA.
8. Habilitar SSH como protocolo de acceso remoto utilizando con contraseña `dondebenja`.

**Direccionamiento IPv4/IPv6**

1. Según la red 172.16.0.0/16, calcule la máscara de subred de longitud variable IPv4.

| Nombre de Red | Host | Bit | Red | Rango IP | Broadcast | Prefijo | Mascara |
| ------------- | ---- | --- | --- | -------- | --------- | ------- | ------- |
| Cajas         | 300  |     |     |          |           |         |         |
| Ventas        | 100  |     |     |          |           |         |         |
| Bodega        | 50   |     |     |          |           |         |         |
| Servidor      | 20   |     |     |          |           |         |         |
| WAN1          | 2    |     |     |          |           |         |         |
| WAN2          | 2    |     |     |          |           |         |         |
| WAN3          | 2    |     |     |          |           |         |         |

2. Según la red 2019:AAAA:BBBB::/48 , calcule las siguientes Subredes IPv6.

| Nombre Subred | Subred Decimal | Subred Hexadecimal | Subredes | Prefijo |
| ------------- | -------------- | ------------------ | -------- | ------- |
| Cajas         | 300            |                    |          |         |
| Bodega        | 670            |                    |          |         |
| Servidor      | 894            |                    |          |         |
| Ventas        | 489            |                    |          |         |
| WAN1          | 349            |                    |          |         |
| WAN2          | 199            |                    |          |         |
| WAN3          | 79             |                    |          |         |

3. Las interfaces LAN de los router deben utilizar la primera IP válida del segmento.
4. Los dispositivos finales deben utilice las direcciones IP que se muestran en la topología tanto para IPv4 e IPv6.
5. Para la SVI 1 de los SW-CAJAS, SW-BODEGA, SW-SERVIDOR y SW-VENTAS deben tener la última IP válida del segmento correspondiente, recuerda configurar la puerta de enlace predeterminada en los SW´s

**Enrutamiento Estatico para IPv4/IPv6**

1. Habilitar enrutamiento estático para IPv4, indicando la dirección ip del siguiente salto
2. Habilitar enrutamiento estático para IPv6, indicando la dirección ip del siguiente salto

**Resolucion de Problema**

1.  El router de borde de la sucursal “DONDE BENJA” ha sido configurado con rutas estática por defecto IPv4 e IPv6 por la empresa “TODORAPIDO S.A” sin embargo no logra obtener conectividad hacia internet y a la sucursal “DOÑA JUANITA”.

**Equipos Inalambricos**

1. Los dispositivos móviles que se encuentran cercanos al AP-VENTAS deben conectarse a él con las siguientes credenciales

| SSID         | Contraseña WPA2-PSK |
| ------------ | ------------------- |
| Conectividad | esencial            |

2. Una vez finalizada la configuración, acceda desde el smartphone de don Benjamín a `www.juanitaiot.cl`
	- Username `conectividad` y contraseña `esencial` para controlar los dispositivos IoT de la sucursal “DOÑA JUANITA”

