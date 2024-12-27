# Info
¡Hola! Soy Livy. Gracias por interesarte en este vault, el cual esta basado en mis notas ^^

Actualmente estoy cursando mi carrera de [Ingenieria en Conectividad y Redes](https://www.duoc.cl/carreras/ingenieria-redes-telecomunicaciones/) (Le cambiaron el nombre y retocaron la malla curricular) en DuocUC

Si tienes alguna duda, consejo o solamente quieres contactar en alguna red de [Littlelink](https://littlelink.proxylivy.work/), en los cuales estan [Github](https://github.com/proxylivy), [Linkedin](https://www.linkedin.com/in/gabo-z-montecinos), entre otros

Mi principal punto con tomar notas en [Obsidian](https://obsidian.md/) es crear un "2do cerebro", el cual es un sistema o metodo para manejar informacion para mejorar el aprendizaje, la productividad o el crecimiento, este concepto esta acuñado por [Tiago Forte](https://fortelabs.com/), el cual uso para tomar notas unicas que estan interconectadas, esto me permite un uso eficiente de la informacion evitndo la duplicacion de informacion. Tambien me interesa sobre la extension de la mente, explicada en el paper [The Extended Mind by Andy Clark and David Chalmers](https://web-archive.southampton.ac.uk/cogprints.org/320/1/extended.html)

## Carpetas
`.*`
- Archivos ocultos que contienen configuraciones de programas

`000 - Config`: Configuración de plugins para el vault
- `Plantillas`: Notas para acceder de forma rapida y evitar reescribir
- [[000 - Config/Lista To-Do|Lista To-Do]]: Nota donde recopilo dudas, proyectos pendientes, sitios de referencia y más. ¡Si quieres ayudar, mándame tu solución!

`005 - Semestres`: Trabajos y tareas organizados por semestre y por ramo

`010 - Protocolos`: Informacion sobre protocolos de red, dividida en:
- `010.1 - Routing`: Protocolos de capa 3 ([[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]]).
- `010.2 - Switching`: Protocolos de capa 2 ([[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]]).
- `010.3 - Comunicaciones`: Protocolos de capas 4-7 ([[020 - Conceptos/020.3 - Fundamentos/Modelo OSI|Modelo OSI]])
- `010.9 - Estándares` (WIP): Información de estándares como [RFC](https://www.rfc-editor.org/standards) o [IEEE IETF](https://www.ietf.org/).

`020 - Conceptos`: Conceptos Generales, Herramientas y Tecnicas ordenadas por tema
- `020.1 - Administración`: Segmentación y Gestión de redes
- `020.2 - Seguridad`: Minimizar las superficies de ataque
- `020.3 - Fundamentos`: Informacion Base para comprender como funcionan las redes
- `020.4 - Dispositivos de Red`: Diferentes Marcas y Modelos que se usan en redes

`999 - Archivado`: Notas incompletas o en proceso de clasificación, similar a [[000 - Config/Lista To-Do|Lista To-Do]].

---
## Extensiones
Uso pocos plugins para mantener mi vault en orden, recomiendo explorar los siguientes:
- [scambier/obsidian-omnisearch](https://github.com/scambier/obsidian-omnisearch): Motor de Busqueda basado en Fuzzy Search
- [l1xnan/obsidian-better-export-pdf](https://github.com/l1xnan/obsidian-better-export-pdf): Exportación de PDF con funciones avanzadas.
- [kepano/obsidian-minimal-settings](https://github.com/kepano/obsidian-minimal-settings): Mas opciones en base al tema [kepano/obsidian-minimal](https://github.com/kepano/obsidian-minimal), ademas incluye una [Guia de uso](https://minimal.guide/home)
- [mgmeyers/obsidian-style-settings](https://github.com/mgmeyers/obsidian-style-settings): Configurar aun mas detalles en profundidad, como el tamaño de los bloques H1, H2, entre muchas otras cosas mas
- [Vinzent03/find-unlinked-files](https://github.com/Vinzent03/find-unlinked-files): Busca archivos que apuntan a ningun lado y te los recopila en una pagina para hacer debug o los eliminas de plano, ayuda a mantener un vault cohesionado
- [LBF38/obsidian-syncthing-integration](https://github.com/LBF38/obsidian-syncthing-integration): Integra [Syncthing](https://syncthing.net/) al vault para ayudarte a detectar conflictos en los documentos y solucionarlos, ignorar archivos y mas cosas desde obsidian
- [ms3056/Tokei](https://github.com/ms3056/Tokei): Un simple widget de reloj para poder ver la hora en el vault.



---
## Flujo de trabajo
Se basa en varias herramientas y/o servicios que pueden ser autohosteados para Capturar, Procesar, Sincronizar y Publicar informacion, las estapas son las siguientes:
1. Utilizo [Firefox ESR](https://www.mozilla.org/es-ES/firefox/enterprise/) + [Ublock Origin](https://ublockorigin.com/) como navegador principal
	- Permite buscar Feed RSS nuevos
	- Articulos especializados
	- Autores
	- Repositorios
	- Documentacion, Wikis y Foros
2. Configuro la informacion a [Freshrss](https://www.freshrss.org/)
	- Permite hacer seguimiento de nuevas publicaciones
	- Tener un orden de investigacion
	- 0 Distracciones
3. Clasifico la informacion segun su categoria y funcionalidad en Obsidian
	- Protocolos: Segun [[020 - Conceptos/020.3 - Fundamentos/Modelo OSI|Modelo OSI]]
	- Conceptos: Fundamentos, administracion, seguridad, etc.
	- Dispositivos: Switches, routers y firewalls
	- Extraigo las imagenes a [Slink](https://github.com/andrii-kryvoviaz/slink)
	- Si la informacion no tiene un lugar claro va a [[000 - Config/Lista To-Do|Lista To-Do]] o `999 - Archivado`
4. Se conectan las notas con WikiLinks
	- Conceptos en comun para mejorar la comprension
	- Ideas que no deben ser reescritas
5. Se sincronizan entre mis dispositivos usando [Syncthing](https://syncthing.net/)
	- Nota: Hize una [charla en Youtube](https://www.youtube.com/live/bVE38L8jteM?si=6SGVhERlULggcX0s) de como usar Dominios de Cloudflare, lo puedes usar para guiarte y conectar Syncthing en cualquier parte del mundo :D
	- Excluyo paginas ocultas "`.*`" y configuraciones para evitar conflictos entre dispositivos
	- Mantengo versiones incrementales en caso de errores
6. Publicacion en Quartz
	- Las notas de Obsidian tambien se envian a Github en el repo [proxylivy/quartz-notes](https://github.com/proxylivy/quartz-notes)
	- Usando [Cloudflare Pages](https://pages.cloudflare.com/) para publicar notas en [notes.proxylivy.work](https://notes.proxylivy.work/) basado en la [guia oficial](https://quartz.jzhao.xyz/hosting#cloudflare-pages)
7. Notas Temporales
	- Cuando no tengo acceso a mi vault, utilizo [Docmost](https://docmost.com/)
	- Notas de clase, ideas espontáneas, o apuntes sin referencias claras.
8. Al regresar a casa, transfiero estas notas a Obsidian reescribiéndolas a mano
   - Elimino ideas sin sentido.
   - Vuelvo al paso 3-6

---
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

# Extra
Para crear las imagenes de topologias en red con [[020 - Conceptos/020.4 - Dispositivos de Red/IOU WEB|IOU WEB]] se puede usar Visio + "[Network Topology Icons Web for Visio](https://www.cisco.com/c/en/us/about/brand-center/network-topology-icons.html)" by Cisco.

Una vez, un profesor de redes dijo en una prueba:
> *"Le pueden pedir ayuda a Diosito, pero Diosito no sabe sobre topologías de red."*