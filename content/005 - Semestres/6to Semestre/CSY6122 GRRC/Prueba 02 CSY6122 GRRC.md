## Informacion General del Proyecto
- _Falco is a work in progress, so features and behaviors may change. This technical Report is based on version 0.39.1; always consult the [official documentation](https://falco.org/docs/) for updates._

**Introduccion**
Falco es una herramienta de codigo abierto diseñada para entornos de nube. Su principal funcion es detectar comportamientos/acciones anomalos en tiempo real mediante la recopilacion de datos de multiples fuentes, generando alertas para mostrar posibles problemas de seguridad. Fue desarrolado originalmente por Sysdig en 2016 y donado a la CNCF en 2018, en 2024 tiene mas de 60M+ descargas en Dockerhub, 40+ integraciones, 5.800+ estrellas de Github, 170 Contribuidores en todo el mundo.[^1]

**Funcionamiento**
Falco se encarga de monitorear el comportamiento del sistema operativo y su nucleo mediante la captura de llamadas al sistema, que luego procesa en su motor de reglas. Cuando el motor de reglas detecte un comportamiento incorrecto o la violacion de una regla, enviara una alerta al sistema.

Los comportamientos que Falco monitorea provienen de multiples fuentes:
- Llamadas al sistema: Monitoreo de interacciones entre el espacio de usuario y el kernel
- Eventos de K8s: Recopila eventos auditados y acciones a nivel API dentro de los clusters
- Eventos en la nube: Monitorea plataformas como AWS CloudTrail para detectar cambios en la infraestructura
- Sistemas externos: Utilizando Plugins, se pueden recopilar eventos de servicios como Github, Okta, nomad, etc.

Alguno de los comportamientos que Falco puede detectar, se encuentran:
- Escalada de privilegios en contenedores
- Manipulacion de espacio de nombres (`netns`)
- Acceso no autorizado a directorios criticos como: `/etc`, `/bin`, `/root`, etc.
- Cambio de permisos de carpetas o carpetas, creacion de enlaces simbolicos, y mutaciones en conexines o sockets
- Ejecucion inesperada de binarios o scripts (`execve`)

**Componentes de Falco**
- **Falco (Userspace)**: Procesa los eventos enviados por "*Falco (Driver)*" y aplica las reglas definidas en la "*Configuracion*"
- **Falco (Driver)**: Actua como puente entre el kernel y el espacio del usuario, recopila eventos y los envia a "*Falco (Userspace)*" para su analisis
- **Configuracion**: Define el funcionamiento de "*Falco (Driver)*" en un archivo YAML, especificando reglas y manejo de alertas
- **Reglas**: Politicas escritas en YAML que definen las condiciones para generar alertas
- **Plugins (Opcional)**: Extiende las funcionalidades de "*Falco (Driver)*" para añadir nuevos eventos o campos para detectar, puede ser desplegado en sistemas activos con **Falcoctl** para que interactue con *Falco(Userspace)*

## Licenciamiento, Valores, Ventajas
El codigo fuente de Falco esta licenciado bajo Apache-2.0, Disponible en: https://www.apache.org/licenses/LICENSE-2.0.txt
Licencia permisiva muy usado en proyectos de codigo abierto, hecha por la ASF (Apache Software Foundation), permite su uso sin gastos extras.

Apache-2.0 Permite
- Uso Comercial: Puede ser usado en entornos empresariales sin restricciones
- Proteccion de Patentes: Defiende el codigo original ante infracciones de 3ros
- Permite observar, analizar, modificar, distribuir y crear obras derivadas usando la misma licencia

Apache-2.0 No Permite
- Uso de marcas o nombres sin autorizacion explicita
- No asegura el funcionamiento, garantia o soporte del programa

La documentacion de Falco esta licenciada bajo CC-BY-4.0 (Creative Commons Attribution 4.0), Disponible en: https://creativecommons.org/licenses/by/4.0/
Licencia para cualquier material informativo sea reutilizado y modificado, mientras se acredite a los autores originales.

CC-BY-4.0 Permite
- Copiar, adaptar y distribuir el contenido
- Uso comercial: Permite usar el contenido en entornos empresariales

CC-BY-4.0 No Permite
- Omitir la atribucion: siempre debe atribuir el material original junto a sus a sus autores, incluso la adaptacion para uso interno.

**Valores**
Falco se distribuye bajo una licencia gratuita y de uso ilimitado. Sin embargo, toda implementacion tiene costos asociados, la implementacion inicial puede tener una duracion de 40 horas dependiendo del personal, para poder revisar las necesidades de la infraestuctura, configuracion de reglas y hacer pruebas de funcionamiento, se le suman mas costos si para este proceso se necesita una consultora externa. El mantenimiento por parte de Falco es gratuito, el personal le tomara dentro de 2-4 horas mensuales. La capacitacion del personal puede ser en base a recursos gratuitos en linea como la documentacion oficial, foros, la comunidad, los contribuidores, etc. 
En resumen, Falco no tiene costos directos, pero si indirectos que hay que tomar en cuenta, sin embargo su costo es menor a otras alternativas empresariales.

**Ventajas para la empresa**
- **Deteccion en tiempo real y multinivel**: Falco monitorea las llamadas al sistema tanto del sistema host como de los contenedores, permitiendo ser mas eficiente que las soluciones que dependen de otros tipos de analisis.
- **Ecosistema integrado y en expansion**: Falco se puede integrar con otras herramientas (Prometheus, Grafana, Falcosidekick, Gotify, etc.) permitiendo un enfoque colaborativo entre aplicaciones y evitar depender de un unico punto de falla.
- **Flexibilidad y Adaptabilidad**: Las reglas YAML permiten adaptar Falco a las necesidades de las empresas, estandares del mercado y las auditorias, como pueden ser PCI DSS, NIST Standard, HIPPA, GDPR, RedRAMP, SOC 2, ISO 27001, CIS Benchmarks, etc.
- **Permite automatizar con Falcoctl**: Falcoctl permite modificar las reglas sobre la marcha, lo que es critico en organizaciones que evolucionan con el tiempo, mejorando la eficiencia y eliminando el tiempo abajo.
- **Comunidad activa**: Falco recibe actualizaciones frecuentes, cuenta con el respaldo de grandes empresas (AWS, Google, Red Hat) garantizando su evolucion en el futuro

## Requisitos especificos de implementacion
1. eBPF (**e**xtended **B**erkeley **P**acket **F**ilter)
Permite monitorear las interacciones del kernel de forma eficiente. su driver mas reciente "Modern eBPF Probe CO-RE" es compatible con:
- Arquitectura de CPU: `x86_64` o `arm64`
- Kernel: version minima `>=5.8` (lanzado en 2021)

2. Entornos Compatibles
- **Linux**: Distribuciones como Debian/Ubuntu, CentOS/RHEL/Fedora/Amazon Linux, OpenSUSE
- **Contenedores y Orquestadores** (Solo en Host basados en Linux): Docker, K8s (Helm, Minikube, Kind, MicroK8s), K3s, GKE
- **Windows**(Limitado): A travez de WSL2
- **MacOS**(Limitado): Soporte para CPUs Apple Silicon M1/M2 mediante VMs ejecutando Ubuntu 24.04 mediante lima

**Limitaciones**
Falco tiene dificultades con:
- Sistemas Serverless (Faas y Saas) (Pueden implementar APIs): Muchos entornos no tienen acceso al kernel
- Dispositivos IoT/embebidos: Algunos dispositivos usan kernels antiguos o modificados que limitan el acceso para el usuario final

### Experiencia de Instalacion
La intalacion en Arch Linux con Docker y Docker Compose fue rapida y directa, con una duracion de 10 minutos. La lectura para comprender como funcionaba es lo que mas tiempo llevo.

El siguiente archivo `docker-compose.yaml` configura Falco sin driver externo usando Modern eBPF en modo Menos Privilegiado.
**docker-compose.yaml**
```yaml
services:
    falco-no-driver:
	    container_name: falco-Modern-eBPF
        image: 'falcosecurity/falco-no-driver:latest'
        restart: unless-stopped
        volumes:
            - '/etc:/host/etc:ro'
            - '/proc:/host/proc:ro'
            - '/var/run/docker.sock:/host/var/run/docker.sock'
        cap_add:
            - sys_ptrace
            - sys_resource
            - sys_admin
        cap_drop:
            - all
        tty: true
        stdin_open: true
```

**Ejemplo de alerta con Falco**
Si un usuario externo intenta acceder a archivos sensibles dentro del host, Falco genera una alerta como la siguiente
```
2024-10-09T19:13:43.470868405+0000: Warning Grep private keys or passwords activities found (evt_type=execve user=madkoding user_uid=1002 user_loginuid=1002 process=find proc_exepath=/usr/bin/find parent=bash command=find /root -name id_rsa terminal=34817 exe_flags=0 container_id=host container_name=host)
```
Deglose de la alerta:
- Fecha y hora "`2024-10-09T19:13:43.470868405+0000`":
	- Por defecto usa el formato ISO 8601:2019, incluyendo nanosegundos y formato UTC
- Advertencia: "`Warning Grep private keys or passwords activities found`"
	- Descripcion de la advertencia generada por Falco
- Detalles de los eventos
	- `evt_type=execve`: Se ejecuto un proceso
	- `user=madkoding`: El nombre del usuario que ejecuto el comando
	- `user_uid=1002`: El identificador unico de usuario
	- `user_loginuid=1002`: UID del inicio de sesion, podria ser diferente si se trata de un proceso que tuvo escalamiento de privilegios
	- `process=find`: El comando que ejecuto
	- `proc_exepath=/usr/bin/find`: Ruta absoluta del comando que ejecuto
	- `parent=bash`: El proceso general donde se ejecuto el comando, usualmente una shell
	- `command=find /root -name id_rsa`: El comando completo que se ejecuto
	- `terminal=34817`: Identificador unico de la sesion de terminal
	- `exe_flags=0`: Parametros adicionales que se ejecutaron, en este caso ninguno.
	- `container_id=host`: ID unico del contenedor, host significa que es del mismo sistema y no desde un contenedor
	- `container_name=host`: Nombre del contenedor, host significa que es del mismo sistema y no desde un contenedor

### Demo: Notificaciones al telefono
Falco, con la ayuda de extensiones como Falcosidekick, permite enviar notificaciones automaticas basadas en eventos con Gotify por ejemplo. Para permitir conectar se usa Cloudflare tunnel a travez de dominios.

**Herramientas**
- Falcosidekick: Recibe eventos desde Falco y los envia a otros servicios
- Gotify: Servidor de notificaciones que gestiona y envia alertas a los dispositivos
- Cloudflare Domains: Permite mapear ip locales y convertirlas en subdominios para poder acceder desde cualquier parte del mundo

**Configuracion de Sub-Dominios**
http://192.168.18.24:3030 -> https://gotify.proxylivy.work
http://192.168.18.24:3031 -> https://falcosidekick.proxylivy.work

**Configuraciones**
1. Configuracion de Falcosidekick
El archivo `docker-compose.yaml` con la configuracion necesaria, recuerda modificar "`{api-token}`" segun la seccion Aplicaciones de Gotify
```yaml
services:
  falcosidekick:
    container_name: falcosidekick
    image: falcosecurity/falcosidekick
    restart: unless-stopped
    environment:
      - GOTIFY_HOSTPORT=https://gotify.proxylivy.work
      - GOTIFY_TOKEN={api-token}
      - GOTIFY_FORMAT=markdown
      - GOTIFY_CHECKCERT=true
      - GOTIFY_MINIMUMPRIORITY=""
    ports:
      - '192.168.18.24:3031:2801'
```
2. Configuracion de Gotify
El archivo `docker-compose.yaml` con la configuracion necesaria
```
services:
  gotify:
    container_name: gotify
    image: gotify/server
    restart: unless-stopped
    ports:
      - 192.168.18.24:3030:80
    environment:
      - GOTIFY_DEFAULTUSER_PASS=SuperSecureDefaultPassword
    volumes:
      - "/docker/gotify/data:/app/data"
```
3. Modificar Falco para integrar Falcosidekick
Es necesario modificar el archivo.yaml[^2] para habilitar la salida json y http
```
json_output: true
json_include_output_property: true
http_output:
  enabled: true
  url: "https://falcosidekick.proxylivy.work"
```
Esta configuracion permite que Falco envie notificaciones a Falcosidekick, la cual la reenvia a Gotify y permite notificar diferentes dispositivos.

**Poner a Prueba**
Acceder a Gotify: Accede a `https://gotify.deathgabox.work`
- user: deathgabox
- pass: demodemogotify

Envia una request y recibiras una notificacion
```
curl -XPOST "https://falcosidekick.proxylivy.work/" -d'{"output":"16:31:56.746609046: Error File below a known binary directory opened for writing (user=root command=touch /bin/hack file=/bin/hack)","hostname": "localhost", "priority":"Error","rule":"Write below binary dir","time":"2024-09-10T15:31:56.746609046Z", "output_fields": {"evt.time":1507591916746609046,"fd.name":"/bin/hack","proc.cmdline":"touch /bin/hack","user.name":"root"}}'
```

## Decision Estrategica
En resumen, Falco es una solucion de seguridad de codigo abierto respaldada por la CNCF, que se adapta a las infraestructura modernas debido a su capacidad de monitorear en tiempo real el comportamiento/acciones a travez de eBPF. El punto fuerte de Falco es la seguridad que ofrece para entornos basados en contenedores como Docker, con orquestadores como Kubernetes y sus herramientas derivadas, ademas de poder conectar y monitorear sistemas externos o nubes publicas, lo que permite ampliar el rango de monitoreo a uno aun mas completo.
El ecosistema de Falco continua expandiendose dia tras dia con nuevas reglas, APIs y plugins, lo que le entrega una flexibilidad y versatilidad al sistema principal.


## Bibliografia
- [^1]: _The Falco Project_. (s. f.). Falco. https://falco.org/docs/
- [^2]: Falcosecurity. (s. f.). _falco/falco.yaml at master · falcosecurity/falco_. GitHub. https://github.com/falcosecurity/falco/blob/master/falco.yaml

# Extra
## Further Reading
- [Wikipedia - Kernel Simplified](https://commons.wikimedia.org/wiki/File:Simplified_Structure_of_the_Linux_Kernel.svg)
- [Thomas Krenn - LKinux Storage Stack Diagram](https://www.thomas-krenn.com/en/wiki/Linux_Storage_Stack_Diagram)
- [Threat in eBPF Malware](https://redcanary.com/blog/threat-detection/ebpf-malware/)
	- **Make sure unprivileged eBPF is disabled.** Nowadays, to install an eBPF program, you typically need root—or at least CAP_SYS_ADMIN and/or CAP_BPF. This was not always the case. Unprivileged eBPF was introduced around kernel 4.4. Be sure to check this config option by running:
		- sysctl kernel.unprivileged_bpf_disabled
	- Disable features you don’t need. Admins can programmatically disable things like kprobes:
		- echo 0 > /sys/kernel/debug/kprobes/enabled
	- Create firewall filters on external firewalls to block suspicious packets
	- Build kernels without support for kprobes, eBPF based TC filters, or eBPF entirely (although that may not be an option for many)
	- Ensure `CONFIG_BPF_KPROBE_OVERRIDE` is not set unless absolutely necessary
- [Defcon - eBPF, i throught we were friends](https://defcon.org/html/defcon-29/dc-29-speakers.html#fournier)
- [Blackhat - With friends like eBPF, who needs enemies](https://www.blackhat.com/us-21/briefings/schedule/#with-friends-like-ebpf-who-needs-enemies-23619)
- [Falco Blog Series about Response engine](https://falco.org/blog/falcosidekick-response-engine-part-1-kubeless/)
- [Falco Talon](https://github.com/falco-talon/falco-talon): Motor de respuesta a incidentes | [Falco Talon Docs](https://docs.falco-talon.org/)
- [Medium - Deploy a SOCasS](https://medium.com/@ibrahim.ayadhi/deploying-of-infrastructure-and-technologies-for-a-soc-as-a-service-socass-8e1bbb885149)
- A principios de 2022 Windows agrego soporte para eBPF | Repo [Microsoft/ebpf-for-windows](https://github.com/microsoft/ebpf-for-windows)
- [Blog - Running Falco on Docker Desktop](https://garethr.dev/2019/08/running-falco-on-docker-desktop/)
- [Blog - Micosoft - Secure Threat matrix for Kubernetes](https://www.microsoft.com/en-us/security/blog/2021/03/23/secure-containerized-environments-with-updated-threat-matrix-for-kubernetes/)
- [Blog - Falco - Reestructuring Kubernetes](https://falco.org/blog/restructuring-the-kubernetes-threat-matrix/)
- [Blog - Falco - Three Biggest Github Security Risk](https://falco.org/blog/falco-plugin-github/)

- [Practica con 2 maquinas eBPF](https://play.instruqt.com/embed/sysdig/tracks/falco-modern-ebpf)

- [Diferencias entre RFC3339 e ISO8601](https://ijmacd.github.io/rfc3339-iso8601/)

- falco-no-driver(Modern eBPF): https://hub.docker.com/r/falcosecurity/falco-no-driver
