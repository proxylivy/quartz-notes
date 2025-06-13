# WEAS QUE NO ME FUNCIONARON

## Deprecated from 64 bits

### Parchear Imagenes
Si fallan el test de encendido, posiblemente tendras que parcharlas

Compila bbe
> Para poder parchear las imagenes
```
cd ~/git
git clone https://github.com/hdorio/bbe.git
cd bbe
./configure
make
make install
```

**Parchea Imagenes L3**
```
for F in i86bi_linux-*;do bbe -b "/xfcxffx83xc4x0cx85xc0x75x14x8b/:10" -e  "r 7 x90x90" -o $F.x $F;mv $F.x $F;done;chmod +x ./i86bi_linux-*
```

**Parchea Imagenes L2**
```
for F in i86bi_linuxl2*;do bbe -b "/xa1xffx83xc4x0cx85xc0x75x17x8b/:10" -e "r 7 x74" -o $F.x $F;mv $F.x $F;done;chmod +x ./i86bi_linuxl2*
```

## Testear Imagenes
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


# POSPUESTO DE IOU WEB 64 BITS FUNCIONAL

Esta es una lista de prioridades que pospuse, no estan canceladas, solo que aun no estan integradas a la maquina

## Compilar Programas
Me dio flojera, antes de romper el sistema me gustaria exportar correctamente a otros sistemas y hacer funcionar UEFI

### OpenSSL

https://dev.to/nikolastojilj12/update-openssl-to-3-0-on-centos7-150o
https://openssl-library.org/source/
https://wiki.openssl.org/index.php/Compilation_and_Installation

Aqui si hay que tener miedo uhhh
Usare `3.5 [LTS]` porque mejor volar cerca del sol que nada

> Descarga la fuente de OpenSSL
```
cd /usr/src
wget https://github.com/openssl/openssl/releases/download/openssl-3.5.0/openssl-3.5.0.tar.gz
tar -zxf openssl-3.5.0.tar.gz
rm openssl-3.5.0.tar.gz
```

> Compila xdxd
> `./Configure linux-x86_64 no-shared no-ssl3 no-comp no-zlib --prefix=/opt/openssl-3.5.0`
```
cd /usr/src/openssl-3.5.0
./configure --help
./config
make -j$(nproc)
make test
make clean
make install
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
make
make install
```

## Soporte GLIBC >=2.27
(WIP) debido a que a pesar de que x86_64 L3 funciona a medias, L2 no llega nisiquiera a iniciar

(WIP) Lo otro seria compilar realmente la version 2.27 pero es altamente explosivo y peligroso segun el 99% de internet


> [!TIP] Lectura Recomendada
> - [Download RedHat Linux Packages](https://access.redhat.com/downloads/content/479/ver=/rhel---8/8.10/x86_64/packages)
> - [Github NixOS/patchelf](https://github.com/NixOS/patchelf)

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


## UEFI EN CENTOS7
(WIP)

GRACIAS:
- https://neurealm.com/blogs/booting-centos-from-uefi/
- https://www.bitbull.ch/wiki/index.php?title=Convert_CentOS7_Legacy_Bios_to_Uefi
- https://access.redhat.com/solutions/3486741


> Reinstala Grub2-common
```
dnf reinstall grub2-common
```

Desde http://archive.kernel.org/centos-vault/7.9.2009/os/x86_64/

Se puede ir a EFI/BOOT (http://archive.kernel.org/centos-vault/7.9.2009/os/x86_64/EFI/BOOT/)

Y extraer
[BOOTX64.EFI](http://archive.kernel.org/centos-vault/7.9.2009/os/x86_64/EFI/BOOT/BOOTX64.EFI)


PARECE QUE SE PUEDE DESCARGAR EL EFI BOOT Y CARGARLO EN CENTOS BIOS, QUE GOD

Instale BIOS, y es mas complicado, talvez deba reinstalar todo desde 0 para tener soporte correctamente :D

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
Instantaneo
```

**Crear llaves SSH de 4096**
> Configuracion:
> 
```

```