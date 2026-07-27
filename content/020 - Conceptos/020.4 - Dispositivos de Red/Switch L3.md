Un [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]] con capacidades de [[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]], permite hace un [[010 - Protocolos/010.2 - Switching/Etherchannel|Etherchannel]] de capa 3, 

Para convertir un puerto de un Switch L2 a L3, se usa el comando `no switchport`, y  se le configura una ip.

# Configuracion
## DHCP IP Helper
```
int vlan [vlan-number]
ip helper-address [ip-router-dhcp]
```