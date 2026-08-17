# Info

> [!NOTE] Capacidades de PT
> Packet Tracer sólo simula el proceso para configurar estos servicios. Cada paquete de software DHCP y DNS tiene una instalación e instrucciones de configuración exclusivas. 

Anexos disponibles en [Copyparty - EX 1 - AA4](https://copyparty.proxylivy.work/Duoc/Conectividad%20y%20Redes/2do%20Semestre/VTY2112%20(CE)/Actividades/EX%201/AA4/)
- [[010 - Protocolos/010.3 - Comunicaciones/DHCP|DHCP]]

Objetivo: Configure y verifique el direccionamiento IP y del protocolo DHCP

# Configura IPv4

1. Configura la Impresora con una direccion IPv4 estatica
	1. Haga clic en "`Inkjet`" (Inyección de tinta) y luego en la ficha "`Config`" (Configuración), que mostrará los Global Settings (Ajustes globales).
	2. Asigne la dirección estática `192.168.0.1` a la puerta de enlace y la dirección `64.100.8.8` al servidor DNS.
	3. Haga clic en "`FastEthernet0`" y asigne la dirección IP estática `192.168.0.2` y la dirección `255.255.255.0` a la máscara de subred.
	4. Cierre la ventana Inkjet.
2. Configura DHCP para WRS
	1. Haga clic en "`WRS`" y luego en la ficha GUI y maximice la ventana.
	2. Aparece la ventana Basic Setup (Configuración básica) de manera predeterminada. Configure los siguientes ajustes en la sección Network Setup (Configuración de red):
		1. Cambie la dirección IP a `192.168.0.1`.
		2. Establezca `255.255.255.0` para la máscara de subred.
		3. Habilite el servidor DHCP.
		4. Establezca `64.100.8.8` para la dirección estática DNS 1.
		5. Desplácese hasta la parte inferior y haga clic en Save (Guardar).
	3. Cierre la ventana de WRS.
3. DHCP para Laptop
	1. Haga clic en "`Home Laptop`" y luego en la ficha "`Desktop`" (Escritorio) > "`IP Configuration`" (Configuración de IP).
	2. Haga clic en `DHCP` y espere hasta que la solicitud de DHCP sea aceptada.
	3. Home Laptop ahora debería tener una configuración de IP completa. De lo contrario, vuelva al paso 2 y verifique las configuraciones en WRS.
	4. Cierre la ventana IP Configuration y luego la ventana Home Laptop.
4. DHCP para Tablet
	1. Haga clic en Tableta y luego en la ficha Desktop > IP Configuration.
	2. Haga clic en DHCP y espere hasta que la solicitud de DHCP sea aceptada.
	3. La Tableta ahora debería tener una configuración de IP completa. De lo contrario, vuelva al paso 2 y verifique las configuraciones en WRS.
5. Acceso a Sitios WEB
	1. Cierre la ventana "`IP Configuration`" y haga clic en el navegador web.
	2. En el cuadro de URL, escriba `10.10.10.2` (para el sitio web de CentralServer "Servidor central") o `64.100.200.1` (para el de BranchServer "Servidor de sucursal") y haga clic en Go (Ir). Deberían aparecer ambos sitios web.
	3. Vuelva a abrir el navegador web. Compruebe los nombres de esos sitios web ingresando `centralserver.pt.pka` y `branchserver.pt.pka`. Haga clic en Fast Forward Time (Avance rápido) en la barra amarilla debajo de la topología para acelerar el proceso.

# Registros DNS

**Configure `famous.dns.pka` con registros para CentralServer y BranchServer**

Normalmente, los registros del DNS realizan ante empresas, pero para los fines de esta actividad, usted controla el servidor `famous.dns.pka` en Internet. 

1. Haga clic en la nube de Internet. Aparecerá una nueva red.
2. Haga clic en `famous.dns.pka` y luego en la ficha Services (Servicios) > DNS.
3. Agregue los siguientes registros de recursos:


| Nombre DNS             | Direccion    |
| ---------------------- | ------------ |
| `centralserver.pt.pka` | 10.10.10.2   |
| `branchserver.pt.pka`  | 64.100.200.1 |

4. Cierre la ventana de `famous.dns.pka`.
5. Haga clic en Back (Atrás) para salir de la nube de Internet.

**Verifica DNS en dispositivos finales**

Ahora que ha configurado los registros del DNS, Home Laptop y Tableta deberían poder acceder a los sitios web usando los nombres en lugar de las direcciones IP. En primer lugar, compruebe que el cliente DNS esté funcionando correctamente y luego verifique el acceso al sitio web.

> [!NOTE] Falsos Positivos en PT
> Los primeros dos o tres pings pueden fallar mientras Packet Tracer simula los diversos procesos que deben ocurrir para conectarse satisfactoriamente a un recurso remoto. 

1. Haga clic en Home Laptop o Tableta.
2. Si el navegador web está abierto, ciérrelo y seleccione Command Prompt (Símbolo del sistema).
	- Verifique el direccionamiento IPv4 ingresando el comando ipconfig /all. Debería ver la dirección IP del servidor DNS.
3. Haga ping al servidor DNS en `64.100.8.8` para comprobar la conectividad.
4. Cierre la ventana Command Prompt y haga clic en el navegador web. Verifique que Home Laptop o Tableta puedan acceder a páginas web de CentralServer y BranchServer.