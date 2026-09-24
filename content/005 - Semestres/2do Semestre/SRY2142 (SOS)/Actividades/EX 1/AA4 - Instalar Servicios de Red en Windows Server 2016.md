# Info

Anexos disponibles en [Copyparty - EX 1 - AA4](https://copyparty.proxylivy.work/Duoc/Conectividad%20y%20Redes/2do%20Semestre/SRY2142%20(SOS)/Actividades/EX%201/AA4/)
- [Biblioteca DuocUC](https://bibliotecas.duoc.cl/inicio) (Estudiantes/Docentes/Ex-Alumnos/Colaboradores)
	- [Webezproxy - Francisco Charte Ojeda - Manual Avanzado de Windows Server 2012](http://biblioteca.duoc.cl.webezproxy.duoc.cl/bdigital/elibros/a29528-Manual%20avanzado%20de%20Windows%20Server/)
	- [Exlibris - Windows Server 2016: Preparacion para MCSA](https://duoc.primo.exlibrisgroup.com/permalink/56SBDU_INST/crslg9/alma99276672109531)

Objetivo: conocer funcionalidades de los sistemas Operativo, así como de las características generales de estos. Además, conocerá el procedimiento para trabajar con máquinas virtuales y realizar la instalación de servicios de Red dentro de Windows Server 2016.

Pre-Requisitos
- Tener un VM con la instalacion de Windows Server 2016
- Tener un VM cliente con Windows 7 o Windows 10

Contexto: La empresa donde trabaja, ACME S.A. le solicita configurar el recién Instalado Servidor Windows 2016 con ciertas características para su correcto funcionamiento, entre ellas opciones de red, AD, WEB, DHCP y FTP entre otras. 

## Actividad

Configura la red y hostname del servidor
- Hostname: DC01
- IP: 10.8.x.10
- Mascara: 255.255.255.0
- Gateway: 10.8.x.1
- DNS: 10.8.x.10

Instala AD (Active Directory)
- Usar la instalación en modo avanzado.
- Crear un dominio nuevo en un bosque nuevo
- FQDN del dominio raíz del bosque: `duoc.cl`
- Nombre de NetBios del dominio: `DUOC`
- Nivel funcional del bosque: `Windows Server 2016`
- Ubicación de la base de datos, los archivos de registro y SYSVOL mantener los valores por omisión.
- Contraseña Administrador modo restauración: `SRY2141*2019`

Crea un usuario en AD (Active Directory)
- Nombre de inicio de sesión: `duoc2`
- Contraseña: `Duoc.2019`

Instala y configura el servicio DHCP con las siguientes caracteristicas
- Nombre Ambito: ejercicio_4
- Rango IP: 10.8.x.220 - 250
- Exclusiones: No hay
- WINs: No usar WINs
- Mascara: 255.255.255.0
- Gateway: 10.8.x.1
- DNS: 10.8.x.10
- Tiempo Concesion: 6 horas

Configura el servicio DNS y agregue las zonas necesarias para los siguientes nombres de dominio.

Dentro de `duoc.cl`

| Nombre      | Tipo | IP         |
| ----------- | ---- | ---------- |
| notas       | A    | 10.8.x.150 |
| respaldos   | A    | 10.8.x.161 |
| printserver | A    | 10.8.x.172 |
| webapp01    | A    | 10.8.x.183 |
| reportes    | A    | 10.8.x.194 |

Luego agrega las siguientes zonas de busqueda directa
- `www.intra.local`
- `laboratorio.edu`

Cree los siguientes registros dentro de cada una de las nuevas zonas

Dominio -> Zona -> `www.intra.local`

| Nombre | Tipo | IP        |
| ------ | ---- | --------- |
| www    | A    | 10.8.x.10 |
| ftp    | A    | 10.8.x.10 |

Dominio -> Zona -> `laboratorio.edu`

| Nombre | Tipo | IP        |
| ------ | ---- | --------- |
| www    | A    | 10.8.x.10 |
| ftp    | A    | 10.8.x.10 |

Ve a IIS (Internet Information Service) -> Configura HTTP, con las siguientes caracteristicas:
- Cree un directorio con nombre `www.intra.local` dentro de `C:\inetpub\` el cual almacenara el sitio web asociado a este.
- Luego agregue un nuevo sitio web para el subdominio `www.intra.local`, y de esta manera poder acceder al sitio web a través del protocolo HTTP. (`http://www.intra.local`)
- Cree un directorio con nombre `www.laboratorio.edu` dentro de `C:\inetpub\` el cual almacenara el sitio web asociado a este subdominio. Luego agregue un nuevo sitio web para el subdominio `www.laboratorio.edu`. Y de esta manera poder acceder al sitio web a través del protocolo HTTP. (`http://www.laboratorio.edu`)

Ve a IIS (Internet Information Service) -> Configura FTP, con las siguientes caracteristicas:
- Agregue un nuevo sitio ftp para el subdominio “`ftp.intra.local`”. Debe permitir conectarse y acceder a `C:\inetpub\www.intra.local` y de esta forma poder administrar el contenido de sitio web asociado, de forma remota. El usuario `admin_web_intra` es el administrador de este.
- Agregue un nuevo sitio ftp para el subdominio “`ftp.laboratorio.edu`” Debe permitir conectarse y acceder a `C:\inetpub\www.laboratorio.edu` y de esta forma poder administra el contenido de sitio web asociado, de forma remota. El usuario `admin_web_laboratorio` es el administrador de este. 

## Probar funcionamiento

Inicia la maquina cliente con Windows 7 y/o 10, con las siguientes caracteristicas
- Hostname: Client01
- Debe obtener IP por el DHCP previamente configurado
- Debe conectarse al dominio `duoc.cl`, con el usuario creado (`duoc2:Duoc.2019`)
- Prueba la resolucion de nombres de los dominios creados
- Ingresa a los sitios WEB
- Ingresa a los sitios FTP