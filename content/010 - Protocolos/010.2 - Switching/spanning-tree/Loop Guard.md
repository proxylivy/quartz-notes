## Que hace
Evita Loop de Capa 2, Al activarlo y que no se recibe BPDU en un puerto no designado, ese puerto cambia a `loop-inconsistent-blocking`, semejante del estado aprendizaje a escucha.

Bloquea la negociacion en la puerta frente a la bloqueada para que no cambie aunque la topologia sea diferente.
Solo funciona cuando la topologia tiene 1 vlan y no se usa en el ramo

## Configuracion
`spanning-tree guard loop` -> Activa Loop Guard en un puerto
`spanning-tree loopguard default` -> Activa Loop Guard en todos los enlaces Punto a Punto

# Visualizacion
`show spanning-tree inconsistent ports`
