# Info
Un empalme es la union permamente de dos extremos de una fibra para transmitir luz, ambos extremos se someten a una temperatura tan alta como para fundir sus extremos y unirlos y genera atenuacion dependiendo de la calidad del empalme.

Existen 2 tipos de empalme
- Fusion: Se genera un arco electrico por una fuente de 4000 y 5000 volts mediante una Fusionadora y realiza el alineamiento de las fibras, las fusiona, calcula las perdidas y no se demora mas de 10 segundos, luego tienes que proteger las fibras fusionadas con un manguito termocontraible de 40mm o de 60mm
- Mecanico: Se empalman en un contenedor relleno con gel igualador de indice de refraccion, son conexiones provisorias y que solo se usan en situaciones de emergencia no permanentes

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
- Alcohol Isopropilico
	- Disuelve impurezas sin dejar residuos
- Toallitas para limpieza de fibra
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

## Preparacion de la Fibra Optica
- Se remueven cubiertas (Chaquetas), Buffers y protecciones exteriores hasta dejar la fibra lista para unir o conectar.


## Union de la Fibra Optica
- Una vez que la fibra esta pelada y preparada, se procede a su empalme. Estas herramientas aseguran cortes limpios y alineaciones para un correcto empalme. 


## Limpieza de la Fibra
- Para garantizar la baja atenuacion y evitar perdidas por suciedad. La limpieza aplica tanto a las puntas de los conectores como a la fibra antes de empalmarla


## Fusion de Fibra
Se recomienda ver: [Youtube - PROMAX T&M España - Como hacer una fusion perfecta de fibra optica](https://youtu.be/qiKXaEcyHQE?si=jEW5nlZI80ugdiYz)

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

