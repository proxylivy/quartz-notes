# Info
Son:
- [[#Porfast]]
- [[#BPDU Guard]]
- [[#BPDU Filter]]

## Porfast
### Que hace Portfast?
Caracteristica de [[010 - Protocolos/010.2 - Switching/spanning-tree/802.1W (RSTP)|802.1W (RSTP)]] y [[010 - Protocolos/010.2 - Switching/spanning-tree/802.1S (MSTP)|802.1S (MSTP)]]
Para que un enlace de acceso pase el estado de puerto de bloqueo a reenviando mas rapido, los temporizadores predeterminados son 15 en escuchar a aprender y 15 segundos en aprender a enviar.
### Configurar Portfast
```
int [int S/S/P]
spanning-tree portfast
```
## BPDU Guard
### Que Hace BPDU Guard?
Cuando actives Portfast seguira escuchando [[010 - Protocolos/010.2 - Switching/spanning-tree/BPDU|BPDU]], cuando reciba una BPDU el puerto podria bloquearse, y un loop no puede ser detectado indefinidamente.
Protege la integridad junto a Portfast y hace cumplir la frontera del dominio STP
### Como Funciona BPDU Guard
Cuando se activa si en algun puerto se detecta una BPDU, entra en estado `err-disable`. El puerto se cierra y debe ser activado de forma manual o automatica a traves de la funcion de tiempo de espera de errores.
### Configurar BPDU Guard
## Conf BPDU Guard
```
int [int S/S/P]
spanning-tree bpduguard enable
```

## BPDU Filter
### Que hace BPDU Filter?
Aunque Portfast y BPDUGuard esten habilitados, se continua enviando [[010 - Protocolos/010.2 - Switching/spanning-tree/BPDU|BPDU]], con BPDU Filter dejan de ser enviados
### Como funciona BPDU Filter
Se usa con los proveedores de servicios para que los clientes no sepan sobre su infraestructura de red.
### Configurar BPDU Filter
```
int [S/S/P]
spanning-tree bpdufilter enable
```

# Visualizar
`show run int [int S/S/P]` -> Ver configuracion interfaz
`show spanning-tree summary` -> Ver resumen configuraciones por defecto
`show spanning-tree summary totals` -> Verifica la configuracion de BPDU Guard
`show spanning-tree interface [int S/S/P]`: Ver informacion puerto
`show spanning-tree interface [int S/S/P] portfast` -> Verifica Estado Portfast
`show spanning-tree totals` -> Verifica la configuracion global de BPDU Filter
`show spanning-tree interface [Int S/S/P] detail` -> Informacion extra sobre una interfaz y su mecanismo de estabilizacion
`show interfaces status` -> Ver puerto y estado `err-disable`, vlan asociada, velocidad, duplex y tipo de conexion.

# Extras
## Extras Portfast
Permite configurar portfast automaticamente en todas puertas como acceso
`spanning-tree portfast default`
`spanning-tree portfast trunk` -> Nunca usar en puertos [[010 - Protocolos/010.2 - Switching/VLAN#Troncal|Troncales]], porque puede generar loop fisicos, aunque hay excepciones como pasar vlan en un cluster de switch.
## Extras BPDU Guard
`spanning-tree portfast bpduguard default` -> Configura BPDU Guard en todos los puertos que tengan activado portfast
## Extras BPDU Filter
Permite configurar BPDU Filter en los puertos que tengan Portfast activado
`spanning-tree portfast bpdufilter default`