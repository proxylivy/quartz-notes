Es un tipo de paqute para identificar, clasificar y notificar cambios en la topologia, hay 2 tipos:
- BPDU Configuracion: identifica a [[010 - Protocolos/010.2 - Switching/spanning-tree/Root Bridge|Root Bridge]], Root Port, Designated Port y Blocking Port, sus identificadores son:
	- STP Type
	- Root path cost
	- Root bridge identifier
	- Local bridge identifier
	- Max Age
	- Hello Time
	- Forward Delay
- Topology Change Notification (TCN) BPDU: 

## Tiempos por defectos:
- Hello Time: 2 segundos
- Max Age: 20 Segundos
- Forward Delay: 15 Segundos

NOTA: Cuando se elige el Root Bridge, el costo es 0, y el punto mas lejano tendra el valor mas alto
