# Info

Anexos | Disponibles en [Copyparty - EX 1 - AA2](https://copyparty.proxylivy.work/Duoc/Conectividad%20y%20Redes/2do%20Semestre/VTY2112%20(CE)/Actividades/EX%201/AA2/)
- [[020 - Conceptos/020.3 - Fundamentos/Modelo OSI|Modelo OSI]]
- [Youtube - RealPars - What is OSI Model?](https://youtu.be/Ilk7UXzV_Qc?si=KVD3eSymCD6HUiDX)

Objetivo: Proporcionar una base para comprender la suite de protocolos TCP/IP y la relación con el modelo OSI

# Examina el Trafico Web HTTP

Utiliza el modo de simulación de Packet Tracer (PT) para generar tráfico Web y examinar HTTP.

## Cambia al modo Simulacion

En la esquina inferior derecha de la interfaz de Packet Tracer, hay fichas que permiten alternar entre el modo Realtime (Tiempo real) y Simulation (Simulación). PT siempre se inicia en el modo Realtime, en el que los protocolos de red operan con intervalos realistas. Sin embargo, una excelente característica de Packet Tracer permite que el usuario “detenga el tiempo” al cambiar al modo de simulación. En el modo de simulación, los paquetes se muestran como sobres animados, el tiempo se desencadena por eventos y el usuario puede avanzar por eventos de red.

1. Haga clic en el ícono del modo Simulation (Simulación) para cambiar del modo Realtime (Tiempo real) al modo Simulation.
2. Seleccione HTTP de Event List Filters (Filtros de lista de eventos).
	- Es posible que HTTP ya sea el único evento visible. Haga clic en Edit Filters (Editar filtros) para mostrar los eventos visibles disponibles. Alterne la casilla de verificación Show All/None (Mostrar todo/ninguno) y observe cómo las casillas de verificación se desactivan y se activan, o viceversa, según el estado actual.
	- Haga clic en la casilla de verificación Show all/None (Mostrar todo/ninguno) hasta que se desactiven todas las casillas y luego seleccione HTTP. Haga clic en cualquier lugar fuera del cuadro Edit Filters (Editar filtros) para ocultarlo. Los eventos visibles ahora deben mostrar solo HTTP.

## Genera Trafico Web (HTTP)

El panel de simulación actualmente está vacío. En la parte superior de Event List (Lista de eventos) dentro del panel de simulación, se indican seis columnas. A medida que se genera y se revisa el tráfico, aparecen los eventos en la lista. La columna Info (Información) se utiliza para examinar el contenido de un evento determinado.

> [!NOTE] Ubicacion del panel web
> El servidor Web y el cliente Web se muestran en el panel de la izquierda. Se puede ajustar el tamaño de los paneles manteniendo el mouse junto a la barra de desplazamiento y arrastrando a la izquierda o a la derecha cuando aparece la flecha de dos puntas.

1. Haga clic en Web Client (Cliente Web) en el panel del extremo izquierdo.
2. Haga clic en la ficha Desktop (Escritorio) y luego en el ícono Web Browser (Explorador Web) para abrirlo.
3. En el campo de dirección URL, introduzca `www.osi.local` y haga clic en Go (Ir). Debido a que el tiempo en el modo de simulación se desencadena por eventos, debe usar el botón Capture/Forward (Capturar/avanzar) para mostrar los eventos de red.
4. Haga clic en Capture/Forward cuatro veces. Debe haber cuatro eventos en la lista de eventos.
	- Observe la página del explorador Web del cliente Web. ¿Cambió algo?

## Explora el Paquete HTTP

1. Haga clic en el primer cuadro coloreado debajo de la columna Event List > Info (Lista de eventos > Información). Quizá sea necesario expandir el panel de simulación o usar la barra de desplazamiento que se encuentra directamente debajo de la lista de eventos.
	- Se muestra la ventana PDU Information at Device: Web Client (Información de PDU en dispositivo: cliente Web). En esta ventana, solo hay dos fichas, OSI Model (Modelo OSI) y Outbound PDU Details (Detalles de PDU saliente), debido a que este es el inicio de la transmisión. A medida que se analizan más eventos, se muestran tres fichas, ya que se agrega la ficha Inbound PDU Details (Detalles de PDU entrante). Cuando un evento es el último evento del stream de tráfico, solo se muestran las fichas OSI Model e Inbound PDU Details.
2. Asegúrese de que esté seleccionada la ficha OSI Model. En la columna Out Layers (Capas de salida), asegúrese de que el cuadro Layer 7 (Capa 7) esté resaltado.
	- ¿Cuál es el texto que se muestra junto a la etiqueta Layer 7?
	- ¿Qué información se indica en los pasos numerados directamente debajo de los cuadros In Layers (Capas de entrada) y Out Layers (Capas de salida)?
3. Haga clic en Next Layer (Capa siguiente). Layer 4 (Capa 4) debe estar resaltado. ¿Cuál es el valor de Dst Port (Puerto de dest.)?
4. Haga clic en Next Layer (Capa siguiente). Layer 3 (Capa 3) debe estar resaltado. ¿Cuál es valor de Dest. IP (IP de dest.)?
5. Haga clic en Next Layer (Capa siguiente). ¿Qué información se muestra en esta capa?
6. Haga clic en la ficha Outbound PDU Details (Detalles de PDU saliente). La información que se indica debajo de PDU Details (Detalles de PDU) refleja las capas dentro del modelo TCP/IP.
	- ¿Cuál es la información frecuente que se indica en la sección IP de PDU Details comparada con la información que se indica en la ficha OSI Model? ¿Con qué capa se relaciona?
	- ¿Cuál es la información frecuente que se indica en la sección TCP de PDU Details comparada con la información que se indica en la ficha OSI Model, y con qué capa se relaciona?
	- ¿Cuál es el host que se indica en la sección HTTP de PDU Details? ¿Con qué capa se relacionaría esta información en la ficha OSI Model?
7. Haga clic en el siguiente cuadro coloreado en la columna Event List > Info (Lista de eventos > Información). Solo la capa 1 está activa (sin atenuar). El dispositivo mueve la trama desde el búfer y la coloca en la red.
8. Avance al siguiente cuadro Info (Información) de HTTP dentro de la lista de eventos y haga clic en el cuadro coloreado. Esta ventana contiene las columnas In Layers (Capas de entrada) y Out Layers (Capas de salida). Observe la dirección de la flecha que está directamente debajo de la columna In Layers; esta apunta hacia arriba, lo que indica la dirección en la que se transfiere la información. Desplácese por estas capas y tome nota de los elementos vistos anteriormente. En la parte superior de la columna, la flecha apunta hacia la derecha. Esto indica que el servidor ahora envía la información de regreso al cliente.
	- Compare la información que se muestra en la columna In Layers con la de la columna Out Layers: ¿cuáles son las diferencias principales?
9. Haga clic en la ficha Outbound PDU Details (Detalles de PDU saliente). Desplácese hasta la sección HTTP.
	- ¿Cuál es la primera línea del mensaje HTTP que se muestra?
	- Haga clic en el último cuadro coloreado de la columna Info. ¿Cuántas fichas se muestran con este evento y por qué?

# Elementos de TCP/IP

Utilizará el modo de simulación de Packet Tracer para ver y examinar algunos de los otros protocolos que componen la suite TCP/IP. 

## Eventos Adicionales

> [!NOTE]- Extensibilidad de PT
> Estas entradas adicionales cumplen diversas funciones dentro de la suite TCP/IP. Si el protocolo de resolución de direcciones (ARP) está incluido, busca direcciones MAC. El protocolo DNS es responsable de convertir un nombre (por ejemplo, `www.osi.local`) a una dirección IP. Los eventos de TCP adicionales son responsables de la conexión, del acuerdo de los parámetros de comunicación y de la desconexión de las sesiones de comunicación entre los dispositivos. Estos protocolos se mencionaron anteriormente y se analizarán en más detalle a medida que avance el curso. Actualmente, hay más de 35 protocolos (tipos de evento) posibles para capturar en Packet Tracer.

1. Cierre todas las ventanas de información de PDU abiertas.
2. En la sección Event List Filters > Visible Events (Filtros de lista de eventos > Eventos visibles), haga clic en Show All (Mostrar todo).
	- ¿Qué tipos de eventos adicionales se muestran?
3. Haga clic en el primer evento de DNS en la columna Info. Examine las fichas OSI Model y PDU Detail, y observe el proceso de encapsulación.
	- Al observar la ficha OSI Model con el cuadro Layer 7 resaltado, se incluye una descripción de lo que ocurre, inmediatamente debajo de In Layers y Out Layers: (“1. The DNS client sends a DNS query to the DNS server.” “El cliente DNS envía una consulta DNS al servidor DNS”). Esta información es muy útil para ayudarlo a comprender qué ocurre durante el proceso de comunicación.
4. Haga clic en la ficha Outbound PDU Details (Detalles de PDU saliente). ¿Qué información se indica en NAME: (NOMBRE:) en la sección DNS QUERY (CONSULTA DNS)?
5. Haga clic en el último cuadro coloreado Info de DNS en la lista de eventos. ¿Qué dispositivo se muestra?
	- ¿Cuál es el valor que se indica junto a ADDRESS: (DIRECCIÓN:) en la sección DNS ANSWER (RESPUESTA DE DNS) de Inbound PDU Details?
6. Busque el primer evento de HTTP en la lista y haga clic en el cuadro coloreado del evento de TCP que le sigue inmediatamente a este evento. Resalte Layer 4 (Capa 4) en la ficha OSI Model (Modelo OSI). En la lista numerada que está directamente debajo de In Layers y Out Layers, ¿cuál es la información que se muestra en los elementos 4 y 5?
	- El protocolo TCP administra la conexión y la desconexión del canal de comunicación, además de tener otras responsabilidades. Este evento específico muestra que SE ESTABLECIÓ el canal de comunicación.
7. Haga clic en el último evento de TCP. Resalte Layer 4 (Capa 4) en la ficha OSI Model (Modelo OSI). 
	- Examine los pasos que se indican directamente a continuación de In Layers y Out Layers. ¿Cuál es el propósito de este evento, según la información proporcionada en el último elemento de la lista. (debe ser el elemento 4)










