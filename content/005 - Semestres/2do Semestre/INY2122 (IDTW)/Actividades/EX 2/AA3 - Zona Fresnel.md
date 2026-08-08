# Info

Tal como se estudio en [[005 - Semestres/1er Semestre/SRY1132 (SRAA)/SRY1132 (SRAA)|SRY1132 (SRAA)]], en la actividad [[005 - Semestres/1er Semestre/SRY1132 (SRAA)/Actividades/EX 2 - Realiza Cableado|EX 2 - Realiza Cableado]], las señales de [[020 - Conceptos/020.5 - Red HFC/Ondas|Ondas]] inalambricas se ven afectadas por la difraccion. Tambien se vera la [[020 - Conceptos/020.5 - Red HFC/Zona de Fresnel|Zona de Fresnel]]

## Parte 1

Analiza los siguientes Anexos:
- Ondas de Luz
	- [Universidad de Valencia - Jose Marquez - Difraccion de Ondas](https://www.uv.es/jmarques/_private/Teor%C3%ADa%20m.a.s.%20y%20ondas%20.pdf)
	- [Khan Academy - Doble ranura de Young - Ondas de Luz](https://youtu.be/F5uuAQprw84?si=Kfg4bIPnBTPwxRMh)
- Zona Fresnel
	- [Prored - Zonas de Fresnel](https://www.prored.es/zonas-de-fresnel-en-un-radioenlace/)
	- [Wikipedia - Zona de Fresnel](https://es.wikipedia.org/wiki/Zona_de_Fresnel)
	- [Youtube - Adelina Alcantar - Calculo de Zona Fresnel](https://youtu.be/O35fHh2zDUo?si=xJHKMCNMuzsziCc2)

## Parte 2

Debes crear un informe sobre el procedimiento detallado para el calculo de la zona de Fresnel, agregando los siguientes aspectos
- Definición de difracción de una onda (Incorporar imágenes representativas)
- Consecuencias de la difracción en un enlace de radio
- Zonas de Fresnel (Incorporar imágenes representativas)
- Consecuencias en la comunicación inalámbrica al existir obstáculos que invaden la Zona de Fresnel
- Porcentajes de despejes de las zonas de Fresnel (Incorporar ejemplos aclaratorios)
- Resolución de problema relacionado con la Zona de Fresnel: en base al esquema de la figura, calcule los metros de despeje de la primera zona de Fresnel (entre la LOS y la cima del obstáculo) en el sitio donde se encuentra el obstáculo. Calcule, también, el radio máximo de la primera, segunda y tercera zona de Fresnel (en el centro del enlace). Incorpore un esquema representativo del enlace con las tres zonas de Fresnel calculadas. Considere: 
	- Distancia entre emisor y receptor: 3,25Km
	- Distancia desde el transmisor hasta el obstáculo: 2,5Km
	- Despeje de 80% en zona del obstáculo
	- Frecuencia de transmisión: 450MHz

### Calculo

Datos
- $d_{1}+d_{2}=3,25\,km=3250\,m$
- $d_{1}=2,5\,km=2500\,m$
- $d_{2}=3250-2500=750\,m$
- Despeje Requerido: 80%
- $f=450\,\text{MHz}$

Calcula $\lambda$
$$
\dfrac{3\cdot 10^{8}}{450\cdot 10^{6}}=0,6\overline{6}\,\text{m/ciclo}
$$

**Radio 1ra Zona de Fresnel**

$$
\begin{align*}
r_{1} &= \sqrt{\lambda \cdot \dfrac{d_{1}\cdot d_{2}}{d_{1}+d_{2}}} \\[0.5em]
&= \sqrt{0{,}6\overline{6}\cdot\dfrac{2500\ \text{m}\cdot 750\ \text{m}}{2500\ \text{m}+750\ \text{m}}} \\[0.5em]
&= \sqrt{0{,}6\overline{6}\cdot\dfrac{1{,}875{,}000}{3250}} \\[0.5em]
&= \sqrt{0{,}6\overline{6}\cdot 576{,}92} \\[0.5em]
&= \sqrt{384{,}6} \\[0.5em]
&= 19{,}6 \, \text{m}
\end{align*}
$$

Despeja el 80%
$$
0{,}80 \cdot 19{,}6 \, \text{m}=15{,}69 \, \text{m} \\
$$


**3 Zonas de Fresnel**

Calcula el termino comun $\dfrac{d_{1}\cdot d_{2}}{d_{1}+d_{2}}$
$$
\begin{gather}
\frac{1625\cdot1625}{3250} \\[0.5em]
\frac{2{,}640{,}625}{3250} \\[0.5em]
812{,}5
\end{gather}
$$

Ya que tenemos el termino, comun, la formula queda como $r_{n} = \sqrt{n\cdot\lambda\cdot 812{,}5}$

Zona 1 ($r_{1}$)
$$
\begin{gather}
\sqrt{1\times 0{,}6\overline{6}\times 812{,}5} \\[0.5em]
\sqrt{541{,}67} \\[0.5em]
23{,}27\ \text{m}
\end{gather}
$$

Zona 2 ($r_{2}$)
$$
\begin{gather}
\sqrt{2\times 0{,}6\overline{6}\times 812{,}5} \\[0.5em]
\sqrt{1083{,}33} \\[0.5em]
32{,}91\ \text{m}
\end{gather}
$$

Zona 3 ($r_{3}$)
$$
\begin{gather}
\sqrt{3\times 0{,}6\overline{6}\times 812{,}5} \\[0.5em]
\sqrt{1625} \\[0.5em]
40{,}31\ \text{m}
\end{gather}
$$
