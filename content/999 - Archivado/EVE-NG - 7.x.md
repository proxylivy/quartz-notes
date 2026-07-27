
No me gusto, aun asi me quedo en el viejo [[800 - Extras/Write-Ups/EVE-NG/EVE-NG - Install|EVE-NG - Install]] basado en 6.x...

Lo que estoy escribiendo, se baso en: `7.0.1-21` (3/Julio/2026)

https://www.eve-ng.net/index.php/download/

A partir de la rama 7.x, EVE-NG abandono la separacion tradicional entre Community Edition (CE) y Profesional Edition (PE). En su lugar, ahora solo existe la edicion PE (No se si le cambiaran el nombre), donde el software base es el mismo para todos los usuarios y las funciones profesionales se habilitan con la licencia de toda la vida

Creo que el peor cambio, fue limitar aun mas la cantidad de nodos, de 63 (Community Edition) a 7 (Freemium).

Ahora el ISO es de 10GB...

Debes descargarlo, instalarlo y actualizarlo

En caso de que te lo preguntes, la licencia cuesta 200USD/año, no es el fin del mundo, pero no es aplicable a todo el mundo, sobre todo a estudiantes pobres jajaa

---

DESDE AQUI EMPIEZA LA INSTALACION DE EVE-NG 7.x

Esta basado en Ubuntu 24.04.4 LTS, que es mucho mejor que 22.04 jijiji

1. Al iniciar el VM, entra grub y se selecciona automaticamente "`Install EVE-NG Pro Server 7.0.1-21`"
2. Cargara el Cloud-init y sus comandos y te dara la bienvenida al instalador
3. Selecciona el idioma "`Español`"
4. Selecciona el teclado layout y Variant a `Spanish (Latin America)`
5. Selecciona "`Continuar`" para instalar, borrara todos los discos que tenga la maquina y los convertira en un LVM, se demora unos 10 minutos y se reiniciara automaticamente
6. Iniciara el segundo stage de instalacion, el cual tiene una TUI en vez de ser solo log, interesante y bonito, se demora unos 38 minutos (2300 segundos), luego se reinicia automaticamente
7. Inicia el segundo stage de instalacion, debes iniciar sesion con el usuario "`root`" y la contraseña "`eve`" y los siguientes valores
	- Hostname: `eve-ng`
	- Domain: `example.com`
	- Use DHCP: `X`
	- NTP (optional): Ignoralo
	- Root Password: `eve`
	- Confirm Root Password: `eve`
8. Selecciona "`Apply`" y se reiniciara automaticametne

Ahora tendras instalado EVE-NG 7.x, yeah

Accedes a travez de la IP de pnet0 en el navegador (`ip -br a`)
- User: `admin`
- Pass: `eve`
- Consola: `Native Console`