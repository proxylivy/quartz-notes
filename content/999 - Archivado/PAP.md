
# Configuracion
```
R1(config)#username R2 password cisco2023
R1(config)#interface serial 1/0
R1(config-if)#ppp authentication pap
R1(config-if)#ppp pap sent-username R1 password cisco2023
R1(config-if)#exit

R2(config)#username 1 password cisco2023
R2(config)#interface serial 1/0
R2(config-if)#ppp authentication pap
R2(config-if)#ppp pap sent-username R2 password cisco2023
R2(config-if)#exit
```