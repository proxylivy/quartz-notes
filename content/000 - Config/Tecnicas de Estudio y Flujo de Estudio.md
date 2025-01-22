# Info
Para mi, estudiar es mas que simplemente leer y memorizar, se trata de comprender los conceptos detras de cada tema y conectar las ideas para formar una estructura de conocimiento solida y duradera. Para lograrlo, aplico tecnicas y diseño flujos de trabajo que evitan el desperdicio de esfuerzo.

> Cuando llego a saber más que el profesor en una evaluación, considero el tema dominado

## Obsidian
[Obsidian](https://obsidian.md/) es el nucleo de mi sistema de estudio, esta basada en notas locales con el formato [Markdown](https://daringfireball.net/projects/markdown/). Se configura una boveda y dentro hay notas interconectadas que gestionan y relacionan toda la informacion que proceso, el uso de texto plano permite que sean ligeros, portatiles y compatibles con cualquier editor.

> No es solo un repositorio de notas; es un reflejo dinamico de mi aprendizaje y la forma en que conecto lo que estudio

### Extensiones
Uso pocos plugins para mantener mi vault en orden, recomiendo explorar los siguientes:
- [scambier/obsidian-omnisearch](https://github.com/scambier/obsidian-omnisearch): Motor de Busqueda basado en Fuzzy Search
- [l1xnan/obsidian-better-export-pdf](https://github.com/l1xnan/obsidian-better-export-pdf): Exportación de PDF con funciones avanzadas.
- [kepano/obsidian-minimal-settings](https://github.com/kepano/obsidian-minimal-settings): Mas opciones en base al tema [kepano/obsidian-minimal](https://github.com/kepano/obsidian-minimal), ademas incluye una [Guia de uso](https://minimal.guide/home)
- [mgmeyers/obsidian-style-settings](https://github.com/mgmeyers/obsidian-style-settings): Configurar aun mas detalles en profundidad, como el tamaño de los bloques H1, H2, entre muchas otras cosas mas
- [Vinzent03/find-unlinked-files](https://github.com/Vinzent03/find-unlinked-files): Busca archivos que apuntan a ningun lado y te los recopila en una pagina para hacer debug o los eliminas de plano, ayuda a mantener un vault cohesionado
- [LBF38/obsidian-syncthing-integration](https://github.com/LBF38/obsidian-syncthing-integration): Integra [Syncthing](https://syncthing.net/) al vault para ayudarte a detectar conflictos en los documentos y solucionarlos, ignorar archivos y mas cosas desde obsidian
- [ms3056/Tokei](https://github.com/ms3056/Tokei): Un simple widget de reloj para poder ver la hora en el vault.

## Metodos
Estudiar de forma efectiva requiere un enfoque estructurado. Aquí presento los que utilizo:
### Zettelkasten
Desarrollado por Niklas Luhmann, se caracteriza por la creacion de notas "Atomicas", es decir, cada nota contiene una idea o concepto central. En lugar de agrupar mucha informacion en un solo bloque, se escribe de forma fragmentada. Luego las notas de vinculan entre ellas, esta interconexion facilita la reflexion continua, que a medida que se van añadiendo nuevas notas, se encuentran conexines que no veias antes, enriqueciendo la vision global del tema.

### Progressive Summarization
Desarrollada por Tiago Forte, propone repasar varias veces sobre la misma informacion en capas sucesivas.
En la primera capa, subrayas o destacas lo que te parezca mas importante en un texto, en la segunda, reescribes esos destacados con tus propias palabras, sintetizandolos aun mas. Finalmente, si es necesario, creas una version Ultra Condensada donde solo mantienes lo escencial absoluto del tema.

### Feynman
Es una tecnica para comprender conceptos complejos, simplificando los terminos y detectando lagunas de aprendizaje

Los pasos son:
1. Selecciona un tema que desees aprender
2. Explicalo en terminos simples a alguien real o ficticio que no tenga conocimientos previos (Como un niño)
3. Identifica las secciones que te cuestan y no puedas explicar correctamente
4. Cura y mejora la informacion a travez de investigaciones para poder reescribir secciones mas claras y concisas

### Active Recall y Repaso Espaciado
Consiste en poner a prueba tu memoria en lugar de repasar las lecturas, por ejemplo, en vez de releer tus notas, intenta responder preguntas o exponer la idea sin revisar el material primario

La mejor herramienta es [Anki](https://apps.ankiweb.net/) para hacer flashcard digitales, estas te ayudaran a aprender definiciones, Practicar conceptos tecnicos y retener datos importantes, tambien hay un plugin para Obsidian llamado [Kitschpatrol/Yanki-Obsidian](https://github.com/kitschpatrol/yanki-obsidian)

Los pasos son:
1. Crea preguntas claves del material que estas estudiando
2. Responde sin revisar tus notas, detectando la materia que no recuerdas
3. Refuerza estas areas con practicas repetidas

### Externalizacion de informacion (WIP)
Tambien me interesa sobre la extension de la mente, explicada en el paper [The Extended Mind by Andy Clark and David Chalmers](https://web-archive.southampton.ac.uk/cogprints.org/320/1/extended.html)

## Flujo de Estudio Integrado
1. Búsqueda y Captura Inicial
	- Utilizo [Firefox ESR](https://www.mozilla.org/es-ES/firefox/enterprise/) con [uBlock Origin](https://ublockorigin.com/) para navegar, encontrar fuentes (blogs, foros, wikis), y recopilar o descubrir nuevos feeds RSS.
	- Uso [FreshRSS](https://www.freshrss.org/) para suscribirme a las fuentes que me interesan y hacer seguimiento de las publicaciones (0 distracciones, visión global de nuevos artículos).
	- [Docmost](https://github.com/Docmost/docmost) me permite capturar información en clases, sobre todo cuando el profesor habla de puntos que no están en el PDF.
	- Ejemplos de fuentes:
	    - PDFs, papers académicos, libros (fisicos o digitales), blogs, videos, contenido en internet, etc.
2. Curación y Clasificación
	- Una vez encuentro material relevante, **filtro** la información:
	    - Identifico conceptos clave, ejemplos importantes y evito lo repetido.
	    - Si el contenido es muy extenso (ej. un PDF), lo reduzco a ideas centrales.
	- En [Obsidian](https://obsidian.md/), **clasifico** según categorías o notas maestras (por ejemplo, “Routers”, “VPN”, “Modelo OSI”).
3. Anotación en Obsidian
	- **Reescribo** la información con mis propias palabras: esto mejora mi comprensión y evita duplicar texto.
	- **Divido** los temas en secciones claras, usando encabezados, listas, fragmentos de código o diagramas ([Mermaid](http://mermaid.js.org/intro/)) según sea necesario.
	- **Creo enlaces internos** ([WikiLinks](https://help.obsidian.md/Linking+notes+and+files/Internal+links)) para no volver a explicar conceptos ya existentes (p. ej., si hablo de _VPN_ y ya tengo una nota de _Routers_, enlazo allí en vez de repetir la definición).
	- Las clases escritas en Docmost, se escriben a Obsidian en su nota correspondiente
4. Reescritura y Consolidación
	- Añado detalles, reviso mi redacción y verifico si todo tiene sentido en conjunto.
	- Uso un lenguaje simple para asegurar la comprensión y señalo dudas pendientes.
	- Cada vez que explico un concepto a un amigo o lo escribo en voz alta (estilo Feynman), detecto lagunas y corrijo la nota.
	- Integro preguntas de **Active Recall** en la misma nota o en secciones específicas (al estilo “Preguntas clave”).
5. Optimización de Contenido
	- Transformo PDFs o documentos extensos a Markdown para mantener el peso bajo y tener texto totalmente editable.
	- Evito guardar archivos pesados o imágenes utilizando [Slink](https://github.com/andrii-kryvoviaz/slink) para gestionar las imágenes externamente sin ocupar espacio en la bóveda.
6. Sincronización y Respaldo
	- Mantengo mi vault de Obsidian sincronizado entre dispositivos con [Syncthing](https://syncthing.net/)
	    - Configuré exclusiones para no provocar conflictos (por ejemplo, archivos ocultos `.*`).
	    - Guardo versiones incrementales de mis notas.
	- Paralelamente, **OneDrive** (u otro servicio) guarda copias de seguridad de materiales más pesados (PDFs, documentos Word, etc.).
7. Publicación
	- Las notas se suben a un repositorio de GitHub ([proxylivy/quartz-notes](https://github.com/proxylivy/quartz-notes)) y las publico siguiendo la documentacion oficial de [Quartz - Hosting](https://quartz.jzhao.xyz/hosting#cloudflare-pages) para que funcione con [Cloudflare Pages](https://pages.cloudflare.com/).
	- Así, puedo compartir mi conocimiento en línea en [notes.proxylivy.work](https://notes.proxylivy.work/).
8. Iteración Final
	- El sistema es **cíclico**: cada vez que retomo un tema (por ejemplo, para un examen o un proyecto), reviso, corrijo y amplío mis notas.
	- Aplico [[#Active Recall y Repaso Espaciado]] para generar nuevas preguntas que me ayuden a reforzar o a encontrar huecos en mi comprensión.
	- Si la información no encaja claramente o es una nota vieja, la dejo temporalmente en la carpeta `999 - Archivado` o en mi [[000 - Config/Lista To-Do|Lista To-Do]] hasta encontrarles un mejor lugar.

## Uso de IA
[ChatGPT](https://chatgpt.com/) | [Claude](https://claude.ai/) o "Inserte cualquier otro modelo LLM" son mis compañeros de estudio actualmente (Gracias [Mapachito](https://github.com/nocrop) por pagar el Plan ChatGPT Plus), me permite estudiar cuando tenga tiempo y no depender de horario o agendas externas

- Permite simular situaciones, clarificar dudas, reescribir ideas y explorar perspectivas
- No sustituyen a los profesores, compañeros, ni el estudio personal, la verficacion, reflexion y consulta de fuentes son pasos fundamentales

# Recursos Recomendados
## Recursos Feynman
- Surely You're Joking, Mr. Feynman! by Richard P. Feynman 3rd ed
	- ISBN-13: 978-0606412728
- Feynman's Tips on Physics: Reflections, Advice, Insights, Practice by Richard P. Feynman
	- ISBN-13: 978-0465027972|

### Recursos Active Recall
- "Make It Stick: The Science of Successful Learning" by Peter C. Brown, Henry L. Roediger III, and Mark A. McDaniel
	- ISBN-13: 978-0674729018
- "A Mind for Numbers: How to Excel at Math and Science (Even If You Flunked Algebra)" by Barbara Oakley
	- ISBN-13: 978-0399165245
- "Ultralearning: Master Hard Skills, Outsmart the Competition, and Accelerate Your Career" by Scott H. Young
	- ISBN-13: 978-0062852687

## Recursos Gestion de Conocimiento (PKA)
- "How to Take Smart Notes" by Sönke Ahrens 2nd edition
	- ISBN-13: 978-3982438801
- "Building a Second Brain" by Tiago Forte
	- ISBN-13: 978-1982167387
- "The Bullet Journal Method" by Ryder Carroll
	- ISBN-13: 978-0008261375

## Uso de IA
- Juan Pablo Flores - Adoptando IA en Educacion
	- [Youtube - Nerdearla Chile 2024](https://youtu.be/vU4rtXapTCg?si=XZ8JZ0zsJcdoiZGW)
- Eleanor Konik - Secondary Sources are pretty great
	- [Substack - Obsidian Iceberg](https://www.eleanorkonik.com/p/secondary-sources-are-pretty-great)