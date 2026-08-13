# Info

Anexos
- [Dr. Ing. Jose Joskowicz - Universidad de la Republica - Montevideo, Uruguay - Cableado Estructurado (2013 - v11)](https://www.fing.edu.uy/iie/ense/asign/ccu/material/docs/Cableado%20Estructurado.pdf)
- [Brady - TIA-606-C - Infograma](https://d37iyw84027v1q.cloudfront.net/Common/TIA_606_Labeling_Standards_ebook_Latin_America.pdf)
- [Blog de Fibra Optica - Importancia de un etiquetado correcto en el cableado estructurado (2014)](https://fibraoptica.blog.tartanga.eus/2014/02/08/la-importancia-de-un-etiquetado-correcto-en-las-instalaciones-de-cableado-estructurado/)

La TIA (Telecommunications Industry Association) y la EIA (Electronic Industries Association) comenzaron en la década de 1980 a definir estándares para el cableado estructurado, estableciendo criterios sobre cómo diseñar, construir y administrar sistemas de cableado de forma organizada y consistente. El objetivo era reducir los malentendidos entre fabricantes y compradores, facilitando la interoperabilidad y el desarrollo de un ecosistema común.

Uno de los estándares más conocidos es ANSI/TIA-568, que define las características y prácticas para el cableado de telecomunicaciones. Dentro de este estándar se encuentran los esquemas de terminación T568-A y T568-B, utilizados para ordenar los conductores al terminar cables de par trenzado mediante conectores de 8P8C.

El cable se llama Cable de par trenzado, son 4 pares, y se dividen en categorias, las mas conocidas son CAT5E y CAT6A, con conectores 8P8C llamados "RJ-45"

Necesitas una Ponchadora

Este cableado necesita una infraestructura fisica que permita proteger, organizar y distribuir los cables, ademas de facilitar su instalacion y mantenimiento. Entre los principales, existen

Las canaletas estan destinadas a alojar y proteger los cables, suelen fabricarse en plastico y tienen una tapa para guardar el cableado. 

Las tuberias conduit, existen tuberias de PVC rigido, metal, y otros

Las escalerillas portacables son estructuras abiertas, como un canastillo largo para sostener los cables desde un punto hasta otro, este diseño facilita el tendido, inspeccion y ventilacion de los cables. Puedes ver usualmente en los supermercados

## Rotulacion

Que interesante, ponerle nombre a los cables para que no se te pierdan

Las normativas al momento de escribir son (Siempre corrobora esta info):
- TIA/EIA 606-D (2021)
- ISO/IEC 14763-2 (2019)
- EN 50174-1 (2026)

> [!WARNING] Algunas version desactualizadas
> Mucho material academico, apuntes o infografias no son inmunes al tiempo y pueden indicar normativas viejas, simplemente ignoralas y comparalas con las que aparecen arriba o mas nuevas
> - TIA 606 A/B/C (1993-2017)
> - ISO/IEC 14763-1 (1999)
> - EN 50174-1 (2018) + A1 (2020)

Se debe rotular
- Patch Panel
- Cableado
- Rosetas

Y el nombre? Sigue la siguiente regla
1. Cada rack tiene una letra mayuscula, empezando por la "A", y terminando en "Z"
2. Para el Cableado Horizontal (Distribucion), cada cable tendra a ambos extremos la misma etiqueta. La notacion sera `0HXXY`
	- `0`: Indica el numero de Piso
	- `H`: Indica Cableado Horizontal
	- `XX` Indica el numero de Roseta
	- `Y` Indica el numero de Rack
	- Ejemplo: `1H03A`
3. Para el Cableado Vertical (Upstream/Rack con Rack) cada extremo informa sobre el lugar y posicion de la conexion del otro extremo y viceversa. La notacion sera `0VXXY`
	- `0`: Indica el numero de Piso
	- `V`: Indica Cableado Vertical
	- `XX`: Indica la posicion del otro extremo
	- `Y`: Indica el numero de Rack del otro extremo
	- Ejemplo: `1V05C`


## Certificacion

> [!TIP] Marcas de Certificadoras
> - [Fluke Networks](https://www.flukenetworks.com/)
> - [Viavi Solutions](https://www.viavisolutions.com/en-us)
> - [TREND Networks](https://www.trend-networks.com/us/) (Antes IDEAL Networks)
> - [AEM Networks](https://aemnetworks.com/)
> - [EXFO](https://www.exfo.com/)

Las certificadoras de cableado son equipos especializados (y costosos) que verifican que el cableado cumpla con parametros establecidos por estandares (TIA 606 por ejemplo). Entre sus mediciones se encuentra la *Paradiafonia* (Crosstalk), que es la interferencia electromagnetica entre cables TP cercanos. Para mantener la presicion en sus mediciones, requieren calibracion y mantenimiento periodico anual con el fabricante.

Los valores mas comunes que te entrega una certificadora, son los siguientes
- Mapa del Cableado (Wire Map): Comprueba que los conductores esten conectados correctamente en ambos extremos, verificando continuidad, orden, pares invertidos o algun otro error de conexion.
- Longitud (Length): Determina la longitud del enlace y utiliza el NVP (**N**ominal **V**elocity of **P**ropagation) para calcular cuanto se demora la señal en recorrer el cable
- Perdida de Insercion (Insertion Loss/Attenuation): Mide cuanto se debilita la señal al pasar por el cable. Se expresa en dB y aumenta con la longitud, la frecuencia y las perdidas introducidad por el cable y las conexiones
- Perdida por Paradiafonia en el extremo cercano (NEXT): Mide la interferencia que un par transmisor induce sobre otro par y se mide en el mismo extremo donde se inyecta la señal
- Perdidas por parafonia de suma de potencias (PSNEXT): Mide el efecto de interferencia combinado de interferencia entre otros TP sobre un TP
- Perdida por Paradiafonia en el extremo lejano (FEXT): Mide la interferencia que un par transmisor induce sobre otro par y se mide en el otro extremo del transmisor
- Perdida por paradiafonia de igual nivel (ACR-F (Antes ELFEXT)): Indica cuanto margen queda entre la interferencia FEXT y la señal que llega al receptor
- Perdida por paradiafonia de igual nivel de suma de potencias (PSACR-F (Antes PSELFEXT)): Evalua el efecto combinado de las interferencias ACR-F de los otros TP sobre un TP
- Perdida de Retorno (Return Loss): Mide la cantidad de señal que se refleja debido a discontinuidades de impedancia del cableado.
- Retardo Sesgado (Delay Skew): Mide la diferencia entre los tiempos de propagacion de los distintos TP.

## Instalacion Aerea

Se puede hacer tanto de cable [[020 - Conceptos/020.3 - Fundamentos/Cableado Estructurado/Coaxial|Coaxial]] como de [[020 - Conceptos/020.3 - Fundamentos/Cableado Estructurado/TP|TP]], lo importante es que tenga un cable mensajero, que es una tira de metal, que sirva para poder colgar un cable y tensionarlo sin probocar tension en el cable que entrega los datos

Se utiliza tambien otros materiales
- Grampas (En Cemento debe tener clavo para cemento)
- Cancamos
- 