# Info
**C**hallenge-**H**andshake **A**uthentication **P**rotocol es un enlace basado en el saludo de 3 vias, autenticacion mediante MD5. Su funcionamiento verifica ambas bases de datos locales para poder iniciar sesion

`Username = Hostname Local Router`, la contraseña es compartida

# Configuracion
```
!# R1
username R2 password cisco2023
interface serial 1/0
 encapsulation ppp
 ppp authentication chap
 exit

!# R2
username R1 password cisco2023
interface serial 1/0
 encapsulation ppp
 ppp authentication chap
```

# Visualizacion
- `show ip int brief`
- `show ppp all`
- `show username`
- `debug ppp authentication`: se visualiza los mensajes solicitando autenticacion
