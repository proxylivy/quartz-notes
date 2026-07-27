# Info
**D**ynamic **T**roncal **P**rotocol, negocia estados troncales en el funcionamiento de los puertos de switch conectados a un troncal y seleccionando un protocolo de enlace troncal apropiado, es una practica recomendada en redes multicapa cambiantes para evitar problemas con la configuracion iniciar de troncal, aunque se recomiendas en algunos casos. Usa señales **ISL**([Propietario Cisco](https://www.cisco.com/c/en/us/support/docs/lan-switching/8021q/17056-741-4.html)) o [[010 - Protocolos/010.2 - Switching/VLAN|802.1Q]]

## Tabla de configuracion
NOTE: LC -> Limited Conectivity

|                   | Dynamic Auto | Dynamic Desirable | Trunk     | Access     |
| ----------------- | ------------ | ----------------- | --------- | ---------- |
| Dynamic Auto      | Access       | Trunk             | Trunk     | Access     |
| Dynamic Desirable | Trunk        | Trunk             | Trunk     | Access     |
| Trunk             | Trunk        | Trunk             | **Trunk** | LC         |
| Access            | Access       | Access            | LC        | **Access** |
## No funciona en:
- Catalyst 2900XL
- Catalyst 3500XL

## Configuracion
### Desactivar DTP
NOTA: Debe ser en las interfaces conectadas a otros switch
`switchport mode [trunk|access]` -> Activa la troncal o Access del switch
`switchport nonegotiate` -> Desactiva DTP

### Activa DTP
`switchport mode dynamic auto`

## Visualizacion
`show dtp interface [int S/S/P]` -> Ve el estado de DTP