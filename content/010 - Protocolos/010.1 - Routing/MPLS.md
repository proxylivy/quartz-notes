# Info
**M**ulti-**P**rotocol **L**abel **S**witching es una tecnologia de transporte de datos de alto rendimiento que esta basado en etiquetas de [[999 - Archivado/VRF|VRF]] para separar el trafico entre clientes, trabajando en la [[020 - Conceptos/020.3 - Fundamentos/Modelo OSI#Capas Multiples|Capa 2.5]] del Modelo OSI debido a que agrega una etiqueta al paquete, es usado junto a [[010 - Protocolos/010.1 - Routing/OSPF/OSPF|OSPF]]. Es una alternativa a la "Fibra Oscura" o "Fibra de enlace dedicado", no se conecta mediante internet sino que se usa WAN

## Terminologia
- [[020 - Conceptos/020.4 - Dispositivos de Red/IOU WEB#CEF|CEF]] (**C**isco **E**xpress **F**orwarding)
- PE (**P**rovider **E**dge)
- P (**P**rovider MPLS)
- CE (**C**ustomer **E**dge)
- LSR (**L**abel **S**witching **R**outer): Routers dentro de MPLS que envian y traducen etiquetas
	- LER (**L**abel **E**dge **R**outer): Inicia o termina la comunicacion MPLS, traduce IP a Etiquetas y viceversa
- LDP (**L**abel **D**istribute **P**rotocol)
- [[999 - Archivado/RIB|RIB]] (**R**outing **I**nformation **B**ase)
- FIB (**F**orwarding **I**nformation **B**ase)
- LIB (**L**abel **I**nformation **B**ase)
- LFIB (**L**abel **F**orwarding **I**nstance **B**ase)
- LSP (**L**abel **S**witched **P**ath): Secuencia de ruta y etiquetas que fueron aplicadas e intercambiando entre los P, cuando llegue a un LSR, envia al FIB, la ruta de ida puede ser diferente a la vuelta
- PHP (**P**enultimate **H**op **P**opping OR Pop Labeling)
- RD (**R**outer **D**istinguisher): Se usa para importar o exportar rutas, usado en VPNv4

## Funcionamiento
MPLS separa el plano de control del plano de datos a travez de etiquetas, estas etiquetas son locales (~4096) y pueden cambiar al ser enrutados

La etiqueta contiene toda la informacion del paquete: IP, VPN Label, LDP label para ser entregadas entre PE de MPLS

Para el enrutamiento de [[010 - Protocolos/010.3 - Comunicaciones/VPN|VPN]] L2, el cliente se encarga del enrutamiento mediante los routers CE
Para el enrutamiento de [[010 - Protocolos/010.3 - Comunicaciones/VPN|VPN]] L3, el ISP se encarga del enrutamiento mediante los routers PE


# Sintaxis Configuracion
La diferencia entre capa 2 y 3 es que en la capa 2, los CE se encargan de la configuracion, mientras que en la capa 3, son los PE los que enrutan

## MPLS Capa 2
Pasos
1. Habilitar MPLS en el router (`ip cef` viene activado por defecto)
2. Las loopback permiten identificar si un router esta operativo, MPLS se activa dentro de la interfaz Loopback
3. Se habilita OSPF para que puedan intercambiar etiquetas entre routers

### Habilitar MPLS de forma Global
> Configuracion en todos los router `P` y `PE`
```
ip cef
mpls ip
!
mpls ldp router-id loopback [loopback-number]
!
router ospf 1
mpls ldp autoconfig
!
```

### Conexion entre sucursales
> Configuracion en los `PE` de extremo
> 1. Se ingresa a la interfaz que conecta a router `CE`
> 2. Se configura RID de `CE` y un numero de Circuito aletario (Igual entre clientes)
```
int [int S/S/P]
 xconnect [RID-CE] [local-circuit-number] encapsulation mpls
```

### Configura enrutamiento
> [!NOTE] Nota
> Puedes Configurar protocolos de [[020 - Conceptos/020.3 - Fundamentos/IGP|IGP]]
> Vecindad entre `PE` y `CE`
```
!# RIP
!
int [int S/S/P]
 ip add [ip-network] [dec-mask]
 no shut
exit
router rip
 version 2
 network [ip-network]
 network [ip-network]
 exit
!
!# EIGRP
!
int [int S/S/P]
 ip add [ip-network] [dec-mask]
 no shut
 exit
router eigrp
 no auto-summary
 network [ip-network] [dec-mask]
 exit
```


## MPLS Capa 3
### Habilitar MPLS de Forma Global

Pasos
1. Habilitar MPLS en el router (`ip cef` viene activado por defecto)
2. Las interfaces loopback son las mas confiables dentro de un router
3. Se habilita mpls IP dentro de las interfaces que participan en MPLS

> En todos los routers `P` y `PE`
```
ip cef
mpls ip
!
mpls ldp router-id l0
int [int S/S/P]
 mpls ip
 exit
router ospf 1
 mpls ldp autoconfig
```

### Modifican valores de interfaces que no se usan
> No entendi esta parte la verdad
```
int [S/S/P]
 ip ospf cost 888
```

### No se para que es esto
```
mpls label range [mpls-label-init-number] [mpls-label-end-number]
```

### Implementar VRF
> Solo se configura en los `PE`
> `CN`: Customer Number
```
ip vrf [vrf-name]
 rd [BGP-ASN]:[NN]
 route-target export [BGP-ASN]:[CN]
 route-target import [BGP-ASN]:[CN]
 exit
int [S/S/P]
 ip vrf forwarding [vrf-name]
 ip add [ip-network] [dec-mask]
 no shut
 exit

```

### Implementar MP-BGP y Montar VPNv4
> Solo se configura en los `PE`
> Cuando se crea la adyancencia, se crea un `community-extended` dentro de BGP
```
router bgp [BGP-ASN]
 bgp router-id [RID]
 neighbor [ip-neighbor] remote-as [BGP-ASN]
 neighbor [ip-neighbor] update-source loopback [loopback-number]
router bgp [BGP-ASN]
 address-family vpnv4 unicast
  neighbor [ip-neighbor] activate
```

### BGP PE a CE desde PE
> Se configura entre `PE`
```
router bgp [BGP-ASN]
 address-family ipv4 vrf [vrf-name]
  neighbor [ip-neighbor] remote-as [BGP-ASN]
  exit
```

### BGP CE a PE desde CE
> Se configura en `CE`
```
router bgp [BGP-ASN]
 neighbor [ip-neighbor] remote-as [BGP-ASN]
 netork [ip-network] mask [dec-mask]
```

### Solucionar Problema con AS_PATH
> Se configura en `CE`
```
router bgp [BGP-ASN]
 neighbor [ip-neighbor] allowas-in
 exit
end
ping [ip-neighbor] source loopback 0
traceroute [ip-neighbor] source loopback 0 numeric
```

# Visualizacion
- `sh mpls forwarding table`: Ver MPLS configurado Globalmente
- `sh cdp neighbors`: Ver conexiones entre sucursales
- `sh mpls interfaces`
- `sh mpls lpd neighbors`
- `sh forwarding table`: Ver etiquetas de entrada y salida
- `show ip bgp vpnv4 all`: Comprobar red MPLS
- `sh ip bgp vpnv4 all`

# Extra
- Definido en el [RFC3031](https://www.rfc-editor.org/rfc/rfc3031)
- Ayuda Externa
	- [MPLS Topology](https://loopedback.com/2019/11/24/mpls-lsp-path-selection-lib-lfib-rib-fib-and-cef-forwarding-reviewed-how-to-view-the-tables-and-how-they-all-work-to-get-traffic-to-its-destination-network/)

- Se necesitan certificar las velocidades de los enlaces, se puede hacer con [iperf3](https://github.com/esnet/iperf)