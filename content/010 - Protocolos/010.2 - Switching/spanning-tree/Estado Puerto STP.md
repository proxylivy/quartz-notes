# Info

## Puerto Raiz (RP)
Es el [[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]] principal con el cual se hace el calculo [[010 - Protocolos/010.2 - Switching/spanning-tree/802.1D (STP)|802.1D (STP)]], los otros switch tienen Puertos raiz conectado a este
Cada Switch al iniciar piensa que es el [[010 - Protocolos/010.2 - Switching/spanning-tree/Root Bridge|Root Bridge]], luego escucha los [[010 - Protocolos/010.2 - Switching/spanning-tree/BPDU|BPDU]] vecinos y:
- Si la configuracion del vecino es inferior a su BPDU, ignora esa BPDU
- Si es superior, se incluye el identificador junto al [[010 - Protocolos/010.2 - Switching/spanning-tree/802.1D (STP)#Costos de Enlace|Costo]] de la ruta, este proceso continua hasta que todos los switch coincidan
- Si la prioridad es identica, BPDU con MAC base mas baja

### Seleccion
- Interfaz menor costo para el [[010 - Protocolos/010.2 - Switching/spanning-tree/Root Bridge|Root Bridge]]
- Interfaz Prioridad mas baja
- Mac mas Baja

## Puerto Designado (DP)


## Puerto Bloqueado


## Puerto Alternativo


## Puerto Backup



# Visualizar
`show spanning-tree brief`