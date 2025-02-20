# Info
Las radiofrecuencias abarcan un rango amplio de frecuencias electronicas utilizadas para transmision de datos y señales de audio/video mediante [[020 - Conceptos/020.5 - Red HFC/Elementos Activos|Elementos Activos]] y [[020 - Conceptos/020.5 - Red HFC/Elementos Pasivos|Elementos Pasivos]]

- Sistema
	- Radio (Transmisor, Receptor o Transceptor)
	- [[020 - Conceptos/020.5 - Red HFC/Antenas|Antenas]]
	- Cable [[020 - Conceptos/020.5 - Red HFC/Coaxial|Coaxial]] con conectores para interconectar la radio a la antena

## Impedancia
Es fundamental para entender como se comporta la señal en cables, antenas y equipos de transmision

Terminologia
- Impedancia (Z): Oposicion total que encuentra la corriente alterna al fluir por un circuito o dispositivo
- Resistencia (R): Parte real que disipa energia (ej. en forma de calor)
- Reactancia (X): Parte imaginaria que describe el almacenamiento de energia en campos electricos (Capacitancia) o magneticos (Inductancia)
- Unidad Imaginaria (j): Equivalente a $\sqrt{-1}$ en ing. electrica, A frecuencias altas, se vuelve mas relevante y condiciona el rendimiento de cables y antenas.

Se expresa matematicamente como: $Z=R+jX$

Los cables [[020 - Conceptos/020.5 - Red HFC/Coaxial|Coaxial]] y las antenas se fabrican con un valor de impedancia especificos y que van a depender de sus caraceteristicas
- 50 ohmios: Usado en la mayoria de sistemas de telecomunicaciones (Wi-Fi, Telefonia Movil, Radioaficionado, etc.)
- 75 ohmios: TV cable o satelital, etc.

Cuando la impedancia del cable y la antena no coincide (desadaptacion), la fraccion de señal no aceptada por la antena rebota y regresa al cable. Esto genera ondas estacionarias a lo largo del cable, para poder cuantificar estas reflexiones, se mide el VSWR (Voltage Standing Wave Ratio) o ROE (Razon de Onda Estacionaria), indica la proporcion entre el maximo y el minimo de voltaje que se forma en linea
- VSWR o ROE = 1: Significa que no ha reflexiones
- Se considera que valores < 1.5 se consideran aceptables en la practica

TO-DO: 
- Tipos de modulacion
- Rango de frecuencias
- Perdidas y consideraciones de linea transmision a alta frecuencia


