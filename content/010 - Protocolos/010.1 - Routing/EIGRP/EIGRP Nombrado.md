# Info
A nivel de Funcionamiento es el mismo que [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP|EIGRP]], pero no pueden estar configurado en el mismo AS
Funciona con address family, compatible con VRF
Permite tener una configuracion mas ordenada

## Parametros K
Compatibilidad con [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP#Parametros K|EIGRP: Parametros K]], se agrega
- K6() - Extended (jitter or Energy) (Opcional)


# Configuracion
## Network Nombrada
```
int loopback [loopback-number]
 ip address [ip] [dec-mask]
 exit
int [int S/S/P]
 ip address [ip] [dec-mask]
 exit
router eigrp [eigrp-name]
 address-family ipv4 unicast autonomous-system [AS]
  network [ip-network] [dec-mask]
```
## Autenticacion Nombrada
> Configuracion en modo configuracion Global `RX(config)#`
```
key chain [llavero]
 key 1
  key-string [password]
  exit
 exit
router eigrp TSHOOT 
 address-family ipv4 autonomous-system [AS]
  network [ip-network] [dec-mask]
  af-interface [int S/S/P]
   authentication key-chain [llavero]
   authentication mode hmac-sha-256 [password]
```

## Extras
Funciona en 64 bits