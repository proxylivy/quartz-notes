

# Configuracion
```
!# R1
R1(config)#username R2 password cisco2023
R1(config)#interface serial 1/0
R1(config-if)#encapsulation ppp
R1(config-if)#ppp authentication chap
R1(config-if)#exit

!# R2
R2(config)#username R1 password cisco2023
R2(config)#interface serial 1/0
R2(config-if)#encapsulation ppp
R2(config-if)#ppp authentication chap
R2(config-if)#exit
```

# Visualizacion
`debug ppp authentication`: se visualiza los mensajes solicitando autenticacion
