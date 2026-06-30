# Info
## Descargar
[Onedrive - ET ARY4122 - 2023.pka](https://duoccl0-my.sharepoint.com/:u:/g/personal/ga_zunigam_duocuc_cl/Ea17G8FsMXBPm66T7dHlW9UBwiCkZ-sgeYsyGQkC1-1UIA?e=3lxTLu)
## Requerimientos
### I. Contexto
Debido al Proyecto Nuevo Hospital Dr. Sótero del Río, se requiere iniciar las obras de implementación de los sistemas de telecomunicaciones, en esta línea se requiere instalar, configurar y habilitar de acceso a Internet inalámbrico a las unidades de Urgencia infantil, adulto y admisión, para esto se ha habilitado el servicio WLAN administrado a través una gestión centralizada por un controlador inalámbrico (WLC) con el fin de habilitar dichos accesos a cada una de las redes inalámbricas.

Como representante de una empresa de instalación de servicios inalámbricos se ha adjudicado esta licitación, entre sus bases se ha acordado: la implementación de una red inalámbrica, con el propósito de conectar cerca de 100 dispositivos repartidos en 3 VLAN, estas son VLAN 10 nombre “Urgencia infantil”, VLAN 20 nombre “Urgencia adulto”, VLAN 30 nombre “Admisión”.

Considere que dicha implementación se realiza en un edificio de concreto en una superficie de 300 metros cuadrados aproximadamente. El acceso a internet contempla un enlace punto a punto a un ISP, con un ancho de banda de 100 Mbps. Un solo WLC actuará como central de control de la red inalámbrica, para vigilar todas las frecuencias de los APs.

### II. Diagrama de la red
El diagrama de red propuesto representa una topología lógica. Se deberán realizar las configuraciones fundamentales, dejando todos los equipos de la red en funcionamiento y con conectividad total de extremo a extremo dados los siguientes requerimientos.
![](https://slink.proxylivy.work/image/19cd0c62-fb75-4a99-a5a9-7abb17dcfc20.png)
Desarrollo Simulación Red:
- La red inalámbrica debe dar acceso a internet a 100 usuarios.
- La velocidad mínima del enlace es de 100 Mbps.
- Para la implementación de la solución debe mencionar los canales a utilizar y frecuencia recomendada.
- Explicar los cálculos efectuados en el punto anterior para lograr la conectividad.
- Implementar la conexión WLAN para acceder a internet.
- Configurar un controlador Wireless para vigilar los LAPs que constituyen la solución inalámbrica.
- Implementar seguridad en la WLAN.
- Establecer monitoreo de la WLAN.
- Listar en su totalidad los dispositivos, para cumplir de implementar la WLAN, como LAPs, WLC,antenas, frecuencias de banda y todo lo concerniente a los requerimientos.
- Realizar la presentación con PPT, detallando la implementación dada.
- Dado que la aplicación Packet Tracer no se puede implementar el enlace de largo alcance, se usará un enlace serial conectado al router del ISP, para dar cobertura a internet.

### III. Configuración general de dispositivos de la WLAN.
- Realizar configuración básica de cada dispositivo, según las instrucciones dadas.
- Configurar clave de acceso : admin y password Admin123#
- Asignar las direcciones IPs según la topología de la red y las instrucciones dadas.
- Configurar correctamente las VLAN consideradas.
- Realizar la configuración de WLC, para la administración de los LAPs, considerando las instrucciones dadas.
- El servidor DHCP, protocolo NAT y subinterfaces en el Router de Borde se encuentran configurados.
- Direccionamiento IP entre el router de borde y el router del ISP, ya se encuentra configurado, como también el enrutamiento estático por defecto entre ambos routers.

### IV.- Configuración específica de dispositivos.
#### A. Configuración de Protocolos de Capa 2 : Switch Central (rúbrica 1):
- Configurar las interfaces G0/1, F0/1, F0/2, F0/3, F0/4, F0/5 en modalidad troncal, considerando la vlan nativa por defecto, el resto de las interfaces deberá dejarlas administrativamente apagadas (shutdown)
- Configurar las 3 VLAN, sin asociar ningún puerto al switch central.
- Desactive el protocolo DTP en interfaces troncales según requerimiento.

#### B. Configuración WLC (rúbrica 2):
- Configurar las 3 VLAN con el SSID y seguridad respectiva con la autenticación AAA hacia el servidor Radius de la topología.
- Configurar la interface G0/0 del WLC con IP: 172.16.10.4/24, Gateway 172.16.10.1

| WLAN              | ID VLAN | SSID              | Autenticacion |
| ----------------- | ------- | ----------------- | ------------- |
| Admision          | 10      | Admision          | WPA2          |
| Urgencia_Adulto   | 20      | Urgencia_Adulto   | WPA2          |
| Urgencia_Infantil | 30      | Urgencia_Infantil | WPA2          |
#### C. Configuración DHCP Interno para los LAP (Lightweight Access Point)
- Pool DHCP IP Inicial : 172.16.10.100
- Pool DHCP IP Final : 172.16.10.110
- Network: 172.16.10.0/24
- Default Router: 172.16.10.1
- DNS Server: 8.8.8.8

#### D. Dispositivos finales de cada VLAN (rúbrica 3 - 4 y 5)
- Los dispositivos de cada VLAN obtendrán direccionamiento IP desde el servidor DHCP configurado en el Router de borde y se asociará al SSID de su VLAN respectiva a través del WLC, configurando solo la interfaz Wireless de cada uno según la tabla abajo indicada, posterior verificará que todos los equipos estén correctamente configurados, para esto deberá ingresar usuario según indicación de la tabla que menciona a continuación.

| VLAN | SSID              | Autenticacion   | USER ID                             | PASSWORD                               | ENCRYPTION |
| ---- | ----------------- | --------------- | ----------------------------------- | -------------------------------------- | ---------- |
| 10   | Admision          | WPA2/Enterprise | operario1<br>operario2<br>operario3 | pass_oper1<br>pass_oper2<br>pass_oper3 | AES        |
| 20   | Urgencia_Adulto   | WPA2/Enterprise | operario4<br>operario5<br>operario6 | pass_oper4<br>pass_oper5<br>pass_oper6 | AES        |
| 30   | Urgencia_Infantil | WPA2/Enterprise | operario7<br>operario8<br>operario9 | pass_oper7<br>pass_oper8<br>pass_oper9 | AES        |
Configuración del servidor de autenticación RADIUS:
- Configuración de red: RADIUS
- Radius Port : 1812
- Nombre del cliente: WLC
- IPv4 del cliente: 172.16.10.3/24 – DNS 190.90.90.10
- Encryption: AES
- Secret: wlc_examen
- Username: userexamen
- Password: user_examen
- IPv4 del servidor AAA RADIUS: 172.16.10.4/24, Gateway: 172.16.10.1

#### E. Creación de Grupos de Access-Point en la solución inalámbrica.
- Dentro de los requerimientos de la solución WLAN de la red del hospital, se necesita por parte del equipo de redes que cada uno de los Access Point integrados en cada uno de los pisos del centro entregue el SSID correspondiente a la ubicación del dispositivo, por lo que se necesita generar los siguientes grupos de LAP.

| Nombre Grupo AP  | Descr. Grupo     | N° AP | SSID a Propagar   |
| ---------------- | ---------------- | ----- | ----------------- |
| Admision         | Admision         | AP1   | Admision          |
| UrgenciaAdulto   | UrgenciaAdulto   | AP2   | Urgencia_Adulto   |
| UrgenciaInfantil | UrgenciaInfantil | AP3   | Urgencia_Infantil |


## Requerimientos
- 100 hosts
- 100 mbps
- 300 metros cuadrados -> Frecuencia y canales, explicando los calculos
- Clave de acceso: admin:Admin123#
# Configuracion Basica
## Dispositivos Finales
### Plaforma Medica
- IPv4: 190.90.90.10
- Mask: 255.255.255.0
- DG: 190.90.90.1
- DNS: 190.90.90.10
- HTTP: salud.sotero.cl
### Server RADIUS:
- IPv4: 172.16.10.4
- Mask: 255.255.255.0
- DG: 172.16.10.1
- DNS: 190.90.90.10
## Routers

ISP
!

Router Borde

## Switch 0
```
enable
config t
vlan 10
name Urgencia_Infantil
vlan 20
name Urgencia_Adulto
vlan 30
name Adminision
vlan 16
name networking
int range g0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,16
switchport nonegotiate
no shut
exit
!
int range f0/3-f0/5
switchport mode trunk
switchport trunk allowed vlan 10,20,30,16
switchport nonegotiate
no shut
exit
!
int range f0/1,f0/2
switchport mode access
switchport access vlan 16
switchport nonegotiate
no shut
exit
!
int range f0/6-f0/24
shut
exit

```
## WLC 3504 Hospital
```
enable
config t
int g0/1
ip add 172.16.10.3 255.255.255.0
int g0/0
ip add 172.16.10.4 255.255.255.0

VLAN 10 -> SSID y WLAN: Admision -> Pass: Admin123#
VLAN 20 -> SSID y WLAN: Urgencia_Adulto -> pass: Admin123#
VLAN 30 -> SSID y WLAN: Urgencia_Infantil -> pass: Admin123#

!#AUTENTICACION AAA RADIUS

```
## LWA P0
Vlan 30 -> 192.168.30.0/24
## LWA P1
Vlan 20 -> 192.168.20.0/24
## LWA P2
Vlan 10 -> 192.168.10.0/24