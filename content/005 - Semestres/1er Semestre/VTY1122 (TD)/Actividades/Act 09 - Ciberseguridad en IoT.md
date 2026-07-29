# Info

Actividad de [[005 - Semestres/1er Semestre/VTY1122 (TD)/VTY1122 (TD)|VTY1122 (TD)]]

Se basa en "Actividad 09 - IoT.pka" desde [Copyparty - Actividades](https://copyparty.proxylivy.work/Duoc/Conectividad%20y%20Redes/1er%20Semestre/VTY1122%20(TD)/Actividades/)

El propietario de una casa ha estado leyendo sobre IOT y está muy entusiasmado con las capacidades de los sistemas de IOT de automatización del hogar. El propietario de la casa quiere instalar dicho sistema en su casa, pero no sabe como hacerlo. Se ha contactado con una compañía que puede diseñar e instalar el sistema. La compañía se centra en la seguridad en todo el proceso de diseño, aprovisionamiento y el desarrollo del sistema. Actualmente están desarrollando un modelo de amenaza para el sistema. Ha sido contratado por la empresa y su primera tarea es completar el modelo de amenaza.

La casa tiene 325 metros cuadrados e incluye dos pisos y un ático. Los dueños de casa están regularmente fuera de ella, y han solicitado que la casa esté lo más segura posible. El cliente desea poder monitorear la casa de forma remota y desea los siguientes sistemas sean compatibles con IOT:
- Control Climático
- Humo/Fuego
- Problemas de temperatura fuera del rango normal.
- Cerraduras de puertas y ventanas
- Riego del pasto
- Alarma local y mensajes del departamento de emergencia

El sistema debe ser controlable localmente a través de la nube. El usuario debe poder acceder al controlador desde un navegador web dentro de la red, así como de forma remota a través de una aplicación de teléfono inteligente. Esto permitirá a los clientes monitorear o controlar el sistema cuando están ausentes.

El sistema debe recopilar y almacenar datos de los sensores remotos y deben realizarse varias acciones basadas en la entrada de estos sensores. Por ejemplo, si la temperatura supera el rango máximo es probable que el AC no esté funcionando y que se deba notificar a alguien lo antes posible. Si el sistema detecta humo, la alarma local debe sonar, donde el cliente y bomberos deben recibir una alerta. Los datos del sistema deben ser retenidos y analizados. Además, el cliente debe poder cambiar los umbrales que activan los diferentes activadores y eventos según sean necesarios, ya sean localmente o a través de una aplicación móvil. Los activadores y detectores de movimiento, el análisis de datos y el acceso por control remoto están disponibles a través de un servicio de aplicación de la nube de automatización del hogar con el sistema que interactuará.

Los propietarios de la vivienda deben tener cuentas protegidas por contraseñas para acceder al sistema. Además, la compañía debe tener acceso al diagnóstico del sistema en caso de que ocurran problemas con el sistema. Solo el propietario debe tener acceso a las aplicaciones en la nube.

También es importante tomar nota de estos otros detalles de la casa:
- 3 habitaciones, 2 baños
- 2 pisos y un ático
- 1 puerta de entrada principal y 1 puerta de entrada lateral
- 2 puertas corredizas al patio trasero, una que viene del dormitorio principal.
- Garaje para dos autos

Comenzarás creando un modelo de amenaza para el sistema. El sistema domótico ha sido prototipado en Packet Tracer según lo definido en la Experiencia 9. El sistema es muy similar al sistema de la red hogareña de IOT que exploró en una experiencia pasada. Debido a que el proceso de modelado de amenazas es muy detallado, se dividirá en diferentes etapas vinculadas entre ellas.

# Desarrollo

Existen seis categorías de objetivos de seguridad que a menudo se usan para definir las necesidades de seguridad de un sistema u organización. Responda las siguientes preguntas para ayudar a desarrollar estos objetivos. Responda las preguntas desde el punto de vista tanto de la empresa instaladora del servicio como del cliente, según sea necesario.

**Identidad**

- ¿Qué controles de acceso y autorización deben existir para documentar quién accede al sistema IOT?

R: 

- ¿Debería haber algún control de acceso máquina a maquina (M2M) en su lugar?

R:

**Financiero**

Documente las pérdidas financieras que podrían producirse debido a una falla del sistema, los componentes del sistema o el cierre de seguridad.
- ¿Cuál es el impacto financiero potencia para el cliente si los componentes del sistema no funcionan correctamente?

R: 

- Si un actor de amenaza pudo obtener acceso a la red doméstica en una brecha de seguridad ¿Qué pérdidas podrían ocurrir?

R: 

**Reputacion**

Documente cualquier posible impacto en la reputación del cliente si se ataca el sistema de seguridad de IOT.
- ¿Cuáles serían las repercusiones para el propietario si su sistema de seguridad en el hogar fuera atacado?

R: 

**Privacidad y Regulacion**

Documentar el impacto de cualquier inquietud sobre la privacidad, así como los requisitos reglamentarios para este sistema e identifique cualquier dato que pueda causar problemas de privacidad para el propietario de este sistema.

- ¿Existe alguna inquietud sobre la privacidad de los datos recopilados o utilizados por este sistema?

R: 

- ¿Le importa al propietario que se mantenga registros sobre el acceso y movimiento en toda la casa (sensores de movimiento)?

R: 

**Garantias de Disponibilidad**

Documentar la disponibilidad esperada y el tiempo de actividad garantizado del sistema de IOT. ¿Se requiere que este sistema esté disponible en todo momento?
- ¿Hay algún tiempo de inactividad aceptable que pueda tolerarse para este sistema? Explique.

R:

**Seguridad**

Documentar los impactos potenciales para el bienestar físico de las personas y el daño físico a los equipos e instalaciones. Esto particularmente importante en entornos de sistemas de control industrial (ICS)

> [!IMPORTANT] Contenido PKA
> El .pka tiene dos partes mas a abordar, estan son complementarias, las que los alumnos de forma libre pueden ejecutar