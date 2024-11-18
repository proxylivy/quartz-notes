# Info
Modifica el funcionamiento de las clases credas con [[020 - Conceptos/020.1 - Administracion/Class-Map|Class-Map]] y controla el trafico a velocidades determinadas para minimazar la carga. Se procesa de arriba hacia abajo en busqueda de alguna politica (la ultima es poliica por defecto) para que se aplique a la clase correspondiente.


> [!IMPORTANT] Importante
> Encontrar la tasa correcta sin afectar el sistema es complicado

El mapa de politicas es aplicado al plano de control y debe ser verificado

## Terminologia
- `[policy-name]`: Nombre de la regla, Case-Sensitive
- `[class-name]`: Vease [[020 - Conceptos/020.1 - Administracion/Class-Map|Class-Map]]
- `[speed-value]`: Velocidad medida en CIR(Committed Information Rate) -> **bps**(bits por segundo) o RATE -> **pps**(paquetes por segundo), se puede ajustar despues
- `[conform-action | exceed-action | violation-action]`: Umbral para ser aplicadas reglas segun el estado, `violation-action` es opcional, se pueden aplicar varios en la misma regla
- `[transmit | drop]`: Accion Aplicada segun el umbral con el que es procesado el paquete

# Sintaxis Configuracion
> Creacion de Politica
```
policy-map [policy-name]
 class [class-name]
  police [speed-value] [conform-action | exceed-action | violation-exceed] [transmit | drop]
```

> Aplicar al Plano de Control
```
control-plane
 service-policy input [policy-name]
```

# Ejemplos Configuracion
```
policy-map POLICY-CoPP
 class CLASS-CoPP-ICMP
  police 8000 conform-action transmit exceed-action transmit
   violate-action drop
 class CLASS-CoPP-IPSEC
  police 64000 conform-action transmit exceed-action transmit
   violate-action transmit
 class CLASS-CoPP-Initialize
  police 8000 conform-action transmit exceed-action transmit
   violate-action drop
 class CLASS-CoPP-Management
  police 32000 conform-action transmit exceed-action transmit
   violate-action transmit
 class CLASS-CoPP-Routing
  police 64000 conform-action transmit exceed-action transmit
   violate-action transmit
 class class-default
  police 8000 conform-action transmit exceed-action transmit
   violate-action drop
```

# Visualizar
- `show policy-map control-plane`: Ver mapas de politicas aplicados al plano de control
- `show policy-map control-plane input`

# Troubleshooting
- Solo puede ser aplicado a la interfaz de plano de control
- Solo pueden aplicarse reglas a direcciones que usen el plano de control sea de entrada o salida