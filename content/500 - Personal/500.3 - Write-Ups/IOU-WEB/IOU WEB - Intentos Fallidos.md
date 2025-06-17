# Intentos
## Compilacion para IOU WEB 64 bits
Adaptar un sistema es muy complicado cuando es versionado, por lo que el alcance de [[500 - Personal/500.3 - Write-Ups/IOU-WEB/IOU WEB - 64 Bits CentOS 7|IOU WEB - 64 Bits CentOS 7]] esta completo, por lo que a pesar de decir 64 bits, solo es compatible con imagenes de 32 bits, pero permite utilizar muchisimo mejor la aceleracion por hardware de distintos sistemas, por lo que es una version muy agradable para continuar usando como reemplazo rapido a IOU WEB

La intencion era compilar GLIBC, debido a que las maquinas de 64 bits, necesitan una version mayor a 2.27, por lo que la unica opcion, era compilar una version compatible, con la actualizacion del kernel, me permitia no instalar solo GLIBC 2.27 sino una un poco mayor, me convenci con 2.34 por la compatibilidad del sistema.

En un futuro podria hacer Make y GCC dentro de Icarus para poder compilar correctamente OpenSSL y OpenSSH, pero eso sera otro dia, el siguiente reemplazo directo a esta maquina, es la instalacion experimental de [[999 - Archivado/IOU WEB - 64 Bits Arch Linux (WIP)|IOU WEB - 64 Bits Arch Linux (WIP)]]

El mejor prefijo para poner los programas compilados correctamente es en "/usr/local", de esta forma no sobreescribe las herramientas del sistema, se le puede configurar como PATH prioritario, y sirve para poder configurar una cadena `LD_LIBRARY_PATH` y `LD_RUN_PATH` que ayudara al sistema a ejecutar todo correctamente

Se me ocurre que la lista de prioridad deberia ser asi
```
export PATH=/usr/local/make-4.2.1/bin:/usr/local/sbin:/usr/local/bin:/opt/iou/bin:/usr/bin:/bin
```

La explicacion puede ser la siguiente
- `/usr/local/sbin`: Binarios de administracion local para root
- `/usr/local/bin`: Binarios locales compilados para usuarios
- `/opt/iou/bin`: Binarios de IOU WEB
- `/usr/bin`: Binarios instalados por el adminitrador de archivos (`yum o dnf`)
- `/bin`: Binarios del sistema (`ls`, `cp`, etc.)


Y que los agregados se agregan de la siguiente manera, es muy importante el $PATH al final
```
export PATH=/usr/local/make-4.2.1/bin:$PATH
```

Y ahora que lo pienso se puede hacer lo mismo con LD_LIBRARY_PATH
TO-DO: deberia extraer la base limpia de 32 bits y revisar cual es
```
export LD_LIBRARY_PATH=/usr/local/lib64:/usr/local/lib
```

Estas pueden estar en rc.local y .bashrc
```
source ~/.bashrc
```

Tambien para configurar las librerias, se puede utilizar el archivo `/etc/ld.so.conf.d/local.conf` y le agregas las rutas de tus librerias
```
/usr/local/lib64
/usr/lib32
```

> Luego se configuran esas rutas con
```
sudo ldconfig
```

## Compilar Programas
> [!TIP] Sitios Recomendados Generales
> - [GNU.org Manual Docs](https://www.gnu.org/manual/manual.html)
> - [GNU.org Software List](https://www.gnu.org/software/software.html)

### GNU GCC 10.5.0
> [!TIP] Sitios Recomendados
> - [GNU GCC - The GNU Compiler Collection](https://www.gnu.org/software/gcc/)
> - [GNU.org Docs - GCC Online Docs](https://gcc.gnu.org/onlinedocs/)
> 	- [Invoing GCC Options](https://gcc.gnu.org/onlinedocs/gcc/Invoking-GCC.html)
> 	- [C++ Standard Supports in GCC](https://www.gnu.org/software/gcc/projects/cxx-status.html)
> - [GCC GNU Docs - Installing GCC](https://gcc.gnu.org/install/)
> 	- [1. Prerequisites for GCC](https://gcc.gnu.org/install/prerequisites.html)
> 	- [2. Downloading GCC Source](https://gcc.gnu.org/install/download.html)
> 	- [3. Configuration](https://gcc.gnu.org/install/configure.html)
> 	- [4. Building](https://gcc.gnu.org/install/build.html)
> 	- [5. Testing (Optional)](https://gcc.gnu.org/install/test.html)
> 	- [6. Final Install](https://gcc.gnu.org/install/finalinstall.html)
> - Mirror List
> 	- [GCC Mirror Sites](https://www.gnu.org/software/gcc/mirrors.html)
> 	- [GNU.org - Mirror List](https://www.gnu.org/prep/ftp.html)
> - [GCC Releases](https://gcc.gnu.org/releases.html) | [Timeline](https://www.gnu.org/software/gcc/develop.html#timeline) | [CppReference - Compiler Support](https://en.cppreference.com/w/cpp/compiler_support.html)
> 	- [GNU GCC - GCC 4.8 Release Series](https://www.gnu.org/software/gcc/gcc-4.8/): Instalada en CentOS 7
> 	- [GNU GCC - GCC 10 Release Series](https://www.gnu.org/software/gcc/gcc-10/): Compilando en CentOS 7
> 	- [GNU GCC - GCC 15 Release Series (Latest)](https://www.gnu.org/software/gcc/gcc-15/): Branch Actual cuando escribo esto
> - Blogs
> 	- [The Linux Cluster - Compile GCC 10.4.0 on CentOS 7](https://thelinuxcluster.com/2022/07/01/compiling-gcc-10-4-0-on-centos-7/)
> 	- [Cyberithub - Easy Steps to Intall GCC on CentOS 7](https://www.cyberithub.com/install-gcc-and-c-compiler/)
> 	- [Jwillikers - Build GCC from source on CentOS 7](https://www.jwillikers.com/build-gcc-from-source-on-centos-7)
> 	- [Red Hat Devs - Compiler and Linker Flags for GCC](https://developers.redhat.com/blog/2018/03/21/compiler-and-linker-flags-gcc)
> 	- [Github Gist - nchaigne - Building and Compile GCC 9.2.0 on CentOS 7](https://gist.github.com/nchaigne/ad06bc867f911a3c0d32939f1e930a11)

La forma correcta de hacer esto y con el visto weno de GNU (Prerequisites for GCC), es instalar una version intermedia de GCC
- GCC 4.8.5 -> GCC 9.5 -> GCC 15


> Ve a la carpeta de fuentes
```
cd /usr/src
```

> Descarga GCC 10.5.0 (07 de Julio de 2023)
```
wget https://ftp.gnu.org/gnu/gcc/gcc-10.5.0/gcc-10.5.0.tar.gz
```

> Descomprime el TAR
```
tar -xzvf gcc-10.5.0.tar.gz
```

> Elimina el TAR
```
rm -f gcc-10.5.0.tar.gz
```

> Ve a la carpeta de GCC
```
cd gcc-10.5.0
```

> Ejecuta el instalador de prerequisitos (Descargara `gmp`, `mpfr`, `mpc`, `isl`)
```
./contrib/download_prerequisites
```

> Crea la carpeta build y ve a ella
```
mkdir build && cd build
```

> Configura el Prefijo temporal (parece ser que ahora si funciona), habilita los lenguajes c y c++ que son los que usaremos, y habilita la compilacion para la libreria de 32 bits
```
../configure --prefix=/usr/local/gcc-10.5.0 --enable-languages=c,c++ --enable-multilib 
```

> Compila usando el 100% de la CPU (Tengo 4 Nucleos y se demoro aprox 2 horas y media)
```
make -j$(nproc)
```

> Confia en los dioses de la demo e instala la version compilada
```
make install -j$(nproc)
```

> Limpia los archivos de compilacion
```
make clean -j$(nproc)
```

> Exportar temporalmente al PATH
```
export PATH=/usr/local/gcc-10.5.0/bin:$PATH
```

> Exportar temporalmente la libreria
```
export LD_LIBRARY_PATH=/usr/local/gcc-10.2.0/lib64:$LD_LIBRARY_PATH
```

> Revisa si esta funcionando con `gcc --version`
```
gcc (GCC) 10.5.0
Copyright (C) 2020 Free Software Foundation, Inc.
```

#### Compila un programa en C

> Escribe dentro de "`hello.c`"
```
#include <stdio.h>

int
main()
{
  printf("hello, world\n");
  return 0;
}
```

> Compila el programa hello.c
```
gcc -o hello hello.c
```

> Ejecuta el programa compilado
```
./hello
```

Si todo resulta correcto, tenermos GCC 10.5.0 instalado en el sistema

### GNU C Library (GlibC) 2.34
> [!TIP] Sitios Recomendados
> - [GNU.org Software - GNU C Library](https://www.gnu.org/software/libc/)
> - [GNU.org Docs - GNU C Library](https://www.gnu.org/software/libc/manual/)
> 	- [Docs 2.34 - Appendix C.1 - Configuring and compiling the GNU C Library](https://sourceware.org/glibc/manual/2.34/html_node/Configuring-and-compiling.html) | [Latest (2.41)](https://sourceware.org/glibc/manual/2.41/html_node/Configuring-and-compiling.html)
> 	- [Docs 2.34 - Appendix C.2 - Installing the Library](https://sourceware.org/glibc/manual/2.34/html_node/Running-make-install.html) | [Latest (2.41)](https://sourceware.org/glibc/manual/2.41/html_node/Running-make-install.html)
> 	- [Docs 2.34 - Appendix C.3 - Recommended Tools for Compilation](https://sourceware.org/glibc/manual/2.34/html_node/Tools-for-Compilation.html) | [Latest (2.41)](https://sourceware.org/glibc/manual/2.41/html_node/Tools-for-Compilation.html)
> - [GNU.org - Mirror List](https://www.gnu.org/prep/ftp.html)
> - Blogs
> 	- [Tiknkink - How to Update GlibC](https://tutorials.tinkink.net/en/linux/how-to-update-glibc.html)

GBLIBC 2.28 Necesita
- GNU `make` 4.0 or newer (Tengo 4.4.1)
- GNU `GCC` 6.2 or newer (Tengo 4.8.5, compilando GCC 10.5.0)
- GNU `binutils` 2.25 or later (Compile con 2.44 y dio problemas, la wiki dice `2.35.1`, oops)
- GNU `texinfo` 4.7 or later (Tengo texi2any (GNU texinfo) 5.1)
- GNU `awk` 3.1.2, or higher (Tengo 4.0.2)
- GNU `bison` 2.7 or later (Tengo 3.0.4)
- GNU `sed` 3.02 or newer (Tengo 4.2.2)
- Perl 5 (OPCIONAL, De todas formas tengo perl-4:5.16.3-299)

Envia mensajes UNSUPPORTED si no estan, no son criticos, pero no tendra soporte para los debugging symbols
- Python 3.4 or later (Python 3.6.8)
- The Python `abnf` module. (Successfully installed abnf-2.2.0)
- PExpect 4.0 (python36-pexpect-4.8.0-1)
- GDB 7.8 or later with support for Python 2.7/3.4 or later (Tengo GDB 7.6.1-120.el7, el modulo python gdb_plus no lo pilla, pero parece que puedo vivir perfectamente sin el)

> NOTE FROM DOCS
```
Unless Python, PExpect and GDB with Python support are present, the printer tests will report themselves as `UNSUPPORTED`. Notice that some of the printer tests require the GNU C Library to be compiled with debugging symbols.
```

> NOTE FROM DOCS
```
For multi-arch support it is recommended to use a GCC which has been built with support for GNU indirect functions. This ensures that correct debugging information is generated for functions selected by IFUNC resolvers. This support can either be enabled by configuring GCC with ‘--enable-gnu-indirect-function’, or by enabling it by default by setting ‘default_gnu_indirect_function’ variable for a particular architecture in the GCC source file gcc/config.gcc.
```

> Crear carpetas necesarias
```
mkdir -p /src/gnu/glibc-build
```

NOTE
- glibc-build -> Object
- glibc-2.28 -> Unpack library Packages

> Ve al directorio de glibc-2.28
```
cd /src/gnu/
```

> Descarga la version de GLIBC necesaria
```
wget https://gnu.c3sl.ufpr.br/ftp/libc/glibc-2.34.tar.gz
```

> Descomprime el tar
```
tar -xzvf glibc-2.34.tar.gz
```

> Elimina el archivo tar
```
rm -f glibc-2.34.tar.gz
```

> Ve a la carpeta gblic-build
```
cd /src/gnu/glibc-build
```

> Configura GLIBC
```
../glibc-2.34/configure --prefix=/opt/glibc-2.34
```

> Compilar utilizando el 100% de la CPU
```
make -j$(nproc)
```

> Checkear si la version de GLIBC es funcional con el 100% de uso de CPU
```
make check -j$(nproc)
```

> Instala la version compilada con el 100% de la CPU
```
make install -j$(nproc)
```

> Prueba ???
```
/opt/glibc-2.34/lib/ld-2.34.so --library-path /opt/glibc-2.34/lib /opt/glibc-2.34/lib/libc.so.6
```

> Revisa las versiones de GLIBC Soportadas
```
strings /lib64/libc.so.6|grep GLIBC_
```

> Probar el funcionamiento con GLIBC-2.34 y con todas las demas librerias, teniendo como prioridad la de GLIBC2.34
```
/opt/glibc-2.34/lib/ld-linux-x86-64.so.2 --library-path /opt/glibc-2.34/lib:/lib64:/usr/lib64:/usr/lib /bin/ls
```

> Ejecuta un binario con la nueva version de GLIBC
```
LD_LIBRARY_PATH=/opt/glibc-2.18/lib /opt/glibc-2.18/lib/ld-2.18.so /bin/ls
```

> Limpia la carpeta del build
```
make clean -j$(nproc)
```

### OpenSSL
> [!TIP] Sitios Recomendados
> - [OpenSSL Library - Source](https://openssl-library.org/source/)
> - [OpenSSL Wiki - Compilation and Instalation](https://wiki.openssl.org/index.php/Compilation_and_Installation)
> - [dev-to Blog - Nikola - Update SSL to 3.0 on CentOS7](https://dev.to/nikolastojilj12/update-openssl-to-3-0-on-centos7-150o)

Aqui si hay que tener miedo uhhh
Usare `3.0.16 [LTS]` porque mejor volar cerca del sol que nada, ademas que la 3.5 pesa 5 veces mas y me da miedito

> Descarga la fuente de OpenSSL
```
cd /usr/src
wget https://github.com/openssl/openssl/releases/download/openssl-3.0.16/openssl-3.0.16.tar.gz
tar -zxf openssl-3.0.16.tar.gz
rm openssl-3.0.16.tar.gz
```

> muevete a la carpeta
```
cd openssl-3.0.16
```

> Crea la carpeta build y ve a ella
```
mkdir build && cd build
```

> Revisa las plataformas soportadas
```
./cnfigure LIST
```

> TEST
```
gcc -dM -E - </dev/null | grep __SIZEOF_INT128__
```


> Compila xdxd
> `./Configure linux-x86_64 no-shared no-ssl3 no-comp no-zlib --prefix=/opt/openssl-3.5.0`
```
cd /usr/src/openssl-3.5.0
./config
make depend -j$(nproc)
make -j$(nproc)
make test -j$(nproc)
make install -j$(nproc)
make clean -j$(nproc)
```

```
ln -s /usr/local/lib64/libssl.so.3 /usr/lib64/libssl.so.3
ln -s /usr/local/lib64/libcrypto.so.3 /usr/lib64/libcrypto.so.3
```

```
openssl version
```

```
sudo ln -s /usr/local/bin/openssl /usr/bin/openssl
```

### OpenSSH Portable
La ultima version compatible es la de 32 bits porque no tiene la ultima version con soporte o OpenSSL1.1.1
```
cd ~/git
mkdir openssh && cd openssh
wget https://cdn.openbsd.org/pub/OpenBSD/OpenSSH/portable/openssh-10.0p2.tar.gz
tar xvf openssh-10.0p2.tar.gz
rm -drf openssh-10.0p2.tar.gz
cd openssh-10.0p1
./configure -q
make -j$(nproc)
make install -j$(nproc)
```


## Soporte GLIBC >=2.27

(WIP) debido a que a pesar de que x86_64 L3 funciona a medias, L2 no llega nisiquiera a iniciar

> Ver versiones de GLIBC
```
strings /lib64/libc.so.6|grep GLIBC_
```

> [!TIP] Lectura Recomendada
> - [Download RedHat Linux Packages](https://access.redhat.com/downloads/content/479/ver=/rhel---8/8.10/x86_64/packages)
> - [Github NixOS/patchelf](https://github.com/NixOS/patchelf)

---
---

Aunque tambien se puede hacer del propio sistema, OMG

> Apago la maquina

> Creas una nueva, importar desde el disco

> Monto el disco como unidad 2 en QEMU como Dispositivo de Disco con drivers virtio porque sino pa que

> En Opciones de arranque seleccionamos el disco 2 primero que el 1

> Y le damos en Iniciar la instalacion

> Inicio el sistema

> Seleccionas "Troubleshooting"

> Vas a donde dice "Rescue a CentOS sytem"

> Luego se quedara en blanco, simplemente esperas, hasta que inicie

> Iniciara el instalador

> El sistema del instalador probara a montarse en /mnt/sysroot

> Selecciona 1 "Continue"

> Escribe "chroot /mnt/sysimage"

> Ahora tendras disponible una SH

> Ejecuta Bash para tener mejor control sobre la terminal

> Habilita SSH con `systemctl start sshd`

> Ejecuta otra vez "chroot /mnt/sysimage"

```
lsblk
fdisk -l
cat /etc/fstab
df -h
blkid
gcc --version (4.8.5)
wich gcc (/mnt/sysimage/bin/gcc)
g++ --version (4.8.5)
wich g++ (/mnt/sysimage/bin/g++)
ldd --version (2.17)
make --version (3.82)
curl --version (7.29.0)
wget --version (1.14)
rsync --version (3.1.2)
```

> Dentro de `scl enable devtoolset-11 bash` (No funciona dentro de Troubleshooting, pero si dentro de chroot /mmt/sysroot)
```
gcc --version (11.2.1)
g++ --version (11.2.1)
make --version (4.3)
```

> Ejecuta dhclient

> Ahora puedes probar ping 1.1.1.1

Soluciona problemas menores
> Crea llaves ssh
```
ssh-keygen -A
```

> Deja corriendo sshd de fondo
```
`/usr/sbin/sshd -D -e`
```

> Ejecuta
```
echo "nameserver 1.1.1.1" > /etc/resolv.conf
```

> Ahora podras hacer ping al dns
```
ping google.cl
```

> Ahora podras conectarte con ssh desde tu maquina, y poder copiar y pegar, que alivio

> NOTA, PRIMERO PROBARE SIN SCL DEVTOOL A VER SI COMPILA O NO

---


AHORA EMPIEZA LA INSTALACION DE GLIBC

Como pequeño entorno de pruebas de CentOS 7.9, instalare 2.28, una actualizacion menor, de GLIBC 2.27 que nos deberia dejar intentar probar y documentar el funcionamiento



---
---
---


Para el correcto funcionamiento de las maquinas de 64 bits, se necesitan otras dependencias, la principal es la presencia de GLIBC <=2.27, en CentOS 7.9, usa el paquete GLIBC 2.17, por lo que nos lanzara el siguiente error si intentamos ejecutar una imagen:
```
./x86_64_crb_linux-adventerprisek9-ms.bin: /lib64/libc.so.6: version `GLIBC_2.27' not found (required by ./x86_64_crb_linux-adventerprisek9-ms.bin)
```

Una solucion no tan destructiva, es descomprimir las librerias de GLIBC 2.28 disponibles para RedHat 8.10 mediante el programa "Red Hat Enterprise Linux Packages", necesitaremos descargar los siguientes paquetes RPM para no tener problemas:
```
glibc-2.28-251.el8_10.22.x86_64.rpm
glibc-all-langpacks-2.28-251.el8_10.22.x86_64.rpm
glibc-common-2.28-251.el8_10.22.x86_64.rpm
glibc-gconv-extra-2.28-251.el8_10.22.x86_64.rpm
```

Entonces debemos enviar las imagenes a la maquina virtual, luego parchear las rutas usando patchelf y este programa les dira a las imagenes que usen GLIBC 2.28 sin modificar el resto de los programas

> Crear carpeta GLIBC
```
mkdir /opt/iou/glibc/
```

> Envia los paquetes RPM descargados en tu pc
```
rsync -Phvar *.rpm root@{server-ip}:/opt/iou/glibc/
```

> Nos vamos a la carpeta de glibc
```
cd /opt/iou/glibc
```

> Descomprimes el primer RPM
```
rpm2cpio glibc-2.28-251.el8_10.22.x86_64.rpm | cpio -idmv
```

> Luego el segundo
```
rpm2cpio glibc-all-langpacks-2.28-251.el8_10.22.x86_64.rpm | cpio -idmv
```

> Finalmente el tercero
```
rpm2cpio glibc-common-2.28-251.el8_10.22.x86_64.rpm | cpio -idmv
```

> Creas la carpeta source
```
mkdir /opt/iou/glibc/source
```

> Mueves los .rpm a la carpeta source
```
mv /opt/iou/glibc/*.rpm /opt/iou/glibc/source
```

> Crea un backup de la imagen original, capaz explota
```
cp /opt/iou/bin/x86_64_crb_linux-adventerprisek9-ms.bin /opt/iou/bin/x86_64_crb_linux-adventerprisek9-ms.bin.back
```

Ahora bien, existen 3 archivos que son importantes
- Interprete: `/opt/iou/glibc/usr/lib64/ld-linux-x86-64.so.2`
- Rpath: `/opt/iou/glibc/usr/lib64`

```
patchelf --set-interpreter /opt/iou/glibc/usr/lib64/ld-linux-x86-64.so.2 --set-rpath /opt/iou/glibc/usr/lib64 x86_64_crb_linux-adventerprisek9-ms.bin
```

> Ejecutamos el parche para la imagen
```
patchelf --set-interpreter /opt/iou/glibc/usr/lib64/ld-linux-x86-64.so.2 --set-rpath /opt/iou/glibc/usr/lib64 /opt/iou/bin/x86_64_crb_linux-adventerprisek9-ms.bin
```

> Ejecutamos el parche para la imagen
```
patchelf --set-interpreter /opt/iou/glibc/usr/lib64/ld-linux-x86-64.so.2 --set-rpath /opt/iou/glibc/usr/lib64 /opt/iou/bin/x86_64_crb_linux_l2-adventerprisek9-ms.bin
```

> Verificamos el cambio del loader
```
readelf -l /opt/iou/bin/x86_64_crb_linux-adventerprisek9-ms.bin | grep interpreter
```

> Verificamos el cambio del loader en L2
```
readelf -l /opt/iou/bin/x86_64_crb_linux_l2-adventerprisek9-ms.bin | grep interpreter
```

> Verificamos el cambio de rpath
```
readelf -d /opt/iou/bin/x86_64_crb_linux-adventerprisek9-ms.bin | grep RUNPATH
```

> Verificamos el cambio de rpath
```
readelf -d /opt/iou/bin/x86_64_crb_linux_l2-adventerprisek9-ms.bin | grep RUNPATH
```


---


---


---
REVISION AVANZANDA DE IMAGENES

```
readelf -a ./x86_64_crb_linux_l2-adventerprisek9-ms.bin | less
```

PARCHEAR INTERPRETER
```
patchelf --set-interpreter /opt/glibc-2.34/lib/ld-linux-x86-64.so.2 ./x86_64_crb_linux_l2-adventerprisek9-ms.bin
```

PARCHEAR RUN
```
patchelf --set-rpath /opt/glibc-2.34/lib:/lib64:/usr/lib64 ./x86_64_crb_linux_l2-adventerprisek9-ms.bin
```

> REVISION PIOLA CON STRACE
```
strace -e openat ./x86_64_crb_linux_l2-adventerprisek9-ms.bin 4
```

> REVISAR ABSOLUTAMENTE TODO CON STRACE
```
strace -f -v -s 1024 -e trace=all ./x86_64_crb_linux_l2-adventerprisek9-ms.bin 4
```

## UEFI EN CENTOS7

GRACIAS:
- https://neurealm.com/blogs/booting-centos-from-uefi/
- https://www.bitbull.ch/wiki/index.php?title=Convert_CentOS7_Legacy_Bios_to_Uefi
- https://access.redhat.com/solutions/3486741

> Reinstala Grub2-common
```
dnf reinstall grub2-common
```

Creo que para instalar UEFI, deberia hacer lo siguiente
- Apagar la maquina
- Hacer otra con el mismo disco
- Encender como UEFI
- Instalar correctamente el grub
```
grub2-install --target=x86_64-efi --efi-directory=/boot/ --bootloader-id=GRUB --modules="tpm" --disable-shim-lock --removable --recheck
```
- Luego volver a la anterior maquina y ver si funcionan ambos

o tambien podria ser simplemente
```
grub2-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=centos --recheck
```


# SOBRE IOU WEB

El archivo NETMAP dentro de un laboratorio no puede llevar el valor 0, el rango es de 1 a 1023

netio??

---
# Que salga a internet
(WIP)
Se puede utilizar
- [Github jlgaddis/iou2net](https://github.com/jlgaddis/iou2net) | [How to connect using iou2net.pl](https://brezular.com/2013/10/09/how-to-connect-iou-to-a-real-cisco-gear-using-iou2net-pl/)
- [Github jlgaddis/ioulive86](https://github.com/jlgaddis/ioulive86)

Aunque es muy complicado y no lo tomaria encerio hasta haber terminado casi todos los puntos

Si quieres utilizar el comando [IOU2NET.pl](https://github.com/jlgaddis/iou2net/) | [Blog](https://brezular.com/2013/10/09/how-to-connect-iou-to-a-real-cisco-gear-using-iou2net-pl/) deberas usar realmente la imagen que sale en la nota

**Note**: Seems that problems are not presented when IOU binary i86bi_linux_l2-ipbasek9-ms.may8-2013-team_track is used. Use this particular IOU binary whenever trunk connection between IOU and a real gear is required.

# BENCHMARKS

Creo que no hay nada mejor que simplemente quejarse de manera tecnica sobre que mi version es mejor 

**IOU WEB 32 Bits ORIGINAL**


**IOU WEB 32 Bits Actualizado**

**IOU WEB 64 Bits**

> Version del Kernel con `uname -a`
```
Linux iou.example.com 6.9.7-1.el7.elrepo.x86_64 #1 SMP PREEMPT_DYNAMIC Thu Jun 27 10:58:15 EDT 2024 x86_64 x86_64 x86_64 GNU/Linux
```

> Demora en encender con `systemd-analyze`
```
[root@iou ~]# systemd-analyze 
Startup finished in 1.037s (kernel) + 2.290s (initrd) + 3.710s (userspace) = 7.038s
```

**Crear Interfaz con `ip nat inside`**
```

```

**Crear llaves SSH de 4096**
> Configuracion:
> 
```

```


# Testear Imagenes
Nota 2222 puerto TCP ; 200 ID-APP
```
./wrapper-linux -m ./i86bi_linux-adventerprisek9-ms -p 2222 -- -e 1 -s 1 200
```

**Conectate al router puerto TCP**
```
telnet localhost 2222
```

**Dejar de correr imagenes**
```
ps -aux | grep wrapper-linux | grep 200 | kill `echo $(cut -d " " -f2)`
```

# Deprecated o Ideas Abandonadas
## Git
> [!TIP] Sitios Recomendados
> [Fuente - Github Gist - zrsmithson - RHEL_Install_git_cmake](https://gist.github.com/zrsmithson/8a1b7923a8f37dcb2e6b12b7e408fd50)

Buena la idea, pero no es necesario realmente

La ultima version disponible es la [2.49](https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.49.0.tar.gz), pero deberia realmente compilarla??

# Parchear Imagenes
Si fallan el test de encendido, posiblemente tendras que parcharlas

**Parchea Imagenes L3**
```
for F in i86bi_linux-*;do bbe -b "/xfcxffx83xc4x0cx85xc0x75x14x8b/:10" -e  "r 7 x90x90" -o $F.x $F;mv $F.x $F;done;chmod +x ./i86bi_linux-*
```

**Parchea Imagenes L2**
```
for F in i86bi_linuxl2*;do bbe -b "/xa1xffx83xc4x0cx85xc0x75x17x8b/:10" -e "r 7 x74" -o $F.x $F;mv $F.x $F;done;chmod +x ./i86bi_linuxl2*
```


# Actualizar Xinha

Se puso raro, y no tiene sentido realmente actualizarlo

## Actualizar Xinha (SUSPENDIDO)
> [!TIP] Lectura Recomendada
> - [Xinha](https://trac.xinha.org/): Editor HTML usado por IOU WEB (usa la version 0.96)
> 	- Posiblemente se pueda actualizar con un drag and drop a la version mas actual, tendria cuidado con la carpeta `lang` y 
> 	- Existen algunos `plugins` perdidos
> 		- `CSS` (Ahora es CSSDropDowns)
> 		- `plugins/ExtendedFileManager` (esta en "unsupported_plugins")
> 		- `plugins/ImageManager` (esta en "unsupported_plugins")
> 		- `plugins/PersistentStorage` (esta en "unsupported_plugins")
> 		- `plugins/PSFixed` (esta en "unsupported_plugins")
> 		- `plugins/PSLocal` (esta en "unsupported_plugins")
> 		- `plugins/PSServer` (esta en "unsupported_plugins")
> 		- `plugins/SpellChecker` (esta en "unsupported_plugins")
> 		- `plugins/UnFormat` (esta en "unsupported_plugins")

Descarga la ultima version de Xinha desde su [pagina oficial](https://trac.xinha.org/trac/DownloadXinha.html), yo usare la version 1.5.6 Full Distribution

> Muevete a la carpeta `~/iou`
```
cd /opt/iou/html
```

> Descarga xinha-1.5.6
```
wget https://s3-us-west-1.amazonaws.com/xinha/releases/xinha-1.5.6.zip
```

> Descomprime el archivo zip, selecciona "`[N]`" para ser conservador
```
unzip xinha-1.5.6.zip
```

> Elimina el zip
```
rm -f xinha-1.5.6.zip
```

> Actualiza SpellChecker (NOOOOOOOOOOOOOOO)
```
rsync -Phvr /opt/iou/html/xinha/unsupported_plugins/SpellChecker /opt/iou/html/xinha/plugins/
```



> [!TIP] Para ahorrar tiempo viendo los errores
> Como recomendacion, abre cada opcion para editar texto para luego ver en el logs, con errores `404` y `403` para solucionarlos y que IOU WEB funcione siempre

> Actualiza SpellChecker.js desde plugins a plugins
```
ln -s /opt/iou/html/xinha/plugins/SpellChecker/SpellChecker.js /opt/iou/html/xinha/plugins/SpellChecker/spell-checker.js
```

> Elimina SpellChecker.js qliao
```
rm -drf /opt/iou/html/xinha/unsupported_plugins/SpellChecker
```

> Actualiza SpellChecker.js desde plugins a unsupported_plugins ?????
```
ln -s /opt/iou/html/xinha/plugins/SpellChecker /opt/iou/html/xinha/unsupported_plugins/SpellChecker 
```
