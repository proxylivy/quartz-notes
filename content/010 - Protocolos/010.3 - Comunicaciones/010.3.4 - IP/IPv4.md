# Info
**I**nternet **P**rotocol v4, basado en [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IP|IP]], fue implementado en 1983 como parte del proyecto ARPANET. Emplea direcciones de 32 bits en formato Binario que equivalen a un total de $2^{32}=4.294.967.296$ direcciones disponibles. En notacion Decimal, cada octeto oscila entre 0 y 255 (por ejemplo, 255.255.255.255).
Para segmentar redes y subredes, IPv4 utiliza la [[020 - Conceptos/020.3 - Fundamentos/Mascara|Mascara]] de red, que delimita que parte de la direccion corresponde a la red y cual al host

## Rangos IP Privados
Una Direccion IP Privada no son enrutables en Internet y se usan para redes Locales (LAN)
### Rangos Clasificados
Historicamente, los rangos IPv4 se clasificaron en "Clases", como se describe en el [RFC1918](https://www.rfc-editor.org/rfc/rfc1918). Sin Embargo, hoy se usa mas a menudo CIDR (Classless Inter-Domain Routing). Aun asi, se muestran a continuacion los rangos privados tradicionales
- Clase A
	- 10.0.0.0 - 10.255.255.255
	- Mascara Minima: 8 Bits
	- Hosts Maximos: $2^{24}-2=16.777.214$
- Clase B
	- 172.16.0.0 - 172.31.255.255
	- Mascara Minima: 12 Bits
	- Hosts Maximos: $2^{20}-2=1.048.576$
- Clase C
	- 192.168.0.0 - 192.168.255.255
	- Mascara Minima: 16 Bits
	- Hosts Maximos: $2^{16}-2=65.534$

### Rangos Especiales
Existen rangos especificos y reservados para fines especificos, pueden ser consultados en el [RFC6890](https://datatracker.ietf.org/doc/html/rfc6890) e [IANA - IPv4 Special-Purpose Address Registry](https://www.iana.org/assignments/iana-ipv4-special-registry/iana-ipv4-special-registry.xhtml)

Cuando un Host no obtiene respuesta de un servidor DHCP, se asigna una direccion Link-Local (APIPA), definido en el [RFC3927](https://datatracker.ietf.org/doc/html/rfc3927):
- APIPA o Clase B Simple
	- 169.254.0.0 - 169.254.255.255
	- Mascara Minima: 16 Bits
	- Host Maximos: $2^{16}-2=65.534$

Para paliar la falta de direcciones IPv4 en 2012, la IANA asigno el siguiente rango definido en el [RFC6598](https://datatracker.ietf.org/doc/html/rfc6598) para la implementacion de CG-NAT
- Espacio Compartido
	- 100.64.0.0 - 100.127.255.255
	- Mascara Minima: 10 Bits
	- Hosts Maximos: $2^{22}-2=4.194.302$

El [RFC5737](https://datatracker.ietf.org/doc/html/rfc5737) explica las direcciones de documentacion
- TEST-NET-1
	- 192.0.2.0 - 192.0.2.255
	- Bits Minimos: 24 Bits
	- Host Maximos: $2^{8}-2=254$
- TEST-NET-2
	- 198.51.100.0/24 - 198.51.100.255
	- Bits Minimos: 24 Bits
	- Host Maximos: $2^{8}-2=254$
- TEST-NET-3
	- 203.0.113.0/24 - 203.0.113.255
	- Bits Minimos: 24 Bits
	- Host Maximos: $2^{8}-2=254$

El [RFC3171](https://datatracker.ietf.org/doc/html/rfc3171) explica la Clase D
- Espacio Multicast
	- 224.0.0.0 - 239.255.255.255
	- Bits Minimos: 4 Bits
	- Host Maximos: $2^{28}-2=268.435.454$

La Clase E es nombrada ligeramente en el [RFC1112 - Section 4](https://www.rfc-editor.org/rfc/rfc1112.html)
- Espacio Reservado para Experimentacion por la IETF
	- 240.0.0.0 - 255.255.255.255
	- Bits Minimos: 4 Bits
	- Host Maximos: $2^{28}-2=268.435.454$

La Red Localhost se creo para usarse como Loopback siendo exactamente 127.0.0.1/32 para enviar informacion a si mismos, no tiene RFC Exacto pero se nombra [RFC1122 - Section 3.2.1.3 - Page 29](https://www.rfc-editor.org/rfc/rfc1122#page-29)
- 127.0.0.0 - 127.255.255.255
- Bits Minimos: 8 Bits
- Hosts Maximos: $2^{24}-2=16.777.214$

# Configuracion
## IPv4 Router
> Nota: secondary deja una segunda ip disponible para usar
```
int [int S/S/P]
ip address [ipv4] [dec-mask]
ip address [ipv4] [dec-mask] secondary
```

# Visualizacion
`show ip int brief`: Ver IPv4 asignadas a una interfaz

# Extra
IPv4 a formato Decimal, puede ser usado para programas especificos o en bases de datos

Siguen la siguiente formula
$(\text{Oct1}\times{256^3})+(\text{Oct2}\times{256^2})+(\text{Oct3}\times{256^1})+(\text{Oct4}\times{256^0})$

Ejemplo con 192.168.1.1
$(192\times{256^3})+(168\times{256^2})+(1\times{256^1})+(1\times{256^0})=3232235777$

Para pasar de Formato Decimal a IPv4

Sigue la siguiente Formula (Dec es el resultado decimal)
$\text{Oct1}=(\dfrac{\text{Dec}}{256^3})$
$\text{Oct2}=\text{Dec}-(\dfrac{\text{Oct1}\times256^3}{256^2})$
$\text{Oct3}=\text{Dec}-\dfrac{(\text{Oct1}\times256^3)-(\text{Oct2}\times256^2)}{256^1}$
$\text{Oct4}=\text{Dec}-(\text{Oct1}\times256^3)-(\text{Oct2}\times256^2)-(\text{Oct3}\times256^1)$