# Info

Anexos disponibles en [Copyparty - ET](https://copyparty.proxylivy.work/Duoc/Conectividad%20y%20Redes/2do%20Semestre/SRY2142%20(SOS)/ET/)
- [Profesionalreview - Instalar Active Directory en Windows Server 2016](https://www.profesionalreview.com/2018/12/17/active-directory-windows-server-2016/)


Materiales
- Maquina Virtual
- ISO RedHat/Centos | Recomiendo [Rocky Linux](https://rockylinux.org/)
- ISO Windows Server 2016 | Recomiendo 2022 desde [Massgrave](https://massgrave.dev/windows-server-links)
- Acceso a Internet

Topologia Visual a utilizar
```
                      WebServer AWS
                          │
                      Internet
                          │
                   LAN 10.20.1.0/24
                          │
                  ┌───────────────┐
                  │     Switch    │
                  └───────┬───────┘
                 /        │        \
                /         │         \
               /          │          \
              /           │           \       
        Cliente 1       Linux       Windows Server
      10.20.1.100/24  10.20.1.1/24   10.20.1.2/24
```

**Contexto del Caso**

La empresa KameHouse desea implementar servidores para brindar una amplia gama de opciones para sus clientes.

Es por esta razón que la empresa le ha solicitado la implementación de servidores en diferentes plataformas, con la idea de mostrar a los clientes diversas opciones y estos puedan decidir cuál es la opción que más se acomoda a sus necesidades. 

Como requisito, la empresa le solicita la implementación de servidores en las plataformas Windows Server, Linux y también la administración de servicios en la nube, utilizando como proveedor para esto a la empresa AWS (Amazon Web Service). Esto, con el fin de poder ofrecer a los clientes una amplia gama de posibilidades. 

En cuanto al servidor Windows, este se debe realizar sobre la versión Server 2016, y en caso de Linux, puede utilizar RedHat 8 o CentOS 8. La misma situación para los servidores en AWS, el cliente podría solicitar cualquier versión del S.O.

**Requisitos**

1. Instalación de una VM (Virtual Machine)
	- OS: Windows Server 2016 (Estandar)
	- RAM: 2GB Ram
	- HDD: 60GB
	- Nombre VM: “AD_NOMBRE” (Remplaza "NOMBRE" por su propio nombre)
2. Configuración de Almacenamiento
	- Agregue dos discos duros de 5GB y cree un volumen de arreglo 0, asignando la letra de unidad “E”
	- Agregue dos discos duros de 5GB y cree un volumen de arreglo 1, asignando la letra de unidad “F”
	- Agregue 5 discos de 5GB y cree un volumen de arreglo 5, asignando la letra de unidad “G”
3. Configure el servicio de Active Directory (AD DS)
	- Instale el Rol de Active Directory
	- Promueva el dominio “KameHouse.cl”
	- Cree las unidades organizativas “Shen” y “Long”
	- Cree los usuarios “Krillin” y “Roshi” dentro de la unidad organizativa "Shen"
	- Cree los usuarios “Yamcha” y “Puar” dentro de la unidad organizativa "Long"
4. Habilitación de Sitio web
	- Instale el rol de IIS
	- Agregue FTP al rol de IIS
	- Habilite un sitio web que muestre los siguientes datos
		- Nombre de los integrantes, Asignatura y Sección.
5. Acceso desde equipo cliente
	- Utilizando un equipo cliente (Windows 7 o 10) acceda al dominio utilizando alguno de los usuarios creados
	- Acceda al sitio web y ftp comprobando el correcto funcionamiento de estos
6. Instalación de una VM (Virtual Machine) Utilizando el sistema Operativo RedHat Enterprise Linux 8/ CentOS 8
	- RAM: 2GB
	- HDD: 40GB
	- Nombre VM: “RHEL/CENTOS KameHouse APELLIDO” (Remplaza APELLIDO por tu apellido)
	- Instale el S.O Servidor RHEL 8 / CentOS 8
7. Habilitación de Sitio web
	- Habilite el servicio Apache
	- Habilite el servicio FTP configurando correctamente el enjaulamiento
	- Cree un usuario FTP llamado “Goku”, para administrar el contenido del sitio web
	- Habilite un sitio web que muestre la siguiente información
		- Nombre de los integrantes, Asignatura y Sección
8. Acceso desde equipo cliente
	- Utilizando un equipo cliente, acceda por medio de FTP utilizando el usuario creado anteriormente, validando la visualización de los archivos correspondientes al sitio web
	- Desde un navegador web, visualice el contenido del sitio web creado en el servidor
9. Habilitación de Servidor en la nube con AWS
	- Utilizando la capa gratuita en AWS, cree una instancia utilizando el sistema operativo RHEL
	- Genere las llaves de acceso remoto para administrar su instancia de RHEL desde putty
	- Acceda al servidor creado en AWS desde un terminal remoto en su equipo (Putty)
	- Configure un servidor web en la instancia creada anteriormente
	- Habilite una página web dentro de la instancia AWS
	- Desde un equipo cliente, acceda al sitio web creado para confirmar que este funcione correctamente

