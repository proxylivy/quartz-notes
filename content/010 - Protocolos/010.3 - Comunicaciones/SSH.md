
Se usa dominio para poder generar los certificados correctamente
# Configure
## SSH v2
[[020 - Conceptos/020.4 - Dispositivos de Red/Switch|Switch]] con [[010 - Protocolos/010.2 - Switching/VLAN#Vlan Administrativa|VLAN Administrativa]]
```
ip ssh version 2
ip sub authentication-re [retries-number]
ip ssh time-out [time-sec]
```
## Habilitar SSH en VTY
```
username [user] secret [password]
ip domain-name [domain]
crypto key generate rsa
1024
line vty [session-number-start] [session-number-end]
transport input ssh
login local
exit
```
