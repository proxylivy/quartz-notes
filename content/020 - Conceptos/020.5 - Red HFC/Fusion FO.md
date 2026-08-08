# Info

> [!TIP] Lecturas Recomendadas
> - [Youtube - PROMAX T&M España - Como hacer una fusion perfecta de fibra optica](https://youtu.be/qiKXaEcyHQE?si=jEW5nlZI80ugdiYz)

La Fusion de [[020 - Conceptos/020.5 - Red HFC/Fibra Optica|Fibra Optica]] es la union permamente de dos extremos de fibra para transmitir luz, ambos extremos se someten a una temperatura tan alta como para fundir sus extremos y unirlos y genera atenuacion dependiendo de la calidad del empalme.

Existen 2 tipos de empalme
- Fusion: Se genera un arco electrico por una fuente de 4000 y 5000 volts mediante una Fusionadora y realiza el alineamiento de las fibras, las fusiona, calcula las perdidas y no se demora mas de 10 segundos, luego tienes que proteger las fibras fusionadas con un manguito termocontraible de 40mm o de 60mm
- Mecanico: Se empalman en un contenedor relleno con gel igualador de indice de refraccion, son conexiones provisorias y que solo se usan en situaciones de emergencia no permanentes

# EPP

Estos son los Elementos de Proteccion personal que debes utilizar al momento de fusionar [[020 - Conceptos/020.5 - Red HFC/Fibra Optica|Fibra Optica]].

Para el Trabajo con fibra optica
1. Zapatos de seguridad dielectrico, con puntera de acero y con caña alta
2. Casco de Seguridad dielectrico con Barboquejo
3. Gafas (Antiparras) de protección Ocular
4. Cotona de protección de residuos en la ropa
5. Guantes de Látex o de Nitrilo
6. Mascarilla de boca

Para el trabajo en alturas, ademas debes utilizar
1. Uso de Arnés (obligatorio en todo momento)
2. Manejo apropiado de elementos de anclaje y soporte de antena en los cables y postes.

Para el trabajo en cámaras subterráneas, ademas debes utilizar
1. Uso OBLIGATORIO SIEMPRE de medidor de monóxido de carbono, considerando siempre el tiempo de ventilación de la cámara.
2. Uso de equipos de Iluminación adecuados, de PILAS o Baterías.
3. Evaluar la necesidad de retiro de agua o humedad.

Para el trabajo en vía pública, ademas debes utilizar
1. Uso de conos de protección de zona de trabajo
2. Uso de Barandas metálicas de cierre del perímetro en el caso de trabajo en cámaras subterráneas.

# Herramientas
La instalacion y el mantenimiento de fibra optica requieren un conjunto de herramientas especializadas. En general, podemos agruparlas en tres grandes categorias segun la fase del trabajo

## Preparacion de cables y tubos
- Cutter de precisión para F.O
- Pinzas para retiro de pintura PVC en cables F.O
- Cortador de minitubos
	- Permite seccionar minitubos o microductos de hasta 3,17 mm
- Deschaquetadora de F.O. (Cable Slitter)
	- Diseñada para cortar con precisión cubiertas de cables hasta 44,5 mm
- Deschaquetadora para fibras Tight Buffer
	- Para diámetros de 500 $\mu{m}$
- Tijeras corta fibras Kevlar
	- Para cortar fibras de aramida (Kevlar) que refuerzan el cable
## Pelado y preparacion de la fibra
- Pelador de Fibra Optica (Stripper) de 3 medidas
	- Quita la Chaqueta "Tight Buffer" de 3mm
	- Retira el buffer de 900$\mu \text{m}$
	- Pela la cubierta de 250$\mu \text{m}$
	- Retira el acrilato, dejando la fibra desnuda
## Corte y union de la fibra
- Cortadora de presicion de 3 pasos
	- Indicada para empalmes mecanicos y conectores pre-pulidos
- Cortadora de precision (Fiber Cleaver)
	- Compatibles con fibras de 250$\mu \text{m}$ y 900$\mu \text{m}$
	- Cortes ajustables entre 5mm y 20mm
- Microcospio optico de inspeccion (200x)
	- Adaptador Universal (ST, LC y SC), compatible con fibras MM y SM
	- Permite deteccion visual de suciedad, grietas o roturas en conectores
## Limpieza de la fibra y conectores
- Alcohol Isopropilico Puro
	- Disuelve impurezas sin dejar residuos
- Toallitas para limpieza de fibra (Sin pelusas)
	- Alta absorcion y sin residuos
- Limpiador de conectores (Tipo Cinta)
	- Limpieza rapida del ferrule del conector
- Limpiador One Click
	- Sistema rapido que limpia extremos de conectores con un solo movimiento
## Medicion, diagnostico y certificacion
- OTDR (Optical Time Domain Reflectometer)
	- Mide retrodispersion para detectar atenuacion y fallas
- OLTS (Optical Loss Test Set)
	- Mide la perdida total del cable
- Localizador Visual de Fallas (VFL)
	- Laser visible rojo clase 2 a 650nm
	- Detecta roturas y fallas hasta 5km
	- Conexion Universal ST, FC y SC
- Medidor de Redes PON
	- Compatible con redes FTTH (GPON, GEPON)
	- Capaz de transportar señal Triple Play (1310nm, 1490nm, 1550nm)

# Preparacion de la Fibra

Se remueven cubiertas (Chaquetas) de PKP (Polietileno-Kevlar-Polietileno) o LAP (Alumminio-Polietileno), Buffers, Geles Hidrofugos y protecciones exteriores hasta dejar la fibra lista para unir o conectar. Las terminaciones deben estar limpias de impurezas y suciedades

El lugar debe ser cerrado, sin vientos, ni cambio de temperatura o humedad. No debe presentar riesgos para la seguridad del personal (Material Cortante, Desechos, productos toxicos, etc.) y debe mantener una temperatura estable para facilitar la fusion de las empalmadoras

El corte de la fibra debe ser perpendicular al eje de la mmisma, y hacerse con seguridad en un solo paso y el material debe estar calibrado

# Fusion de Fibra

Para empalmar un cable de [[020 - Conceptos/020.5 - Red HFC/Fibra Optica|Fibra Optica]] se siguen los siguientes pasos
1. Desenchaquetar el cable de Fibra Optica
	- Usa un Stripper calibrado a 125$\mu \text{m}$, retira entre 3 y 4 centimetros de Coating, dejando la fibra desnuda
2. Retirar el recubrimiento primario de la fibra optica
3. Retira todos los elementos de proteccion axial
4. Limpieza total de las fibras
	- Usa una toallita suave con alcohol isopropilico
5. Corte de la fibra optica
	- Usa el cortador de precision entre 8mm y 15mm, dependiendo del largo de la proteccion termocontraible
6. Introduce la fibra en la Fusionadora
	- Deja no menos de 1mm de distancia entre ambas fibras y que esten rectas
	- Al fusionar, la maquina Alinea los ejes X e Y, y acerca los extremos a una distancia de 1$\mu$ o 2$\mu$
Nota: Considera la "Matriz de Riesgo", condiciones de entorno y configuracion correcta de la maquina de fusion

# Verificacion Enlace

Se usan 3 calculos

El primero de ellos, IL (Insertion Loss / Perdida de Insercion): Se mide en dB y relaciona la potencia que entra (Pi) vs cuanta sale (Pt). El resultado siempre sera negativo (*Pt* siempre es menor que *Pi*) y mientas su valor sea mas cercano a cero, menos perdida existe en la conexion. Este valor se puede obtener con un Power Meter en conjunto a un Generador de Luz

Formula: $IL=10 \times \log (\dfrac{Pt}{Pi})$

El segundo es, ORL (Optical Return Loss / Perdida de Retorno): Se mide en dB y compara la potencia de entrada (Pi) con la potencia que se refleja o se devuelve (Pr). Mientras que *Pr* sea bajo, mejor, por lo que el resultado mas alejado de cero, mejor 

Formula: $ORL=10 \times \log (\dfrac{Pr}{Pi})$

# Riesgos

A. Trabajo en Alturas
1. Riesgo de caída desde escaleras
2. Riesgo de golpes por caídas de herramientas u otros elementos usados en altura.

B. Trabajo en Cámaras subterráneas.
1. Riesgo de MUERTE por inhalación de monóxido de carbono
2. Riesgo de caída por mala iluminación
3. Riesgo de electrocución, por temas de humedad y uso de equipos eléctricos de iluminación o herramientas.

C. Trabajo en Vía pública
1. Riesgo de accidentes con vehículos o peatones

A. El tamaño de los residuos de Fibras. Son tan pequeños que son difíciles de ver (recuerde que una fibra es comparable al grosos de un cabello humano):
1. Riesgo de tragar un pedazo de fibra
2. Riesgo de que ingrese en un ojo un pedazo de fibra o residuo
3. Riesgo de enterrarse un pedazo de fibra en la piel
4. Riesgo de llevar residuos entre las uñas y en el pelo
5. Riesgo de llevar los residuos del trabajo con fibra en la ropa y llevar el riesgo hacia fuera del laboratorio.

B. Los emisores de Luz (Laser y Led) emiten energía que puede dañar la vista, esto a pesar de que las longitudes de onda de transmisión están fuera del rango visible:
1. Riesgos de daños o pérdida en la visión ocular

C. El uso de elementos cortantes en el trabajo con fibra. Se usan cortantes, tijeras.
1. Riesgo de cortes en la piel

D. Contaminantes químicos
1. Cuando se estén manipulando sustancias químicas como el limoneno y alcohol isopropílico evitaremos el contacto en la piel y ojos, para ello utilizaremos guantes de látex o PVC, gafas de seguridad contra líquidos y vapores, así como mascarillas para gases y vapores para la limpieza de conectores de fibra óptica.


Por lo que debes demarcar y señalizar tu zona de trabajo, usar permanentemente el chaleco reflectante, no transportar elementos que obstruyan la vision, no aproximarse a los bordes de excavacion y usar lentes de seguridad

# Extra

Existe un codigo de colores para cada fibra, la cual se llama TIA-598-A (1995), aunque la ultima version es la TIA-598-D (Julio 2014)
- [MSP Docs - TIA 598-D](https://docs.msp-ict.ir/images/PDF-Files/tia-598-d.pdf)
