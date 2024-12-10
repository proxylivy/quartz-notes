# Que es
## Port-Security
Permite asegurar las conexiones de los puertos de [[010 - Protocolos/010.2 - Switching/VLAN#Access|Acceso]] al limitar y controlar el trafico en funcion a las direcciones MAC de los dispositivos conectados.

Funciones:
- Limitar direcciones MAC
- Limitar transito de [[010 - Protocolos/010.2 - Switching/spanning-tree/BPDU|BPDU]]
- Acciones de Seguridad
- Persistencia de direcciones MAC

# Configurar
## Habilitar Seguridad Puerto Dinamico
Al habilitarlo, por defecto solo aprende 1 [[020 - Conceptos/020.3 - Fundamentos/MAC|MAC]], se usa en puertos de [[010 - Protocolos/010.2 - Switching/VLAN#Access|Acceso]]
```
int [int S/S/P]
 switchport port-security
```
## Accion Violacion Puerto
Diferencias entre reportes
- Shutdown: Err-Disable
- Protect: Ignora nuevas mac, pero continua funcionando con las viejas mac
- Restrict: Ignora nuevas mac, notifica por SNMP o SYSLOG, y funcionan viejas MAC
```
int [int S/S/P]
 switchport port-security violation {protect | restrict | shutdown | shutdown vlan}
```
## Seguridad de Puerto por Aprendizaje
```
int [int S/S/P]
 switchport port-security
 switchport port-security maximum [number]
 switchport port-security mac-address [mac-address]
 switchport port-security mac-address sticky
```
## Habilitar puertas de un err-disable
```
int [int S/S/P]
 shutdown
 no shutdown
```
## Enlazado Con
- [[020 - Conceptos/020.2 - Seguridad/Mitigacion de ataques#Spoofing MACs|Spoofing MACs]]

# Visualizacion
- `sh port-security`
- `sh port-security int [int S/S/P]`
- `sh interfaces status` → Ver estado de puertas, como “Connected“ o “err-disable“