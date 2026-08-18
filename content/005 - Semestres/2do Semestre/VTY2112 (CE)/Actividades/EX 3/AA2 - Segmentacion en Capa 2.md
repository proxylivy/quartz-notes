# Info

Anexos disponibles en [Copyparty - EX 3 - AA2](https://copyparty.proxylivy.work/Duoc/Conectividad%20y%20Redes/2do%20Semestre/VTY2112%20(CE)/Actividades/EX%203/AA2/)

Objetivo: Implemente una solución de enlaces troncales, para permitir que redes que comparten un mismo segmento de red tengan conectividad a nivel IPv4/IPv6

Escenario: La empresa CASE S.A necesita segmentar el tráfico de sus áreas para optimizar el rendimiento de la red LAN. Implemente segmentación de red según el paso a paso que se indica a continuación.

# Asignacion Direccionamiento

**Configuracion de IPv4**

1. Se tiene direccionado las subredes para cada red LAN que tiene la topología a nivel IPv4, por tal motivo, deberá asignar la IP a los equipos fínales según lo señalado en la topología. Utilice como default-gateway la última IP asignable.

**Configuracion de IPv6**

1. Se tiene direccionado las subredes para cada red LAN que tiene la topología a nivel IPv6, por tal motivo, deberá asignar la IPv6 a equipos finales según lo señalado en la topología. Utilice como default-gateway la primera IPv6 asignable

# Configuracion de Switches

**Configuracion Basica**

1. Asignar nombres a todos los switches de la topología.
2. En switch "SWCORE" deberá configurar SSH, donde el equipo debe pertenecer al dominio `www.duoc.cl`. Utilizar llave criptográfica de 1024 bits. Además, en las conexiones remotas solo debe permitirse conexiones entrantes SSH. La cantidad simultánea de conexiones remotas será de 3 sesiones. Utilizar como usuario el nombre del dispositivo y la contraseña será `conectividad`

**VLAN e Interfaces de Acceso**

1. Configurar las VLANs 10, 20, 30 y 99 según los nombres asignados.
2. En los switches de acceso, asignar el rango de interfaces señalado en la topología a la VLAN correspondiente.
3. Las interfaces sobrantes y que no sean utilizadas como enlaces troncales serán asignado a la VLAN 99, y deben quedar en estado de shutdown. Esta configuración aplica para todos los switches de la red.
4. Comprobar la asignación de interfaces a VLAN con el comando `show vlan brief`

**Enlaces Troncales**

1. Implementar enlaces troncales en todos los switches correspondientes.
2. Permitir en los enlaces troncales solo el paso de la VLAN 10, 20 y 30.
3. Permitir el paso de la VLAN 99 como Nativa, para esto utilizar configuración apropiada para este fin.
4. Comprobar el estado de los enlaces troncales, el paso de las VLANs y la asignación de VLAN Nativa con el comando `show interfaces trunk`

**Asigna IP a Switches**

1. Asignar IPv4 a todos los switches de la red, utilizando para esto la VLAN de Administración
2. Es importante recordar que la interfaz SVI 30, debe quedar encendida
3. Compruebe conectividad entre los switches haciendo un PING en el modo privilegiado del dispositivo de conmutación.

# Pruebas de Conectividad

1. Realizar pruebas de conectividad entre equipos finales de una misma VLAN a nivel IPv4/IPv6.
2. Es importante recalcar que solo habrá conectividad entre dispositivos de una misma red.

