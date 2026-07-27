# Info
Las interfaces pueden ser:
- Gigabit (`g`)
- Serial (`s`)
- Fast Ethernet (`f`)
- Ethernet (`e`)
- Vlan (`vl`)
- Port-channel (`po`)

Se separa en (`S/S/P.SV`) o (`Stack/Slot/Port.Sub-vlan`):
- `S`: Stack
- `S`: Slot or Module
- `P`: Port
- `S`: Subvlan

# Visualizacion
`show ip int brief` -> Ver estado interfaces
`show ip int brief | include up` -> Ver interfaces up

# Extra
Dado Freak: En conexiones Seriales se configura el `[clock-rate]` el cual es para que la sincronizacion de paquetes no chocara, se usaba en topologias de DCU-DSU / DTE-(DCE)