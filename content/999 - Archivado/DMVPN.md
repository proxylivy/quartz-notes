# Info
**D**ynamic **M**ulti-Point **V**irtual **P**rivate **N**etwork, no es un protocolo sino una tecnica para crear VPN de forma dinamica

Estas se logra usando 3 tecnologias
- [[010 - Protocolos/010.1 - Routing/Tunel GRE|Tunel GRE]] Mejorado -> mGRE: Permite conexiones Multipunto, IP Unicast, Multicast y Broadcast
- [[010 - Protocolos/010.1 - Routing/IPSEC|IPSEC]]
- NHRP: Configuracion Hub/Spoke, el cual es un servidor para que los Spoke puedan encontrar rutas a multiples Spoke bajo demanda

# Configuracion


# Extra
- NHRP Definido en [RFC2332](https://datatracker.ietf.org/doc/html/rfc2332)