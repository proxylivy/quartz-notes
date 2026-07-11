# Descargo de Responsabilidad

> [EVE-NG Docs - Out of EVE-NG Support](https://www.eve-ng.net/index.php/out-of-eve-ng-support/)

Las modificaciones y experimentos descritos en esta nota no forman parte del software oficial de EVE-NG ni cuentan con el respaldo del equipo de desarrollo. Cualquier modificacion realizado en el sistema, incluyendo la sustitucion de binarios, compilaciones o modificacion de compomentes del entorno, se hace con fines de investigacion, aprendizaje y experimentacion.

Si alguno de estos cambios entra en conflicto con los terminos de licencia o con las politicas del proyecto, las responsabilidades recae unicamente en quien decida aplicarlos. El objetivo de este documento es estudiar y comprender el funcionamiento de la plataforma y extender puntos tecnicos, no representar ni sustituir el trabajo del equipo de EVE-NG

De todas formas no afectara a la corrupcion futura de EVE-NG debido a que la version Community Murio. Asi que no tiene soporte de ninguna forma.

# QEMU

## Arregla QEMU Version

EVE-NG tiene un array hardcodeado en `/opt/unetlab/html/includes/apu_nodes.php` entre las lineas 645 y 656 que define que versiones de QEMU aparecen disponibles en la interfaz. Las versiones instaladas en `/opt` que no esten en este array simplemente no aparecen como opcion al configurar un nodo, existen en el disco pero son invisibles para EVE-NG

Antes, el array original solo tenia hasta la version 6.0.0, aunque en `/opt` ya existen versiones mas nuevas instaladas por EVE-NG

```
'list'  => Array ( '1.3.1' => '1.3.1' ,
	'2.0.2' => '2.0.2',
	...
	'6.0.0' => '6.0.0',
	'' => 'tpl'.( isset($p['qemu_version'])?'('.$p['qemu_version'].')':"(default 2.4.0)")));
```

Asi que solo debes agregar las versiones que faltan antes de la linea vacia `''`:
```
'6.0.0' => '6.0.0',
'7.2.9' => '7.2.9',
'8.2.1' => '8.2.1',
'9.2.2' => '9.2.2',
'' => 'tpl'...
```

## Compila nueva version

> [!TIP] Lecturas Recomendadas
> - [Download QEMU - Source](https://www.qemu.org/download/#source)
> - [Qemu - Docs](https://www.qemu.org/docs/master/)
> 	- [Build Env](https://www.qemu.org/docs/master/devel/build-environment.html)
> 	- [Build System](https://www.qemu.org/docs/master/devel/build-system.html)
> - [Xilinx Wiki - Build and Run QEMU from Source Code](https://xilinx-wiki.atlassian.net/wiki/spaces/A/pages/822312999/Building+and+Running+QEMU+from+Source+Code)
> - Blog
> 	- [arcsin2 - Install QEMU on Ubuntu 22.04](https://arcsin2.cloud/en/2023/03/03/Install%20QEMU%20on%20Ubuntu%2022.04/): Instalo QEMU 7.2.0
> 	- [Domenico Mustara - Create Linux VM under Ubuntu](https://domenicomustara.blogspot.com/2023/11/how-to-create-virtual-linux-machine.html): Instalo QEMU 8.2.0

Compilado y probado en Ubuntu 22.04 LTS (Jammy), usando la version de Qemu `11.0.2` sin problemas

> Instala las dependencias de compilacion para Ubuntu 24.04LTS
```
sudo apt install python3-pip python3-tomli autoconf automake bison build-essential cmake flex libasound2-dev libepoxy-dev libfdt-dev libgbm-dev libgcrypt20-dev libglib2.0-dev libgtk-3-dev libpipewire-0.3-dev libpixman-1-dev libpulse-dev libslirp-dev libspice-server-dev libsdl2-dev libsdl2-net-dev libsdl2-image-dev libsdl2-ttf-dev libtool libusb-1.0-0-dev libbz2-dev libcbor-dev liblzo2-dev libncurses-dev libvirglrenderer-dev ninja-build zlib1g zlib1g-dev libaio-dev liburing-dev libseccomp-dev libcap-ng-dev libzstd-dev libcurl4-openssl-dev libnuma-dev libgnutls28-dev libusbredirparser-dev libbpf-dev libssh-dev libcapstone-dev libfuse3-dev libsnappy-dev libiscsi-dev
```

> Crea una carpeta temporal de compilacion y ve alli
```
mkdir /root/test && cd /root/test
```

> Revisa las fuentes disponibles para QEMU y descarga el TAR | [QEMU Download Source](https://www.qemu.org/download/)
```
wget https://download.qemu.org/qemu-11.0.2.tar.xz
```

> Descomprime el TAR
```
tar xvJf qemu-11.0.2.tar.xz
```

> Entra a la carpeta creada
```
cd qemu-11.0.2
```

> Configura QEMU
```
./configure --prefix=/opt/qemu-11.0.2 --target-list=i386-softmmu,x86_64-softmmu --enable-kvm --enable-vhost-net --enable-vnc --enable-vnc-jpeg --enable-spice --enable-spice-protocol --enable-slirp --enable-libusb --enable-virtfs --enable-opengl --enable-virglrenderer --enable-guest-agent --disable-docs
```

> Compila la configuracion de QEMU (Se demoro 1:24min en 20 hilos)
```
make -j$(nproc)
```

> Instala la version compilada en el prefijo `/opt/qemu-11.0.2`
```
make install
```

> Escribe la nueva version en el array de EVE-NG para que aparesca en la interfaz, agrega la linea en `/opt/unetlab/html/includes/apu_nodes.php`
```
'11.0.2' => '11.0.2',
```

> Crea la carpeta en el skeleton (`/opt/unetlab/skeleton/`) para que EVE-NG pueda crear correctamente el punto de montaje dentro de jails para cada nodo
```
mkdir /opt/unetlab/skeleton/opt/qemu-11.0.2
```

EVE-NG ejecuta cada nodo dentro de un chroot jail, QEMU 11.0.2 enlaza librerias mas nuevas que las incluidas en el jail base, por lo que se deben copiar

> Verifica y quita el flag de inmutable a la carpeta
```
lsattr /opt/unetlab/jail/lib/x86_64-linux-gnu/

chattr -R -i /opt/unetlab/jail/lib/x86_64-linux-gnu/
```

> Copia las librerias que faltan (1 por linea)
```
cp /lib/x86_64-linux-gnu/libcapstone.so.4 /opt/unetlab/jail/lib/x86_64-linux-gnu/

cp /lib/x86_64-linux-gnu/libSDL2_image-2.0.so.0 /opt/unetlab/jail/lib/x86_64-linux-gnu/

cp /lib/x86_64-linux-gnu/libcurl.so.4 /opt/unetlab/jail/lib/x86_64-linux-gnu/

cp /lib/x86_64-linux-gnu/libtiff.so.5 /opt/unetlab/jail/lib/x86_64-linux-gnu/

cp /lib/x86_64-linux-gnu/libwebp.so.7 /opt/unetlab/jail/lib/x86_64-linux-gnu/

cp /lib/x86_64-linux-gnu/libjbig.so.0 /opt/unetlab/jail/lib/x86_64-linux-gnu/

cp /lib/x86_64-linux-gnu/libdeflate.so.0 /opt/unetlab/jail/lib/x86_64-linux-gnu/

cp /usr/lib/x86_64-linux-gnu/pulseaudio/libpulsecommon-15.99.so /opt/unetlab/jail/lib/x86_64-linux-gnu/
```

> Arregla los permisos
```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

# Kernel

> [!TIP] Documentacion
> - [EVE-NG Repo - Noble/dists/noble/main/binary-amd64/Packages](https://www.eve-ng.net/noble/dists/noble/main/binary-amd64/Packages)

Fuentes (Lo unico util de aqui es el nuevo kernel)
- https://www.eve-ng.net/noble/ (EVE-NG 7.x)
	- https://www.eve-ng.net/noble/dists/noble/main/binary-amd64/Packages
	- https://www.eve-ng.net/noble/pool/main/
- https://www.eve-ng.net/jammy/ (EVE-NG 6.x)
	- https://www.eve-ng.net/jammy/dists/jammy/main/binary-amd64/Packages
	- https://www.eve-ng.net/jammy/pool/main/

EVE-NG requiere un kernel con soporte para KSM (**K**ernel **S**ame-page **M**erging), una funcion que fusiona paginas de memoria identicas entre multiples VMs, permitiendo que varios nodos compartan la misma base de imagen sin duplicar el uso de RAM. Es lo que hae que levantar 10 nodos con la misma imagen no consuma 10 veces la misma memoria

Un kernel mainline normal puede no incluir los parches especificos de KSM que EVE-NG utiliza (no parecen ser parches publicos hasta donde se).

La solucion es utilizar los kernels compilados por el equipo de EVE-NG para la version freemium de Eve-NG Pro 7.x, viene con KSM habilitado y soporte para Hypr-V, y no tiene prerequisitos que eviten su uso en Ubuntu 22.04LTS

> [!WARNING] Versiones Disponibles
> El equipo de EVE-NG solo mantiene la ultima version en su repositorio. Revisa los repos, copia cada link y los descargas en la maquina utilizando wget
> - [EVE-NG - noble/pool/main/l](https://www.eve-ng.net/noble/pool/main/l/)
> 	- [linux-hv-utils-7.1.1-eve-ksm+](https://www.eve-ng.net/noble/pool/main/l/linux-hv-utils-7.1.1-eve-ksm+/)
> 	- [linux-upstream](https://www.eve-ng.net/noble/pool/main/l/linux-upstream/)

> Crea una carpeta temporal y ve alli
```
mkdir /root/linux && cd /root/linux
```

> Descarga las versiones, corrobora con el callout de arriba
```
wget https://www.eve-ng.net/noble/pool/main/l/linux-hv-utils-7.1.1-eve-ksm+/linux-hv-utils-7.1.1-eve-ksm+_7.1.1-eve-ksm+-1_amd64.deb

wget https://www.eve-ng.net/noble/pool/main/l/linux-upstream/linux-headers-7.1.1-eve-ksm+_7.1.1-eve-ksm-g0fc53eda58f0-1_amd64.deb

wget https://www.eve-ng.net/noble/pool/main/l/linux-upstream/linux-image-7.1.1-eve-ksm+_7.1.1-eve-ksm-g0fc53eda58f0-1_amd64.deb

wget https://www.eve-ng.net/noble/pool/main/l/linux-upstream/linux-libc-dev_7.1.1-eve-ksm-g0fc53eda58f0-1_amd64.deb
```

> Instala los paquetes descargados
```
sudo apt install ./*.deb
```

> Reinicia y deberia eligir automaticamente el nuevo kernel
```
Reboot
```

> Antes
```
uname -r

6.7.5-eveng-6-ksm+
```

> Despues
```
uname -r

7.1.1-eve-ksm+
```

# Cierre

Bueno, no pense que funcionaria, El Kernel y Qemu son el 80% de la base de EVE-NG

Para un futuro, espero lograr hacer lo que hize con [[800 - Extras/Write-Ups/IOU WEB/IOU WEB - Rocky Linux 8.10|IOU WEB - Rocky Linux 8.10]]. La base es del mismo autor, Andrea Dainese, en los tiempos de UNL, es factible

Aunque haya sido el hater n°1, le deseo lo mejor al equipo de EVE-NG, es una herramienta estupenda y util y eso no se borra con simples y llanas malas desiciones. Solo espero que se centren en la comunidad otra vez

Mientras que siga encendiendo hay esperanza

-- Keep Learning --