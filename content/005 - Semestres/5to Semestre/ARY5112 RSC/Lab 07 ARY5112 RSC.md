# Info
## Imagen Topologia
![500](https://slink.proxylivy.work/image/0a057f08-4197-4e4b-8935-4094b87804d8.png)
## Sobre
Configurando [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP|EIGRP]] IPv4/IPv6
## Requerimientos
- En esta topología existen algunas configuraciones de EIGRP realizadas por un especialista, lamentablemente no funcionan de forma correcta, por lo cual su labor será la detección, corrección para el funcionamiento  respectivo.
- Habilitar EIGRP en IPv4 de la forma más exacta posible, además de implementar router-id con formato "X.X.X.X" donde "X" representa el número del router. No olvide configurar interfaces pasivas según corresponda.
- Habilitar EIGRP en IPv6 a nivel de interfaces, además de configurar router-id con formato "XX.XX.XX.XX" donde "X" representa el número del router. No olvide configurar interfaces pasivas según corresponda.
- Garantizar salida hacia Internet en IPv4/IPv6.
- R1 deberá mandar de forma sumarizada las rutas a R2 a nivel de IPv4/IPv6.
- Deberá redistribuir entre AS de EIGRP en IPv4/IPv6. Utilizar los valores que contiene las interfaces como parámetros K.
- Generar un balanceo de carga donde el tráfico que proviene desde la loopback hacia el ISP y viceversa sea balanceada su carga, utilizando los routers R2 y R4 además de utilizar R3, a nivel de IPv4/IPv6.
- A nivel de IPv4, deberá solo utilizar los primeros 4 parámetros K para el cálculo de métrica.
- A nivel de IPv6, modificar temporizadores, dejando a 3 segundos los mensajes hello y 9 segundos el hold-time.
- Comprobar conectividad completa en la topología.

## Tabla Sumarizacion 
IP 1: 172.16.10.1/24
IP 2: 172.16.20.1/24
IP 3: 172.16.30.1/24

| 1er <br>Octeto | 2do <br>Octeto | 128 | 64  | 32  | Corte | 16  | 8   | 4   | 2   | 1   | 4to <br>Octeto |
| -------------- | -------------- | --- | --- | --- | ----- | --- | --- | --- | --- | --- | -------------- |
| 172            | 16             | 0   | 0   | 0   | /     | 0   | 1   | 0   | 1   | 0   | 1              |
| 172            | 16             | 0   | 0   | 0   | /     | 1   | 0   | 1   | 0   | 0   | 1              |
| 172            | 16             | 0   | 0   | 0   | /     | 1   | 1   | 1   | 1   | 0   | 1              |
IP Sumarizada: `172.16.0.0/19`
## IPV6
0-9

| A   | 10  |
| --- | --- |
| B   | 11  |
| C   | 12  |
| D   | 13  |
| E   | 14  |
| F   | 15  |

| IPv6                       | 1er Sexteto | 2do<br>Octeto | 3er Octeto | 4to Octeto | 8   | 4   | 2   | 1   | \|  | 8   | 4   | 2   | 1   | \|  | 8   | 4   | Corte | 2   | 1   | \|  | 8   | 4   | 2   | 1   | \|  |
| -------------------------- | ----------- | ------------- | ---------- | ---------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | ----- | --- | --- | --- | --- | --- | --- | --- | --- |
| 3001:ABCD:ABCD:A:10::1/128 | 3001        | ABCD          | ABCD       | A          | 0   | 0   | 0   | 0   | \|  | 0   | 0   | 0   | 0   | \|  | 0   | 0   | /     | 0   | 1   | \|  | 0   | 0   | 0   | 0   | \|  |
| 3001:ABCD:ABCD:A:20::1/128 | 3001        | ABCD          | ABCD       | A          | 0   | 0   | 0   | 0   | \|  | 0   | 0   | 0   | 0   | \|  | 0   | 0   | /     | 1   | 0   | \|  | 0   | 0   | 0   | 0   | \|  |
| 3001:ABCD:ABCD:A:30::1/128 | 3001        | ABCD          | ABCD       | A          | 0   | 0   | 0   | 0   | \|  | 0   | 0   | 0   | 0   | \|  | 0   | 0   | /     | 1   | 1   | \|  | 0   | 0   | 0   | 0   | \|  |

IPv6 Sumarizada es: `3001:ABCD:ABCD:A::/74`
# Configuracion
## R1
```
enable
config t
int e0/0
no shut
exit
router eigrp 20
network 172.16.10.1 0.0.0.0
network 172.16.20.1 0.0.0.0
network 172.16.30.1 0.0.0.0
passive-interface loopback 1
passive-interface loopback 2
passive-interface loopback 3
no passive-interface e0/0
exit
ipv6 router eigrp 40
eigrp router-id 11.11.11.11
passive-interface loopback 1
passive-interface loopback 2
passive-interface loopback 3
no pasive-interface e0/0
exit
int range loopback 1-3,e0/0
ipv6 eigrp 40
exit
int e0/0
ip summary-address eigrp 20 172.16.0.0/19
ipv6 summary-address eigrp 40 3001:ABCD:ABCD:A::/74
ipv6 hello-interval eigrp 20 3
ipv6 hold-time eigrp 20 9
exit
router eigrp 20
metric weights 0 1 1 1 1 0
exit
end
copy running-config unix:
```
## R2
```
enable
config t
ipv6 unicast-routing
router eigrp 20
eigrp router-id 2.2.2.2
network 192.168.12.2 0.0.0.0
exit
router eigrp 5
no network 192.168.24.0 0.0.0.0
no network 192.168.23.0 0.0.0.0
network 192.168.24.2 0.0.0.0
network 192.168.23.2 0.0.0.0
exit
ipv6 router eigrp 40
eigrp router-id 22.22.22.22
exit
int e0/0
ipv6 eigrp 40
exit
ipv6 router eigrp 10
eigrp router-id 22.22.22.22
exit
int range e0/1,e0/3
ipv6 eigrp 10
exit
!# Redistribuir en AS
router eigrp 5
redistribute eigrp 20 metric 10000 100 255 1 1500
exit
router eigrp 20
redistribute eigrp 5 metric 10000 100 255 1 1500
exit
ipv6 router eigrp 40
redistribute eigrp 10 metric 10000 100 255 1 1500 include-connected
exit
ipv6 router eigrp 10
redistribute eigrp 40 metric 10000 100 255 1 1500 include-connected
exit
int e0/0
ipv6 hello-interval eigrp 20 3
ipv6 hold-time eigrp 20 9
exit
int range e0/1,e0/3
ipv6 hello-interval eigrp 10 3
ipv6 hold-time eigrp 10 9
exit
int e0/3
delay 50
exit
router eigrp 5
variance 2
exit
ipv6 router eigrp 10
variance 2
exit
router eigrp 20
metric weights 0 1 1 1 1 0
exit
router eigrp 5
metric weights 0 1 1 1 1 0
exit
end
copy running-config unix:
```
## R3
```
enable
config t
ipv6 unicast-routing
router eigrp 5
eigrp router-id 3.3.3.3
network 192.168.34.3 0.0.0.0
network 192.168.23.3 0.0.0.0
exit
ipv6 router eigrp 10
eigrp router-id 33.33.33.33
exit
int range e0/2-3
ipv6 eigrp 10
exit
int range e0/2-3
ipv6 hello-interval eigrp 10 3
ipv6 hold-time eigrp 10 9
delay 50
exit
router eigrp 5
metric weights 0 1 1 1 1 0
exit
end
copy running-config unix:
```
## R4
```
enable
config t
ipv6 unicast-routing
router eigrp 5
eigrp router-id 4.4.4.4
network 192.168.24.0 0.0.0.0
network 192.168.34.0 0.0.0.0
exit
ipv6 router eigrp 10
eigrp router-id 44.44.44.44
exit
int range e0/1-2
ipv6 eigrp 10
exit
ip route 0.0.0.0 0.0.0.0 210.0.0.6
ipv6 route ::/0 3001:ABCD:ABCD:210::6
router eigrp 5
redistribute static
exit
ipv6 router eigrp 10
redistribute static
exit
interface range e0/1-2
ipv6 hello-interval eigrp 10 3
ipv6 hold-time eigrp 10 9
int e0/2
delay 50
exit
router eigrp 5
variance 2
exit
ipv6 router eigrp 10
variance 2
exit
router eigrp 5
metric weights 0 1 1 1 1 0
exit
end
copy running-config unix:
```
## ISP
```
enable
config t
ipv6 unicast-routing
ip route 0.0.0.0 0.0.0.0 210.0.0.5
ipv6 route ::/0 3001:ABCD:ABCD:210::5
end
copy running-config unix:
```

# Visualizacion
## R2
```
enable
!# Ver Parametros K
show interface e0/0
!# BW 10000 | DLY 100(1000/10) | Reliability 255 | Txload+rxload/2 1 | MTU 1500
```
## R1
```
enable
!# Ping Correcto
ping 8.8.8.8 source loopback 1
```
## R2
```
enable
!# Revisar rutas alternativas
show ip eigrp topology all-links
```