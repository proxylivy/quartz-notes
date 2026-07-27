Linux es un kernel desarrollado por Linus Torvalds, anunciado en 1991 en el grupo de [noticias de Minix](https://web.archive.org/web/20130509134305/http://groups.google.com/group/comp.os.minix/msg/b813d52cbc5a044b), |es un kernel de tipo monolitico, lo que significa que la mayoria de sus componentes principales se ejecutan en el espacio del kernel.

Por si solo, Linux no es un [[020 - Conceptos/020.6 - Sistemas/OS|OS]] completo. Para ser utilizable, requiere herramientas de espacio de usuario (Como Shells, Utilidades Basicas, gestores de paquetes, etc.). La combinacion de este kernel con estas herramientas origina las distribuciones de Linux (o "distros").

Las distribuciones existen porque no hay un unico conjunto de decisiones optimas para cada caso. Cada distro define elecciones distintas, con software, maneras de gestionarlos, si ciclo de actualizaciones, que tanta estabilidad quieren, entre otras.

Aunque existen categorias claras segun escenarios, como el uso general en computadores personales, [[020 - Conceptos/020.4 - Dispositivos de Red/Servidor|Servidores]] o sistemas embebidos. No implica que una distribucion sea mejor que otra, o que haya alguna que te solucione todos los problemas (Distro Hopping), sino que sirve la que mas se adecua para un contexto especifico

Las distribuciones son realmente un conjunto donde lo unico que cambia entre ellas es el administrador de paquetes, por eso hay pocas familias donde elegir realmente
- Slackware: slackpkg
- Debian: apt
- RedHat: dnf
- ArchLinux: pacman
- Gentoo: portage
- Alpine: apk
- NixOS: nix
- Void Linux: xbps
- CRUX: pkgutils
- LFS (Linux From Scratch)

