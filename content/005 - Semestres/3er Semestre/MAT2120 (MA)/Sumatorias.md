# 1.- Apoyo
[[extras/Adjuntos/G2_MAT2120.pdf]]
[[extras/Adjuntos/Soluciones_G2_MAT2120.pdf]]

---
# 2.- Formulas:
## 2.1- Notacion de sumatoria
$\sum\limits_{i=1}^{n}A_{i}$
$\sum\limits$:Limite
$n$: Posicion Final
$i=1$: Indice Posicion Inicial
$A_{i}$: Sucesion a sumar

---
## 2.2- Sumatoria Aritmetica
$\sum\limits^{n}_{i=1}a_{i}=\dfrac{n}{2}*(2*a_{1}+(n-1)*d)$

![[002 - Ordenar/3er Semestre/MAT2120 (MA)/Actividades/1.1- Sucesiones#2.1- Aritmetica]]

## 2.3- Sumatoria Geometrica
$\sum\limits^{m}_{i=1}b_{i}=\dfrac{b_{1}*(r^{m}-1)}{(r-1)}$

![[002 - Ordenar/3er Semestre/MAT2120 (MA)/Actividades/1.1- Sucesiones#2.2- Geometrica]]

---
# 3.- Ejercicios:
## P1.-
Utilizando notacion de sumatorias, exprese las siguientes sumas  
1.1 $a5 + a6 + a7 + a 8 + a 9 + a10 + a11 + a 12 + a 13$
1.1-R: $\sum\limits_{i=5}^{13}a_{i}$
1.2- $b38 + b39 + b 40 + b 41 + b42 + b43 + b 44 + b 45 + b46 + b47 + b 48$
1.2-R:$\sum\limits_{i=38}^{48}b_{i}$
1.3- $c21 + c22 + c 23 + c 24 + · · · + c 91 + c92 + c 93$
1.3-R:$\sum\limits_{i=21}^{93}a_{i}$

## P2.-
2.1- Considere la sucesion $a_{n}=3n^{2}+7$. Determine:
2.1.1- La suma de los 12 primeros terminos. En el codigo Python, utilice listas para almacenar los terminos de la sucesion a n considerados en la suma.
2.1.1-R:
2.1.2- La suma desde el decimotercer al vigesimocuarto termino. En este caso, idear un codigo Python que sume valores con ciclos for o while pero sin acumularlos en una lista.
2.1.2-R:

## P3.-
Sean (S n ) y (Tn ) las sucesiones de los multiplos de 6 y 13, respectivamente. Se pide  
3.1- Escribir la expresi ́on algebraica del t ́ermino de lugar n para cada sucesi ́on.  
3.2- Calcular las siguientes sumas, usando Python pero sin necesidad de listas  
3.2.1- La suma de los terminos de (Sn ) desde el de posicion 35 hasta el de posicion 61, inclusive.  
3.2.2- La suma de los terminos de (Tn ) desde el de posicion 15 hasta el de posicion 36, inclusive.

## P4.-
3.1.- para el calculo de la nota de presentacion de una asignatura se debe tomar encuentra las tres pruebas parciales, que tienen la misma ponderacion. Ximena obtuvo en estas pruebas notas modeladas por la sucesion cuyo termino generico es $x_{n}=-1,5n^{2}+6n$. Para calcular el promedio, debemos calcular la expresion $\sum\limits_{i=1}^{3}=1^{x_{i}}$
Determine el valor de la expresion anterior mediante un codigo Python e interprete este resultado.

## P5.-
5.1- Para cada una de las siguientes sumatorias
$\sum\limits_{i=1}^{560}(61+7i), \sum\limits_{i=22}^{650}(45+4i), \sum\limits_{i=1}^{54}9300*0,82^{i}, \sum\limits_{i=21}^{80}7500*0,97^{i}$
5.1.1- Calcule su valor determinando en cada bucle el valor del termino con la expresion de la sucesion.
5.1.2- Calcule su valor determinando en cada bucle el valor del termino con alguna expresion recursiva.
5.1.3- Calcule su valor, esta vez aplicando formula de suma aritmetica o geometrica.

## P6.-
Un nuevo canal de YouTube registra 50 visitas la primera semana de funcionamiento, registra 66 visitas la segunda, 82 la tercera y as´ı sucesivamente. Representando por vi la cantidad de visitas en la semana i, a) Escriba la expresion algebraica para el t ´ ermino ´ vi. b) Escriba la expresion algebraica para el n ´ umero total de visitas durante las ´ n primeras semanas. c) Determine Â40 i=1 vi mediante la expresion anterior e interpr ´ etelo. ´ d) Determine Â320 i=120 vi usando bucles en interpretelo. ´ e) Determine usando codigo el valor de ´ n para el que Ân i=1 vi = 2.430. f) Usando bucles, determine la cantidad de visitas registradas desde la semana 41 a la semana 190. Presente este resultado en notacion de sumatoria.

## P7.-
Diego decide ahorrar en dolares para un viaje durante dos a ´ nos y medio. El ˜ primer mes ahorra 900 dolares, el segundo mes 810 d ´ olares, el tercer mes 729 ´ dolares, y as ´ ´ı sucesivamente. Represente por di la cantidad de dolares que ahorra ´ Diego en el mes i. a) Escriba la expresion algebraica para el t ´ ermino ´ di. b) Escriba la expresion algebraica que calcula el total de dinero ahorrado por ´ Diego durante los primeros n meses. c) Determine e interprete Â12 i=1 di. d) Determine e interprete Â30 i=25 di. e) Exprese en notacion de sumatoria y determine el total ahorrado por Diego ´ durante el segundo ano. ˜ f) Exprese en notacion de sumatoria y determine el total ahorrado por Diego ´ para el viaje que tiene programado hacer.

## P8.-
Un electricista tiene un contrato, en donde se especifica que el primer mes el sueldo sera de ´ $500.000 y le ofrecen $2.000 de aumento mensual a modo de incentivo para que no se cambie de empresa. Si si representa el sueldo que recibe el electricista en el mes i, a) Escriba la expresion algebraica para el t ´ ermino ´ si. b) Escriba la expresion algebraica que calcula el total de dinero que percibe ´ el electricista por concepto de sueldo, durante los n primeros meses de contrato. c) Escriba un codigo Python, que involucre bucles, y que calcule el total de ´ dinero recibido por el electricista durante los meses de su tercer ano de ˜ contrato. d) Exprese en notacion de sumatoria el total aludido en la parte ´ c). Determine este resultado en un codigo Python aplicando f ´ ormulas de suma aritm ´ etica ´ o geometrica, seg ´ un sea el caso. ´ e) Determine e interprete Â24 i=13 si.

## P9.-
Una casa se arrienda por 2 anos, considerando que durante este periodo el arriendo ˜ se incrementara todos los meses en un ´ 2 %. Considere que ai representa el valor a pagar por el arriendo del mes i. Si para el primer mes de arriendo se debe pagar $180.000, entonces: a) Escribir la expresion algebraica para el t ´ ermino ´ ai. b) Escriba la expresion algebraica que calcula el total cancelado por el arriendo ´ correspondiente a los n primeros meses. c) Escriba un codigo Python, que involucre bucles, y que calcule el total de ´ dinero cancelado por el arriendo correspondiente a los meses del primer semestre del segundo ano. Al redactar su respuesta, fuera del c ˜ odigo, utilice ´ notacion de sumatoria. ´ d) Exprese en notacion de sumatoria el total aludido en la parte ´ c). Determine este resultado en un codigo Python aplicando f ´ ormulas de suma aritm ´ etica ´ o geometrica, seg ´ un sea el caso. ´ e) Determine e interprete Â24 i=19 ai.