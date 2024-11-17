# Info
Hola, soy Livy, gracias por interesarte en este vault, el cual esta basado en mis notas de [Obsidian](https://obsidian.md/) :D.
Se centra mayormente en notas sobre la carrera de [Ingenieria en Conectividad y Redes](https://www.duoc.cl/carreras/ingenieria-redes-telecomunicaciones/) (a la cual le estan cambiando el nombre) en DuocUC.

Puedes enviarme un mensaje Unicast en mi [Github](https://github.com/proxylivy) o [Linkedin](https://www.linkedin.com/in/gabo-z-montecinos) y mandare un ACK con mi mejor esfuerzo para ayudarte

Lo importante de [Obsidian](https://obsidian.md/) es crear un "2do cerebro", en otras palabras, crear un espacio donde hayan notas entrelazadas como si fueran neuronas transmitiendo solo lo importante y dejando otra info a las otras notas, esto hace un uso mas eficiente de las notas y ayuda a no duplicar informacion innecesariamente

## Carpetas
`.*`: Todos los archivos ocultos son de la configuracion de los programas, no estan hechos para modificarlos a mano a excepcion de saber lo que estas haciendo, por ejemplo "`.obsidian`" esta bloqueado de la sincronizacion de dispositivos para que cada equipo pueda configurar su entorno sin generar conflicto de archivos.

`000 - Config`: Configuracion del funcionamiento del vault en obsidian
- `Plantillas`: Notas para acceder rapidamente con `Ctrl+o` y evitar escribir cosas de mas, como por ejemplo, "Sumarizacion", estructura de laboratorios, callouts, entre otros
- [[000 - Config/Lista To-Do|Lista To-Do]]: Nota que recopila mis dudas, proyectos pendientes, dudas generales, preguntas, sitios donde extraer informacion para reescribirlas, hacer resumenes, corregir notas, entre otras cosas. Perfectamente puedes ayudarme solucionando algunos problemas que tenga y enviandome la solucion ^^.

`005 - Semestres`: Mi coleccion de trabajos y tareas de cada ramo que he pasado durante los semestres, se centra en "Actividades", "Evaluaciones" y "Casos de Estudio", toda la materia y clases entregada por los profesores es clasificada en su propia categoria repartido en el vault en carpetas con el conocimiento general y especifico.

`010 - Protocolos`: Informacion general de Protocolos, se separan en:
- `010.1 - Routing`: Informacion de protocolos que funcionan en Capa 3, usualmente pertenecen a la configuracion de routers
- `010.2 - Switching`: Informacion de protocolos que funcionan en Capa 2, usualmente pertenecen a la configuracion de routers
- `010.3 - Comunicaciones`: Informacion de protocolos que funcionan en Capa 4-7, usualmente pertenecen a la configuracion de Servidores/Dispositivos Finales
- `010.9 - Estandares`: (WIP) Informacion de estandares importantes que regulan el comportamiento de los protocolos para que funcionen correctamente, por lo general, entran estandares de la [RFC](https://www.rfc-editor.org/standards) o la [IEEE IETF](https://www.ietf.org/)

`020 - Conceptos`: Componentes generales que no aplican al [[020 - Conceptos/020.3 - Fundamentos/Modelo OSI|Modelo OSI]] perse
- `020.1 - Administracion`: Tecnicas para poder separar redes o administrarlas
- `020.2 - Seguridad`: Tecnicas o herramientas relacionadas a la seguridad de un sistema
- `020.3 - Fundamentos`: Raiz o reunion de informacion base para el funcionamiento de los dispositivos, como `mac` o `ip`
- `020.4 - Dispositivos de Red`: Dispositivos o Herramientas que se usan dentro de una red

`999 - Archivado`: Notas en un estado incompleto, en proceso de contruccion temprana, con errores o buscando un lugar donde ser clasificadas, podrian considerarse parte extendida de [[000 - Config/Lista To-Do|Lista To-Do]].

## Extensiones
Intento mantener mi vault con los menos plugins posibles, aunque son maravillosos los que uso, hacen una experiencia mas agradable

- [scambier/obsidian-omnisearch](https://github.com/scambier/obsidian-omnisearch): Motor de busqueda que simplemente funciona a travez de fuzzy search
- [l1xnan/obsidian-better-export-pdf](https://github.com/l1xnan/obsidian-better-export-pdf): Exportar pdf pero con mas funciones
- [kepano/obsidian-minimal-settings](https://github.com/kepano/obsidian-minimal-settings): Mas opciones en base al tema [kepano/obsidian-minimal](https://github.com/kepano/obsidian-minimal), ademas incluye una [Guia de uso](https://minimal.guide/home)
- [mgmeyers/obsidian-style-settings](https://github.com/mgmeyers/obsidian-style-settings): Configurar aun mas detalles en profundidad, como el tamaño de los bloques H1, H2, entre muchas otras cosas mas
- [Vinzent03/find-unlinked-files](https://github.com/Vinzent03/find-unlinked-files): Busca archivos que apuntan a ningun lado y te los recopila en una pagina para hacer debug o los eliminas de plano, ayuda a mantener un vault cohesionado
- [LBF38/obsidian-syncthing-integration](https://github.com/LBF38/obsidian-syncthing-integration): Integra [Syncthing](https://syncthing.net/) al vault para ayudarte a detectar conflictos en los documentos y solucionarlos, ignorar archivos y mas cosas desde obsidian
- [ms3056/Tokei](https://github.com/ms3056/Tokei): Un simple widget de reloj para poder ver la hora en el vault.

## Shortcut del Vault
Atajos de teclado que tengo configurado para que
Los comandos son case-sensitive, por lo que `p != P`
**Sistema**
- `Ctrl+p`: Abrir Consola de Comandos
- `Ctrl+o`: Abrir Selector de Plantillas

**[smambier/obsidian-omnisearch](https://github.com/scambier/obsidian-omnisearch)**
- `Ctrl+f`: Buscar Archivo en toda la boveda
- `Ctrl+s`: Buscar dentro de la nota

## Flujo de trabajo
Para tomar notas desde mi hogar, uso [Obsidian](https://obsidian.md/), para leerlas, estan en mi [sitio web](https://obsidian.deathgabox.work/), que esta siendo hosteado gracias a [jackyzha0/Quartz](https://github.com/jackyzha0/quartz) y ~~[shommey/dockerized-quartz](https://github.com/shommey/dockerized-quartz)~~ Puedo enviarlas a internet con Cloudflare Pages mediante la [Guia Hosting Oficial](https://quartz.jzhao.xyz/hosting#cloudflare-pages).
En caso de no estar en mi hogar, acceso a una instancia autohosteada de [Docmost](https://docmost.com/), lo que me permite anotar y colaborar con otras personas sin tener que configurar todo el vault de obsidian, la sincronizacion de archivos y configurar el servidor, ahora se puede ver a travez de la pagina web :D.
Para crear las imagenes de topologias en red con [[020 - Conceptos/020.4 - Dispositivos de Red/IOU WEB|IOU WEB]] se puede usar Visio + "[Network Topology Icons Web for Visio](https://www.cisco.com/c/en/us/about/brand-center/network-topology-icons.html)" by Cisco.

## Bibliografia

**Coleccion de Libros Redes**
[Onedrive - Bibliografia y Libros](https://duoccl0-my.sharepoint.com/:f:/g/personal/ga_zunigam_duocuc_cl/Eo1dZhto_UtMrIXkIvQ050oBQKfSUaNb63JxabRAwwf68g?e=zTIzS3)
- [CCDE 400-007, 1st ed](https://www.ciscopress.com/store/cisco-certified-design-expert-ccde-400-007-official-9780137601042) (2023)
	- DRY7112 - Diseño de Arquitectura de Red
- [CCNA 200-301 Vol 1, 2nd ed](https://www.ciscopress.com/store/ccna-200-301-official-cert-guide-volume-1-9780138229634) (2024) y [CCNA 200-301 Vol 2, 2nd ed](https://www.ciscopress.com/store/ccna-200-301-official-cert-guide-volume-2-9780138214951) (2024)
	- VTY1112 - Transformacion Digital
	- VTY2112 - Conectividad Escencial
	- ARY3112 - Routing y Switching
	- ARY4112 - Redes Escalables y WAN
- [CCNA CyberOps - CBROPS 200-201, 1st ed](https://www.ciscopress.com/store/cisco-cyberops-associate-cbrops-200-201-official-cert-9780136807834) (2020)
	- CSY4132 - Operaciones en Ciberseguridad
	- CSY5122 - Seguridad en Redes Corporativas
- [CCNA Cyber Ops - SECFND 210-250, 1st ed](https://www.ciscopress.com/store/ccna-cyber-ops-secfnd-210-250-official-cert-guide-9781587147029) (2017)
	- CSY4132 - Operaciones en Ciberseguridad
- [CCNP ENARSI 350-301, 2nd ed](https://www.ciscopress.com/store/ccnp-enterprise-advanced-routing-enarsi-300-410-official-9780138217525) (2023)
	- ARY6112 - Troubleshooting
- [CCNP CCIE - ENCOR 350-401, 2nd ed](https://www.ciscopress.com/store/ccnp-and-ccie-enterprise-core-encor-350-401-official-9780138216764) (2023)
	- ARY5112 - Routing y Switching Corporativo
- [CCNP CCIE - SCOR 350-701, 2nd ed](https://www.ciscopress.com/store/ccnp-and-ccie-security-core-scor-350-701-official-cert-9780138221263) (2023)
- [CCNP CCIE - CLCOR 350-801, 2nd ed](https://www.ciscopress.com/store/ccnp-and-ccie-collaboration-core-clcor-350-801-official-9780138200947) (2023)
	- CUY5132 - Comunicaciones Unificadas
	- CUY6142 - Telepresencia y entornos innovadores de colaboracion humana
- [CCST Networking 100-150, 1st ed](https://www.ciscopress.com/store/cisco-certified-support-technician-ccst-networking-9780138213428) (2023)
- [Cisco SD-WAN, 1st ed](https://www.ciscopress.com/store/cisco-software-defined-wide-area-networks-designing-9780136533177) (2020)
	- DRY8111 - Programacion y Redes Virtualizadas (SDN-NFV)
- [DevNet DEVASC 200-901, 1st ed](https://www.ciscopress.com/store/cisco-certified-devnet-associate-devasc-200-901-official-9780136642961) (2020)
	- DRY8111 - Programacion y Redes Virtualizadas (SDN-NFV)

**Acceso Onedrive Duoc Modo Lectura**
[Onedrive - Duoc](https://duoccl0-my.sharepoint.com/:f:/g/personal/ga_zunigam_duocuc_cl/Et3dYEWc6GpKlGCNntyIS90BeIeDJM0zPPRTCG4jdM7WZQ?e=jWwSEw)