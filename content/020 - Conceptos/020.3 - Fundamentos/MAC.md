# Info
**M**edium **A**cess **C**ontrol controla el hardware que se conecta a internet

## Formato
Tiene 48 Bits y se escribe en Notacion Hexadecimal
- `00-11-22-33-44-55`
	- `00-11-22`: OUI (MA-x) 6 digitos HEX (24 digitos binarios)


Los primeros 6 digitos HEX (24 digitos binarios) corresponden al OUI (Organizationally Unique Identifier), el cual es un identificador para detectar el fabricante de la MAC, mas detalles:
- MA-L (OUI): MAC Address Block Large -> 2\^24 ~= 16 Million
- MA-M: MAC Address Block Medium -> 2\^20 ~= 1 Million
- MA-S (OUI-36): Mac Address Block Small -> 2\^12 ~= 4,096

Fuente: 
- [RFC9542](https://www.rfc-editor.org/rfc/rfc9542.html)
- https://standards.ieee.org/products-programs/regauth/oui/