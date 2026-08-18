# Info

Anexos disponibles en [Copyparty - EX 3 - AA4](https://copyparty.proxylivy.work/Duoc/Conectividad%20y%20Redes/2do%20Semestre/VTY2112%20(CE)/Actividades/EX%203/AA4/)

**Escenario**

En este laboratorio, la empresa para la que trabaja está experimentando problemas con su red de área local (LAN). Se le ha pedido que resuelva problemas y resuelva los problemas de red. En la Parte 1, se conectará a los dispositivos de la LAN y utilizará herramientas de solución de problemas para identificar los problemas de red, establecer una teoría de causa probable y probar esa teoría. 

En la Parte 2, establecerá un plan de acción para resolver e implementar una solución. En la Parte 3, comprobará que se ha restaurado la funcionalidad completa. La Parte 4 proporciona espacio para documentar los hallazgos de solución de problemas junto con los cambios de configuración que realizó en los dispositivos LAN.

**Materiales**
- 2 Routers
- 1 Switches
- 1 PCs Finales
- 4 Cables de Red UTP CAT6a
- 1 Cable Consola Rollout 8P8C (RJ45) a USB
	- Alternativo: 1 Cable Consola Cisco 8P8C (RJ45) a DB9 + Adaptador DB9 a USB

En un computador con Windows, necesitas
- Driver CH341 (Chip Barato): [Github - DecaturMakers/CH340_drivers](https://github.com/DecaturMakers/CH340_drivers-Linux-Mac-Windows)
- Saber que dentro de "Administrador de Dispositivos" buscar como se llama el serial, deberia aparecer como "COMn" (Donde la N es el numero de la conexion)

En Linux, los drivers estan dentro del Kernel, y deberia reconocerlo como `/dev/ttyUSB0` (Va incrementando 1 en 1 por cada conexion Serial USB)

Para ambos se utiliza [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/)

*Tabla Conectividad*


| Dispositivo | Interfaz | IPv4            | Mascara         | Gateway     |
| ----------- | -------- | --------------- | --------------- | ----------- |
| R1          | G0/0     | 192.168.1.1     | 255.255.255.0   | N/A         |
|             | S0/0/0   | 10.1.1.1        | 255.255.255.252 | N/A         |
| R3          | S0/0/0   | 10.1.1.2        | 255.255.255.252 | N/A         |
|             | Lo0      | 209.165.200.226 | 255.255.255.255 | N/A         |
| SWA         | VLAN 1   | 192.168.1.2     | 255.255.255.0   | 192.168.1.1 |
| PC-A        | N/A      | 192.168.1.10    | 255.255.255.0   | 192.168.1.1 |

**Configuracion**

Los siguientes valores deben configurarse en los dispositivos que se muestran en la topología. Pegue las configuraciones en los dispositivos especificados antes de iniciar el laboratorio. 

**SWA**

```plain
no ip domain-lookup 
hostname SWA 
ip domain-name ccna-lab.com 
username admin01 privilege 15 secret cisco12345 
interface FastEthernet0/1
 shutdown
interface FastEthernet0/2
 shutdown 
interface FastEthernet0/3
 shutdown
interface FastEthernet0/4
 shutdown
interface FastEthernet0/5
 speed 10
 duplex half 
interface Vlan1 
 ip address 192.168.1.2 255.255.255.0 
ip default-gateway 192.168.1.0 
banner motd $ Authorized Users Only! $ 
line vty 0 4 
 login local 
 transport input ssh 
line vty 5 15 
 login local 
 transport input ssh 
crypto key generate rsa general-keys modulus 1024 
end 
```

**R1**

```
hostname R1 
no ip domain lookup 
ip domain name ccna-lab.com 
username admin01 privilege 15 secret cisco12345 
interface GigabitEthernet0/0/1 
 ip address 192.168.1.1 255.255.255.0 
 no negotiation auto  
 speed 100 
 no shutdown 
interface GigabitEthernet0/0/0 
 ip address 10.1.1.1 255.255.255.252 
 no shutdown 
banner motd $ Authorized Users Only! $ 
line vty 0 4 
 login local 
 transport input ssh 
crypto key generate rsa general-keys modulus 1024 
end
```

**R3**

```
hostname R3 
no ip domain lookup 
interface GigabitEthernet0/0/0 
 ip address 10.1.1.2 255.255.255.252 
 no shut 
interface Lo0 
 ip address 209.165.200.226 255.255.255.255 
ip route 0.0.0.0 0.0.0.0 10.1.1.1 
end
```

**Identifica el problema**

> [!NOTE] Acceso
> El nombre de usuario `admin01` con una contraseña del `cisco12345` será requerido para iniciar sesión en el equipo de red. 

La única información disponible sobre el problema de la red es que los usuarios están experimentando tiempos de respuesta lentos y que no pueden llegar a un dispositivo externo en Internet en la dirección IP 209.165.200.226. Para determinar las causas probables para estos problemas de red, usted necesitará utilizar los comandos de red y las herramientas en el equipo LAN mostrado en la topología.

**Soluciona el problema**

1. Utilice las herramientas disponibles para solucionar problemas de la red, teniendo en cuenta que el requisito es restaurar la conectividad con el servidor externo y eliminar los tiempos de respuesta lentos.
2. Enumere las causas probables de los problemas de red que están experimentando los empleados.
3. *Ha comunicado los problemas que descubrió en la en los puntos anteriores. El ha aprobado estos cambios y ha solicitado que los implemente.*

**Verifica la funcionalidad**

Compruebe que se ha restaurado la funcionalidad completa. Pc-A, SWA y R1 deben poder alcanzar el servidor externo, y las respuestas de ping del PC-A al servidor externo no deben exhibir ninguna variación significativa en los tiempos de respuesta.

