# Info

Objetivo: Realizar una implementación de Capa 2 en las sucursales de empresas Doña Juanita S.A, que permita la interconexión de ambas sucursales, permitiendo un tráfico más eficiente y efectivo en la red.

**Contexto**

Empresas Doña Juanita S.A, ha sido el renombre que le ha dado su gerente General Benjamín Toledo a su empresa, esto es debido a que su rubro de minimarket y la tienda de óptica han mantenido el crecimiento de ventas sostenido y deseado durante el presente año. 

Este crecimiento ha implicado, que han tenido que incorporar más personal para realizar nuevas labores en la empresa, por lo cual se ha necesitado incorporar de más equipamiento de red. Por tal motivo el uso de solo routers para la cantidad de redes no está siendo muy eficiente, por tal motivo su asesor tecnológico le ha sugerido a Benjamín Toledo, que adquiera el uso de switches para poder realizar un ordenamiento de las redes a nivel de capa 2, pero manteniendo el enrutamiento de su red a nivel IPv4/IPv6, todo esto para las sucursales “Doña Juanita” y “Donde Benja”. 

Benjamín Toledo ha generado una licitación pública para realizar la mejora de infraestructura, y esta fue adjudicada por la empresa “TODORAPIDO S.A”, debido a que su costo era rápido, pero con muy poca trayectoria en el mercado. 

Empresas Doña Juanita compró todos los switches necesarios para la empresa, y persona de la empresa “TODORAPIDO S.A” comenzó a realizar cambios en el protocolo y en el acceso a Internet, lo cual implicó la pérdida de conectividad, haciendo que la red dejara de funcionar de forma inmediata, lo cual molestó de sobremanera a su gerente , terminando de forma anticipada el contrato con la empresa “TODORAPIDO S.A. Bajo este nuevo escenario, el gerente buscó un especialista en Conectividad Esencial, y ha dado con usted, donde deberá realizar el trabajo de corregir el problema de acceso a Internet que dejó la empresa anterior, además de realizar la implementación de capa 2 solicitada para mejorar el rendimiento de la red. 

# Requerimientos

> [!NOTE] Configuracion Prehecha
> Las interfaces locales del ROUTER-CENTRAL (Sucursal Doña Juanita) y ROUTER-CENTRAL-2 (Sucursal Donde Benja), ya se encuentran configuradas para las VLAN solicitadas.

**Calculo Direccionamiento IPv4/IPv6**

1. Realizar cálculo de VLSM a partir de la dirección de red 172.16.0.0/18, para todas las redes LAN que tienen ambas sucursales. Asignar direccionamiento IPv4 a equipos finales y dispositivos de IoT.
2. Realizar cálculo de subredes en IPv6 a partir de la dirección de red 2019:AAAA:BBBB::/64. Asignar direccionamiento IPv6 a equipos finales y dispositivos de IoT, procurando que aquellos dispositivos realizar el cálculo a su representación hexadecimal correspondiente.
3. El default-gateway a nivel IPv4 será la primera IP asignable, para el caso de IPv6 será la ::1 de la subred calculada.
4. El DNS a nivel IPv4/IPv6, corresponde al servidor que contiene la página web que se encuentra en el clúster de “Internet”.

**Implementacion de Capa 2**

1. Colocar nombre a los dispositivos de capa 2 de ambas sucursales.
2. Crear las VLANs en todos los switches con los nombres, y números señalados para cada sucursal.
3. Asignar el rango de interfaces solicitados a la VLAN de Datos.
4. Configurar enlaces troncales, permitiendo solo el paso de VLAN de Datos asociado a la sucursal.
5. Realizar configuración apropiada en los enlaces troncales para permitir el paso de la VLAN Nativa, utilizando configuración apropiada para este fin.

**Enrutamiento Estatico IPv4/IPv6**

1. En ROUTER-CENTRAL Y ROUTER-CENTRAL 2, configurar ruta estática por defecto a nivel de IPV4 e IPv6, indicando interface de salida.
2. En ROUTER-BORDE Y ROUTER-BORDE 2, aplicar enrutamiento estático estándar a nivel de IPv4 e IPv6, indicando la interface de salida.

**Resolucion de Problemas Red Core Empresas Doña Juanita**

1. Debido al trabajo de la empresa contratista anterior, la red core de empresa de Doña Juanita, ha perdido la conectividad entre las sucursales “Doña Juanita” y “Donde Benja”, además del acceso hacia Internet. Su labor será corregir estos inconvenientes, aplicando los comandos del protocolo ICMP para resolver los problemas de conectividad, permitiendo que la red entre las sucursales y hacia Internet pueda volver funcionar.
2. Aplicar comandos del protocolo ICMP, para probar conectividad entre los PC de una sucursal a otra y hacia Internet.

**Implementacion y Comprobacion Funcionamiento de Servicios**

1. En la VLAN30 ubicada en la sucursal “Doña Juanita” deberá habilitar servidor IOT, creando el usuario `conectividad` y password `esencial`.
2. Los dispositivos IoT ubicados en la VLAN40 deben conectarse al servidor de IOT de la VLAN30.
3. Probar desde un PC ubicado en la VLAN20, probar si puede manipular el funcionamiento de los dispositivos de IoT.
4. En la VLAN70 ubicada en la sucursal “Donde Benja” hay un servidor FTP. Deberá crear una cuenta con el usuario conectividad y password esencial. Además, la cuenta deberá tener los permisos para Leer, Escribir y Listar archivos.
5. Probar desde cualquier PC de la sucursal “Doña Juanita” para probar que tenga acceso al servidor mediante comando.
6. Probar desde cualquier PC que puedan acceder a la página web `www.donajuanita.cl`

