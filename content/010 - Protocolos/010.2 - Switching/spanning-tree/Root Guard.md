# Info
## Que hace
Asegura la seleccion del [[010 - Protocolos/010.2 - Switching/spanning-tree/Root Bridge|Root Bridge]] en [[010 - Protocolos/010.2 - Switching/spanning-tree/802.1D (STP)|802.1D (STP)]], desactiva la escucha de [[010 - Protocolos/010.2 - Switching/spanning-tree/BPDU|BPDU]] en un puerto para que no negocie un nuevo [[010 - Protocolos/010.2 - Switching/spanning-tree/Root Bridge|Root Bridge]].
Protegue de anomalias de red en capa 2 como Loops, se usa solo en topologias de una [[010 - Protocolos/010.2 - Switching/VLAN|VLAN]]
## Funcionamiento
Si un puerto habilitado con Root Guard escucha un [[010 - Protocolos/010.2 - Switching/spanning-tree/BPDU|BPDU]] con un B-ID superior al que envia [[010 - Protocolos/010.2 - Switching/spanning-tree/Root Bridge|Root Bridge]] el puerto pasa a `root-incosistent`, similar el [[010 - Protocolos/010.2 - Switching/spanning-tree/802.1D (STP)#Estados de Puertos|Estado]] de "Escucha (Listening)" y no reenvia mas datos.

## Configuracion
Se configura en las interfaces troncales estaticas de [[010 - Protocolos/010.2 - Switching/spanning-tree/Root Bridge|Root Bridge]]
```
int [S/S/P]
spanning-tree guard root
```

# Extra
Si un switch no autorizado se conecta a la topologia STP con un B-ID superior, se puede convertir en el [[010 - Protocolos/010.2 - Switching/spanning-tree/Root Bridge|Root Bridge]] y cambiar toda la topologia

A pesar de que la prioridad del puente pueda ser configurado en `0` esto no asegura la seleccion obligatoria, por lo que root guard seria mejor opcion