# Info
Windows es una familia de [[020 - Conceptos/020.6 - Sistemas/OS|OS]] desarrollado por [Microsoft](https://www.microsoft.com/), esta principalmente escrito en C/C++

Windows es la version enfocada en usuarios finales, diseñada para computadoras personales con aplicaciones y escritorio.

Windows Server esta diseñado para entornos de servidores. Permite implementar infraestructuras centralizadas mediante dominios, donde la autenticacion, autorizacion y politicas de usuarios y equipos se gestionan de forma centralizada a travez de servicios de AD (Active Directory)

Puedes consultar las versiones con soporte desde [EOL - Windows Server](https://endoflife.date/windows-server) 

> [!TIP] Lecturas Recomendadas
> - [Microsoft - Roles in Windows Server](https://learn.microsoft.com/en-us/windows-server/administration/server-core/server-core-roles-and-services?tabs=roles)

Un Rol es una funcion especifica que cumple el servidor dentro de una red. Cada rol habilita un conjunto de servicios relacionados que permiten al sistema desempeñar tareas concretas, como autenticacion, almacenamiento de archivos o servicios web.

AD DS (Dominio de Directorio Activo) es un servicio de directorio que centraliza la gestion de identidades, autenticacion y politicas dentro de un dominio

Dentro del dominio tienes las Unidades Organizativas (OU), que son contenedores lógicos para organizar objetos. Sirven para delegar administración y aplicar políticas específicas. No son seguridad por sí mismas, son estructura.

Los objetos son las entidades reales dentro del directorio: usuarios, computadores, impresoras, servicios, etc. Todo lo que AD maneja es un objeto.

Los grupos son colecciones de objetos (generalmente usuarios) que se usan para simplificar la asignación de permisos. En vez de dar acceso usuario por usuario, asignas permisos al grupo.

# Instalacion

> [!TIP] Lecturas Recomendadas
> - [Microsoft Docs - Compare Edition](https://learn.microsoft.com/en-us/windows-server/get-started/editions-comparison?pivots=windows-server-2025)

> [!WARNING] Sobre obtener ISO Windows
> El canal oficial, posiblemente te de una version "Evaluation", esta version esta limitada y es mas molesta, por lo que omite: https://www.microsoft.com/en-us/evalcenter
> Mas informacion en [Massgrave - Windows Evaluation Edition](https://massgrave.dev/evaluation_editions)

1. Puedes obtener una copia de ISO a traves de [massgrave](https://massgrave.dev/windows-server-links)
2. Utiliza el ISO para levantar un VM con 4vCPU, 8GB RAM y 60GB Storage
3. Selecciona "Instalar"
4. Standard y Datacenter a efectos practicos es lo mismo, es importante elegir "Desktop Experience" para tener una GUI
5. Acepta la licencia
6. Selecciona la instalacion de Windows (Avanzada)
7. Seleccionas el disco duro y le das a "Siguiente"
8. Comenzara la instalacion y se reiniciara un par de veces
9. Escribe una contraseña para el usuario "Administrator"
10. Presiona CTRL+ALT+SUPR para logear y pon la contraseña configurada
