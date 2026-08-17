# Info

Anexos disponibles en [Copyparty - EX 3 - AA1](https://copyparty.proxylivy.work/Duoc/Conectividad%20y%20Redes/2do%20Semestre/VTY2112%20(CE)/Actividades/EX%203/AA1/)

Objetivo: Realizar la configuración básica de equipo de conmutación LAN

Escenario: La empresa CASE S.A necesita configurar los nuevos equipos de conmutación que adquirió. Por esta razón es que le ha solicitado que ejecute esta tarea de acuerdo a las siguientes instrucciones.

## Conoce un Switch

Utilice los Laptop para acceder a la configuración de los switch a través del terminal con los siguientes parámetros:
- Bit por segundo: 9600
- Bit de datos: 8
- Paridad: ninguno
- Bit de parada:1
- Control de flujo: ninguno

**Entre al modo privilegiado**

Puede acceder a todos los comandos del switch en el modo privilegiado. Sin embargo, debido a que muchos de los comandos privilegiados configuran parámetros operativos, el acceso privilegiado se debe proteger con una contraseña para evitar el uso no autorizado.

El conjunto de comandos EXEC privilegiados incluye aquellos comandos del modo EXEC del usuario, así como el comando configure a través del cual se obtiene el acceso a los modos de comando restantes.

**Examina la configuracion del Switch**

1. Ingrese el comando `show running-config`
2. Responda las siguientes preguntas
	- ¿Cuántas interfaces FastEthernet tiene el switch?
	- ¿Cuántas interfaces Gigabit Ethernet tiene el switch?
	- ¿Cuál es el rango de valores que se muestra para las líneas vty?
	- ¿Qué comando muestra el contenido actual de la memoria de acceso aleatorio no volátil (NVRAM)?
	- ¿Por qué el switch responde con startup-config is not present?

## Configuracion basica de Switch

**Asigna un nombre al Switch**

Para configurar los parámetros de un switch, quizá deba pasar por diversos modos de configuración. Observe cómo cambia la petición de entrada mientras navega por el switch. 


| Switch | Nommbre          |
| ------ | ---------------- |
| SW1    | CONECTIVIDAD-SW1 |
| SW2    | ESENCIAL-SW2     |

**Proporciona un acceso seguro a consola**

1. Para proporcionar un acceso seguro a la línea de la consola, acceda al modo config-line y establezca la contraseña de consola en conectividad.
	- ¿Por qué se requiere el comando login?
2. Verifique que el acceso a la consola sea seguro.
3. Salga del modo privilegiado para verificar que la contraseña del puerto de consola esté vigente.

**Proporcionar un acceso seguro al modo privilegiado**

Establezca la contraseña esencial en el modo de configuración privilegiado, este debe ser segura a traves del hash MD5.

**Verificar que el acceso al modo privilegiado sea seguro**

1. Introduzca el comando exit nuevamente para cerrar la sesión del switch.
2. Presione `<Entrar>`; a continuación, se le pedirá que introduzca una contraseña
	- User Access Verification
	- Password:
3. La primera contraseña es la contraseña de consola que configuró para line con 0. Introduzca esta contraseña para volver al modo EXEC del usuario.
4. Introduzca el comando para acceder al modo privilegiado.
5. Introduzca la segunda contraseña que configuró para proteger el modo EXEC privilegiado.
6. Para verificar la configuración, examine el contenido del archivo de configuración en ejecución, `show running-configuration`

Observe que las contraseñas de consola es de texto no cifrado, conocido también como texto plano. Esto podría presentar un riesgo para la seguridad si alguien está viendo lo que hace, mientras con la contraseña del modo privileagido esta encriptada. 

**Encriptar las contraseñas de consola**

Como pudo observar en el paso 7, la contraseña secreta de enable estaba encriptada, pero las contraseñas de enable y de consola aún estaban en texto no cifrado. Ahora encriptaremos estas contraseñas de texto no cifrado con el comando service password-encryption.

Si configura más contraseñas en el switch, ¿se mostrarán como texto no cifrado o en forma 
encriptada en el archivo de configuración? Explique por qué.

## Configura MOTD

**Configurar un mensaje del día (MOTD)**

El conjunto de comandos IOS de Cisco incluye una característica que permite configurar los mensajes que cualquier persona puede ver cuando inicia sesión en el switch. Estos mensajes se denominan “mensajes del día” o “mensajes MOTD”. Encierre el texto del mensaje entre comillas o utilice un delimitador diferente de cualquier carácter que aparece en la cadena de MOTD.

1. ¿Cuándo se muestra este mensaje?
2. ¿Por qué todos los switches deben tener un mensaje MOTD?

## Guarda los archivos de configuracion

Verificar que la configuración sea precisa mediante el comando show run

Usted ha completado la configuración básica del switch. Ahora realice una copia de seguridad del archivo de configuración en ejecución en la NVRAM para garantizar que no se pierdan los cambios realizados si el sistema se reinicia o se apaga. 

1. Guardar el archivo de configuración
	1. ¿Cuál es la versión abreviada más corta del comando copy running-config startup-config?
	2. Examinar el archivo de configuración de inicio
	3. ¿Qué comando muestra el contenido de la NVRAM?
	4. ¿Todos los cambios realizados están grabados en el archivo?

