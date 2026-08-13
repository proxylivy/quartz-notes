# Info

Objetivo: Instalar redes de [[020 - Conceptos/020.3 - Fundamentos/Cableado Estructurado/Cableado Estructurado|Cableado Estructurado]], realizar procedimientos de certificación, reconocimiento y solución de problemas típicos, cumpliendo con normas de la industria, identificando e implementando diferentes topologías de red. En 4 Partes
- Implementación de una Red LAN con topología estrella
- Implementación de una Red LAN con topología estrella extendida
- Implementación de una red LAN con topología Árbol
- Certificación de una Red LAN e identificación y solución de Fallos típicos

La informacion de esta actividad resulta en un Informe, y las evidencias se entregan en una presentacion de portafolio con el contenido de la Experiencia 3, estos siendo
- [[005 - Semestres/1er Semestre/INY1122 (IyCR)/Actividades/EX 3/AA1 - Propiedades de Transmision de Datos|AA1 - Propiedades de Transmision de Datos]]
- [[005 - Semestres/1er Semestre/INY1122 (IyCR)/Actividades/EX 3/AA2 - Señales para las telco|AA2 - Señales para las telco]]
- [[005 - Semestres/1er Semestre/INY1122 (IyCR)/Actividades/EX 3/AA3 - Verificando velocidad de internet|AA3 - Verificando velocidad de internet]]

## Parte 1

Implementación de una red con topología en estrella.

Materiales
- 1 Rack 
- 15m de Cable UTP CAT6
- 22 Conectores UTP RJ45 CAT6
- 1 Patch Panel 19”
- 3 Faceplates con conector RJ45
- 1 Canaleta 50x105 de 2m tipo Legrand
- 1 Ponchadora de presion RJ45
- 1 Alicate Cortante
- 1 Tester de cableado LAN
- 3 Switch 19" (ej. Cisco 2960)
- 1 Cable de Consola Rollover (Cisco)

Con el Cable UTP CAT6 deberas hacer los siguientes cables
- 4 Cables CAT6 de 4m
- 1 Cable CAT6 de 3m
- 3 Patch Cord CAT6 de 0.5m
- 3 Patch Cord CAT6 de 0.5m

Instrucciones Cableado Horizontal
1. Instalen un Patch Panel 19" en un Rack
2. Contruye 3 puntos de red de 4m entre el Patch Panel, mediante una canaleta Legrand para llegar a Faceplates
3. Realiza el ponchado de los puntos utilizando terminaciones T568B
4. Prueba el cableado para cada instalacion
5. Conecta los puntos finales en el Faceplate con computadores

Instrucciones Cableado Vertical
1. Instala el Switch en el Rack
2. Utiliza 3 Patch Cord para unir los 3 puntos cableados a los puertos FE (FastEthernet) del Switch
3. Utiliza 1 cable de 3m para conectar un puerto GE (GigabitEthernet) (ej. G0/0) del Switch a un punto de red con internet

Topologia Visual
```ascii
              Internet
                 │
                 │
            [Switch "0"]
             /   |   \
            /    |    \
          PC0   PC1   PC2
```


**Configuracion del Switch**

Verifica que la configuracion del Switch este sin configurar, se deberia ver algo asi
```plain
interface FastEthernet0/1
!
interface FastEthernet0/2
!
interface FastEthernet0/3
.
.
.
interface GigabitEthernet0/1
!
interface GigabitEthernet0/2
!
interface Vlan1
 no ip address
 shutdown
```

En caso de estar configurado, debes borrarla, los comandos son
```plain
enable
config t
erase startup-config
reload
```

Los computadores finales deberian tener IP de la red conectada por el puerto GE (GigabitEthernet) (ej. G0/0)

**Fallas Comunes**

1. Quita un patch cord conectado a los PC
	- Verifica que ocurre con los led de la interfaz FE al que estaba conectado, ¿Que ocurrio?
	- Verifica las otras conexiones ¿Siguen teniendo internet y ping entre ellas? ¿Y hacia la puerta de enlace?
2. Vuelve a conectar el Patch Cord que previammente desconectamos, y ahora desconecta el puerto GE del Switch que va hacia la red de internet
	- ¿Que ocurre con las interfaces del Switch?
	- ¿Tienen ping entre PCs? ¿Y hacia internet?

Ahora responde
1. Si le comentan que un PC no tiene internet y otros de la misma red si lo hacen, la o las 3 fallas más probable están en:

R: 

2. Si le comentan que un PC no tiene internet, y no tiene ping con los otros PCs que están en esa red, pero los led están en verde en el Switch, el problema más probable es que:

R: 

## Parte 2

Implementación de una red con topología en estrella extendida

Deberan utilizar los mismos materiales que se usaron en la [[#Parte 1]]

Instrucciones
1. Instalen un Switch ("0") en el rack
2. Conecten 2 Patch Cord (0.5m) a 2 interfaces FE del Switch
3. Instalen un 2do Switch ("Servidor") en el rack
4. Conecten 1 Cable UTP desde el Switch ("Servidor") a una red con internet
5. Conecten 1 Patch Cord (0.5m) desde un puerto FE del Switch ("Servidor") a la interfaz G0/0 del Switch 0
6. Conecten 1 Patch Cord (0.5m) desde un puerto FE del Switch ("Servidor") al Patch panel donde conectaron el PC2

Topologia a recrear
```ascii

              Internet
                 │
                 │
          [Switch "Servidor"]
              /     \
             /       \
      [Switch "0"]   PC2
         /    \
       PC0    PC1
```

Conecta el cable consola a cada Switch para verificar que su configuracion esta limpia y que tambien esten encendidas, en el caso de limpiar la configuracion, el comando es
```plain
enable
config t
erase startup-config
reload
```

**Fallas Comunes**

1. Quita uno de los Patch Cord conectados entre el Switch "0" y alguno de los PCs
	- Verifica que ocurre con el color de los leds de la interfaz FE en el Switch "0" y en el Switch "Servidor", ¿Que significan estos colores?
	- Verifique las otras conexiones horizontales, ¿Siguen teniendo internet, los otros PC? ¿tienen ping entre ellos?
2. Vuelve a conectar el cable previamente desconectado y quita el Patch Cord que esta conectado entre el Switch "Servidor" y el Switch "0"
	- ¿Que ocurre con los leds de las interfaces de los switches?
	- ¿Tienen internet los otros PCs? ¿Tienen ping entre ellos?

## Parte 3

Implementación de una red con topología en Árbol

Deberan utilizar los mismos materiales que se usaron en la [[#Parte 1]]

Instrucciones
1. Instalen un Patch Panel en el Rack de 19"
2. Instalen un Switch ("0") en el Rack de 19"
3. Utilizen 2 Patch Cord (0.5m) para conectar 2 puertos FE del Switch ("0")
4. Instalen un Switch ("1") en el Rack de 19"
5. Instalen un Switch ("Servidor")
6. Utilizen 1 Cable para el Switch ("Servidor") conectar 1 puerto FE a Internet
7. Conecta el Switch ("Servidor") con los Switches "0" y "1" en los puertos GE
8. Conecten 2 Patch Cord (0.5m) desde el Switch "0" a los PC0 y PC1
9. Conecten 2 Patch Cord (0.5m) desde el Switch "1" a los PC2 y PC3

Topologia Visual de la conexion
```ascii
                     Internet
                         │
                  [Switch "Servidor"]
                     /         \
                    /           \
                [Switch0]    [Switch1]
                 /    \       /    \
                /      \     /      \
              PC0      PC1  PC2     PC3
```

> [!TIP] Conexiones Switch "Servidor"
> No cuenta con puntos de cableado horizontal, solo tiene las conexiones directas a los Switches "0" y "1" y hacia internet

Conecta el cable consola a cada Switch para verificar que su configuracion esta limpia y que tambien esten encendidas, en el caso de limpiar la configuracion, el comando es
```plain
enable
config t
erase startup-config
reload
```

**Fallas Comunes**

1. Quita una conexion entre los PC y el Switch "0"
	- Verifica que ocurre con los leds de la interfaz FE y en los otros 2 ¿Que ocurrio?
	- Verifica las otras conexiones horizontales ¿Siguen teniendo internet, los otros PCs tienen ping entre ellos?
2. Vuelve a conectar el cable previamente desconectado y quita la conexion entre el Switch "Servidor" y el Internet
	- ¿Que ocurre con los leds de las interfaces de los Switches?
	- ¿Tiene ping entre los PCs de Switch "0" y Switch "1"? ¿Por que?

Responde las siguientes preguntas
1. En una red de Árbol, si me aparecen conectados en el Switch los PCs pero no tengo Internet, cuáles son 2 probables causas?

R: 

2. Si uno o más PCs de una estrella de distribución horizontal deja de tener Internet, pero en otro distribuidor horizontal ligado al mismo árbol si tienen, ¿cuál es una de las posibles fallas en el cableado?

R: 

3. Si en los Switch de una red árbol vemos todos los led de conectividad en verde, pero no tenemos Internet, ni ping de un PC a otro, ¿cuál es la posible falla a nivel de cableado?

R:

