# Info
La sumarizacion de redes es el proceso de combinar varias subredes contiguas en una sola red de mayor tamaño. De esta forma poder reducir la cantidad de rutas que guarda RIB, optimizando el proceso de seleccion de rutas 

## Funcionamiento General
Agrupas varias subredes que comparten un prefijo comun, determinando un espacio donde abarque todas las subredes.
1. Reconoce el octeto donde empiezan a cambiar las subredes IP
2. Convierte esa seccion de octeto de IP a Binario
3. Define la nueva mascara de red


## Ejemplo IPv4
Tenemos las siguientes redes
- 192.168.10.0/24
- 192.168.11.0/24
- 192.168.12.0/24

Paso 1: Podemos ver, que los dos primeros octetos son iguales, desde el tercero cambia, por lo que esa seccion la convertimos en binario marcando con x.

| IPs            | 128 | 64  | 32  | 16  | 8   | Linea de Corte | 4   | 2   | 1   |
| -------------- | --- | --- | --- | --- | --- | -------------- | --- | --- | --- |
| 192.168.(10).0 |     |     |     |     | x   | /              |     | x   |     |
| 192.168.(11).0 |     |     |     |     | x   | /              |     | x   | x   |
| 192.168.(12).0 |     |     |     |     | x   | /              | x   |     |     |

**Paso 2**: Comparamos las direcciones en binario y buscamos el punto donde los bits empiezan a cambiar. La "Linea de Corte" va justo en ese punto. Todo lo que este antes del cambio se convierte en la parte fija de la direccion sumarizada, lo que esta despues, se ignora (valor 0).

**Paso 3**: Define la nueva marcada de red. Cada octeto tiene 8 bits, asi que si cuentas desde el 3er octeto (empezando en /16), suma los bits que no han cambiado hasta la linea de corte, solo suma los bits continuos sin interrupciones.

La red final sera: 192.168.8.0/21


---
## IPv6
La teoria de IPv6 es la misma de IPv4 pero la forma practica cambia

## Definiciones a Considerar
- **Hexteto**: 16 bits (Representado en 4 digitos Hexadecimales)
- IPv6 tiene una estructura de 8 Hextetos, lo que nos da 128 bits ($8\times16=128$)
Al contar en IPv6 trato cada Hexteto como unidades de 16 bits, lo que me ayuda a identificar la longitud de la mascara de prefijo de manera similar a IPv4

### Valores Hexadecimales IPv6
0-9: Mantienen sus valores

| A   | 10  |
| --- | --- |
| B   | 11  |
| C   | 12  |
| D   | 13  |
| E   | 14  |
| F   | 15  |
## Ejemplo IPV6
Tenemos las siguientes redes
- 3001:ABCD:ABCD:A::/64
- 3001:ABCD:ABCD:B::/64
- 3001:ABCD:ABCD:C::/64

Paso 0: Para no confundirte, puedes re-escribir las redes el hexteto "Completo", debido a las cualidades de IPv6, los 0 se pueden omitir si estan a la izquierda
- 3001:ABCD:ABCD:000A::/64
- 3001:ABCD:ABCD:000B::/64
- 3001:ABCD:ABCD:000C::/64

Paso 1: Podemos ver que los 3 primeros Hextetos son iguales, desde el 4to cambian, por lo que **convertimos en binario cada digito hexadecimal (4 Bits)**.

| IPv6                     | 8   | 4   | 2   | 1   | \|  | 8   | 4   | 2   | 1   | \|  | 8   | 4   | 2   | 1   | \|  | 8   | Linea de Corte | 4   | 2   | 1   | \|  |
| ------------------------ | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | -------------- | --- | --- | --- | --- |
| 3001:ABCD:ABCD:000A::/64 |     |     |     |     | \|  |     |     |     |     | \|  |     |     |     |     | \|  | x   | /              |     | x   |     | \|  |
| 3001:ABCD:ABCD:000B::/64 |     |     |     |     | \|  |     |     |     |     | \|  |     |     |     |     | \|  | x   | /              |     | x   | x   | \|  |
| 3001:ABCD:ABCD:000C::/64 |     |     |     |     | \|  |     |     |     |     | \|  |     |     |     |     | \|  | x   | /              | x   |     |     | \|  |

Paso 3: Comprobamos las direcciones en binario y buscamos el punto donde los bits empiezan a cambiar. La "Linea de Corte" va justo en ese punto.
Todo lo que este antes del cambio, se convierte en la parte fija de la direccion sumarizada, lo que esta despues, se ignora  (Valor 0).

Paso 4: Defines la nueva marca de red. Cada Hexteto tiene 16 bits separado en 4 Digitos Hexadecimales, asi que si cuentas desde el 4to hexteto (empiezas en /48), suma los bits que no han cambiado hasta la linea de corte, solo suma los bits contiguos sin interrupciones (Fila de X sin cambios).

La red Sumarizada es: 3001:ABCD:ABCD:8::/61