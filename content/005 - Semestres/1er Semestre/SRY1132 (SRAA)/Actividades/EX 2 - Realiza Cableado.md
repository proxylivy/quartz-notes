# Info

Actividad de [[005 - Semestres/1er Semestre/SRY1132 (SRAA)/SRY1132 (SRAA)|SRY1132 (SRAA)]]

Encuentras las rubricas en [Copyparty - EX2](https://copyparty.proxylivy.work/Duoc/Conectividad%20y%20Redes/1er%20Semestre/SRY1132%20(SRAA)/Actividades/EX2/)

## Parte 1

Materiales
- Router AP (Linksys o TPLink)
- Cables (U)TP
- EPP

Reinicia la configuracion del Router AP mediante el boton "RESET", luego conecta el cable de internet al puerto WAN y la laptop en un puerto LAN del Router AP.

Desde la laptop accede al puerto de gateway por defecto (ej. `192.168.1.1`). Luego acceden al panel de control, las credenciales estan detras del Router AP, deberian ser `admin` tanto como usuario y contraseña.

Habilita DHCP y comprueba que un dispositivo tenga salida a internet mediante el Router AP

Debes generar escenarios para ver el comportamiento del Router AP, te dejo alguas ideas
- Que pasa si el router se pone detras de un objeto metalico
- A los cuantos metros se pierde la señal?

## Parte 2

En la actividad anterior, se realizó un enlace inalámbrico entre un router y un PC, el cual se sometió a diferentes escenarios físicos. Uno de los fenómenos que se pudo observar es la variación en el nivel de la potencia de la señal recibida. Las variaciones de potencia pueden deberse a varias causas, y una de las más importantes es la distancia entre el transmisor y el receptor.
En la presente actividad, aprenderemos cómo calcular las pérdidas de potencia en relación a la distancia entre el equipo que transmite la señal, y el equipo que la recibe.
Para este efecto, trabajaremos sobre el estudio de un caso, en el cual se involucran un transmisor y un receptor inalámbricos, y un transmisor y un receptor óptico.

**Contexto**

La Empresa Conectividad S.A, trabaja en un proyecto de implementación de un sistema de comunicación en una ciudad de la zona Norte de Chile, donde debe unir dos estaciones de comunicaciones que se encuentran separadas dos Kilómetros entre sí.
El sistema a implementar se compone de un tramo con radioenlace y un tramo con fibra óptica, ambos de un Kilómetro de distancia. La empresa a cargo del proyecto necesita conocer las pérdidas de potencia que se originarán entre ambas estaciones. 

**Tareas**

El grupo curso deberá efectuar los cálculos y estudios respectivos para determinar las pérdidas de potencia de cada uno de estos enlaces.
En la primera etapa, se realizarán los cálculos matemáticos correspondientes al radioenlace, lo que se complementará con un estudio a través de un software de modelación de datos.
En la segunda etapa, se realizará el cálculo matemático del enlace de fibra óptica.

**1. Calculo de enlace inalambrico**

Usando su calculadora científica, resuelva el siguiente ejercicio, determinando la pérdida de potencia de este enlace. 

Se debe determinar la atenuación de un enlace de $1\,\text{km}$, entre un transmisor (*TX*) y un receptor (*RX*). El transmisor está conectado a una antena con ganancia $10\,\text{dBi}$, y tiene una potencia de transmisión de $10\,\text{dBm}$. El receptor está conectado a una antena de $12\,\text{dBi}$, y tiene una sensibilidad de recepción de $-82\,\text{dBm}$. La pérdida de los cables y conectores en ambos extremos es $2\,\text{dB}$ cada uno a la frecuencia de 1,2GHz. La potencia recibida en el equipo receptor es $-72\,\text{dBm}$.

**Calcule la pérdida de potencia en el trayecto.**

R:


Usando el modelo Hata, **calcule la pérdida de potencia del mismo enlace**. Este ejercicio le servirá para entender la lógica matemática que sustenta el ejercicio (esquema) del paso anterior, utilizando la siguiente formula
$$
L_{b} = 69,55 + 26,16\,(\log{F}) - 13,82\,(\log{h_{b}}) - a\,(h_{m}) + (44,9 - 6,55\,(\log{h_{b}}))\,\log{d}
$$
Donde
$$
\begin{aligned}
L_b    &&& \text{Pérdida de potencia (atenuación), en dB} \\
F      &&& \text{Frecuencia, en MHz} \\
h_b    &&& \text{Altura de la antena transmisora, en metros} \\
h_m    &&& \text{Altura de la antena receptora, en metros} \\
d      &&& \text{Distancia, en km} \\
a(h_m) &&& \text{Factor de corrección para la antena receptora} \\
& && \qquad a(h_{m}) = (1,1\,(\log{F}) - 0,7)\,h_{m} - (1,56\,(\log{F})-0,8)
\end{aligned}
$$

**2. Calculo Inalambrico usando SW**

Luego de haber realizado los cálculos matemáticos correspondientes, deberán considerar los parámetros entregados y obtenidos para realizar una simulación de estos mismos enlaces mediante el software **Radio Mobile**, determinando la pérdida total del enlace considerando la distancia del enlace de 1 Kilometro. Este ejercicio le servirá para entender los algoritmos que utilizan los software de modelamiento, además de poder realizar un análisis comparativo entre los modelos matemáticos y aquellos modelos automatizados.

**3. Calculos de enlace FO**

Una vez realizado los cálculos de radioenlaces, los alumnos deberán calcular la atenuación total del enlace de Fibra Óptica (Tx y Rx). Los parámetros del enlace de fibra óptica son los siguientes:
- Para el enlace de Fibra Óptica de $1\,\text{km}$, se emplean 5 empalmes con atenuación promedio de $0,2\,\text{dB}$, conectores de transmisión (1) y recepción (1) con atenuación de $0,3\,\text{dB}$ y un cable de reserva que se fija en una atenuación de $0,5\frac{dB}{km}$
- El enlace funciona con una potencia de transmisión de $0\,\text{dBm}$ y un coeficiente de atenuación de $0,70\frac{dB}{km}$

Formula
$$
a_T = (L \times a_L) + (n_e \times a_e) + (n_c \times a_c) + (a_r \times L)
$$
Donde
$$
\begin{aligned}
a_T &: \text{Atenuacion Total} \\
L  &: \text{longitud del cable en Km} \\
a_L &: \text{coeficiente de atenuación en dB/Km} \\
n_e &: \text{número de empalmes} \\
a_e &: \text{atenuación por empalme} \\
n_c &: \text{número de conectores} \\
a_c &: \text{atenuación por conector} \\
a_r &: \text{reserva de atenuación en dB/Km} \\
\end{aligned}
$$

R:

Una vez termminado deberán responder las siguientes preguntas:
- ¿Qué relación existe entre los cálculos matemáticos y la simulación mediante software, del radioenlace?
- ¿Por qué existen diferencias en la atenuación del enlace aéreo y el enlace de FO?
- ¿Cuál enlace es mejor? Fundamente su respuesta, considerando otros aspectos, además de la atenuación.

