# Info
Es un filamento de vidrio de alta pureza con un diametro muy fino. Su funcion principal es transportar luz entre dos extremos, permitiendo:
- Altas velocidades de transmision (Tb/s)
- Baja Atenuacion a lo largo de grandes distancias
- Inmunidad a interferencias electromagneticas
## Composicion
1. Nucleo (Core)
	- Principal elemento de propagacion de la luz
	- Fabricado con *dioxido de silicio* ($SiO_{2}$) al que se añaden dopantes para ajustar el indice de refraccion ($n_{1}$)
2. Revestimiento (Cladding)
	- Tambien de $SiO_{2}$ pero con un indice de refraccion ($n_{2}$) menor que el nucleo, para permitir una **reflexion interna total**
3. Recubrimiento (Coating)
	- Capa protectora frente a polvo, humedad y otros contaminantes
	- Suele ser de acrilato u otro polimero especial

## Longitudes de Onda
La luz en fibra optica se transmite habitualmente en el rango de 660nm a 1675nm, dividido en "Bandas" segun recomendaciones de la [ITU-T](https://www.itu.int/en/ITU-T/publications/Pages/latest.aspx) e [Incab](https://incab.co/useful-information/characteristics-optical-fiber/). 

Ventanas de Transmision Clasicas
- Primera Ventana
	- 800nm a 900nm (principalmente 850nm)
- Segunda Ventana
	- 1250nm a 1350nm (principalmente 1310nm)
- Tercera Ventana
	- 1500nm a 1600nm (principalmente 1550nm)

 Bandas de Transmision Optica
- Banda O (Original): 1260nm - 1360nm
- Banda E (Extended): 1360nm - 1460nm
- Banda S (Short): 1460nm - 1530nm
- Banda C (Conventional): 1530nm - 1565nm
- Banda L (Long): 1565nm - 1625nm
- Banda U (Ultra-Long): 1625nm - 1675nm

## Multiplexacion
La DWDM (Dense Wavelenght Division Multiplexing) en español "Multiplexacion por Division de Longitud de Onda" permite combinar hasta 160 longitudes de onda dentro de una misma fibra. Cada canal (longitud de onda) transporta datos independientes, multiplicando la capacidad total sin aumentar el numero de fibras fisicas.

## Ventajas
- Baja Atenuacion
- Gran Ancho de Banda
- Diametro Reducido / Poco Peso
- Inmune a Interferencias
- Largas Distancias
- Facil Mantenimiento luego de ser instalada

## Desventajas
- No tranporta corriente electrica (Por ahora)
- Materia Prima y dopantes muy puros: Fabricacion Especializada
- Manipulacion Fragil: Requiere personal calificado
- Herramientas y equipos costosos (Fusionadoras, OTDR, Cortadoras, etc.)
- Alto costo de Implementacion
- Necesidad de conversion Optico-Electrico en los extremos (Transceptores)

## Comportamiento
La **Reflexion interna total** permite confinar la luz en el nucleo. Se produce cuando el angulo de incidencia es mayor que el **Angulo Critico** ($\alpha_{c}$), lo que evita que el rayo "escape" al revestimiento

En el vacio la velocidad de la luz es aproximadamente $3 \times 10^{8}\text{m/s}$, cuando se desplaza por un medio (Agua, Vidrio, etc.) su velocidad se reduce

### Indice de Refraccion
Se representa como $n$ y relaciona la velocidad de la luz en el vacio ($C$) con su velocidad de algun medio ($V_{p}$) $$n=\dfrac{C}{V_{p}}$$
Donde:
- $C$ es la velocidad de la luz en el vacio
- $V_p$ es la velocidad de la luz en el medio
- $n$ es el indice de refraccion

Ejemplos de indices de refraccion
- $\text{Aire} \approx 1,0003$
- $\text{Agua} \approx 1,33$
- $\text{Vidrio} \approx 1,6$
- $\text{Diamante} \approx 2,417$

### Ley de Snell
Al cambiar de un medio con indice $n_1$ a otro de indice $n_2$: $$n_1 \sin(\theta_{1})=n_2\sin(\theta_{2})$$
- $\theta_{1}$: Angulo de incidencia
- $\theta_{2}$: Angulo de refraccion

Cuando $\theta_{2}$ es exactamente $90^\circ$, se define el **Angulo critico** ($\alpha_{c}$)

### Apertura Numerica (NA)
El **Angulo de aceptacion** ($\phi_{NA}$) es el maximo angulo (medido respecto al eje de la fibra) que permite que los rayos se confinen en el nucleo. La **Apertura Numerica** (NA) se expresa de varias formas equivalentes:
1. $NA=\sqrt{n1^{2}-n2^{2}}$
2. $NA=\sin(\phi_{NA})$
3. $NA=\sin(\alpha_{c})$

### Ejemplo de Calculo
Si $n_{1}=1,45$ y $n_{2}=1,35$: $$NA=\sqrt{(1,45)^{2}-(1,35)^{2}}\approx0,529$$
El angulo de aceptacion: $$\phi_{NA}=\arcsin{(0,529)}\approx31,94^\circ$$
En este ejemplo, las ondas con un grado menor a $31,94^\circ$ (medido respecto al eje de la fibra) quedara confinado dentro del nucleo por reflexiones internas totales. 

## Fabricacion
Se elabora a partir de Dioxido de silicio ($SiO_{2}$) con dopantes como Fluor ($F$), Boro ($B$), Germanio ($Ge$) y Fosforo ($P$) para ajustar el indice de refraccion, El proceso general:

1. Barra Preformada
	- Se inyectan vapores de dopantes y se calientan con llama de hidrogeno a ~2000°C
	- Los dopantes se condensan y forman el "Nucleo" con las propiedades deseadas
2. Estirado en la Torre
	- La pregorma se introduce en un horno de alta temperatura (~2000°C) y se estira de ella para obtener la fibra del diametro requerido ($125\micro m$ con nucleo y revestimiento)
	- Se controla mediante sistemas automatizados y computarizados

Los metodos mas habituales:
- MCDV (AT&T), VAD (Japon), PCVD (Philips)
- [OVD](https://www.corning.com/optical-communications/emea/en/home/products/fiber/manufacturing-excellence.html) (Outside Vapor Deposition, [Corning](https://www.corning.com/optical-communications/emea/en/home.html))

## Tipos de Fibra
1. Monomodo (SM)
	- Nucleo Estrecho (Tipicamente $9\micro{m}$)
	- Permite propagar una Longitud de Onda por fibra
	- Ideal para largas distancias (50Km entre repetidores)
	- Ejemplo de nomenclatura: SM 9/125 (Nucleo/Revestimiento)
2. Multimodo (SM)
	- Nucleo mas ancho (ej. 50 o 62,5 $\micro{m}$)
	- Permite propagar varias Longitudes de Onda por fibra
	- Adecuado para distancias mas cortas (Hasta 2Km)
	- Ejemplo de nomenclatura:
		- MM 62,5/125 (OM1)
		- MM 50/125 (OM2,OM3,OM4)

Los emisores son 2
- LED o VSCEL (Vertical Cavity Surface Emitting Laser) para fibras Multimodo (MM)
- Diodo Laser para fibras monomodo (SM)

Los receptores son 2, ambos convierten fotones en corriente electrica mediante fotodeteccion
- PIN: Barato
- APD (Avalanche Photodiode): Mas Caro 

El cableado se distingue entre:
1. Tubo Suelto (Loose Tube)
	- Para instalaciones en planta externa: Ductos, Tendidos Aereos, Soterrados, etc.
	- Nucleos en tubos semirrigidos, a menudo con elementos de refuerzo
2. Estructura Ajustada (Tight Buffer)
	- Para instalaciones en planta interna: Conductos Verticales, salas de equipos, cableado estructurado de edificios.
	- Capa de proteccion pegada al revestimiento para mayor flexibilidad

La norma [ANSI/TIA/EIA 598-D (Autodescarga)](https://incab.co/files/tia-598-d.pdf) (o equivalentes) regula los colores de los hilos en el interior del cable, facilitando la identificacion y la gestion de varios hilos/fibras, tambien hay [explicaciones](https://www.daenotes.com/electronics/communication-system/EIA-598-A-Standard) al respecto

# Instalacion
## Materiales
La instalacion y el mantenimiento de fibra optica requieren un conjunto de herramientas especializadas. En general, podemos agruparlas en tres grandes categorias segun la fase del trabajo: 
- *Preparacion de la Fibra*, se remueven cubiertas (Chaquetas), Buffers y protecciones exteriores hasta dejar la fibra lista para unir o conectar.
- *Union de la Fibra*, Una vez que la fibra esta pelada y preparada, se procede a su empalme. Estas herramientas aseguran cortes limpios y alineaciones para un correcto empalme. 
- *Limpieza de la Fibra*, para garantizar la baja atenuacion y evitar perdidas por suciedad. La limpieza aplica tanto a las puntas de los conectores como a la fibra antes de empalmarla

1. Preparacion de la Fibra
	- Deschaquetadora de cable (Cable Slitter)
		- Diseñada para cortar con precision la cubierta de cables con diametros de hasta 44,5mm
	- Cortador de minitubos
		- Permite seccionar minitubos o microductos de hasta 3,17mm
	- Pelador de Fibra Optica (Stripper) de 3 medidas
		- Quita la Chaqueta "Tight Buffer" de 3mm
		- Pela la cubierta de 250$\micro{m}$
		- Retira el buffer de 900$\micro{m}$
		- Retira el acrilato, dejando la fibra desnuda
	- Tijeras de Kevlar
		- Cortan la aramida (Kevlar) que refuerza el cable
	- Deschaquetadora para fibras Tight Buffer
		- Preparada para diametros de 500$\micro{m}$
2. Union de la Fibra
	- Cortadora de presicion de 3 pasos
		- Indicada para empalmes mecanicos o para conectar fibras a conectores pre-pulidos
	- Cortadora de precision (Fiber Cleaver)
		- Apta para fibras de 250$\micro{m}$ y 900$\micro{m}$
		- Permite cortes ajustable entre 5mm y 20mm
	- Microcospio de 200x
		- Adaptador Universal (ST, LC y SC), apto para fibras MM y SM
		- Permite enfocar manualmente para detecta suciedad, grietas o roturas en la superficie de contacto
3. Limpieza de la Fibra
	- Toallitas de limpieza de fibra
		- Alta Absorcion, no deja residuos
	- Alcohol Isopropilico
		- Disuelve y remueve impurezas sin dejar rastro
	- Limpiador de conectores (Tipo Cinta)
		- Retira particulas y suciedad del ferulo del conector
	- Limpiador One Click
		- Un sistema rapido y efectivo que limpia el extremo del conector con un solo movimiento
4. Medicion y Certificacion
	- OTDR (Optical Time Domain Reflectometer)
		- Utiliza Tecnica de Retrodispersion, dibuja su atenuacion a lo largo de todo el enlace
	- OLTS (Optical Loss Test Set)
		- Medir la perdida total en el cable
	- Localizador Visual de Fallas (VFL)
		- Emite un Laser de luz Visible Roja clase 2 a 650mm para detectar roturas y fallas hasta 5km de distancia
		- Conexion Universal ST, FC y SC
	- Medidor de Redes PON
		- Para redes FTTH (GPON, GEPON)
		- Debe transportar la señal triple play en tres ventanas (1301nm, 1490nm, 1550nm)

Un empalme es la union permamente de dos extremos de una fibra para transmitir luz, ambos extremos se someten a una temperatura tan alta como para fundir sus extremos y unirlos y genera atenuacion dependiendo de la calidad del empalme.

Existen 2 tipos de empalme
- Fusion: Se genera un arco electrico por una fuente de 4000 y 5000 volts mediante una Fusionadora y realiza el alineamiento de las fibras, las fusiona, calcula las perdidas y no se demora mas de 10 segundos, luego tienes que proteger las fibras fusionadas con un manguito termocontraible de 40mm o de 60mm
- Mecanico: Se empalman en un contenedor relleno con gel igualador de indice de refraccion, son conexiones provisorias y que solo se usan en situaciones de emergencia no permanentes

## Fusion de Fibra
Para empalmar un cable de [[020 - Conceptos/020.5 - Red HFC/Fibra Optica|Fibra Optica]] se siguen los siguientes pasos
1. Desenchaquetar el cable de Fibra Optica
	- Usa un Stripper calibrado a 125$\micro{m}$, retira entre 3 y 4 centimetros de Coating, dejando la fibra desnuda
2. Retirar el recubrimiento primario de la fibra optica
3. Retira todos los elementos de proteccion axial
4. Limpieza total de las fibras
	- Usa una toallita suave con alcohol isopropilico
5. Corte de la fibra optica
	- Usa el cortador de precision entre 8mm y 15mm, dependiendo del largo de la proteccion termocontraible
6. Introduce la fibra en la Fusionadora
	- Deja no menos de 1mm de distancia entre ambas fibras y que esten rectas
	- Al fusionar, la maquina Alinea los ejes X e Y, y acerca los extremos a una distancia de 1$\micro$ o 2$\micro$
Nota: Considera la "Matriz de Riesgo", condiciones de entorno y configuracion correcta de la maquina de fusion

