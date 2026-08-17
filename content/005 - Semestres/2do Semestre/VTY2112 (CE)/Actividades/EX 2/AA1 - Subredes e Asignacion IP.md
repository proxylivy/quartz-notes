# Info

Anexos Disponibles en [Copyparty - EX 2 - AA1](https://copyparty.proxylivy.work/Duoc/Conectividad%20y%20Redes/2do%20Semestre/VTY2112%20(CE)/Actividades/EX%202/AA1/)
- [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv4|IPv4]]
- [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv6|IPv6]]

# Parte 1

Objetivo: Segmentar una red utilizando cálculo de subredes

Anexos
- [Youtube - Nicolas Contador - VLSM](https://youtu.be/VN6TBB30Ekk?si=VGKVsbZ7KRtOIj4Z)
- [VLSM Calc](https://vlsmcalc.vercel.app/)

Escenario: La empresa CASE S.A necesita optimizar la asignación de redes y le ha solicitado implementar una solución de subredes. 

**Direccionamiento y Calculo de IPv4/IPv6**

Calcule subred IPv4 e IPv6 de los siguientes segmentos.
- IPv4: 10.0.0.0/20
- IPv6: 2019:ACAD:ACAD::/64

| Red   | Subred |
| ----- | ------ |
| LAN-A | 8      |
| LAN-B | 7      |
| LAN-C | 13     |
| WAN-1 | 2      |
| WAN-2 | 4      |

Asigne direccionamiento IPv4 e IPv6 a los equipos de acuerdo al siguiente direccionamiento IP.


| Dispositivo | Interfaz | IPv4      | Mascara     | IPv6                 | Prefijo |
| ----------- | -------- | --------- | ----------- | -------------------- | ------- |
| PC0         | NIC      | 10.x.x.10 | 255.240.0.0 | 2019:ACAD:ACAD:X::10 | /64     |
| PC1         | NIC      | 10.x.x.30 | 255.240.0.0 | 2019:ACAD:ACAD:X::30 | /64     |
| PC2         | NIC      | 10.x.x.20 | 255.240.0.0 | 2019:ACAD:ACAD:X::20 | /64     |
| PC3         | NIC      | 10.x.x.5  | 255.240.0.0 | 2019:ACAD:ACAD:X::5  | /64     |
| PC4         | NIC      | 10.x.x.9  | 255.240.0.0 | 2019:ACAD:ACAD:X::9  | /64     |
| PC5         | NIC      | 10.x.x.7  | 255.240.0.0 | 2019:ACAD:ACAD:X::7  | /64     |
| PC6         | NIC      | 10.x.x.12 | 255.240.0.0 | 2019:ACAD:ACAD:X::12 | /64     |
| RT1         | G0/0     | 10.x.x.1  | 255.240.0.0 | 2019:ACAD:ACAD:X::1  | /64     |
|             | G0/1     | 10.x.x.1  | 255.240.0.0 | 2019:ACAD:ACAD:X::1  | /64     |
| RT2         | G0/0     | 10.x.x.2  | 255.240.0.0 | 2019:ACAD:ACAD:X::2  | /64     |
|             | G0/1     | 10.x.x.1  | 255.240.0.0 | 2019:ACAD:ACAD:X::1  | /64     |
|             | G0/2     | 10.x.x.1  | 255.240.0.0 | 2019:ACAD:ACAD:X::1  | /64     |
| RT3         | G0/0     | 10.x.x.2  | 255.240.0.0 | 2019:ACAD:ACAD:X::2  | /64     |
|             | G0/1     | 10.x.x.1  | 255.240.0.0 | 2019:ACAD:ACAD:X::1  | /64     |

**Configuracion Basica**

1. Nombre según topología.
2. Username `duoc` password `uc`
3. Habilitar autenticación en la línea de consola de modo local.
4. Contraseña duoc al modo privilegiado.
5. Mensaje de consola “`Conectividad_Esencial`”

# Parte 2

Objetivo: Segmentar una red utilizando cálculo de subredes

Escenario: La empresa CASE S.A necesita optimizar la asignación de host y le ha solicitado implementar una solución de VLSM.

**Direccionamiento y Calculo de IPv4**

Calcule VLSM en IPv4 de los siguientes segmentos.

Red IPv4: 10.0.0.0/8


| RED   | Host | RED | 1ra IP Valida | Ult. IP Valida | Prefijo | Mascara |
| ----- | ---- | --- | ------------- | -------------- | ------- | ------- |
| LAN-A | 2450 |     |               |                |         |         |
| LAN-B | 580  |     |               |                |         |         |
| LAN-C | 876  |     |               |                |         |         |
| WAN-1 | 2    |     |               |                |         |         |
| WAN-2 | 2    |     |               |                |         |         |

Asigne direccionamiento IPv4 e IPv6 a los equipos de acuerdo al siguiente direccionamiento IP.


| Dispositivo | Interfaz | IPv4      | Mascara |
| ----------- | -------- | --------- | ------- |
| PC0         | NIC      | 10.x.x.10 | ?       |
| PC1         | NIC      | 10.x.x.30 | ?       |
| PC2         | NIC      | 10.x.x.20 | ?       |
| PC3         | NIC      | 10.x.x.5  | ?       |
| PC4         | NIC      | 10.x.x.9  | ?       |
| PC5         | NIC      | 10.x.x.7  | ?       |
| PC6         | NIC      | 10.x.x.12 | ?       |
| RT1         | G0/0     | 10.x.x.1  | ?       |
|             | G0/1     | 10.x.x.1  | ?       |
| RT2         | G0/0     | 10.x.x.2  | ?       |
|             | G0/1     | 10.x.x.1  | ?       |
|             | G0/2     | 10.x.x.1  | ?       |
| RT3         | G0/0     | 10.x.x.2  | ?       |
|             | G0/1     | 10.x.x.1  | ?       |

**Configuracion Basica de Router**

1. Nombre según topología
2. Username `duoc` password `uc`
3. Habilitar autenticación en la línea de consola de modo local.
4. Contraseña `duoc` al modo privilegiado.
5. Mensaje de consola “`Conectividad_Esencial`”