# Info
Es la forma que se comunica el internet, el protocolo mas usado
Usa subredes en forma de [[020 - Conceptos/020.3 - Fundamentos/Mascara|Mascara]] para encontrar otras ip

## Estructura IPv4
- Formato: 32 Bits (Binario) -> 4 bytes (Decimal)
- Encabezado minimo 20 Bytes
## Clases de IPv4 Privada
explicado en [RFC1918]([https://www.rfc-editor.org/rfc/rfc1918](https://www.rfc-editor.org/rfc/rfc1918))
### Clase A
Usado en Hosts
0.0.0.0 - 127.255.255.255
Mascara 8 Bits
### Clase B
Usado en Hosts
128.0.0.0 - 191.255.255.255
Mascara 16 Bits
### Clase C
Usado en Hosts
192.0.0.0 - 223.255.255.255
Mascara 24 Bits
### Clase D
224.0.0.0 -> 239.255.255.255
Usado para Multicast
### Clase E
240.0.0.0 - 255.255.255.255
Usado de forma Experimental
### Extra: Localhost
127.0.0.0 - 127.255.255.255

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