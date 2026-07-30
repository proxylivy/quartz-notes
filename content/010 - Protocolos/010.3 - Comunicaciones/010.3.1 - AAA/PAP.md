# Info
PAP (**P**assword **A**uthentication **P**rotocol) es un metodo de autenticacion en texto plano y verifica ambas bases de datos locales

`Username = Hostname Local Router` y la contraseña es compartida

# Configuracion
```
!# R1
username R2 password cisco2023
interface serial 1/0
 ppp authentication pap
 ppp pap sent-username R1 password cisco2023
 exit
!
!# R2
username 1 password cisco2023
interface serial 1/0
 ppp authentication pap
 ppp pap sent-username R2 password cisco2023
```