# Info
## Imagen Topologia
![1000](https://slink.proxylivy.work/image/46af1a26-0274-4b8d-9163-a37ba375c53f.png)
## Sobre
Resolviendo Problemas de MP-BGP en una Red
## Requerimientos
**Ticket 1: EIGRP en IPv4/IPv6**

- Revisar el funcionamiento de EIGRP a nivel IPv4-IPv6. Todo pertenece al AS1.
- Revisar y mejorar configuración de interfaces pasivas.
- Equipo de 172.16.22.0/24 debe poder utilizar ambos caminos disponible para llegar hacia red OSPF.
- Documente todos los problemas detectados.

**Ticket 2: OSPF en IPv4/IPv6**

- Revisar el funcionamiento de OSPF a nivel IPv4-IPv6. 
- Revisar y mejorar configuración de interfaces pasivas.
- Garantizar salida hacia Internet a nivel IPv4-IPv6.
- Documente todos los problemas detectados.

**Ticket 3: MP-BGP**

- Revisar el funcionamiento de MP-BGP. 
- Revisar que los routers tengan next-hop válido a nivel de tabla MP-BGP
- Se solicita que las loopback de R5 de vean sumarizadas a nivel de MP-BGP.
- Documente todos los problemas detectados.

**Ticket 4: Manipulación de Rutas**

- Revisar funcionamiento de redistribución a nivel de IPv4-IPv6.
- Se ha solicitado que solo R1 vea la loopback 11.
- Se ha solicitado redistribuir protocolo IGP y EGP mediante route-maps.
- Se ha solicitado manipular el atributo AS-PATH permitiendo que a nivel de IPv4 tenga 2 AS adicionales cuando apunte a sucursal remota, y de tres saltos cuando apunte a casa matriz en IPv6.
- Comprobar conectividad completa.
# Configuracion
