# Info

Actividad de [[005 - Semestres/1er Semestre/VTY1122 (TD)/VTY1122 (TD)|VTY1122 (TD)]]

Se basa en "Actividad 11 - Soluciones IoT.pkz" desde [Copyparty - Actividades](https://copyparty.proxylivy.work/Duoc/Conectividad%20y%20Redes/1er%20Semestre/VTY1122%20(TD)/Actividades/)

**Contexto**

IoE Vineyards implementó recientemente una solución de IdT de punta a punta con varios tipos de conexiones M2M, M2P y P2P. En el viñedo y en las instalaciones de procesamiento, conéctese a la red para controlar los datos de los sensores. En la sala de cata, conecte un dispositivo del cliente a la red para ver el sitio web de IoE Vineyards y proporcionar comentarios.

1. Abra el archivo "Act 11 - Soluciones IOT.pkz"
	- Haga clic en Herramientas para ocultar las barras de herramientas y permita que haya más espacio para navegar por la actividad.
	- Existen tres clústeres para explorar: **Vineyard** (Viñedo), **Winery Processing** (Procesamiento en la bodega) y **Tasting Room** (Sala de cata).
2. Haga clic en **Vineyard** para abrir y explore el clúster.
	- Ubique los sensores y los actuadores.
	- Abra el sensor **Soil Moisture** (Humedad del suelo), el actuador **Irrigation Pump Valve** (Válvula de la bomba de irrigación) y el sensor **Air-Temp** (Temperatura del aire).
	- Observe mientras el valor del sensor de **Soil Moisture** determina cuándo se activa **Irrigation Pump Valve**.
	- Utilice la ventana de control **Environment** (Entorno) para manipular la temperatura o para generar lluvia.
	- Observe cómo estos cambios en el entorno afectan el valor del sensor **Soil Moisture** y cuándo se activa **Irrigation Pump Valve**.
3. Configure los ajustes inalámbricos para la tablet PC **Winemaker** (Vinicultor) y consulte la herramienta de control de software.
	- En la tablet PC, haga clic en la ficha **Config** (Configurar) > **Wireless0*** (Inalámbrico0).
	- Establezca el SSID en `Vines`.
	- Habilite la autenticación **WPA2-PSK** y establezca la contraseña en `IoEVines`.
	- Establezca la configuración IP en **DHCP** y verifique que la tablet PC ahora tenga direccionamiento IP.
	- Haga clic en **Desktop** (Escritorio) > **Web Browser** (Navegador web). Introduzca `vines` como el URL. Haga clic en `Go` (Ir) para ver la herramienta de supervisión de software.
4. Haga clic en **Back** (Atrás) > **Winery Processing** para abrir el clúster y navegar por este.
	- Ubique y explore los sensores y los actuadores. En esta simulación, algunos sensores y accionadores no están conectados.
	- Abra los sensores Weight-11, Tank1-Temp, y PinotT-13.
	- Utilice la ventana de controles Environment para activar la centrifugadora, llenar el tanque y elevar la temperatura del área de fermentación.
	- Haga clic en **Vmaster-4** > **Desktop** > **Web Browser** para ver la herramienta de supervisión de software de la instalación de procesamiento de la bodega. Escriba `sensors` como el URL, y haga clic en Go.
	- Intente acceder al mismo URL de `sensors` desde el dispositivo **Outside User** (Usuario externo) para verificar que la seguridad esté habilitada.
5. Hacer clic en **Back** > **Tasting Room** para abrir el clúster y navegar por este.
6. Configure los ajustes inalámbricos para el smartphone **Customer** (Cliente) y consulte el sitio web de IoE Vineyards.
	- En el smartphone, haga clic en la ficha **Config** > **Wireless0**.
	- Establezca el SSID en **Taste**.
	- Habilite la autenticación **WPA2-PSK** y establezca la contraseña en **IoEGuest**.
	- Establezca la configuración IP en DHCP y verifique que la tablet PC ahora tenga direccionamiento IP.
	- Haga clic en **Desktop** (Escritorio) > **Web Browser** (Navegador web). Introduzca `wine` como el URL. Haga clic en **Go** para ver el sitio web de IoE Vineyards.
