# Info

Caso de Estudio de [[005 - Semestres/7mo Semestre/DRY7112 (DAR)/DRY7112 (DAR)|DRY7112 (DAR)]]

# Apuntes
Un buen enfoque es mostrar los problemas y alertas reales de la red actual y posible futura para asustar al cliente

# Caso Estudio
Este caso de estudio analiza la infraestructura de la clínica “Los Olmos”. La clínica te ha provisto de su información actual y de sus planes.
Ustedes como diseñadores de red, se les encomienda identificar los requerimientos de la organización y datos que les permitirá dar una solución efectiva.

**Organización**:
La clínica “Los Olmos” es medianamente grande, tiene aproximadamente un staff de 300 empleados y hasta 4000 pacientes. La Clínica, está muy interesada en mejorar el campus de Layer 2.

Tú deberás tener una reunión con el cliente para tomar notas de los requerimientos.

La clínica, tiene 15 edificios más cinco clínicas remotas pequeñas. Existen dos edificios principales y un edificio auxiliar. Los dos edificios principales tienen siete pisos cada uno, con cuatro switch en cada piso.

El edificio auxiliar, llamado “Pediatría” – se encuentra conectado a los dos edificios principales; Los Switches de estos tres edificios están conectados con fibra en anillo. El edificio de “Pediatría” tiene tres pisos, con cuatro switch en cada piso.

La red es nueva para la clínica que crece agresivamente expandiéndose, junto con la sala de emergencia, debido al crecimiento de la población en general y contingencia de salud mundial.

**Situación Actual**:
La actual red usa switch baratos de múltiples vendedores, Los Switches no son administrables, y se dispone de poca información desde cada switch, sea a través de web o línea de comando.

Dentro de cada uno de los tres edificios principales se encuentra un switch principal.

Un switch por piso y desde cada piso se conecta al switch principal. Los otros switch se conectan directamente a los otros Switches del mismo piso.

Las oficinas pequeñas tienen uno o dos Switches de 24 puertas. Cada uno de estos se conecta de vuelta a uno de los Switches principal a través de fibra. Si existe un segundo switch este se conecta a través del primero.

Actualmente no hay VLAN en toda la red. No existe Switches de Layer 3. La dirección IP de red asignada es 172.16.0.0/16. Las direcciones son asignadas secuencialmente dentro de los PCs. Los miembros del staff han tenido la intención de implementar DHCP, pero no han tenido el tiempo.

La organización utiliza aplicación como el Office, más algunas herramientas médicas especializadas que corren sobre IP.

Radiología, oncología, imagenología son departamentos que adquieren nuevas herramientas, ellos necesitan agregar movimientos en tiempo real a imágenes médicas altamente detalladas, requiriendo gran cantidad de ancho banda. Todos los nuevos servidores tienen la capacidad de usar conectividad giga o EtherChannel giga.

Muchos servidores están actualmente localizados en varios armarios.

Muchos pierden el control ambiental correcto o la alimentación por interrupción. Un miembro del staff tiene que respaldar la información para cada uno de los servidores. Existen hasta 40 servidores localizados centralmente en un piso del edificio principal, llamado “sala de servidores”.

Otros 30 servidores distribuidos alrededor de los campus cercanos a sus usuarios. La sala de servidores está en el primer piso del edificio principal 1 junto con la cafetería y otras áreas que no son de redes.

La clínica debe soportar servicios a través de estaciones móviles, desplazándolas y conectándolas en el conector Ethernet o Jack de las paredes que no están funcionando muy bien.

Los enlaces WAN usan 56 kb para 3 de las clínicas remotas y conectividad vía telefónica para las otras 2.

El único router existente, usa una ruta estática, hacía internet y fue configurado por un diseñador previo.

Los miembros del staff frecuentemente reclaman por el tiempo de respuestas lentos. Existen al parecer severas fallas de la LAN especialmente en horas peak.

**Plan y requerimiento**:
La introducción de nuevas aplicaciones resultara en una carga adicional en las clínicas remotas.

La expectativa de integración y crecimiento de oficinas remotas será de aumentar la carga de los enlaces WAN.

La clínica necesita actualizar la infraestructura WAN para proveer suficiente ancho banda, entre las clínicas remotas y casa central, al mismo tiempo buscar una solución para mejorar la convergencia durante las fallas de la red.

La compañía está preocupada por el actual esquema de dirección IP que se aplica y quiere ver una solución mejor.

**Preguntas del caso de estudio**:
1. Documentar requerimiento de la clínica.
2. Documentar cualquier información que no se consideró en el caso de estudio y que consideras necesaria para el diseño. Asume que tú ya conversaste con el cliente y recopilaste la información necesaria para comenzar con el diseño. Tu no necesitas asumir que toda la información fue provista por el cliente, quizás nunca estará disponible.
3. Describe las áreas de diseño mayor, que tú necesitarías para llevar a cabo la solución. Listas estas tareas y provee un resumen de cada uno de ellos. 

**Confección del informe del caso 1**:
Para lograr confeccionar el informe de tu primer caso, es muy relevante que organiza la información que el cliente te ha proporcionado, revisa las informaciones provistas en los conceptos, especialmente en la presentación “1.1.1 – levantamiento de la red “y luego de establecer un orden de desarrollo, respetar la siguiente estructura:

# Caso Presentacion
El informe debe desarrollar los siguientes puntos
- Organizacion e Infraestructura Actual
	- Detallar la infraestructura actual ordenando los equipos actuales con su ubicacion y cantidad
- Requerimientos
	- Proponer al cliente soluciones a sus requerimientos actuales
- Diagrama de Topologia Actual
	- Realizar el diseño de la red actual sin cambiar nada y luego de acuerdo al modelo de Cisco Enterprise

Para el diseño de una red como también para la construcción de una casa, se debe tener claro los pasos y etapas que deberán realizarse con el fin de tener una estructura sólida, escalable y a prueba de fallas.
En este contexto es imprescindible tener una primera reunión con el cliente, que seguramente no tendrá las mismas experticias que las
tuyas, ya que no es un especialista y donde te entregara una serie de información que tendrás que depurar para luego organizarlas con el fin de tener claro cuál es la situación actual y como implementar un plan de mejoras que culminará con el diseño estructurado y funcional del proyecto.

Para lograr entender claramente lo que necesita el cliente, deberás primero escucharlo y recibir los antecedentes necesarios
para establecer en una primer instancia una depuración de la información, separando los puntos importantes y basándote en los modelos jerárquicos que aprendiste en las capsulas anteriores.
Es importante no dejar ningún elementos y tratar de visualizar el proyecto con toda su perspectiva, ya que es muy probable que el primer análisis sea solamente una muestra de la situación actual de la red o de su ante proyecto.

Luego de haber recibida la información por parte del cliente, estarás en condición de realizar el primer levantamiento, que deberá consistir en un listado ordenado de elementos que se encuentran actualmente instalados y disponibles, para luego confeccionar el primer diseño, junto con las informaciones recopiladas, sea buenas o problemas manifestado en la primer entrevista.
Este diseño deberá ser lo más claro posible, ya que deberás reunirte nuevamente con el cliente para explicarle y mostrarle que tu diseño representa la situación actual, junto con tus comentarios respecto de la segmentación de los detalles de la red como también se haría para la construcción de una casa

No olvides ser lo más claro posible, incluyendo obviamente el lenguaje técnico que se necesita y explicaciones detalladas para que el cliente entienda de qué manera procederás para mejorar su red y un informe ejecutivo donde incorporaras los puntos siguientes:

