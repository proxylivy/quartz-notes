# Info
**I**nternet **P**rotocol v6, Basado en [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IP|IP]], se diseño para resolver las limitaciones de [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv4|IPv4]], aumentando el espacio de direcciones de 32 a 128 bits. Esto permite contar con $2^{128}=340.282.366.920.938.463.463.374.607.431.768.211.456$ direcciones unicas, aproximadamente 340 sextillones. Cada direccion IPv6 se compone de 8 grupos (Hextetos), y cada Hexteto tiene 4 digitos hexadecimales equivalente a 16 bits.

La mitad de los bits son para redes publicas y la otra mitad para red publica, cada espacio tendra un espacio de $2^64=18.446.744.073.709.551.616$ IPv6 Disponibles

IPv6 no distingue entre mayusculas y minusculas, su notacion es Hexadecimal

0-9: Mantienen sus valores
A: 10
B: 11
C: 12
D: 13
E: 14
F: 15

Ademas, existen reglas de simplificacion:
- Se pueden eliminar los ceros a la izquierda en cada hexteto
	- Por ejemplo, "`2001:00cd:0f0e::0/128`" es lo mismo que "`2001:cd:f0e::0/128`"
- Se pueden Usar "::" una sola vez por direccion para representar grupos consecutivos de ceros.
	- Por Ejemplo, "`2001:cd::0/128`" implica que hay hextetos de "0000" omitidos en medio.

Este dibujo esta disponible en [Excalidraw](https://excalidraw.com/#json=AKcY3AQk_eNB-MDLoE1Du,SVkOte64SL7QGg2Zh9I4nw)
![](https://slink.proxylivy.work/image/2e26d3e5-43a4-4054-b3b2-9425ee2a28c9.svg)

Migrar a esta nueva arquitectura es crucial para poder continuar expandiendo internet, existen 3 categorias
1. Dual-Stack: Coexistencia de IPv4 e IPv6 en las misma red, ejecutan ambos protocolos de manera simultanea
2. Tunneling: Transportar paquetes IPv6 a travez de redes IPv4 a travez de la encapsulacion IPv4
3. Translation: Usar NAT64 para comunicar dispositivos IPv4 con dispositivos IPv6 mediante tecnicas similares a NAT

## Estructura IPv6
Link Local es una direccion que se mantiene en la interfaz o enlace sin modificar necesariamente la ip global de ese enlace

## Asignacion
### Estatica
Usa los valores
- Direccion IPv6
- Prefijo de subred 
- Gateway Predeterminado 



# Configuracion
## IPv6
> Nota: Las ipv6 se van apilando y usando segun sea necesario
```
ipv6 address [ipv6/prefix]
ipv6 address [ipv6/prefix]
```

# Visualizacion
`show ipv6 int brief`: Ver IPv6 asignados a una interfaz

# Extra
RFC Interesantes
- Enterprise IPv6 Deployment Guidelines | [RFC7381](https://www.rfc-editor.org/rfc/rfc7381)


