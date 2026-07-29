# Info

Actividad de [[005 - Semestres/1er Semestre/VTY1122 (TD)/VTY1122 (TD)|VTY1122 (TD)]]

Se basa en "Actividad 06 - Equipos IOT.pka" desde [Copyparty - Actividades](https://copyparty.proxylivy.work/Duoc/Conectividad%20y%20Redes/1er%20Semestre/VTY1122%20(TD)/Actividades/)

**Parte 1: Explorar la casa Inteligente**

Normalmente, los ISP entregan datos y videos a través de un solo cable coaxial. Comenzando desde el ático, se utilizar un splitter coaxial para separar la señal de video de la señal de datos.
- Dos cables coaxiales dejan el splitter coaxial en la topología vista. ¿A qué dispositivo se conecta el cable coaxial?
R: 

- El cable módem es la interfaz entre la red del ISP y la red del hogar. ¿A qué dispositivos se conecta el cable modem?
R: 

El Home Gateway actúa como un concentrador y un router para todos los dispositivos domésticos internos. También proporciona una interfaz web que permite a los usuarios monitorear y controlar varios dispositivos domésticos inteligentes. Tener en cuenta que los dispositivos domésticos pueden conectarse a la puerta de enlace doméstica a través de una conexión inalámbrica o por cable.

> [!NOTE] Desactiva Wireless Connection
> Packet Tracer utiliza haces discontinuos para representar las conexiones inalámbricas, pero puede dificultar la lectura cuando hay demasiados dispositivos presentas. Para activarlo, vaya a Options > Preferences > Hide Tab > desmarque Hide Wireless/Cellular Connection

- Enumere todos los dispositivos domésticos conectados al Home Gateway.
R: 

Los dispositivos en la casa inteligente se pueden monitorear y controlar de forma remota a través de cualquier computador de la casa. Debido a que todos los dispositivos inteligentes se conectan al Home Gateway, que usan una interfaz basada en la web, se pueden utilizar tablests, teléfonos inteligentes, computadores portátiles o computadores de escritorio para interactuar con los dispositivos inteligentes.
1. Haga clic en el Tablet (La Tablet se encuentra en la cama del dormitorio principal).
2. Vaya a Desktop > Web Browser
3. En la barra de direcciones, escriba 192.168.25.1 y presione Enter. Esta es la dirección IP del Home Gateway.
4. Utilice como nombre de usuario admin y password admin para iniciar sesión en el Home Gateway.
- ¿Qué es lo que se muestra?

R: 

5. La puerta inteligente está actualmente desbloqueada (representada por una luz verde en el pomo de la puerta) pero se puede bloquear de forma remota. Haga clic en la puerta inteligente en el navegador para ampliar la opción.
6. Hacer click en Lock para bloquear la puerta.
- ¿La puerta fue bloqueada? ¿Cómo puedes comprobarlo?

R:

7. Click en Unlock para desbloquear la puerta.
8. Haga clic en el detector de humo en el navegador para expandir la sección
- ¿Cuál es la lectura del nivel de humo que proporciona el detector de humo?

R: 

- ¿Se puede controlar el detector de humo?

R:

9. Dentro del área de trabajo lógica de Packet Tracer, mantener presionada la tecla de `ALT` + `click` en el Smart Coffe Maker para encenderlo o apagarlo.

---

**Parte 2: Computacion en la niebla con la casa inteligente**

La MCU agregada a la casa inteligente se usa para monitorear los niveles de humos leidos por el sensor de humo y decidir si la casa debe ser ventilada. Si los niveles de monóxido de carbono aumentan por encima de las 10.3 unidades, la MCU está programada para abrir automáticamente la ventana, la puerta delantera, la puerta del garaje y encender el ventilador a alta velocidad. Esta acción solo es revertida (cierre las puertas y ventanas y deteniendo el ventilador) cuando los niveles de monóxido de carbono caigan por debajo de 1 unidad)

El propietario guarda un automóvil clásico en el garaje y debe usarse ocasionalmente. El automóvil clásico genera monóxido de carbono que eleva los niveles dentro de las instalaciones.

1. Haga clic en la Table ubicada en la cama del dormitorio principal.
2. Vaya a `Desktop` > `Web Browser`.
3. En la barra de direcciones, escriba `192.168.25.1`. (La dirección IP del Home Gateway.)
4. Use como nombre de usuario `admin` y password `admin` para iniciar sesión en el Home Gateway.
5. Haga clic en el detector de humo dentro de la casa inteligente; deje esta ventana visible para que pueda controlar los niveles de humo.
6. Arranque el motor del automóvil presionando la tecla ALT y haciendo clic en el automóvil clásico.
- ¿Qué sucede con el aire dentro de la casa con el auto corriendo dentro del garaje?

R: 

- ¿Qué sucede con el aire dentro de la casa después de que el MCU abre las puertas, ventanas y enciende el ventilador?

R: 

- ¿La MCU cierra las puertas, ventanas y detiene el ventilador?

R: 

7. Mientras sigue monitoreando los niveles, detenga el motor del automóvil clásico presionando la tecla ALT y haciendo clic en el automóvil clásico
- ¿Qué sucede con la calidad del aire dentro de la casa después de detener el motor?

R: 

- ¿Qué pasa con las puertas, la ventana y el ventilador?

R: 

# Conclusion

Este ejemplo muestra la decisión entre la nube y el procesamiento de niebla depende de la aplicación.

En el ejemplo de la casa inteligente, la informática de la niebla era la mejor opción. En este ejemplo, los datos generados por los sensores de humo fueron procesados y utilizados para tomar decisiones con respecto a la calidad del aire de la casa. En este escenario, no era necesario enviar los datos de los sensores a la nube para su procesamiento. El procesamiento de la nube generaría un retardo en la respuesta y podría poner vidas en peligro. Otro posible problema se relaciona con el enlace de Internet; si se perdiera el acceso a conexión a Internet, todo el sistema fallaría, poniendo en riesgo la vida de las personas.