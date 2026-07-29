# Info

Para mí, estudiar es más que simplemente leer y memorizar: se trata de comprender los conceptos detrás de cada tema y conectar las ideas para formar una estructura de conocimiento sólida y duradera. Para lograrlo, aplico técnicas y diseño flujos de trabajo que evitan el desperdicio de esfuerzo. Aunque todo depende como lo enfrentes, una cosa son ordenar los estudios, y otro la vida, este me sirve mas para los estudios jajaja.
## Herramientas

**Obsidian**

[Obsidian](https://obsidian.md/) es el núcleo de mi sistema de estudio, basado en notas locales en formato [Markdown](https://daringfireball.net/projects/markdown/). Se configura una bóveda y dentro hay notas interconectadas que gestionan y relacionan toda la información que proceso. Es el lugar donde transcribo de forma atómica, separando el conocimiento en piezas pequeñas para luego reordenarlas correctamente sin repetir información, algo que para mí funciona casi a la perfección. El uso de texto plano además permite que las notas sean ligeras, portátiles y compatibles con cualquier editor.

**Plugins Comunitarios de Obsidian**

Mi experiencia con los plugins es mas vanilla, aunque puedo recomendar los siguientes
- [scambier/obsidian-omnisearch](https://github.com/scambier/obsidian-omnisearch)
	- Motor de Busqueda basado en Fuzzy Search.
- [l1xnan/obsidian-better-export-pdf](https://github.com/l1xnan/obsidian-better-export-pdf)
	- Exportación de PDF con funciones avanzadas.
- [ms3056/Tokei](https://github.com/ms3056/Tokei)
	- Un simple widget de reloj para poder ver la hora en el vault.
- [Vinzent03/find-unlinked-files](https://github.com/Vinzent03/find-unlinked-files)
	- Te permite encontrar archivos con enlaces rotos
- [LBF38/obsidian-syncthing-integration](https://github.com/LBF38/obsidian-syncthing-integration)
	- Consulta tu instancia de [Syncthing](https://syncthing.net/) para detectar conflictos de documentos y solucionarlos desde Obsidian
- [kepano/obsidian-minimal-settings](https://github.com/kepano/obsidian-minimal-settings)
	- Extra personalizacion para el tema [kepano/obsidian-minimal](https://github.com/kepano/obsidian-minimal), incluye [Guia de uso](https://minimal.guide/home)
- [mgmeyers/obsidian-style-settings](https://github.com/mgmeyers/obsidian-style-settings)
	- Aun mas personalizacion para el look de Obsidian

**Syncthing**

[Syncthing](https://syncthing.net/) permite sincronizar constantemente el contenido de mi boveda entre dispositivos en 2do plano

**Copyparty**

[Copyparty](https://github.com/9001/copyparty) es un servidor de ficheros auto-alojado, me permite tener mas control sobre los documentos y anexos que guardo

La uso de forma paralela a OneDrive (u otro servicio) para guardar copias de seguridad del contenido mas pesado (PDFs, DOCX, PPTx, Videos, etc.)

**Slink**

[Slink](https://github.com/andrii-kryvoviaz/slink) es una plataforma especificamente a imagenes, es ligera y rapida

**Quartz**

[Quartz](https://github.com/jackyzha0/quartz) es un generador de sitios estaticos y completos que transforma el contenido Markdown en sitios web funcionales. De esta forma solo te preocupas en escribir en Markdown

El metodo de funcionamiento es que toma una copia del contenido de Obsidian, lo sube al repositorio de Github en [proxylivy/quartz-notes](https://github.com/proxylivy/quartz-notes) y mediante una Github Action, despliega en [Cloudflare Pages](https://pages.cloudflare.com/) basado en la opcion [Quartz - Hosting](https://quartz.jzhao.xyz/hosting#cloudflare-pages), quedando disponible en [[index|notes.proxylivy.work]]

## Metodos de Estudio

Conozco y me inspiro de varios metodos de estudio, aunque siendo honesto, no los sigo al pie de la letra. Pero nunca esta de mas repasarlos

**Zettelkasten**

Desarrollado por Niklas Luhmann, se caracteriza por la creación de notas "atómicas", es decir, cada nota contiene una idea o concepto central en lugar de agrupar mucha información en un solo bloque. Luego las notas se vinculan entre ellas. Esta interconexión facilita la reflexión continua, a medida que se añaden nuevas notas, aparecen conexiones que no veías antes, enriqueciendo la visión global del tema.

**Feynman**

Es una técnica para comprender conceptos complejos, simplificando los términos y detectando lagunas de aprendizaje. Como no siempre tengo a alguien disponible para explicarle un tema, lo hago conmigo mismo o con un chat de IA (Vease [[#Uso de IA]]), intentando siempre explicarlo como si tuviera enfrente a alguien de 5 años (al estilo del [r/eli5](https://old.reddit.com/r/explainlikeimfive/)). Los pasos son:
1. Selecciona un tema que desees aprender.
2. Explícalo en términos simples a alguien real o ficticio que no tenga conocimientos previos (como un niño).
3. Identifica las secciones que te cuestan y no puedas explicar correctamente.
4. Cura y mejora la información a través de investigaciones para reescribir secciones más claras y concisas.

**Active Recall y Repaso Espaciado**

Consiste en poner a prueba tu memoria en lugar de repasar las lecturas, por ejemplo, en vez de releer tus notas, intentar responder preguntas o exponer la idea sin revisar el material primario.

Si quieres aplicarlo de forma más estructurada, la mejor herramienta es [Anki](https://apps.ankiweb.net/) para hacer flashcards digitales, útiles para aprender definiciones, practicar conceptos técnicos y retener datos importantes. También existe un plugin para Obsidian disponible, es [Kitschpatrol/Yanki-Obsidian](https://github.com/kitschpatrol/yanki-obsidian).

## Flujo de Estudio

1. Busqueda e Inbox
	- Utilizo [Firefox ESR](https://www.mozilla.org/es-ES/firefox/enterprise/) con [uBlock Origin](https://ublockorigin.com/) para navegar, encontrar fuentes (blogs, foros, wikis) y de paso, recopilar nuevos feeds RSS.
	- Me suscribo a los feed rss mediante [FreshRSS](https://www.freshrss.org/) para seguir fuentes y publicaciones nuevas
	- Todo debe ser capturado (PDFs, papers, blogs, apuntes de clases, etc.) y guardado en un Inbox
2. Normalizacion
	- Todo el texto que sirve se extrae y se pasa a Markdown, sin excepcion. Nada de mantener PDFs sueltos o datos dispersos, si es informacion, va a ir en texto plano
3. Curacion
	- Lo que ya no tiene sentido despues de un rato se elimina, y las ideas repetidas se dejan una sola vez
	- Los documentos que son externos por naturaleza (papers, guias con muchas imagenes, o cualquier cosa que no sea razonable pasar a texto) se dejan como anexos fuera de la boveda
4. Clasificacion
	- Ordeno la informacion por ideas y lo que queda afuera, se guarda en Copyparty
5. Reescritura y Consolidacion en Obsidian
	- Tomo la informacion ya clasificada y filtrada, la reescribo con mis propias palabras, dividiendo los temas en secciones claras mediante encabezados, listas y fragmentos de codigos, ademas de diagramas usando [Mermaid](http://mermaid.js.org/intro/)
	- Creo [WikiLinks](https://obsidian.md/help/links) para mantener la atomicidad y evitar duplicar conceptos ya existentes
		- p. ej. Si hablo de *VPN* y necesito explicar sobre *Routers*, en vez de explicar alli mismo, creo un enlazo a la nota de *Router*, evitando repetir definiciones
6. Optimizacion
	- Las imagenes (Topologias de red, configuraciones, explicaciones visuales, etc.) se gestionan con Slink y el resto de contenido se maneja mediante Copyparty
7. Iteracion Continua
	- El sistema es ciclico, cada vez que retomas un tema, tengo que revisar, corregir para encontrar huecos en mi comprensión.
	- Si la información no encaja claramente o es una nota vieja, la dejo temporalmente en la carpeta `999 - Archivado` o en mi Lista To-Do hasta encontrarle un mejor lugar.

## Uso de IA

Cualquier LLM ([ChatGPT](https://chatgpt.com/), [Claude](https://claude.ai/), etc.) puede ser un compañero de estudio, me permiten estudiar cuando tengo tiempo, sin depender de horarios, disponibilidad o agendas de otras personas.

Lo utilizo para simular situaciones, conversar sobre un tema, resolver dudas, contrastar ideas, reescribir y refinar apuntes, y explorar distintas perpectivas hasta llegar a una conclusion que tenga sentido

Sin embargo, no reemplaza a los profesores, compañeros, libros ni al estudiar por cuenta propia. Su mayor valor esta en servir como un primer filtro para comprender y detectar vacios en mi conocimiento y formular mejores preguntas cuando hablo con personas que realmente dominan un tema.

La IA tampoco puede sustituir el criterio ni el pensamiento critico. Sus respuestas deben cuestionarse, verificarse y compararse con otras fuentes. Si en algun momento no confias en lo que sabes y solo aceptas lo que dice un modelo, el problema ya no es de la IA, es de haber delegado tu capacidad de pensar. Y alli estamos muy mal :(.

# Recursos Recomendados

**Feynman**

- Surely You're Joking, Mr. Feynman! by Richard P. Feynman 3rd ed
	- ISBN-13: 978-0606412728
- Feynman's Tips on Physics: Reflections, Advice, Insights, Practice by Richard P. Feynman
	- ISBN-13: 978-0465027972

**Active Recall**

- "Make It Stick: The Science of Successful Learning" by Peter C. Brown, Henry L. Roediger III, and Mark A. McDaniel
	- ISBN-13: 978-0674729018
- "A Mind for Numbers: How to Excel at Math and Science (Even If You Flunked Algebra)" by Barbara Oakley
	- ISBN-13: 978-0399165245
- "Ultralearning: Master Hard Skills, Outsmart the Competition, and Accelerate Your Career" by Scott H. Young
	- ISBN-13: 978-0062852687


**Gestion de Conocimiento (PKM)**

- "How to Take Smart Notes" by Sönke Ahrens 2nd edition
	- ISBN-13: 978-3982438801
- "Building a Second Brain" by Tiago Forte
	- ISBN-13: 978-1982167387
- "The Bullet Journal Method" by Ryder Carroll
	- ISBN-13: 978-0008261375

**Uso de IA**

- Juan Pablo Flores - Adoptando IA en Educacion
	- [Youtube - Nerdearla Chile 2024](https://youtu.be/vU4rtXapTCg?si=XZ8JZ0zsJcdoiZGW)
- Eleanor Konik - Secondary Sources are pretty great
	- [Substack - Obsidian Iceberg](https://www.eleanorkonik.com/p/secondary-sources-are-pretty-great)


# Extra
## Externalizacion de informacion (WIP)

También me interesa la extensión de la mente, explicada en el paper [The Extended Mind by Andy Clark and David Chalmers](https://web-archive.southampton.ac.uk/cogprints.org/320/1/extended.html).