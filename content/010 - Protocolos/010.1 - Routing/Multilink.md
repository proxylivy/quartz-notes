# Info
Multilink es una tecnologia para agrupar seriales, parecido a [[010 - Protocolos/010.2 - Switching/Etherchannel|Etherchannel]] pero en routers
# Configuracion
## Habilitar Multilink en Interfaz
```
int serial [S/S/P]
encapsulation ppp
ppp multilink
ppp multilink group [multilink-group-number]
```
## Configurar Multilink
```
int multilink [multilink-group-number]
ip address [ip] [dec-mask]
no shut
```

# Visualizacion
- `sh ip int brief`
- `sh int multilink [multilink-group-number]`
- `sh ppp all`