# Info
**M**ulti-**P**rotocol **L**abel **S**witching es un metodo para configurar rutas dedicadas aislando el trafico entre la red de clientes a travez de multiples [[999 - Archivado/VRF|VRF]]

# Configuracion
```
int [int S/S/P]
xconnect 4.4.4.4 222 encapsulation mpls
```

```
ip cef
mpls ldp autoconfig loopback [loopback-number]
int range [int S/S/P-P]
mpls ip
```

```
!# ???
mpls label range 500 599
```

```
ip vrf CLIENTE-A
rd 65200:1
route-target export 65200:1
route-target export 65300:1
int [int S/S/P]
ip vrf forwarding CLIENTE-A
```
# Visualizacion
`show mpls forwarding table`: Enlace L3
`show ip bgp vpnv4 all`: Comprobar red MPLS

# Extra
- Definido en el [RFC3031](https://www.rfc-editor.org/rfc/rfc3031)