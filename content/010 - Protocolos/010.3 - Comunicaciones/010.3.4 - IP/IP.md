# Info
**I**nternet **P**rotocol, permiten ubicar dispositivos en una red y permiten que se comuniquen entre si para intercambiar datos, su asignacion puede ser estatica (Manual) o mediante [[010 - Protocolos/010.3 - Comunicaciones/DHCP|DHCP]]

Versiones
- [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv4|IPv4]]
- [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv6|IPv6]]

Existen distintas entidades que se encargan de distribuir estas IPs entre los [[020 - Conceptos/020.3 - Fundamentos/ISP|ISP]]
- [IANA](https://www.iana.org/): Mundial
- [ARIN](https://www.arin.net/): Norteamerica
- [LACNIC](https://www.lacnic.net/): Sudamerica
- [RIPE NCC](https://www.ripe.net/): Europa
- [AfriNIC](https://afrinic.net/): Africa
- [APNIC](https://www.apnic.net/): Asia

## Transmision
Para poder comunicarse dentro de una red, tienen 3 formas
- Unicast: Enviar un paquete de un host a otro host individualmente
- Broadcast: Enviar un paquete de un host a todos los host de la red
- Multicast: Enviar un paquete de un host a un grupo seleccionado de hosts

## Tipos de Direcciones
- Direccion de Red (Network): Segmento de red que representa el conjunto de dispositivos. Ejemplo: `192.168.10.0/24` o `2001::0/16`
- Direccion de Host: Rango de IP asignables dentro de la direccion de red y varian entre dispositivos. Ejemplo: `192.168.10.1 - 192.168.10.254`
- Direccion de Broadcast: Es la ultima IP de un segmento de direccion que indica el termino de una red, la cual no es asignable a un dispositivo final. Ejemplo: `192.168.10.255`
