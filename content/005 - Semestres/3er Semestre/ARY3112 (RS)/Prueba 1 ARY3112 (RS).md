# Info
## Descargar
- [Onedrive Nuevo](https://duoccl0-my.sharepoint.com/:u:/g/personal/ga_zunigam_duocuc_cl/EetD78vyxfFDoIi4Lb5BSh0B3IdOiGi6WaY6g4wrDEQcjg?e=HhB9PW)
- [Onedrive Terminado](https://duoccl0-my.sharepoint.com/:u:/g/personal/ga_zunigam_duocuc_cl/EYf1jXiWivxJknHDLoSEsOUBacQK1k-5T6tQfyPY1u7jRw?e=1G3t7G)

## Topologia
![](https://slink.proxylivy.work/image/a3b49168-2881-4576-9083-34829eeeb445.png)

## Contenido
Objetivos: Aplicar elementos practicos estudiados en la primera experiencia de la asignatura.

Esta actividad de caracter Sumativa. En la ejecucion de esta actividad deberas poner en practica los conceptos teoricos y practicos vistos durante la Experiencia 1

**Instrucciones iniciales:**
- La actividad se desarrollara de manera individual.
- Debe leer las instrucciones antes de comenzar.
- Configure topologia en Packet Tracer segun las indicaciones.
- Enviar el ejercicio resuelto segun indicaciones de su docente.

**Indicaciones para configurar la topologia**

1. **Asignacion de IP**
	- Asignar [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv4|IPv4]] segun la informacion indicada en la topologia para equipos finales, [[020 - Conceptos/020.4 - Dispositivos de Red/Servidor|Servidor]], [[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]] y [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]].
2. **Configuracion Basica De Equipos**
	- Realizar configuracion basica de routers y switch considerando los siguientes datos:
		- Hostname segun topologia
		- Clave acceso remoto telnet: "*oeste*"
		- Clave consola: "*ccna*"
		- Mensaje de bienvenida "*Solo permitido ingreso de personal autorizado*"
3. **Implementacion De Tecnologias De Capa 2**
	1. Crear las [[010 - Protocolos/010.2 - Switching/VLAN|VLAN]] en todos los switches, segun ID y nombres solicitados.
	2. En los switches de acceso, asignar rango de interfaces a VLANs correspondientes, las puertas sobrantes deberan ser asignadas a VLAN900 y quedar apagadas.
	3. Permitir en enlaces troncales el paso de todas las VLANS, excepto la de "*CONTROL*".
	4. En los enlaces troncales deshabilitar protocolo [[010 - Protocolos/010.2 - Switching/DTP|DTP]].
	5. En los enlaces troncales, permitir paso de VLAN Nativa.
	6. En todos los switches implementar PVST ([[010 - Protocolos/010.2 - Switching/spanning-tree/802.1D (STP)|802.1D (STP)]]).
	7. Realizar las configuraciones necesarias para que la VLAN 100, 200 y 300 sean root primary para S2, y la VLAN 400, 500 y 900 sean root primary para S3. | [[010 - Protocolos/010.2 - Switching/spanning-tree/802.1D (STP)#SW con Root Primary y Secondary|STP Root Primary]]
	8. En interfaces de acceso, implementar [[010 - Protocolos/010.2 - Switching/spanning-tree/Mecanismos de estabilizacion|Mecanismos de estabilizacion]] de STP (portfast y BPDU).

## Tabla VLSM
RED: `172.16.0.0/19`

| Nombre  | VLAN ID | Host | Address     | CIDR Mask | Dec MASK        |
| ------- | ------- | ---- | ----------- | --------- | --------------- |
| CAMARAS | 200     | 410  | 172.16.0.0  | /23       | 255.255.254.0   |
| TI      | 100     | 220  | 172.16.2.0  | /24       | 255.255.255.0   |
| IOT     | 300     | 205  | 172.16.3.0  | /24       | 255.255.255.0   |
| SOPORTE | 400     | 60   | 172.16.4.0  | /26       | 255.255.255.192 |
| NATIVA  | 900     | 13   | 172.16.4.64 | /28       | 255.255.255.240 |
| CONTOL  | 500     | 6    | 172.16.4.80 | /29       | 255.255.255.248 |

4. **Implementacion De Tecnologias De Capa 3**
	- Asignar IPv4 a los switches a traves de la VLAN Administrativa.
	- Todos los switches deberan tener configurado default-gateway.
	- Realizar [[020 - Conceptos/020.1 - Administracion/Enrutamiento Inter-Vlan|Enrutamiento Inter-Vlan]] en RA. Debera utilizar la IP .1 en IPv4, para cada una de las VLANs correspondiente.
	- En los equipos RA y RB, debera implementar enrutamiento estatico por defecto en IPv4 con next-hop.
5. **Servicios**
	- Verifique que servicio WEB se encuentre activo en `www.plazaoeste.cl`
	- Configure [[010 - Protocolos/010.3 - Comunicaciones/DNS|DNS]] para permitir a usuarios acceder a `www.plazaoeste.cl`
6. **Verificacion**
	- Realizar pruebas de conectividad por ping entre equipos terminales
	- Verifique la operacion para los servicios solicitados (WEB-DNS)

# Configuracion
## Dispositivos Finales
### PC_VLAN_300
- IPv4: 172.16.3.5
- Mascara: 255.255.255.0
- Default Gateway: 172.16.3.1
- DNS: 10.10.10.8

### PC_VLAN_400
- IPv4: 172.16.4.8
- Mascara: 255.255.255.192
- Default Gateway: 172.16.4.1
- DNS: 10.10.10.8

### DNS SERVER
- IPv4: 10.10.10.8
- Mascara: 255.255.255.0
- Default Gateway: 10.10.10.1
- DNS: 10.10.10.8

### DHCP
- IPv4: 10.10.10.250
- Mascara: 255.255.255.0
- Default Gateway: 10.10.10.1
- DNS: 10.10.10.8

## RB
```
hostname RB
!
line console 0
pass ccna
exit
!
line vty 0 4
pass oeste
exit
!
banne motd "#Solo ingreso de personal autorizado#"
!
int g0/0
ip add 10.10.10.1 255.255.255.0
no shut
exit
!
int g0/1
ip add 12.12.12.6 255.255.255.252
no shut
exit
!
ip route 0.0.0.0 0.0.0.0 12.12.12.5
```
## RA
```
hostname RA
!
line console 0
pass ccna
exit
!
line vty 0 4
pass oeste
exit
!
banne motd #Solo ingreso de personal autorizado#
!
int g0/1
ip add 12.12.12.5 255.255.255.252
no shut
exit
!
ip route 0.0.0.0 0.0.0.0 12.12.12.6
!
int g0/0
no shut
exit
!
int g0/0.100
encapsulation dot 100
ip address 172.16.2.1 255.255.255.0
no shut
exit
!
int g0/0.200
encapsulation dot 200
ip address 172.16.0.1 255.255.254.0
no shut
exit
!
int g0/0.300
encapsulation dot 300
ip address 172.16.3.1 255.255.255.0
no shut
exit
!
int g0/0.400
encapsulation dot 400
ip address 172.16.4.1 255.255.255.192
no shut
exit
!
int g0/0.500
encapsulation dot 500
ip address 172.16.4.81 255.255.255.248
no shut
exit
!
int g0/0.900
encapsulation dot 900
ip address 172.16.4.65 255.255.255.240
no shut
exit
!
```
## S5
```
hostname S5
vlan 100
name TI
exit
!
vlan 200
name CAMARAS
exit
!
vlan 300
name IOT
exit
!
vlan 400
name SOPORTE
exit
!
vlan 500
name CONTROL
exit
!
vlan 900
name NATIVA
exit
!
int fa0/1
Switchport mode access
switchport access vlan 400
spanning-tree portfast
spanning-tree bpduguard enable
no shut
exit
!
int range fa0/2-22
Switchport mode access
switchport access vlan 900
shut
exit
!
int range fa0/23-24
switchport mode trunk
switchport trunk allowed vlan 100,200,300,400
switchport trunk native vlan 900
switchport nonegotiate
no shut
exit
!
int vlan 900
ip add 172.16.4.69 255.255.255.240
exit
!
spanning-tree mode pvst
ip default-gateway 172.16.4.65
```
## S4
```
hostname S4
vlan 100
name TI
exit
!
vlan 200
name CAMARAS
exit
!
vlan 300
name IOT
exit
!
vlan 400
name SOPORTE
exit
!
vlan 500
name CONTROL
exit
!
vlan 900
name NATIVA
exit
!
int fa0/21
Switchport mode access
switchport access vlan 300
spanning-tree portfast
spanning-tree bpduguard enable
no shut
exit
!
int range fa0/1-20,fa0/23
Switchport mode access
switchport access vlan 900
shut
exit
!
int range fa0/22,fa0/24
switchport mode trunk
switchport trunk allowed vlan 100,200,300,400
switchport trunk native vlan 900
switchport nonegotiate
no shut
exit
!
int vlan 900
ip add 172.16.4.68 255.255.255.240
exit
!
spanning-tree mode pvst
!
ip default-gateway 172.16.4.65
```
## S3
```
hostname S3
vlan 100
name TI
exit
!
vlan 200
name CAMARAS
exit
!
vlan 300
name IOT
exit
!
vlan 400
name SOPORTE
exit
!
vlan 500
name CONTROL
exit
!
vlan 900
name NATIVA
exit
!
int range fa0/1-20,fa0/23
Switchport mode access
switchport access vlan 900
no shut
exit
!
int range fa0/21,fa0/22,fa0/24
switchport mode trunk
switchport trunk allowed vlan 100,200,300,400
switchport trunk native vlan 900
switchport nonegotiate
no shut
exit
!
int vlan 900
ip add 172.16.4.67 255.255.255.240
exit
!
spanning-tree mode pvst
!
spanning-tree Vlan 400,500,900 root Primary
!
ip default-gateway 172.16.4.65
```
## S2
```
enable
config t
hostname S2
vlan 100
name TI
exit
!
vlan 200
name CAMARAS
exit
!
vlan 300
name IOT
exit
!
vlan 400
name SOPORTE
exit
!
vlan 500
name CONTROL
exit
!
vlan 900
name NATIVA
exit
!
int range fa0/1-19,fa0/21-22
Switchport mode access
switchport access vlan 900
no shut
exit
!
int range fa0/20,fa0/23,fa0/24
switchport mode trunk
switchport trunk allowed vlan 100,200,300,400
switchport trunk native vlan 900
switchport nonegotiate
no shut
exit
!
int vlan 900
ip add 172.16.4.66 255.255.255.240
exit
!
spanning-tree mode pvst
!
spanning-tree Vlan 100,200,300 root Primary 
!
ip default-gateway 172.16.4.65
!
```
## S1
```
enable
config t
hostname S1
vlan 100
name TI
exit
!
vlan 200
name CAMARAS
exit
!
vlan 300
name IOT
exit
!
vlan 400
name SOPORTE
exit
!
vlan 500
name CONTROL
exit
!
vlan 900
name NATIVA
exit
!
int range fa0/1-19,fa0/22-24
Switchport mode access
switchport access vlan 900
no shut
exit
!
int range fa0/20,fa0/21
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 100,200,300,400
switchport trunk native vlan 900
switchport nonegotiate
no shut
exit
!
int vlan 900
ip add 172.16.4.74 255.255.255.240
exit
!
spanning-tree mode pvst
!
ip default-gateway 172.16.4.65
!
```
## SWA
```
hostname SWA
int g0/1
no shut
exit
!
```