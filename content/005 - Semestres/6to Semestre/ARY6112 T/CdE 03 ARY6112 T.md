# Info
## Imagen Topologia
![1000](https://slink.proxylivy.work/image/c465592e-6871-425b-b80c-b59c345452e8.png)
## Contexto
Usted trabaja en la empresa “CASO 3 LTDA.”, la cual se encarga de prestar servicios de artículos de peras y plantaciones de perales en el país. El éxito en los últimos años ha sido tan grande, que la empresa ha tenido un crecimiento explosivo. Por tal motivo, usted como especialista de redes, deberá revisar el funcionamiento de esta red para que pueda quedar en un óptimo funcionamiento y eficiencia.
## Requerimientos
### A. Ticket 1: PPP Multilink
- La empresa ha solicitado revisar el funcionamiento de los PPP - [[010 - Protocolos/010.1 - Routing/Multilink|Multilink]] que se encuentran conectados entre dos sucursales y el backbone [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP|EIGRP]]. **No se solicita autenticación.**
- Realice las configuraciones pertinentes para solucionar los problemas.
- Debe documentar todos los errores encontrados en la hoja de documentación, señalando de forma correcta lo solicitado.

### B. Ticket 2: Capa 2
- La empresa tiene varios [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]] repartidos en varias sucursales. Es importante poder revisar el funcionamiento, realizar correcciones y realizar mejoras. Entre las cuales, se incluye que debe existir [[010 - Protocolos/010.2 - Switching/Seguridad de Puerto|Seguridad de Puerto]] para un máximo de 3 host. En el caso que se viola la seguridad, la interface se debe apagar.
- Realice las configuraciones pertinentes para solucionar los problemas.
- Debe documentar todos los errores encontrados en la hoja de documentación, señalando de forma correcta lo solicitado.

### C. Ticket 3: Enrutamiento IGP
- La empresa le ha solicitado revisar el funcionamiento de los protocolos IGP. Según informaciones requeridas, los temporizadores son por defecto.
- Las redistribuciones deben ser mediante mapas de rutas. | [[020 - Conceptos/020.1 - Administracion/Route-Map|Route-Map]]
- Se ha solicitado, que el backbone EIGRP sea configurado con autenticación mediante SHA-256. | [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP#Autenticacion|EIGRP]]
- Realice las configuraciones pertinentes para solucionar los problemas.
- Debe documentar todos los errores encontrados en la hoja de documentación, señalando de forma correcta lo solicitado.

### D. Ticket 4: Enrutamiento EGP
- Se ha solicitado revisar el funcionamiento de protocolo [[020 - Conceptos/020.3 - Fundamentos/EGP|EGP]] en la red. Toda sesión será mediante IP de siguiente salto. Los hosts deben participar como prefijos.
- Se ha solicitado que ISP, pueda utilizar a R13 (como PATH preferido) para esto asegurar influencia mediante route-maps. | [[020 - Conceptos/020.1 - Administracion/Route-Map|Route-Map]]
- Realice las configuraciones pertinentes para solucionar los problemas.
- Debe documentar todo

### E. Ticket 5: MPLS
- La empresa tiene implementado un backbone [[010 - Protocolos/010.1 - Routing/MPLS|MPLS]]. Se ha solicitado verificar la generación de etiquetas dentro de esta ubicación.
- Compruebe la generación de etiquetas, donde se solicita, cambiar el rango para un seguimiento más específico.
- Realice las configuraciones pertinentes para solucionar los problemas.
- Debe documentar todos los errores encontrados en la hoja de documentación, señalando de forma correcta lo solicitado.

### F. Ticket 6: Enrutamiento EGP (VPN MPLS)
- La empresa desea implementar una [[010 - Protocolos/010.1 - Routing/MPLS#Implementar MP-BGP y Montar VPNv4|VPNv4]] entre sucursales Pera Menor 1 y Pera Menor 2. Por tal motivo, es importante realizar las configuraciones pertinentes, para que ambas sucursales puedan comunicarse mediante esta forma, a través del backbone [[010 - Protocolos/010.1 - Routing/MPLS|MPLS]].
- Realice las configuraciones pertinentes para solucionar los problemas.
- Debe documentar todos los errores encontrados en la hoja de documentación, señalando de forma correcta lo solicitado.

### G. Ticket 7: DMVPN
- Implementar solución de [[999 - Archivado/DMVPN|DMVPN]] de Fase 2, entre Sucursales Perita 1 y Perita 2, hacia "Sucursal Pera Mayor". Es importante que las conexiones se encuentren cifradas.
- Compruebe el funcionamiento de túnel y que exista conectividad entre las sucursales mediante el túnel.
- Debe documentar todos los errores encontrados en la hoja de documentación, señalando de forma correcta lo solicitado.

### H. Ticket 8: Conectividad Equipos Finales
- Permitir que los equipos finales de las sucursales, puedan llegar al ISP, mediante IP pública (debe enviar captura de pantalla)
- Los equipos deben poder llegar a `www.caso3.cl` (`8.8.8.8`) (debe enviar captura de pantalla)
- Debe documentar todos los errores encontrados en la hoja de documentación, señalando de forma correcta lo solicitado.

# Configuracion
## Equipo: CASO3

- Problema Detectado (Descripcion Breve): Le falta configuracion iBGP con ISP

| Comando para encontrar el problema       | Comando Aplicado                                                                        |
| ---------------------------------------- | --------------------------------------------------------------------------------------- |
| show ip bgp summary<br>show ip protocols | CASO3(config)#router bgp 10000<br>CASO3(config-router)#neighbor 8.8.8.1 remote-as 10000 |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---
## Equipo: ISP

- Problema Detectado (Descripcion Breve): Falta Vecindad BGP con CASO3 y compartir rutas

| Comando para encontrar el problema       | Comando Aplicado                                                                                                                             |
| ---------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| show ip bgp summary<br>show ip protocols | ISP(config)#router bgp 10000<br>ISP(config-router)#neighbor 8.8.8.8 remote-as 10000<br>ISP(config-router)#network 8.8.8.0 mask 255.255.255.0 |

---
- Problema Encontrado (Descripcion Breve): Falta IPv6 hacia R13 en interfaz e0/0

| Comando para encontrar el problema | Comando Aplicado                                                            |
| ---------------------------------- | --------------------------------------------------------------------------- |
| show ipv6 int brief                | ISP(config)#int e0/0<br>ISP(config-if)#ipv6 address 2019:ACAD:ACAD:3::2/112 |

---
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---
