Son elementos escenciales para unir cables con equipos de radio (Antenas, Routers, Modems, etc.) Existen muchos tipos y tamaños, algunos estandar y otros no estandar. En el ambito de la [[020 - Conceptos/020.5 Red HFC/RF|RF]] es comun encontrar:
- Conectores de polaridad invertida (RP, Reverse Polarity): se intercambian pines o generos (Macho/Hembra) de manera diferente a los conectores normales
- Roscas Invertidas: Algunas roscas se diseñan al revez de lo habitual

Estas variaciones surgieron, en buena parte, porque la FCC (Federal Communications Commision) en EEUU busca limitar que los usuarios finales cambien libremente las antenas de sus dispositivos de bandas libres (como Wi-Fi), para controlar interferencias y cumplimiento normativo. Por ello, se exige a los fabricantes que integren conectores no estandar, dificultando el uso de antenas "genericas" sin el adaptador apropiado

Los conectores mas frecuentes son los siguientes
- SMA (SubMiniature version A)
	- SMA Macho / SMA Hembra
	- RP-SMA Macho / RP-SMA Hembra
- TNC (Threaded Neill-Concelman)
	- TNC Macho / TNC Hembra
	- RP-TNC Macho / RP-TNC Hembra
- N (Usado en antenas y equipos de mayor potencia)
	- N Macho / N Hembra 
- MC-Card, MMCX, RP-MMCX (Usados en modulos internos Wi-Fi o Bluetooth)
- U.FL (Extremadamente Pequeño, tipico en PCB y Modulos Wi-Fi)

Un pigtail es un tramo corto de cable flexible que conecta un dispositivo (ej. una tarjeta mini PCIe WiFi de conector U.FL) a un conector mas robusto (ej. SMA o RP-SMA) y de esta forma evitar que el conector pequeño sufra tensiones mecanicas directas debido a los cables mas gruesos

Ejemplo
- Pigtail U.FL a RP-TNC Macho
- Pigtail U.FL a N Macho

Los adaptadores sirven para hacer la transicion entre diferentes tipos de conectores. Son utiles cuando tu cable o tu equipo no coincide con el conector de la antena o el dispositivo

> [!IMPORTANT] Importante
> Se recomienda usar el minimo numero de adaptadores, ya que cada empalme extra introduce atenuacion en la señal y puede convertirse en un punto debil mecanicamente

Ejemplos
- SMA hembra a N macho
- N macho a N hembra
- N hembra a N hembra
- SMA macho a TNC macho

