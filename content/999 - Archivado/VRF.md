**V**irtual **R**outing and **F**orwarding es una tecnologia que permite la coexistencia de multiples instancias de tablas de enrutamiento en un mismo router, lo que permite manetener separadas las rutas de diferentes clientes o redes

`VRF-Lite` es la implementacion de VRF sin la necesidad de [[999 - Archivado/MPLS|MPLS]], Se utiliza para crear instancias de enrutamiento separadas en [[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]] sin la complejidad de MPLS, permitiendo la segmentacion de redes en entornos empresariales o para soportar multiples conexiones [[010 - Protocolos/010.3 - Comunicaciones/VPN|VPN]]

Recuerda dar ip de las interfaces troncales entre routers antes de configurar VRF

# Sintaxis Configuracion
## VRF-LITE
### Crear Definciones
> en cada `PE` y `CE`
```
vrf definition [vrf-name]
 address-family ipv4
  exit-address-family
 exit
```

### Crear Sub-Interfaces
> En cada `PE` hacia los dispositivos finales | Relacionado a [[020 - Conceptos/020.1 - Administracion/Enrutamiento Inter-Vlan|Enrutamiento Inter-Vlan]]
```
int [int S/S/P]
 no shut
 exit
int [int S/S/P.V]
 encapsulation dot1q [vlan-id]
 vrf forwarding [vrf-name]
 ip add [ip-network] [dec-mask]
 exit
```

### Asignar Etiquetas segun definicion
> En cada `PE` y `CE` 
```
vrf definition [vrf-name]
 vnet tag [vrf-tag-number]
 exit
```

### Aplicar Etiquetas
> En cada `PE` y `CE` en sus conexiones troncales a otros routers con VRF
```
int [int S/S/P]
 vnet trunk
 exit
```

## Configura Protocolo IGP
> [!IMPORTANT]- Importante
> Compatible con [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv2|OSPFv2]], [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP Nombrado|EIGRP Nombrado]] a nivel IPv4

### VRF OSPF
```
router ospf [proceso] vrf [vrf-name]
 network [ip-network] [wildcard] area [area-id]
exit
```

### EIGRP Nombrado
```
router eigrp [AS-name]
 address-family ipv4 vrf [vrf-name] autonomous-system [AS-number]
  network [ip-network] [dec-mask]
  network [ip-network] [dec-mask]
  exit-address-family
 exit
```

# Ejemplo Configuracion
> Ejemplo Completo basado en la siguiente imagen

> [!NOTE]- Nota
> ![1000](https://slink.proxylivy.work/image/38e52e5a-58cb-4cd9-a04f-20dbe60e2947.png)

## SW-1
```
!
!# Crear VLANS
!
vlan 10,20
exit
vtp mode transparent
!
!# Asignar Interfaces Acceso
!
int range e0/0,e0/3
 switchport mode access
 exit
int e0/0
 switchport access vlan 10
 exit
int e0/3
 switchport access vlan 20
 exit
int e0/2
 swithport trunk encapsulation dot1q
 switchport mode trunk
 exit
```
## SW-2
```
!
!# Crear VLANS
!
vlan 10,20
exit
vtp mode transparent
!
!# Asignar Interfaces Acceso
!
int range e0/2,e0/3
 switchport mode access
 exit
int e0/2
 switchport access vlan 10
 exit
int e0/3
 switchport access vlan 20
 exit
int e0/0
 swithport trunk encapsulation dot1q
 switchport mode trunk
 exit
```
## PE1
```
!
!# Definir VRF
!
vrf definition CLIENTE-A
 address-family ipv4
  exit-address-family
 exit
vrf definition CLIENTE-B
 address-family ipv4
  exit-address-family
 exit
!
!# Crear Sub-Interfaces
!
int e0/2
 no shut
 exit
int e0/2.10
 encapsulation dot1q 10
 vrf forwarding CLIENTE-A
 ip add 192.168.10.1 255.255.255.0
 exit
int e0/2.20
 encapsulation dot1q 20
 vrf forwarding CLIENTE-B
 ip add 192.168.10.1 255.255.255.0
 exit
!
!# Direccionamiento IPv4
!
int e0/0
 ip add 20.20.20.1 255.255.255.0
 no shut
 exit
!
!# Etiquetas VRF
!
vrf definition CLIENTE-A
 vnet tag 100
 exit
vrf definition CLIENTE-B
 vnet tag 200
 exit
!
!# Asignar interfaces troncales VRF
!
int e0/0
 vnet trunk
 exit
!
!# Configuracione de OSPF con VRF
!
router ospf 1 vrf CLIENTE-B
 network 192.168.10.0 0.0.0.255 area 0
 network 20.20.20.0 0.0.0.255 area 0
 exit
!
!# Configuracion de EIGRP Nombrado con VRF
!
router eigrp TSHOOT
 address-family ipv4 vrf CLIENTE-A autonomous-system 100
  network 192.168.10.0 255.255.255.0
  network 20.20.20.0 255.255.255.0
  exit
 exit
```
## INTERNET
```
!
!# Definir VRF
!
vrf definition CLIENTE-A
 address-family ipv4
  exit-address-family
 exit
vrf definition CLIENTE-B
 address-family ipv4
  exit-address-family
 exit
!
!# Direccionamiento IPv4
!
int e0/0
 ip add 20.20.20.2 255.255.255.0
 no shut
 exit
int e0/1
 ip add 30.30.30.2 255.255.255.0
 no shut
 exit
!
!# Etiquetas VRF
!
vrf definition CLIENTE-A
 vnet tag 100
 exit
vrf definition CLIENTE-B
 vnet tag 200
 exit
!
!# Asignar interfaces troncales VRF
!
int range e0/0-1
 vnet trunk
 exit
!
!# Configuracione de OSPF con VRF
!
router ospf 1 vrf CLIENTE-B
 network 20.20.20.0 0.0.0.255 area 0
 network 30.30.30.0 0.0.0.255 area 0
 exit
!
!# Configuracion de EIGRP Nombrado con VRF
!
router eigrp TSHOOT
 address-family ipv4 vrf CLIENTE-A autonomous-system 100
  network 20.20.20.0 255.255.255.0
  network 30.30.30.0 255.255.255.0
  exit
 exit
```
## PE2
```
!
!# Definir VRF
!
vrf definition CLIENTE-A
 address-family ipv4
  exit-address-family
 exit
vrf definition CLIENTE-B
 address-family ipv4
  exit-address-family
 exit
!
!# Crear Sub-Interfaces
!
int e0/0
 no shut
 exit
int e0/0.10
 encapsulation dot1q 10
 vrf forwarding CLIENTE-A
 ip add 192.168.11.1 255.255.255.0
 exit
int e0/0.20
 encapsulation dot1q 20
 vrf forwarding CLIENTE-B
 ip add 192.168.11.1 255.255.255.0
 exit
!
!# Direccionamiento IPv4
!
int e0/1
 ip add 30.30.30.1 255.255.255.0
 no shut
 exit
!
!# Etiquetas VRF
!
vrf definition CLIENTE-A
 vnet tag 100
 exit
vrf definition CLIENTE-B
 vnet tag 200
 exit
!
!# Asignar interfaces troncales VRF
!
int e0/1
 vnet trunk
 exit
!
!# Configuracione de OSPF con VRF
!
router ospf 1 vrf CLIENTE-B
 network 192.168.11.0 0.0.0.255 area 0
 network 30.30.30.0 0.0.0.255 area 0
 exit
!
!# Configuracion de EIGRP Nombrado con VRF
!
router eigrp TSHOOT
 address-family ipv4 vrf CLIENTE-A autonomous-system 100
  network 192.168.11.0 255.255.255.0
  network 30.30.30.0 255.255.255.0
  exit
 exit
```

# Visualizar
- `show ip route | b Gateway`: Ver tabla de enrutamiento global
- `show ip route vrf [vrf-name] | b Gateway`: Rutas de VRF
- `show ip vrf int`: Ver interfaces asociadas con VRF
- `ping vrf [vrf-name] [ip-host]`: Hace ping desde el contexto de una VRF


# Troubleshooting
El orden de creacion es muy importante, primero se crea la regla VRF, luego se aplica a la interfaz, y despues va la ip, configurar la ip primero hara que exista fuera del dominio de VRF