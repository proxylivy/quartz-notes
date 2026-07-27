# Info

Se basa en "Actividad 07 - IP.pka" desde [Copyparty - Actividades](https://copyparty.proxylivy.work/Duoc/Conectividad%20y%20Redes/1er%20Semestre/VTY1122%20(TD)/Actividades/)

La presenta actividad, tiene como finalidad en los alumnos de individual puedan realizar la interconexión de dispositivos intermedios, equipos finales y de IOT, además de la asignación de direccionamiento IPv4/IPv6 según las técnicas correspondientes.

La empresa “Digital S.A” necesita poder interconectar sus dispositivos de red entre sí, mediante cables de red, y la asignación de IPv4/IPv6 a sus dispositivos finales e IoT. Por lo cual, su labor como especialista será la implementación de dicha red.

1. Interconexion De Equipos: Interconectar dispositivos finales e intermedios, con cable de red adecuado. Para lo cual, utilizará las siguientes tablas en donde le indicará en que puerto debe ir conectado el equipo final o IoT, a su respectivo switch.

| Switch | Conecta  | Puerto |
| ------ | -------- | ------ |
| SWB    | SWA      | Gi0/1  |
|        | PC3      | Gi0/4  |
|        | IoT1     | Fa0/18 |
|        | IoT2     | Fa0/12 |
|        | IoT3     | Fa0/16 |
|        |          |        |
| SWA    | SWB      | Gi0/1  |
|        | PC1      | Fa0/5  |
|        | PC2      | Fa0/10 |
|        | Servidor | Fa0/15 |
|        | Notebook | Fa0/20 |

2. Asignacion Direccionamiento IPv4/IPv6: Asignar direccionamiento IPv4 a equipos finales y dispositivos de Internet de las Cosas, para lo cual debe utilizar la siguiente tabla, y realizar la transformación de sistema numérico binario a decimal, según corresponda.

| Dispositivo | Direccion IP                        | Mascara | Puerta de Enlace |
| ----------- | ----------------------------------- | ------- | ---------------- |
| PC1         | 10101100.00010000.00001111.00001010 | /16     | 172.16.0.1/16    |
| PC2         | 10101100.00010000.00010100.11111000 | /16     | 172.16.0.1/16    |
| Servidor    | 10101100.00010000.00000101.10000001 | /16     | 172.16.0.1/16    |
| Notebook    | 10101100.00010000.11101001.11111010 | /16     | 172.16.0.1/16    |
| PC3         | 10101100.00010000.11001000.01100100 | /16     | 172.16.0.1/16    |
| IoT1        | 10101100.00010000.00000101.11111000 | /16     | 172.16.0.1/16    |
| IoT2        | 10101100.00010000.10010110.01100011 | /16     | 172.16.0.1/16    |
| IoT3        | 10101100.00010000.11100110.10100000 | /16     | 172.16.0.1/16    |

3. Asignar direccionamiento IPv6 a equipos finales y dispositivos de Internet de las Cosas, para lo cual debe utilizar la siguiente tabla, y asignar la dirección IPv6 más reducida posible, además del prefijo de dirección IPv6, link-local e Puerta de Enlace Predeterminada para cada dispositivo.

| Dispositivo | Direccion IPv6                          | Mascara | Link Local | Puerta de Enlace |
| ----------- | --------------------------------------- | ------- | ---------- | ---------------- |
| PC1         | 2019:ACAD:1111:2222:0000:0000:0000:9999 | /48     | FE80::1    | 2019:ACAD::FFFF  |
| PC2         | 2019:ACAD:0000:ABCD:0000:0000:0123:5670 | /48     | FE80::2    | 2019:ACAD::FFFF  |
| Servidor    | 2019:ACAD:0000:0000:0000:0000:123A:4567 | /48     | FE80::3    | 2019:ACAD::FFFF  |
| Notebook    | 2019:ACAD:0003:0004:ABC0:55AC:89A0:0012 | /48     | FE80::4    | 2019:ACAD::FFFF  |
| PC3         | 2019:ACAD:ACAD:5555:0000:0001:0000:0005 | /48     | FE80::8    | 2019:ACAD::FFFF  |
| IoT1        | 2019:ACAD:FFFF:AAAA:BBBB:0000:0000:CCCC | /48     | FE80::7    | 2019:ACAD::FFFF  |
| IoT2        | 2019:ACAD:5555:4444:3333:0000:0000:FFFF | /48     | FE80::6    | 2019:ACAD::FFFF  |
| IoT3        | 2019:ACAD:7777:0000:0000:0000:5555:0400 | /48     | FE80::5    | 2019:ACAD::FFFF  |

