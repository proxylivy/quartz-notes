NOTA: si no piden nat, se configura ruta por defecto en el router antes del ISP

# Configuracion
## PAT
Tambien conocido como **NAT con sobrecarga**
> - Nota: Debes crear una [[020 - Conceptos/020.1 - Administracion/ACL#ACL Estandar Nombrada (Denegando 1 ip)|ACL]] estandar para permitir las ip-network
> - Nota 2: `int` de overload es la que va hacia afuera
```
ip nat inside source list [list-name] int [int S/S/P] overload
!# Aplicar PAT en las interfaces
int [int S/S/P]
ip nat inside
int [int S/S/P]
ip nat outside
```
## NAT DINAMICA
```
enable
config t
!# Permitir Trafico con ACL Privado
access-list [list-number] permit [ip-network] [dec-mask]
!
!# Crear grupo de direcciones para NAT IP Publicas
ip nat pool [pool-name] [first-ip-dhcp] [last-ip-dhcp] netmask [dec-mask]
!
!# Asociar ACL con el grupo NAT
ip nat inside source list [list-number] pool [pool-name]
!
!# interfaces NAT
int [int S/S/P]
ip nat inside
exit
!
int [int S/S/P]
ip nat outside
exit
```

# Visualizacion
`show ip nat translations` -> Ver las traducciones que ha hecho, primero debe haber trafico
`show ip nat statistics`