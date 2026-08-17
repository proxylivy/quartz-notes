# Info

Anexos | Disponible en [Copyparty - EX 1 - AA1](https://copyparty.proxylivy.work/Duoc/Conectividad%20y%20Redes/2do%20Semestre/VTY2112%20(CE)/Actividades/EX%201/AA1/)

Objetivo: Interiorizarse en los dispositivos del Internet de las Cosas, específicamente en el contexto de una ciudad inteligente

Esta actividad esta basado en el Curso de Cisco: Introduction to Networks v7.0, y pueden profundizar con los Modulos 1 al 3

**Contexto**

En este laboratorio, explorarás el ejemplo de una ciudad inteligente. La idea de una ciudad inteligente se basa en la premisa de una ciudad que aprovecha el software para monitorear y analizar diversos eventos y tomar decisiones para mejorar la vida de sus residentes. 

La ciudad inteligente en este ejemplo se ha divido en grupos. Dependiendo de la aplicación, algunos datos se procesan y monitorean mejor desde una ubicación remota. Las oficinas de Smart City ofrecen capacidades que el permite a un administrador de la ciudad, monitorear varios eventos en toda la ciudad.

> [!NOTE] Etiquetas en PT
> Packet Tracer etiqueta las conexiones entre dispositivos de red, pero se puede desactivar para facilitar la lectura.
> 
> Para desactivar el etiquetado, vaya a `Options` > `Preferences` > `Interface Tab` > desmarque `Always Show Port Labels` en el área de trabajo lógica. 

# Explora la Ciudad
## Dispositivos en la Ciudad

Debido a que los sensores y los dispositivos de IoT se propagan a través de la ciudad inteligente, la infraestructura de red adecuada debe estar en su lugar antes de que pueda comunicarse. La ciudad inteligente es una red de área metropolitana (MAN) compuesta por redes más pequeñas. Estas redes a menudo están conectadas por enlaces WAN que permiten la comunicación a través de grandes ciudades geográficas. La nube ISP proporciona acceso a toda la ciudad a varias personas y organizaciones.

1. Haga clic en la nube ISP y examine los recursos que ofrece a la ciudad.
2. Haga clic en el botón de atrás. ¿Qué redes de la ciudad están utilizando los cables seriales rojos?
3. ¿Qué redes de la ciudad están usando los cables coaxiales azules?
4. Haga clic en el clúster City Offices. ¿Por qué hay dos conexiones que van desde la nube ISP?
5. Haga clic en el botón de Atrás. ¿Qué redes de la ciudad están conectadas de forma inalámbrica a la torre de telefonía móvil?
6. ¿Qué dispositivos de la casa inteligente están conectado a la torre de torre de telefonía móvil?
7. ¿Qué dispositivos en el clúster Smart Parking están conectado a la torre de telefonía móvil?

# Estacionamiento Inteligente

La ciudad inteligente está formada por una serie de sistemas representados en este ejemplo por grupos. El grupo de estacionamiento inteligente permite a que los funcionarios de la ciudad y a los ciudadanos beneficiarse de su implementación de IoT. 

**Interactua con el Estacionamiento Inteligente (City Offices Personnel)**

> [!NOTE] Tiempos de Espera
> Es posible que transcurran unos minutos para que todos los dispositivos de la red se conecten y los parquímetros puedan emitir los registros al servidor IoT. 

Los dispositivos en el grupo de estacionamiento inteligente se pueden monitorear y controlar de forma remota a través de cualquier computador en el cluster de oficina de la ciudad. Debido a que los todos los dispositivos del cluster de estacionamiento inteligente se conectar al City IoT Server que aloja una interfaz web, se pueden usar tablets, teléfonos inteligentes, computadores portátiles o computadores de escritorio para interactuar con los dispositivos inteligentes.

1. Haga clic City IT Laptop en el cluster City Offices.
2. Navergar a Desktop > Web Browser.
3. En la barra de direcciones, escriba 195.0.0.2. Esta es la dirección IP del City IoT Server
4. Use Park/Park como usuario y contraseña para validarse en el City IoT Server.
	-  ¿Qué es lo que se despliega?
5. Los parquímetros se registran en el servidor y envían actualizaciones del estado de forma periodica. Haga clic en el medidor P-Space-1 para expandirlo. ¿Cuál es el valor mostrado?
6. Sin cerrar la ventana de la computadora portátil City IT, vuelva al cluster de estacionamiento inteligente y haga clic, arrastrando el auto rojo hasta el lugar de estacionamiento 1. El lugar de estacionamiento 1 es el lugar de estacionamiento que se encuentra más a la izquierda del grupo.
7. Regrese a la ventana de la computadora portátil City IT y busque P-Space-1 (amplíelo si es necesario). ¿Cuál es el valor que se muestra ahora?

Los sensores de los espacios de estacionamiento son sensores PT metálicos configurados para responder a objetos metálicos (los automóviles en este caso), cuando se colocan suficientemente cerca

**Interactua con el Cluster de Estacionamientos Inteligentes (Regular Citizens)**

Si bien es útil como herramienta de monitoreo para la administración de la ciudad, los ciudadanos comunes no deben tener acceso a la interfaz en el servidor. Para permitir a los ciudadanos controlar que espacios de estacionamiento están disponibles en una calle determinada, se ha diseñado otra página web. Cierre la ventana de la computadora portátil City IT y navegue de regreso al cluster Smart Parking.

> [!NOTE] Tiempos de Espera
> Pueden pasar algunos segundos antes de que la página se cargue en el navegador web del smartphone.

1. Haga clic en el Smartphone y abra su navegador web, navegando en la pestaña Desktop Tab > Web Browse.
2. Escriba la barra la direcciones la IP 10.10.10.10. La IP 10.10.10.10 es la dirección del servidor de estacionamiento representado por el PT-MCU.
3. ¿Qué ve después de que se carga la página?
4. Sin cerrar la ventana del smartphone, arrastre el automóvil verde hacia el Estacionamiento 5. (El estacionamiento 5 es el lugar de estacionamiento más a la derecha en el cluster de estacionamiento inteligente)
5. Regrese a la ventana del smartphone (el navegador todavía debe mostrar la página cargada desde el servidor de estacionamiento de MCU). ¿Qué ves después de que se cargue la página?
6. Los lugares de estacionamiento no solo informan al servidor de registro de IoT alojado en el clúster City Offices, sino que también informan al servidor de estacionamiento local. Esto permite a los ciudadanos navegar y aprender sobre la disponibilidad de lugares de estacionamiento incluso antes de que llegue a la calle.
7. Tome un tiempo para explorar el código que se ejecuta en el MCU del servidor de estacionamiento.

# Trafico Inteligente

Otro componente de la ciudad inteligente es el tráfico inteligente. En este ejemplo, el tráfico inteligente permite que los vehículos de emergencia como ambulancias o paramédicos se comuniquen con el sistema de semáforos y soliciten el paso libre en caso de emergencia. Navegue hacia el cluster Smart Traffic.

En este ejemplo, Stree Light 1 y Stree Light 2 juegan el papel de semáforos. Los parámedicos están respondiendo a una emergencia. A medida que el vehiculo de los paramédicos se acerca al semáforo, se pone verde.

> [!NOTE] MCU Advanced
> Es posible que deba hacer clic en el botón de Advanced de la ventana de los dispositivos antes que pueda mostrar la pestaña Programming. 

1. Haga clic y arrastre el camión del paramédico y colóquelo cerca del semáforo a la derecha que está en rojo.
2. ¿Qué pasa con el semáforo de la derecha?
3. Aleje los paramédicos del semáforo de la derecha y colóquelos cerca del auto rojo. ¿Qué pasa con el semáforo?
4. El vehículo de paramédicos envía un mensaje a la MCU que controla los semáforos y solicita el paso. La MCU reconoce a los paramédicos como un vehículo de emergencia legítimo y le otorga el paso al encender la luz verde. La MCU también enciende las luces rojas para garantizar un paso seguro. Cuando el vehículo de emergencia ha pasado de manera segura, la MCU vuelve al sistema a su funcionamiento normal.
5. Tomar un tiempo para analizar el código que se ejecuta en la MCU en la ambulancia, navegando a la pestaña Programming. 



