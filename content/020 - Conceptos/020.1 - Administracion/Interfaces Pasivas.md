# Info
Una interfaz pasiva en protocolos de enrutamiento dinamico (Como [[010 - Protocolos/010.1 - Routing/EIGRP/EIGRP|EIGRP]] o [[010 - Protocolos/010.1 - Routing/OSPF/OSPFv2|OSPFv2]]) **desactiva el envio y recepcion** de mensajes de descubrimiento y adyacencia en una interfaz especifica. La red asociada a la interfaz puede ser anunciada y enrutada por routers vecinos.

La configuracion `default`, activa todas las interfaces como pasivas y tendras que desactivar una por una.
## Ventajas
- Reduccion del trafico del protocolo al evitar enviar mensajes extra
- Añade seguridad al evitar que routers no autorizados sean parte de la red
## Desventajas
- Puede generar problemas de adyacencia con routers vecinos
- El diagnostico puede volverse mas complejo debido a que se desactiva una funcion especifica
## Ejemplos de configuracion
- Dentro del [[020 - Conceptos/020.1 - Administracion/Enrutamiento Inter-Vlan|Enrutamiento Inter-Vlan]], las subinterfaces se configuran pasivas pero sus redes se integran al protocolo
- Las interfaces de [[010 - Protocolos/010.2 - Switching/VLAN#Access|Acceso]] que van a dispositivos finales irian con pasivas pero sus rutas se agregan al protocolo
- en el ASBR donde no se espera una conexion con otros routers

# Configuracion
## EIGRP
```
router eigrp 1
passive-interface [int S/S/P]
```
## OSPF
```
router ospf 1
passive-interface [int S/S/P]
```
### Default en OSPF
```
router ospf 1
passive-interface default
no passive-interface [int S/S/P]
```
