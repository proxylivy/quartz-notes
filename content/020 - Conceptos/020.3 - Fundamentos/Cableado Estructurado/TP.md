# Info

Anexos
- [Youtube - Moracho - Como hacer cable de red UTP/RJ45](https://youtu.be/m0BZkHj6p7U?si=svshuHL_e-aKdeEb)
- [Youtube - Leimor - Como Ponchar un cable de red UTP/RJ45](https://youtu.be/XpeKWEXw7HE?si=BROWQwB3KzWvxFGJ)

TP (Twisted Pair) son cables de grosor 23AWG a 24AWG, que vienen en pares, los famosos cables ethernet son en realidad 4 TP, osea 8 cables en total, conectados con puntas 8P8C llamadas RJ45

Estos cables permiten conectar terminales, computadoras, impresoras, [[020 - Conceptos/020.4 - Dispositivos de Red/Servidor|Servidores]]. [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]], [[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]], [[020 - Conceptos/020.4 - Dispositivos de Red/AP|AP]], entre otros

Existen certificaciones para el cable:
- CAT5 (CAT5E): El estandar mas "viejo" de TP, es capaz y cool
- CAT6 (CAT6A): Viene con una cruz de separacion a la mitad y el conector asociado tiene una mejor separacion de cada cable
- CAT7: Este se ignora realmente, es una certificacion aparte para otro tipo de cableado pero se creo bajo TP, prefiere el CAT8 si quieres algo superior
- CAT8: El ultimo y mas nuevo, innecesario pero alli esta, es caso de estar en un [[020 - Conceptos/020.3 - Fundamentos/Data Center|Data Center]]

Y su presentacion puede ser segun su nivel de proteccion (Independiente de la certificacion)
- UTP (Unshielded): Cable sin proteger, viene tal cual
- S/UTP (Shielded/Unshielded): Envuelto en papel de aluminio de forma general, los cables TP no tienen proteccion individual, no es tan comun realmente
- FTP (Foil): Cada TP viene envuelto con blindaje individual, para proteger del EMI y la diafonia 
- S/FTP (Shielded/Foil): Envuelto en papel aluminio de forma general y cada cable TP esta envuelto en blindaje individual, para proteger del EMI y la diafonia

Cada cable tiene un recubrimiento de un color y existe una normalita para armar cables, la famosa normativa de instalacion ANSI EIA/TIA 568C que contiene 568-A y 568-B

## Crea tu cable

En estos dias, ambas puntas deben estar en la Norma T568B, simple, los dispositivos son lo suficientemente inteligente como para detectarse, eso de cross, y straight es cosa del pasado, es mi mejor consejo tecnico que te puedo dar

Materiales
- Deschaquetadora de cable Universal
- Crimpeadora UTP
- Cable TP (CAT) (Proteccion)
- Conectores 8P8C (a.k.a RJ45)
- Probador de cable UTP (Opcional)

Debes seguir los colores
- Blanco-Naranjo
- Naranjo
- Blanco-Verde
- Azul
- Blanco-Azul
- Verde
- Blanco-Cafe
- Cafe

Como cada uno viene en su propio par, te recomiendo comenzar por el par Blanco-Naranjo y acomodar el resto, el unico que se mueve de lugar de forma "extraña" seria el par Blanco-Verde y Verde. Y te quedara estupendo

**Instrucciones**

1. Deschaqueta el cable, retirando de 3 a 4cm de chaqueta PVC, sin dañar ningun cable TP al interior
2. Separa cada TP y enderezalos
3. Acomoda segun el estandar T568-B
4. Corta con presicion y de forma derecha a un largo de 15mm desde la chaqueta de PVC
5. Introduce los cables al conector 8P8C (RJ45) mirando la cara de los conectores, para que quede la correcta secuencia de colores, estos deben quedar hasta el final tocando con las puntas de cobre
6. Coloca el conector con el cable en la crimpeadora y presiona para apretar
7. Verifica que la cubierta este sujeta en el interior del conector y que los cables esten haciendo contacto con los conectores
8. Revisa con un tester UTP para ver si los cables estan conectados correctamente


## Fenomenos

Existen diversos fenomenos fisicos que pueden afectar la transmision de señales, degradando su calidad o dificultando la correcta interpretacion de la señal

**Diafonia**

Es la interferencia electromagnetica que una señal puede generar sobre un par de conductores cercanos. El campo electromagnetico producido por un par puede inducir una diferencia en otro par, generando una señal no deseada

Es un concepto fundamental para la [[020 - Conceptos/020.3 - Fundamentos/Cableado Estructurado/Cableado Estructurado#Certificacion|Certificacion de Cableado Estructurado]], donde se evaluan distintos tipos de diafonia entre los pares

**Ruido electrico**

Es toda señal o perturbacion no deseada que se superpone a la señal que se desea transmitir, pudiendo alterar su interpretacion

Las corrientes alternas (Usualmente de 50Hz a 60Hz) generan campos electromagneticos que pueden acoplarse al cableado de datos, especialmente cuando ambos se encuentran proximos o no se respetan las condiciones de instalacion

**Ruido Blanco**

Es una señal aleatoria cuya potencia se distribuye uniformemente a travez del espectro de frecuencias. No existe correlacion entre sus valores a travez del tiempo.

Se llama ruido blanco debido a su similitud con la luz blanca, al ser esta un amplio espectro de frecuencias visibles

**Distorsion**

Es la alternacion de la forma de una señal durante su transmision. Puede entenderse como la diferencia entre la señal de entrada y la señal de salida producida por las caracteristicas del medio o del sistema de transmision