# WEAS QUE NO ME FUNCIONARON
# INTENTO 1

## Clona el repositorio de IOU WEB
> Nota: La instalacion va a decir que no lo pillo pero estara instalado, prueba con tirar el ultimo comando 2 veces
```
cd ~/Documentos/git
git clone https://github.com/dainok/iou-web.git
cd iou-web
rpm -Uvh iou-web-1.2.2-23.i386.rpm
```
## Borra el repo de iou-web
> Nota, esto es porque no existe mas en linea, no se necesita
```
rm -f /etc/yum.repos.d/iou-web.repo
```
## Crea Carpetas
```
mkdir -p /opt/iou/bin/
```
## Copia desde iou web al sistema
Esto se hace automaticamente, osea que vaya, no hay que hacer nada, excepto arreglar los .js
```
cd bin
cp * /opt/iou/bin/
cd ../cgi-bin/
cp * /opt/iou/cgi-bin/
cd ../conf
cp sudo.conf /etc/sudoers.d/iou
cp apache.conf /etc/httpd/conf.d/iou.conf
cp logrotate.conf /etc/logrotate.d/iou
```
## Solucionar Rutas .js con HardLink
```
cd /opt/iou/html/xinha/plugins/TableOperations
ln TableOperations.js table-operations.js
cd ../SuperClean
ln SuperClean.js super-clean.js
cd ../Linker
ln Linker.js linker.js
cd ../CharacterMap
ln CharacterMap.js character-map.js
cd ../SpellChecker
ln SpellChecker.js spell-checker.js
cd ../../../css/contextmenu
ln cmenu-gloss-semitrasparent-menu-item-hover.png cmenu-item-gloss-semitransparent-menu-item-hover.png
ln cmenu-xp-bg.gif cmenu-gloss-bg.gif
```
## Reinicia HTTPD y Prueba
```
systemctl stop httpd
systemctl daemon-reload
systemctl start httpd
```

# INTENTO 2

## Arrega el Nuevo Disco

NOTA: No funciona porque el kernel necesita ser instalado, talvez solo me falto reinstalar el kernel desde 6.7

### Tengo que crear una particion para Boot, es lo mejor

Pasos

- Ve a la `Configuracion` de la Maquina Virtual, luego a la seccion de `Almacenamiento`
- Cambia la configuracion de la contraladora a las siguientes
- - Nombre: Sata
    - Tipo: AHCI
    - Cantidad de Puertos: 4
    - Usar cache de I/O de anfitrion: Habilitado
- Elimina el disco IOU WEB-disk002.vdi (swap disk)
- Agrega un nuevo Disco desde la controladora, abre el menu Experto y configuralo con
- - Espacio: 13GB
    - Tipo y Variante de archivo de disco virtual, aqui tienes que elegir segun lo siguiente:
    - - .vdi: Compatibilidad con Virtualbox
        - .vmdk: Compatibilidad con VMWare
        - .qcow: Compatibilidad con Qemu
    - Los otros checkboxes los dejas deseccionados y le das en `Terminar` y Luego en `Seleccionar`
- Guardas Los Cambios
- Enciendes la Maquina

## Instala Paquetes necesarios
```
sudo yum install dosfstools
```

## Crear Particiones

Puedes ver los discos con `lsblk`, en mi caso es `/dev/sdb`  
Pasos:

- Ejecutas `cfdisk /dev/sdb`
- Seleccionas `New`
- Seleccionas `Primary`
- Escribes `500M` + Enter
- Seleccionas `Bootable`
- Seleccionas `Type`
- Escribes: `0C` + Enter
- Seleccionas `New`
- Seleccionas `Primary`
- Escribes `10G` + Enter
- Seleccionas `New`
- Seleccionas `Primary`
- Enter
- Seleccionas `Type`
- Enter + `82` + Enter
- Seleccionas `Write` y escribes yes
- Seleccionas `Quit`

## Crear FS
```
mkfs.vfat -F 32 /dev/sda1
mkfs.ext4 -b 4096 /dev/sdb2
mkswap -c /dev/sdb3
mkswapon /dev/sdb3
```
## Montar Particiones
```
mount /dev/sdb2 /mnt
mkdir /mnt/boot
mount /dev/sda1 -t vfat /mnt/boot
```
## Pasar los archivos de disco 1 al disco 2

NOTA: Ante algun error, ejecuta el comando 2 veces, y revisa que cosas no se sincronizaron, los simlink los puedes ignorar :P
```
rsync -av --exclude='/mnt' --exclude='/proc' --exclude='/sys' --exclude='/dev' --exclude='/tmp' --exclude='/run' --exclude='/lost+found' --exclude='/media' --exclude='/var/run' --exclude='/var/lock' --exclude="/var/log" --exclude="/boot" / /mnt
```

## Modificar /etc/Stab

NOTA: el `UUID` lo puedes obtener de `blkid`  
Modificas `/mnt/etc/fstab`, recuerda eliminar el uuid entero actual de /dev/sda1 y agregas
```
UUID=uuid-disco-1-boot /boot vfat defaults 1 2
UUID=uuid-disco-2-root / ext4 rw,relatime 0 0
UUID=uuid-ram swap swap sw 0 0
```
## Modifica /mnt/boot/ Para que reconosca el nuevo disco como disco de inicio
```
grub-install --root-directory=/mnt /dev/sdb
```
## Mueve los archivos necesarios para partir
```
rsync /boot/vmlinuz* /mnt/boot/
rsync /boot/initramfs* /mnt/boot/
rsync /boot/grub/splash.xpm.gz /mnt/boot/grub/
```
## Modifica /mnt/boot/grub/device.map

deberias tener algo configurado asi
```
(hd0)	/dev/sdb
```
## Escribe /mnt/boot/grub/grub.conf

la ruta aboluta desde el otro disco son

- /mnt/boot/vmlinuz-2.6.32-573.el6.i686
- /mnt/boot/initramfs-2.6.32-573.el6.i686.img
```
default=0
timeout=3
splashimage=(hd0,0)/grub/splash.xpm.gz
hiddenmenu

title CentOS (2.6.32-573.el6.i686)
        root (hd0,0)
        kernel /vmlinuz-2.6.32-573.el6.i686 ro root=/dev/sdb1 rd_NO_LUKS rd_NO_LVM rd_NO_MD rd_NO_DM rd_NO_DMraid loglevel=3 rhgb quiet audit=0
        initrd /initramfs-2.6.32-573.el6.i686.img
```

## Copia grub.conf a menu.lst

no se porque pero no deja hacer un hardlink
```
cp /mnt/boot/grub/grub.conf /mnt/boot/grub/menu.lst
```

## Revisa Antes de Desmontar

Pasos:

- `/mnt/etc/fstab` tiene los UUID Correctos, compara con `blkid`
- `mnt/boot/`este `vmlinux` e `initramfs` y que esten bien configurados dentro de `/mnt/boot/grub`, si las rutas no coinciden, entonces no hay adyacencia
- Revisa los permisos de `grub.conf` con `ls -lha /mnt/boot/grub/`

## Desmontar Disco
```
cd
umount -r /mnt
```

## Reiniciar y TOCAR MADERA CONCHETUMARE AAAAAAAAAAAAAA
```
poweroff
```

## Quita el disco y deja solo el de 12GB

Pasos:

- Ve a las `Configuraciones de Virtualbox`, luego a la seccion de `Almacenamiento`
- Selecciona el disco `IOU WEB-disk001.vdi` y eliminalo

## Si Prende, significa que funiono

Revisa las particiones para ver si funcionan **correctamente**

```
free -hm
blkid
lsblk
ls /etc/fstab
```