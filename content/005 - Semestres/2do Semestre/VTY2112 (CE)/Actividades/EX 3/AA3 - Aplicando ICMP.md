# Info

Anexos disponibles en [Copyparty - EX3 - AA3](https://copyparty.proxylivy.work/Duoc/Conectividad%20y%20Redes/2do%20Semestre/VTY2112%20(CE)/Actividades/EX%203/AA3/)

Objetivo: Solucione problemas de conectividad, si es posible. Además, deberá documentar claramente los problemas, para que puedan resolverlos

Contexto: En la empresa CASE S.A los usuarios están señalando que no pueden acceder al servidor Web, `www.cisco.pka` después de una actualización reciente que incluyó agregar un segundo servidor DNS. 

Debe determinar la causa e intentar resolver los problemas para los usuarios. Documente claramente los problemas y cualquier solución. Usted no tiene acceso a los dispositivos en la nube o al servidor `www.cisco.pka`. Escalar el problema si es necesario.

> [!NOTE] Conectividad
> Al router R1, se puede acceder solamente usando SSH con el nombre de usuario Admin01 y la contraseña cisco12345. El router R2 está en la nube ISP y usted no puede acceder a él.

*Tabla de Direcciones*

| Dispositivo | Interfaz     | Direccion IP    | Mascara de subred | Gateway       |
| ----------- | ------------ | --------------- | ----------------- | ------------- |
| R1          | G0/0         | 172.16.1.1      | 255.255.255.0     | N/A           |
|             | G0/1         | 172.16.2.1      | 255.255.255.0     | N/A           |
|             | S0/0/0       | 209.165.200.226 | 225.255.255.252   | N/A           |
| R2          | G0/0         | 209.165.201.1   | 255.255.255.224   | N/A           |
|             | S0/0/0 (DCE) | 209.165.200.225 | 255.255.255.252   | N/A           |
| PC-01       | N/A          | 172.16.1.3      | 255.255.255.0     | 172.16.1.1    |
| PC-02       | N/A          | 172.16.1.4      | 255.255.255.0     | 172.16.1.1    |
| PC-A        | N/A          | 172.16.2.3      | 255.255.255.0     | 172.16.2.1    |
| PC-B        | N/A          | 172.16.2.4      | 255.255.255.0     | 172.16.2.1    |
| Web         | N/A          | 209.165.201.2   | 255.255.255.224   | 209.165.201.1 |
| DNS1        | N/A          | 209.165.201.3   | 255.255.255.224   | 209.165.201.1 |
| DNS2        | N/A          | 209.165.201.4   | 255.255.255.224   | 209.165.201.1 |

**Problemas de conectividad en PC-1**

1. En PC-01, abra el símbolo del sistema. Ingrese el comando ipconfig para verificar qué dirección IP y gateway predeterminado se han asignado al PC-01. Corrija según sea necesario según la tabla de direcciones.
2. Después de verificar/corregir los problemas de direccionamiento IP en pc-01, publique los ping al default gateway, al servidor Web, y otros PC. ¿Fueron los pings acertados? Registre los resultados.
	1. 1. ¿Ping al default gateway (172.16.1.1)?
	2. ¿Al servidor web (209.165.201.2)?
	3. Ping a PC-02?
	4. A PC-A?
	5. A PC-B?
3. Utilice el navegador web para acceder al servidor web en PC-01. Acceda al servidor web introduciendo primero la dirección URL `http://www.cisco.pka` y, a continuación, utilizando la dirección IP 209.165.201.2. Registre los resultados.
	1. ¿Puede PC-01 acceder a `www.cisco.pka`?
	2. ¿Usando la dirección IP del servidor web?

**Problemas de conectividad en PC-2**

1. En PC-02, abra el símbolo del sistema. Ingrese el comando ipconfig  para verificar la configuración para la dirección IP y el default gateway. Corrija según sea necesario.
2. Después de verificar/corregir los problemas de direccionamiento IP en pc-02, publique los ping al default gateway, al servidor Web, y otros PC. ¿Fueron los pings acertados? Registre los resultados.
	1. ¿Ping al default gateway (172.16.1.1)?
	2. ¿Al servidor web (209.165.201.2)?
	3. ¿Ping a PC-01?
	4. ¿Al PC-A?
	5. ¿Al PC-B?
3. Navegue a `www.cisco.pka` usando el buscador Web en el PC-02. Registre los resultados.
	1. ¿Puede PC-02 acceder a `www.cisco.pka`?
	2. ¿Usando la dirección IP del servidor web?

**Problemas de conectividad en PC-A**

1. En el PC-A, abra el símbolo del sistema. Ingrese el comando ipconfig  para verificar la configuración para la dirección IP y el default gateway. Corrija según sea necesario.
2. Después de corregir los problemas de direccionamiento IP en el PC-A, publique los ping al servidor Web, al gateway predeterminado, y a otros PC. ¿Los pings fueron acertados? Registre los resultados.
	1. ¿Al servidor web (209.165.201.2)?
	2. ¿Ping al default gateway (172.16.2.1)?
	3. ¿Ping a PC-B?
	4. ¿A PC-01?
	5. ¿A PC-02?
	6. ¿Puede PC-A acceder a `www.cisco.pka`?
	7. ¿Usando la dirección IP del servidor web?

**Problemas de conectividad de PC-B**

1. En PC-B, abra el símbolo del sistema. Ingrese el comando ipconfig  de verificar la configuración para la dirección IP y el default gateway. Corrija según sea necesario.
2. Después de corregir los problemas de direccionamiento IP en el PC-B, publique los ping al servidor Web, al gateway predeterminado, y a otros PC. ¿Los pings fueron acertados? Registre los resultados.
	1. ¿Al servidor web (209.165.201.2)?
	2. ¿Ping al default gateway (172.16.2.1)?
	3. ¿Ping a PC-A?
	4. ¿A PC-01?
	5. ¿A PC-02?
3. Navegue a `www.cisco.pka` usando el buscador Web. Registre los resultados.
	1. ¿Puede PC-B acceder a `www.cisco.pka`?
	2. ¿Usando la dirección IP del servidor web?
	3. ¿Podrían resolverse todos los problemas en PC-B y seguir utilizando DNS2? Si no, ¿qué tendrías que hacer?

**Verifica la conectividad**

Verifique que todos los PC puedan acceder al servidor Web `www.cisco.pka`.