# Info
**Charla: Tu servidor en tu propia nube (Contenedores, Git y Cloudflare)**
- Dada por primera vez en GitTogether Santiago, Agosto 2024
- Revision Tecnica en Mayo 2025

Alguna vez te has preguntado ¿Que necesita mi pagina web para estar en internet? e intentar no ser invadido por todos los anuncios posibles?

Bueno, aqui te tengo un metodo para poner cualquier servicio en internet de forma segura.

---
## Sobre Mi
Hola, soy Gabriel Zuñiga
- Estudiante de Ing. en Conectividad y Redes
- Experiencias: 5 Años con Linux & 1 Año con Docker
- Apasionado por el aprendizaje continuo
- RTFD / RTFM: Siempre leyendo la documentacion primero
Como tambien pueden ver, me gusta subir cerros

---
## Sobre la charla

**Materiales**

- Un dominio de internet de una fuente confiable
	- Cloudflare Register
	- Google Names
	- NameCheap
	- NIC.cl
- Cuentas en Cloudflare, Github y Docker

---
**Espero que aprendan lo que he aprendido**

- Saltar los bloqueos del ISP
- Aprender un poco sobre la base de Contenedores
- Usar metodos seguros

---
**¿Porque existe esta charla?**

Muchas personas con las que hablo piensan que poner un servidor en internet es complicado. Quiero desmentir eso y mostrar cómo es posible que cualquiera lo haga.
- Autonomia Personal: Desarrolla ideas y tecnologias con tu equipo sin que el ISP este en la mitad
- Alternativa a los VPS: En muchos casos, solo para poner una pagina web con poco flujo, los VPS son extremadamente caros (~5-100 dolares al mes)
- Uso eficiente del Hardware: Tu computador solo sera usado cuando los microservicios interactuen
- Vendor-Lock: Este metodo es mas agnostico, funciona tanto en entornos donde no haya nube como en entornos donde ya hay, poder elegir luego en el caso de ya no usar cloudflare y tus servicios alli estaran, solo necesitas enrutarlos otra vez

---
**Quien deberia ver esta charla**

**¿Para quien es esta charla?**
- Personas interesadas en explorar servicios en internet estando limitados por un ISP
- Personas que le encanta aprender sobre tecnologias nuevas y redes
**¿Para quien no es esta charla?**
- Los que necesitan un SLA de 99.99% (No puedo garantizar, los cortes de luz y de internte son una realidad)

---
**Que es un servidor**

Bueno, ¿Pero... Mi dispositivo servira?
Un servidor es cualquier sistema (hardware y software) que, conectado a una red, atiende y responde a peticiones de clientes, ofreciendo uno o varios servicios de forma continuada y fiable.

Si tu dispositivo tiene una interfaz de red y puedes mantenerlo conectado el maximo tiempo posible, es un servidor"

> [!IMPORTANT] Sobre sobre Virtualizacion
> Para habilitar la virtualizacion, tu CPU debe aceptar instrucciones VT-X y EPT o AMD-V para soportar instrucciones anidadas.

Quiero hacerlos fijar que no dice "Cpu: xeon 8 nucleos 16 hilos y toneladas de ram", inclusive en la practica tengo un computador del 2007, 2 nucleos 2 hilos y 4gb de ram y he aprendido muchisimo, con un uso promedio de 3% de cpu

---
## Sobre Internet
### Nacimiento y razon de ser
IP o Internet Protocol es el protocolo de identificacion dentro del internet
TCP o Transmission Control Protocol es el protocolo de comunicacion que usa internet

Usualmente se les relaciona cuando se habla de internet como TCP/IP, es algo incorrecto pero dejemoslo existir tranquilo

IP nacio en 1981 en base al proyecto DARPA, tambien definido en el [RFC791](https://datatracker.ietf.org/doc/html/rfc791) como evolucion al proyecto ARPANET

Debido a que IPv4 fue pensado para un proyecto de proporciones universitarias, se encontro rapidamente que un despliegue mundial estaria lleno de muchos problemas y errores de planificacion, luego de varios trabajos y mejoras, se dieron cuenta que era mejor empezar con la planificacion desde cero, enfocandose en ser rapidos, seguros y mucho mas escalables, asi fue como nacio IPv6 ([RFC2460](https://www.rfc-editor.org/rfc/rfc2460) -> [RFC8200](https://www.rfc-editor.org/rfc/rfc8200)) en 1998 y continua evolucionando

Un dato muy interesante es el siguiente:
- Imagina 2 mundos, uno que solo funciona con IPv4 y el otro con IPv6
- Cada mundo tiene 8.100 Millones de personas | [Fuente](https://www.worldometers.info/es/poblacion-mundial/)


El primero mundo tendria:
- Una base matematica de 32 bits -> 4.300 Millones de direcciones Unicas
- Se excluyen unas 554.764.240 IPs debido a distintos espacios reservados
	- [RFC 1918](https://www.rfc-editor.org/rfc/rfc1918): Direcciones Privadas
	- [RFC 5771](https://www.rfc-editor.org/rfc/rfc5771): Direcciones Multicast
	- [RFC 3330](https://www.rfc-editor.org/rfc/rfc3330): Direcciones Experimentales

Esto daria el valor:
- 1 IP publica por cada 2 Personas

El segundo mundo tendria:
- Una base matematica de 128 bits -> 340 sextillones de direcciones IP
- Se excluyen la MITAD o 64 bits para uso privado -> 8 quintillones
- Cada persona tendria 41.975 octillones de IPs Publicas


Si te interesa, puedes leer mas a fondo:
- Compatibilidad con [Test IPv6](https://test-ipv6.com/) o algun [Mirror](https://test-ipv6.com/mirrors.html)
- [Blog - Cloudflare - How does the Internet Work](https://www.cloudflare.com/learning/network-layer/how-does-the-internet-work/)

---
### NAT y CG-NAT

NAT Significa "Network Address Translation" es usado en redes IPv4 y esta definido en el [RFC2663](https://www.rfc-editor.org/rfc/rfc2663)
CG-NAT (NAT444) Significa "Carrier Grade Network Address Translation" y son es usado por la mayoria de ISP y esta definido en el [RFC6888](https://www.rfc-editor.org/rfc/rfc6888)

NAT se creo como un mecanismo para entornos IPv4 debido a la escases de IPs Publicas debido a la creciente demanda desde los años 2000 (Dot Com Boom), Su funcionamiento es sencillo, reciben multiples IPs y traducen mediante una sola o multiples IPs

Alrededor de 2013, los proveedores de internet, debido a que tenian pocas IPs Publicas, implementaron CG-NAT en sus contrataciones de internet domesticos, por lo que su funcionamiento es asi
- Tu usas un segmento privado (no enrutable) para tus dispositivos dentro de tu hogar
	- 10.0.0.0/8
	- 172.16.0.0/12
	- 192.168.0.0/16
	- Etc.
- Tu proveedor de internet, traduce la ip desde el router domestico, en un segmento que conoce el y tu proveedor de internet
- Luego tu ISP traduce tu peticion y alli realmente sale a internet

Para un ejemplo mas visual, el siguiente [Diagrama](https://blog.apnic.net/wp-content/uploads/2022/03/nat-cgnat-1.png) de blog.apnic.net, se puede ver a los dispositivos de tu hogar, conectarse con la red del ISP, que luego traduce y da acceso a Internet, el problema es que es imposible iniciar conexiones entrantes desde internet a tu red domestica.

> [!IMPORTANT] Nota sobre NAT
> Estas tecnologias SOLO aplican para IPv4.

Read More in
- [Blog - Rapidseedbox - CGNAT](https://www.rapidseedbox.com/es/blog/cgnat)
- [Blog - chrisgrundemann - NAT444 CGN/LSN and what it breaks](https://chrisgrundemann.com/index.php/2011/nat444-cgn-lsn-breaks/#)
- [RFC7021 - Assessing the Impact of Carrier-Grade NAT on Network Applications](https://datatracker.ietf.org/doc/html/rfc7021)

**¿Y cual es el problema con CG-NAT?**

NAT es una solucion parche para el gran problema de direcciones IP Publicas disponibles, Dentro de los incovenientes, estan
- Reduce el rendimiento
- Limita la conectividad
- Dificultad para acceder a cámaras de seguridad remotamente
- Problemas con juegos online y aplicaciones P2P
- Imposibilidad de hostear servidores web o servicios propios

---
## Soluciones
### Cloudflare Register (Domains)
**¿Que es un dominio?**
Un dominio es un nombre de alto nivel (TLD) el cual permite identificar un dispositivo a travez de internet

**¿Porque es importante tener un dominio?**
Debido a que la asignacion de IPv4 como mostre arriba, esta agotada, es una forma de poder ser identificable en la gran red

**¿Porque Cloudflare Register y no otro?**
La verdad, no es necesario usar Cloudflare Register, solamente tu proveedor de dominios debe tener la compatibilidad de cambiar los servidores DNS para poder hacer ajustes desde Cloudflare Dash para poder administrar los dominios, asi que puedes elegir el que desees, yo uso por conveniencia el cloudflare, pero eres libre de elegir, de verdad

**¿Como pueedo registrar un dominio de Cloudflare?**
1. Crear una [cuenta](https://developers.cloudflare.com/fundamentals/setup/account/create-account/) y [verificarla](https://developers.cloudflare.com/fundamentals/setup/account/verify-email-address/) en Cloudflare
2. Registrar/Comprar un [Dominio](https://developers.cloudflare.com/registrar/get-started/register-domain/) | Recuerda respetar las [politicas](https://www.cloudflare.com/tld-policies/) de tu dominio
3. Selecciona el plan gratuito | Deberas ingresar tus datos de tarjeta, pero no te hara ningun cobro

**¿¡Y los Precios!?**
Varian segun el nombre del dominio, pueden ir de los 4 a 20 Dolares anuales por dominio, es el unico servicio externo que cuesta dinero de esta charla. deathgabox.work me costo 7 dolares en el lapso 2023-2024 y proxylivy.work me costo 7 dolares en el lapso de 2024-2025

Read More in
- [Blog - Cloudflare - Introducing Cloudflare Registrar](https://blog.cloudflare.com/cloudflare-registrar/)

### Cloudflare Tunnel (Conexion)
**¿Que son los tuneles?**
Los tuneles son enlaces punto a punto, en los cuales se encripta el contenido, permite mantener la informacion segura.

**¿Como es que funciona?**
Se mantiene una instancia ligera corriendo de fondo que encripta la informacion desde el servidor de origen hasta los servidores y datacenter mas cercanos a cloudflare, todo sin abrir ningun puerto y sin que sea enrutable por alguien externo, 
Este servicio se encarga de transformar tu IP Hostname privada (http://hostname:port) -> Hostname Publico (https://uptime-kuma.deathgabox.work)

**¿Como se crean los tuneles?**
(Recuerden que deben tener un dominio en su cuenta para que funcione perfectamente)
1. Accede a [Cloudflare one dash](https://one.dash.cloudflare.com/)
2. Ir a la seccion de Networks > Tunnels
3. Apretas "Create a Tunnel"
4. Seleccionas "Cloudflared" y "Next"
5. Le das un nombre a tunel "GitTogether"
6. Seleccionas el ENV (Docker)
7. Instala y corre el conector

```
docker run -itd --name cloudflare --network host --restart unless-stopped cloudflare/cloudflared:latest tunnel --no-autoupdate run --token eyJhIjoiZjFkODFjOGZjYTdjOTA2MWI2NTk2OTY4ZjBjNGJmOTciLCJ0IjoiZjEzMmE5MWYtY2ZhMy00Y2I0LTkxYmItMGY3ZWVlMWU1MTE2IiwicyI6Ik1qazJNRE13WVdFdE9EWTROeTAwWXpjd0xUZzBOall0WTJVeE16aGpOR1ZpTVRBMiJ9
```

Learn More in
- [Blog - Cloudflare - What is Tunneling](https://www.cloudflare.com/learning/network-layer/what-is-tunneling/)
- [Blog - Cloudflare - Argo Tunnel](https://blog.cloudflare.com/argo-tunnel/)
- [Blog - Cloudflare - Argo Tunnel that live forever](https://blog.cloudflare.com/argo-tunnels-that-live-forever/)

---
### Contenedores
Explicar sobre los containers en el blog desde el libro networking, oreilly - 4.- Cloud - Containers

Docker es una plataforma abierta para desarrollar, enviar y ejecutar aplicaciones, permite aislar de la infraestructura y empaquetar una aplicacion y sus dependencias sin consumir mas recursos de los necesarios, debido a que comparte el Kernel o nucleo del OS con el Host, lo que lo hace mas eficiente que las Maquinas Virtuales clasicas.
Esto nos permite:
- Alojaremos servicios: lo que ayuda en el despliege, actualizacion y escabilidad.
- Simplifica la configuracion: gracias al lenguaje de marcado YAML.
- Mantiene consistencia: Si funciona en mi maquina, funcionara en la tuya.

Necesitas instalar docker en tu maquina, en archlinux solo necesitas
1. Instala `docker`
2. Activa por sistemd el socket `systemctl enable docker.service --now`
3. Agrega tu usuario al grupo docker para mas rapidez `sudo usermod $user -aG docker`

**Arquitectura de los contenedores**
Expliquemos cada parte antes de lo que debes saber para usar docker
Seccion de Registros
Registros de Containers, Docker no es el unico que gestiona y administra imagenes, hay varias, podriamos poner de ejemplo a [hub.docker.com](https://hub.docker.com/), [quay.io](https://quay.io/)(Redhat) o [ghcr.io](https://github.com/features/packages)(Github)

- Imagenes: Son aplicaciones con sus dependencias y librerias empaquetadas bajo un nombre y tag (NGINX, PostgreSQL o Uptime Kuma)
- Extensiones: Herramientas de terceros para extender la funcionalidad de Docker
- Plugins: Herramientas que modifican el comportamiento de docker, para agregar o quitar funcionalidades, como Networking, Volumenes o Autorizacion

Seccion de Host
Esta es tu maquina, donde se guardan en tu disco, los volumenes persistentes, las imagenes, los containers creados y la instancia(daemon) de Docker que va organizando todo esto

Seccion del Cliente
- Docker run: Permite ejecutar alguna imagen, si no esta en tu maquina, se buscara en hub.docker.com 
- Docker build: Permite crear un container desde un archivo Dockerfile, es cual son las instrucciones para poder replicar cada instalacion
- Docker pull: Permite descargar la imagen de algun contenedor de registros
- Docker compose: Un plugin que te permitira crear archivos .yaml que contienen instrucciones (al igual que los comandos run) que te permiten organizar y desplegar los servicios de manera ordenada

Learn more in
- [Blog - Redhat - what is a container registry](https://www.redhat.com/en/topics/cloud-native-apps/what-is-a-container-registry)

---
### Github
**Github y la Creatividad**
Github es una plataforma de control de versiones y colaboracion, permite encontrar repositorios increibles y sera el punto principal que unifica todo lo aprendido detras.

Las mejores listas de aplicaciones que he visto, estan en Github, estas se llaman "Awesome"
un par de ejemplos
[Awesome-Selfhosted](https://github.com/awesome-selfhosted/awesome-selfhosted)| Servicios para tu servidor
[Awesome-Tunneling](https://github.com/anderspitman/awesome-tunneling) | 
[Awesome-PKM](https://github.com/doanhthong/awesome-pkm) | Tomar notas
[Awesome ESP](https://github.com/agucova/awesome-esp) | Proyectos con microcontroladores
[Awesome-ESP-Proyect](https://github.com/hpsaturn/Awesome-ESP-projects) | Mas proyectos con microcontroladores
[Awesome Compose Files](https://github.com/Haxxnet/Compose-Examples) | Ejemplos para comprender Docker Compose
[Self-Hosting-Guide](https://github.com/mikeroyal/Self-Hosting-Guide) | Guia sobre Auto-Hostear



## Mejoras sobre Cloudflare
Luego de comprar tu dominio, debes configurar parametros desde el [dash.cloudflare.com](https://dash.cloudflare.com), luego acceder a `Websites` y apretar en el dominio que quieres configurar, te abrira el siguiente menu de configuracion

- DNS
	- Settings
		- Enable DNSSEC (Demora hasta 2 dias en completarse, a mi estuvo listo en 3 horas)
		- Multi-signer DNSSEC (Luego de que se habilite DNSSEC)
- SSL/TLS
	- Overview
		- Configure SSL/TLS encryption
			- Selecciona `Custom SSL/TLS`
				- Selecciona `Full (Strict)` si es que no manejas certificados autofirmados por [letsencrypt](https://letsencrypt.org/) o [certbot](https://certbot.eff.org/)
	- Edge Certificates
		- Selecciona `Always Use HTTPS`
		- Configura `Minimum TLS Version` en `1.2` o Superior
- Speed
	- Optimization
		- Intenta mejorar las mejoras gratuitas
			- Content Optimization
				- Activar `Cloudflare Fonts`
				- Activar `Early Hints`
				- Activar `Rocket Loader`
			- Protocol Optimization
				- Activar `0-RTT Connection Resumption`
- Network
	- (Opcional) Habilitar gRPC

---
## Manos a la Obra
**Oremos por los dioses de la DEMO, que siempre tengan disponibilidad**

## Gracias a
- Atareao con Linux - Por darme la curiosidad que necesito para descubrir nuevas cosas - [Podcast sobre Tunel](https://atareao.es/podcast/raspberry-en-internet-sin-abrir-puertos/)
- Pelao Nerd - Por enseñarme sobre herramientas para ser una pelade SRE - 
- GitTogether - Por darme la oportunidad de dar esta charla
- Todos los blog que hay en internet
- Toda la documentacion escrita por empresas y usuarios por igual
- A ustedes por escuchar mi charla

- [Nicol Rafalowski - Como Negociar Tu Salario Como si no te importara](https://docs.google.com/presentation/d/1GUayaiOgQ5jggzLt63i67fBpoZ6agHhn7lcexvt2V1M/mobilepresent#slide=id.g1dc15966612_0_0)
- Learnaws.io [Blog](https://learnaws.io/blog/cloudflare-tunnel)

Descarga la ppt
[Charla GitTogether Tu servidor en tu nube.pptx](https://duoccl0-my.sharepoint.com/:p:/g/personal/ga_zunigam_duocuc_cl/EVc9cNmQ2TRPrkSNS-wUjM8BVAAS0HsVy1OkABsR8i18rg?e=4xyfuC)

