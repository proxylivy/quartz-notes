PACL (Port ACL) es una tecnica para aplicar control de acceso en puertos [[020 - Conceptos/020.3 - Fundamentos/Modelo OSI#Capa 2|Capa 2]], pueden ser [[010 - Protocolos/010.2 - Switching/VLAN#Troncal|Troncal]] o de [[010 - Protocolos/010.2 - Switching/VLAN#Access|Acceso]], no son compatibles con algunos switch Cisco Catalyst, inclusive varia su configuracion en dispositivos compatibles. PACL no afecta a los controles de paquetes, como CDP(Cisco Discover Protocol), VTP(Propaga las Vlans), [[010 - Protocolos/010.2 - Switching/DTP|DTP]] y [[010 - Protocolos/010.2 - Switching/spanning-tree/802.1D (STP)|802.1D (STP)]] y el contenido que pase por las interfaces como tramas de ethernet
Tipos de PACL
- ACL IP: Filtra los paquetes IPv4 e IPv6 que vayan al puerto capa 2
- ACL MAC: Filtra paquetes de tipo no soportado basado en los campos de la trama Ethernet (NO IP, ARP o MPLS). Puede ser Nombrada
Las [[020 - Conceptos/020.1 - Administracion/ACL|ACL]] Estandares y Extendidas nombradas son compatibles con ACL MAC

VACL (Vlan ACL) sirve para filtrar el trafico en capa 2 (kinda), y superar las limitaciones de SPAN, mediante el uso de la funcion de puerto de captura
A diferencia de las [[020 - Conceptos/020.1 - Administracion/ACL|ACL]] las VACL se aplican a todos los paquetes que pasan por VLAN o interfaces WAN.
Funciona con mapas de acceso, las cuales pueden tener varias secuencias de match especificando IP y MAC, especificando una accion que se realiza cuando hay una coincidencia,   
Acciones
- Forward: 


# Configuracion
# PACL
```
mac access-list extended [PACL-L2-NAME]
deny host [source-mac] any
permit any any
exit
ip access-list extended [PACL-L3-NAME]
deny ip host [source-ip] any
permit any any
exit
int [int S/S/P] #To Source End-Device
mac access-group [PACL-L2-name] in
ip access-group [PACL-L3-name] in
exit
```
## VACL
```
mac access-list extended [VACL-L2-NAME]
permit host [source-mac] host [destination-mac]
exit
ip access-list extended [VACL-L3-NAME]
permit ip host [source-ip-host] host [destination-ip-host]
exit
vlan access-map [map-name] [VLAN-ID-SOURCE?]
match mac address [VACL-L2-NAME]
match ip address [VACL-L3-NAME]
action drop
exit
vlan access-map [map-name] [VLAN-ID-DESTINATION?]
action forward
exit
vlan filter [map-name???] vlan-list [VLAN-ID,VLAN-ID]
```

# Visualizar
`show mac address-table` -> Ver mac staticas de acceso en el switch
`show vlan access-map`: Ver VACL Creadas
`show access-list`: ACL Para PACL
`sh run int [int S/S/P]`: Verificar la aplicacion de PACL