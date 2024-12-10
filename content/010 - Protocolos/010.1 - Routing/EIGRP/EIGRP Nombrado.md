# Info
Basado en [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP|EIGRP]], Funciona en un rango de 32 bits (AS 1-4294967295) , Protocolo Propietario de Cisco, permite una configuracion mas flexible, granular y modular en comparacion a [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP Numerado|EIGRP Numerado]]

## Datos
- Soporte Multiples Instancias de EIGRP
- Facilita el uso de [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv4|IPv4]] y [[010 - Protocolos/010.3 - Comunicaciones/010.3.4 - IP/IPv6|IPv6]] mediante AF
- Mejor organizacion y mantenimiento de configuraciones
- No es compatible con [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP Numerado|EIGRP Numerado]] en el mismo ASN
- Funciona con address family, compatible con [[999 - Archivado/VRF|VRF]]
- Permite tener una configuracion mas ordenada

## Parametros K
- Mas informacion en [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP#Parametros K|EIGRP]]
	- Agrega parametro `K6` - Extended (jitter or Energy) (Opcional)

# Configuracion
## Network Nombrada
```
int loopback [loopback-number]
 ip address [ip] [dec-mask]
 exit
int [int S/S/P]
 ip address [ip] [dec-mask]
 exit
router eigrp [AS-Name]
 address-family ipv4 unicast autonomous-system [ASN]
  network [ipv4-network] [dec-mask]
  exit
 address-family ipv6 unicast autonomous-system [ASN]
  network [ipv6-network/prefix]
```
## Autenticacion Nombrada
> Configuracion en modo configuracion Global `RX(config)#`
```
key chain [llavero]
 key 1
  key-string [password]
  exit
 exit
router eigrp [AS-Name]
 address-family ipv4 autonomous-system [ASN]
  network [ip-network] [dec-mask]
  af-interface [int S/S/P]
   authentication key-chain [llavero]
   authentication mode hmac-sha-256 [password]
```
