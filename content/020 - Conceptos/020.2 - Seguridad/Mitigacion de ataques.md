# Que es?
Una LAN sigue el [[020 - Conceptos/020.3 - Fundamentos/Modelo OSI|Modelo OSI]], por lo que los dispositivos finales se deben asegurar para cada red, siguiendo una estrategia de seguridad desde adentro hacia afuera (LAN-to-permiter), evitando situaciones de victimas con robo de informacion o dinero.
La [[020 - Conceptos/020.3 - Fundamentos/Modelo OSI#Capa 2|Capa 2]] es el primer pilar configurable del [[020 - Conceptos/020.3 - Fundamentos/Modelo OSI|Modelo OSI]], si se compromete, las capas de arriba estaran comprometidas. estos ataques requieren que un empleado/visitante tenga contacto fisico con los equipos (Insider Atack).
El ataque mas comun es un "desbordamiento de Buffer" el cual es un ataque DoS y son usados para ejecutar codigo albitrario o tener acceso no autorizado
## Spoofing MACs
Las tablas [[020 - Conceptos/020.3 - Fundamentos/MAC|MAC]] tienen limites de espacio, las MAC guardadas duran 300 segundos en ser borradas, si se mandan muchas MAC nuevas, la tabla borra las antiguas y no deja ingresar nuevas, permite al atacante ver el trafico de la VLAN que permita el Switch. Se puede hacer con [macof](https://kalilinuxtutorials.com/macof/)
Se puede ver en [[020 - Conceptos/020.4 - Dispositivos de Red/IOU WEB|IOU WEB]] cuantas mac quedan con `show mac address-table counts`
### Mitigacion con Port-Security
Usa [[010 - Protocolos/010.2 - Switching/Seguridad de Puerto|Seguridad de Puerto]]
```
!# Obligatorio
switchport mode access
switchport port-security
switchport port-security maximum [maximum-value]
!# Opcional
!# Especifica una MAC para que la mantenga la tabla MAC
switchport port-security mac-address [mac-address]
!# Mac aprendidas, se mantienen en la tabla
switchport port-security mac-address sticky
```
### Mitigacion con Aging
Funciona como lista blanca/negra por un tiempo
[[010 - Protocolos/010.3 - Comunicaciones/010.3.1 - AAA/AAA|AAA]] es una alternativa, en la que se configura el puerto con usuario y contraseña para verificar que esta conectado al puerto correctamente.
- `static`: default
- `time`: 0-1440 minutos, 0 es desabilitado
- `type abolute`: cuenta regresiva
- `type inactivity`: solo se desactiva si no pasa actividad por el puerto
```
int [int S/S/P]
switchport port-security aging {static | time [time] | type {absolute | inactivity}}
```
## VLAN Spoofing
Tambien conocido como ataque "Salto de Vlan", el atacante conecta un Switch en modo Trunk y debido al funcionamiento de [[010 - Protocolos/010.2 - Switching/DTP|DTP]], negociara usando señales ISL o 802.1Q, permitiendo ver las VLAN permitidas del switch atacado y permite enviar trafico hacia por ellas.
### Mitigacion VLAN Spoofing
Habilitar enlaces de [[010 - Protocolos/010.2 - Switching/VLAN#Access|Acceso]] y [[010 - Protocolos/010.2 - Switching/VLAN#Troncal|Trunk]] con [[010 - Protocolos/010.2 - Switching/VLAN#Nativa (Trunk)|Vlan Nativa]], apagar interfaces sin usar y desactivar [[010 - Protocolos/010.2 - Switching/DTP|DTP]].
```
int [int S/S/P]
switchport mode access
switchport access vlan [vlan-id]
switchport nonegotiate
no shut
exit
int [int S/S/P]
switchport mode trunk
switchport trunk native vlan [vlan-id-native]
switchport nonegotiate
no shut
exit
int range [int S/S/P-P] !# Puertos no usados
shutdown
exit
```
## DHCP Spoofing
El atacante conecta y redirige el trafico a un servidor DHCP falso, **engañando** a los dispositivos de una red con **Puertas de Enlace**, **Servidor DNS** o **IP** falsas
### DHCP Starvation
El atacante crea DoS a la obtencion de IP con MAC falsas, dejando sin ip disponibles a los clientes
Se puede hacer con [dhcpig](https://www.kali.org/tools/dhcpig/)
### Mitigacion con DHCP Snooping
Controla los DHCP Discover, espeficando las vlan que controla, se configura en todos los [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]].
```
ip dhcp snooping
ip dhcp snooping vlan [vlan,vlan-vlan]
```
Permite Asegurar enlaces ascendentes al correcto DHCP
```
int [int S/S/P]
ip dhcp snooping trust
exit
```
Permite limitar las ip que un cliente pueda pedir, si sobrepasa el `rate-number` enviar un puerto a `err-disable`
Nota: Un valor comun de `rate-number` va de 3-5
```
int [int S/S/P]
ip dhcp snooping limit rate [rate-number]
```
Opcion 82 se debe desactivar
```
no ip dhcp snooping information option
```
## ARP Spoofing
El atacante ataca a la Relacion de Confianza de [[020 - Conceptos/020.3 - Fundamentos/MAC|MAC]]/[[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv4|IPv4]], pidiendo que el [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]] actualize la tabla CAM, es un tipo de ataque MITM, su implementacion se parece a la [[020 - Conceptos/020.2 - Seguridad/Mitigacion de ataques#Mitigacion con DHCP Snooping|Mitigacion con DHCP Snooping]]
### Mitigacion con DAI
Nota: `ip arp inspection validate` solo analizara el ultimo comando puedo, para analizar 2 al mismo tiempo se deben poner juntos
```
ip arp inspection vlan [vlan,vlan-vlan]
int [int S/S/P] 
ip arp inspection trust
!
ip arp inspection vlan [vlan,vlan-vlan]
ip arp inspection validate [src-mac | dst-mac | ip]
```
## IP Spoofing
Ataque centrado en la relacion [[020 - Conceptos/020.3 - Fundamentos/MAC|MAC]]/[[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv4|IPv4]], suplantando las direcciones MAC, modifica la direccion mac del host para que coincida con la MAC conocida del host de destino
### Mitigacion con IP Secure Guard
Permite Verificar el direccionamiento IP origen por lo que va configurado hacia los dispositivos finales
```
int range [int S/S/P-P]
ip verify source
```
## Manipulacion STP
Un atacante puede modificar su prioridad a una mas baja dentro de [[010 - Protocolos/010.2 - Switching/spanning-tree/802.1D (STP)|802.1D (STP)]] enviando [[010 - Protocolos/010.2 - Switching/spanning-tree/BPDU|BPDUs]] que anuncian un puente con prioridad mas baja y la topologia cambie, permitiendo que todo el trafico pase por el puente raiz falso.

### Mitigacion con Estabilizacion
Se usan [[010 - Protocolos/010.2 - Switching/spanning-tree/Mecanismos de estabilizacion|Mecanismos de estabilizacion]]
Habilita [[010 - Protocolos/010.2 - Switching/spanning-tree/802.1W (RSTP)|802.1W (RSTP)]]
```
spanning-tree mode rapid-pvst
```
Configura [[010 - Protocolos/010.2 - Switching/spanning-tree/Mecanismos de estabilizacion#Conf Portfast|Portfast]] en enlaces de [[010 - Protocolos/010.2 - Switching/VLAN#Access|Acceso]]
```
int [int S/S/P]
spanning-tree portfast
```
Configura [[010 - Protocolos/010.2 - Switching/spanning-tree/Mecanismos de estabilizacion#Conf BPDU Guard|Mecanismos de estabilizacion]] en enlaces de [[010 - Protocolos/010.2 - Switching/VLAN#Access|Acceso]]
```
int [int S/S/P]
spanning-tree bpduguard enable
```
Configura [[010 - Protocolos/010.2 - Switching/spanning-tree/Root Guard|Root Guard]] en enlaces [[010 - Protocolos/010.2 - Switching/VLAN#Troncal|Troncales]]
```
int [int S/S/P]
spanning-tree guard root
```

## Tormenta de LAN
Son excesos de trafico y degradan el desempeño de la red, que pueden ser provocadas por errores al implementar algun protocolo o error en la configuracion de red o usuarios creando DoS, tambien conocidas como Tormentas de Broadcast.
Mientras que [[010 - Protocolos/010.2 - Switching/spanning-tree/802.1D (STP)|802.1D (STP)]] evita que sucedan tormentas, **Storm Control** controla el desastre
### Mitigacion con Storm Control
Se configura en un solo extremo y en las puertas troncales que quiera controlar un [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]]
# Configuracion
Nota: `level` es el valor maximo que soporta al tener mensajes de broadcast, el primer valor es el maximo y el segundo es el minimo
```
int range [int S/S/P-P]
storm-control broadcast level 75.5
storm-control action shutdown
```
tambien se pueden usar `storm-control multicast level pps 2k 1k` pero no lo usamos