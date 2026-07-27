# Info
Tecnica propietaria de Cisco para administrar QoS mediante la clasificacion de tipos de trafico (gestion, protocolos de enrutamiento o direcciones IP conocidas). Es recomendado usar [[020 - Conceptos/020.1 - Administracion/ACL#ACL Extendida Nombrada|ACL Extendida Nombrada]]

## Terminologia
- `match-any`: Tipo de instruccion si **Coincide** si se cumple con **al menos una** de las condiciones de una regla
- `match-all`: Tipo de instruccion si **Coincide** si se cumple con **todas** las condiciones de una regla
- `match protocol`: especifica un protocolo si no hay una [[020 - Conceptos/020.1 - Administracion/ACL|ACL]] configurada en las reglas


# Sintaxis Configuracion
```
class-map match-all [class-name]
 match access-group name [acl-name]
```


# Troubleshooting
- `[class-name]`
	- Case-Sensitive: Mayuscula != Minuscula, cuidado con el nombre

# Extra

Existe pero no se usa tanto??
- `match ip precedence`: Coincidir la ip de precencia
- `match ip dscp`: Coincidir con puntos de codigo de servicio diferenciado