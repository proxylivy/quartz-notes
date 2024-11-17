# Info
## Imagen Topologia
![1000](https://slink.proxylivy.work/image/f466f2c0-dc82-4117-9348-0583acb150f3.png)
## Sobre
Arreglando Problemas de Capa 3
## Requerimientos
- Se ha detectado una pérdida de conectividad entre los PC1 y PC2, además de que el equipo PC1 no es capaz de recibir IP de manera dinámica por parte del router R1. Su labor será la detección, corrección y documentación de los errores detectados, para esto deberá apoyarse en comandos show vistos en asignaturas anteriores, así como, las que proporcione su docente.
- Documentar todos los problemas que sean detectados, indicado el o los equipos involucrados, indicar la problemática de manera breve, el comando show utilizado para la detección del inconveniente, y el comando que permitió la corrección del problema.
# Configuracion
12 problemas en la topologia del lab 1
## R1
> 1. DHCP: Tiene excluida todas las ip de la mascara /24 de la subred que deberia entregar 192.168.11.0, visto con `show running-config`
```
no ip dhcp excluded-address 192.168.11.0 192.168.11.254
```
> 2. EIGRP: Network 192.168.12.0 0.0.0.0 es incorrecta, visto con `show ip protocols`
```
router eigrp 10
no network 192.168.12.0 0.0.0.0
network 192.168.12.1 0.0.0.0
```
> 3. EIGRP: Todas las interfaces que funcionan con el protocolo estan pasivas debido a "passive-interface default", Visto con `show running-config`
```
router eigrp 10
no passive-interface default
```
## SW
> 1. Interfaces e0/1 y e0/2 apagadas, visto con `show ip int brief`
```
int range e0/1-2
no shut
```
> 2. Interfaces e0/1 y e0/2 estan con "Switchport protected" | visto con `show running-config`
```
int range e0/1-2
no switchport protected
```
## R3
> 1. Redistribucion de rutas EIGRP en OSPF le falta las subredes, visto con `show ip route` y `show running-config`
```
router ospf 1
redistribute eigrp 10 subnets
```
> 2. Redistribucion de rutas OSPF en EIGRP no existen las metricas, visto con `show ip route` y `show running-config`
```
router eigrp 10
redistribute ospf 1 metric 10000 100 255 1 1500
```
## R4
> 1. OSPF: Interfaz e0/1 pasiva | Visto con `show ip protocols` y `show running-config`
```
router ospf 1
no passive-interface e0/1
```
## R5
> 1. Interfaz e0/1 Apagada | Visto con `show ip int brief`
```
int e0/1
no shut
```
> 2. OSPF: Router-ID es el mismo que R4 | visto con `show ip protocols`
```
router ospf 1
no router-id 4.4.4.4
router-id 5.5.5.5
```
> 3. OSPF: Configurada area incorrecta en network 172.16.45.5 | Visto con `show ip protocols`
```
router ospf 1
no network 172.16.45.5 0.0.0.0 area 10
network 172.16.45.5 0.0.0.0 area 0
```
> 4. IP incorrecta en e0/0 172.16.55.1 -> 172.16.55.5 | Visto con `show ip int brief`
```
int e0/0
no ip address 172.16.55.1
ip address 172.16.55.5
```
> 5. IP incorrecta en e0/1 45.45.45.5 -> 172.16.45.5 | Visto con `show ip int brief`
```
int e0/1
no ip address 45.45.45.5
ip address 172.16.45.5
```
## PC2
> 1. Falta la configuracion de Ruta estatica hacia Router Gateway | Visto con `show ip route`
```
ip route 0.0.0.0 0.0.0.0 172.16.55.5
```
