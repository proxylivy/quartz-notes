# Info

> [!TIP] Lecturas Recomendadas
> - [The FOA (Fiber Optic Association) (Español)](https://www.thefoa.org/ESP/index.htm)
> 	- [Cableado de Fibra Optica](https://www.thefoa.org/ESP-Premises/5%20Cableado%20de%20Fibra%20Optica.html)
> - [Win Blog - Que es la fibra optica](https://win.pe/blog/conectate-al-futuro-todo-sobre-la-fibra-optica/)

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
Se representa como $n$ y relaciona la velocidad de la luz en el vacio ($C$) con su velocidad de algun medio ($V_{p}$): 

$$n=\dfrac{C}{V_{p}}$$

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
Al cambiar de un medio con indice $n_1$ a otro de indice $n_2$: 

$$n_1 \sin(\theta_{1})=n_2\sin(\theta_{2})$$

- $\theta_{1}$: Angulo de incidencia
- $\theta_{2}$: Angulo de refraccion

Cuando $\theta_{2}$ es exactamente $90^\circ$, se define el **Angulo critico** ($\alpha_{c}$)

### Apertura Numerica (NA)
El **Angulo de aceptacion** ($\phi_{NA}$) es el maximo angulo (medido respecto al eje de la fibra) que permite que los rayos se confinen en el nucleo. La **Apertura Numerica** (NA) se expresa de varias formas equivalentes:
1. $NA=\sqrt{n1^{2}-n2^{2}}$
2. $NA=\sin(\phi_{NA})$
3. $NA=\sin(\alpha_{c})$

### Ejemplo de Calculo
Si $n_{1}=1,45$ y $n_{2}=1,35$, entonces: 

$$NA=\sqrt{(1,45)^{2}-(1,35)^{2}}\approx0,529$$

El angulo de aceptacion: 

$$\phi_{NA}=\arcsin{(0,529)}\approx31,94^\circ$$

En este ejemplo, las ondas con un grado menor a $31,94^\circ$ (medido respecto al eje de la fibra) quedara confinado dentro del nucleo por reflexiones internas totales. 

## Fabricacion
Se elabora a partir de Dioxido de silicio ($SiO_{2}$) con dopantes como Fluor ($F$), Boro ($B$), Germanio ($Ge$) y Fosforo ($P$) para ajustar el indice de refraccion, El proceso general:

1. Barra Preformada
	- Se inyectan vapores de dopantes y se calientan con llama de hidrogeno a ~2000°C
	- Los dopantes se condensan y forman el "Nucleo" con las propiedades deseadas
2. Estirado en la Torre
	- La pregorma se introduce en un horno de alta temperatura (~2000°C) y se estira de ella para obtener la fibra del diametro requerido ($125\mu \text{m}$ con nucleo y revestimiento)
	- Se controla mediante sistemas automatizados y computarizados

Los metodos mas habituales:
- MCDV (AT&T), VAD (Japon), PCVD (Philips)
- [OVD](https://www.corning.com/optical-communications/emea/en/home/products/fiber/manufacturing-excellence.html) (Outside Vapor Deposition, [Corning](https://www.corning.com/optical-communications/emea/en/home.html))

## Tipos de Fibra
1. Monomodo (SM)
	- Nucleo Estrecho (Tipicamente 9 $\mu \text{m}$)
	- Permite propagar una Longitud de Onda por fibra
	- Ideal para largas distancias (90Km entre repetidores)
	- Ejemplo de nomenclatura: SM 9/125 (Nucleo/Revestimiento)
2. Multimodo (SM)
	- Nucleo mas ancho (ej. 50 o 62,5 $\mu \text{m}$)
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

# Atenuacion

La atenuacion es la perdida de potencia que sufre la señal al viajar por la fibra. Se mide en decibeles (dB). Es el factor mas importante que afecta la calidad de un enlace de fibra optica. Para calcular la potencia que llega al receptor, se suman las ganancias y se restan las perdidas

Primero se calcula la atenuacion total del enlace. Esta es la suma de todas las perdidas presentes: Conectores, Fusiones y el propio cable

Definiones:
- $A_{T}$ = Atenuacion Total del enlace (dB)
- $A_{C}$ = Atenuacion promedio de un conector (dB)
- $N_{C}$ = Numero de conectores
- $A_{S}$ = Atenuacion promedio de una fusion (dB)
- $N_{S}$ = Numero de fusiones
- $A_{L}$ = Atenuacion del cable por kilometro (dB/km)
- $L$ = Longitud del enlace (km)

La Formula de la atenuacion total es:
$$A_{T} = (N_{C} \times A_{C}) + (N_{S} \times A_{S}) + (L \times A_{L})$$
Esto simplemente suma todas las perdidas individuales del enlace

Una vez obtenida la atenuacion total, se calcula la potencia recibida.

Definiciones:
- $P_{R}$ = Potencia Recibida (dBm)
- $P_{T}$ = Potencia del transmisor (dBm)
- $P_{rep}$ = Ganancia del Repetidor (dB)

La formula es:
$$P_{R}=P_{T} - A_{T} + P_{rep}$$


# Infraestructura

> [!TIP] Lecturas Recomendadas
> - [TeleGeography - Submarine Cable Map](https://www.submarinecablemap.com/) (SCM)

Antes de la fibra Optica, en 1858, se creo un cable de telegrafo entre Europa y America bajo el agua, esa es la base de la fibra optica mmoderna, la cual se utiliza (con muchas protecciones) para interconectar paises, mediante una red de cableado troncal.

Algunos ejemplos, son:
- Cableado Submarino
	- Cable SAC (2000) (**S**outh **A**merican **C**rossing) de [Cirion Technologies](https://www.ciriontechnologies.com/en/) + [Sparkle](https://www.tisparkle.com/) | [SCM](https://www.submarinecablemap.com/submarine-cable/south-american-crossing-sac)
		- [Blog - Gerencia - Un anillo de conectividad garantizada](https://www.gerencia.cl/networking/south-american-crossing-un-anillo-de-conectividad-garantizada/)
		- [Blog - SantaTeresita - Fibra Optica en las Toninas](https://www.santateresita.com.ar/infotonina.htm)
	- Cable SAM-1 (2001) con [Emergia](https://www.emergiacc.com) por Consorcio [Telefonica](https://www.telefonica.com/es/) - [Te Connectivity](https://www.te.com/es/home.html) | [Licenciamiento](https://transition.fcc.gov/Bureaus/International/Orders/2000/da001826.txt) | [SCM](https://www.submarinecablemap.com/submarine-cable/south-america-1-sam-1)
	- Cable Curie (2020) por [Google](https://cloud.google.com/) | [SCM](https://www.submarinecablemap.com/submarine-cable/curie)
	- Cable Mistral (2021) por [America Movil](https://www.americamovil.com/English/overview/default.aspx) + [Telxius](https://telxius.com) | [SCM](https://www.submarinecablemap.com/submarine-cable/south-pacific-cable-system-spcsmistral)
	- Cable Halaihai (aka Humboldt) (2029) por [Google](https://cloud.google.com/) | [SCM](https://www.submarinecablemap.com/submarine-cable/halaihai)
- Cableado Fibra Interna del Pais
	- FOS Quellon-Chacabuco (2015) por [GTD](https://www.gtd.cl/) | [SCM](https://www.submarinecablemap.com/submarine-cable/fos-quellon-chacabuco)
	- Proyecto [FOA](https://foa.subtel.gob.cl/) (Fibra Optica Austral) (2020) por [Internet Archive - CTR + Huawei Marine Press News](https://web.archive.org/web/20201129014048/http://www.huaweimarine.com/en/News/2018/press-releases/PR20180323) + [Silica Networks](https://www.silicanetworks.com/es/foa/) | [SCM](https://www.submarinecablemap.com/submarine-cable/fibra-optica-austral)
	- Cable Prat (2020) por [GTD](https://www.gtd.cl/) | [SCM](https://www.submarinecablemap.com/submarine-cable/prat)
- Enlaces entre Oficinas Centrales con PSTN
- Anillos de distribucion HUBs y Nodales en redes HFC
- PITs o IXP
	- [PIT Chile](https://www.pitchile.cl/wp/)
	- [IXP Chile](https://ixpchile.cl/) via [PIT](https://pit.net/)
	- [PIT Entel](https://www.pitentel.cl/)
	- [PatagoniaIX](https://patagoniaix.cl/)
