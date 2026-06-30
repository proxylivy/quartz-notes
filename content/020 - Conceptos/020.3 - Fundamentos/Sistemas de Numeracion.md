Es el conjunto de simbolos y reglas que permiten contruir todos los numeros validos de ese sistema, ademas de realizar operaciones y establecer relaciones con ellos

Sistemas
- Binario: Usa dos simbolos
	- 0, 1
- Octal: Usa ocho simbolos 
	- 0, 1, 2, 3, 4, 5, 6, 7
- Decimal: Usa diez simbolos
	- 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
- Hexadecimal: Usa dieciseis simbolos
	- 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F}

## Conversion
La conversion entre sistemas consiste en reinsterpretar un numero bajo las reglas de otra base. Por ejemplo, pasar un numero binario (Base 2) a decimal (Base 10) implica entender como funcionan las potencias de la base en cada posicion del numero.

### Tabla Conversion

| Hexadecimal | Decimal | Binario (8 Bits) |
| ----------- | ------- | ---------------- |
| 0           | 0       | 00000000         |
| 1           | 1       | 00000001         |
| 2           | 2       | 00000010         |
| 3           | 3       | 00000011         |
| 4           | 4       | 00000100         |
| 5           | 5       | 00000101         |
| 6           | 6       | 00000110         |
| 7           | 7       | 00000111         |
| 8           | 8       | 00001000         |
| 9           | 9       | 00001001         |
| A           | 10      | 00001010         |
| B           | 11      | 00001011         |
| C           | 12      | 00001100         |
| D           | 13      | 00001101         |
| E           | 14      | 00001110         |
| F           | 15      | 00001111         |


### Binario a Decimal
En el sistema binario, cada digito representa una potencia de 2
- El bit mas a la derecha (Posicion 0) representa $2^0=1$
- EL bit siguiente a la izquierda (Posicion 1) representa $2^1=2$
- El siguiente a la izquierda (Posicion 2) representa $2^2=4$
- Y asi sucesivamente: $2^3=8$, $2^4=16$, $2^5=32$, $2^6=64$, $2^7=128$, $2^8=256$

Paso 1: Identifica cada bit de derecha a izquierda
Paso 2: Asigna la potencia de 2 que corresponde a su posicion
Paso 3: Multiplica cada bit por la potencia 2 asignada a su posicion
Paso 4: Suma todos los productos hasta resultar en el numero decimal

Ejemplo Numero Binario: "`1 0 1 1 0 0`"

| Bit                     | 1        | 0        | 1       | 1       | 0       | 0       |
| ----------------------- | -------- | -------- | ------- | ------- | ------- | ------- |
| Posicion                | 5        | 4        | 3       | 2       | 1       | 0       |
| Potencia de 2           | $2^5=32$ | $2^4=16$ | $2^3=8$ | $2^2=4$ | $2^1=2$ | $2^0=1$ |
| Multiplica el valor "1" | 32       | 0        | 8       | 4       | 0       | 0       |
Suma Total: 32+0+8+4+0+0=44

## Operacion AND IPv4
Se usan direcciones de Red
Debes escribir la direccion IP en binario y la submascara de red y luego revisar los numeros solapados con "1" que se mantienen en "1", los diferentes quedan en "0" al igual que dos "0s"

Direccion IP: 192.168.10.200
Mascara: 255.255.255.224 o /27
```
    11000000.10101000.00001010.11001000
AND 11111111.11111111.11111111.11100000
=   11000000.10101000.00001010.11000000
```

Al convertir de Binario a Decimal da
- 1er Octeto: 11000000 -> 192
- 2do Octeto: 10101000 -> 168
- 3er Octeto: 00001010 -> 10
- 4to Octeto: 11000000 -> 192

La direccion de red es: 192.168.10.192

