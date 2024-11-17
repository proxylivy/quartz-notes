Actividad:

Apoyo:
[[extras/Adjuntos/LVM Centos7 - Tagged.pdf]]

Solucion:
Nota$_{1}$: Agrega 1 disco de 5GB
Nota$_{2}$: Orden de desmontaje para evitar errores: `lv > vg > pv > /dev/sdxX > /dev/sdx` 
Revisar si los discos estan correctamente instalados
```
lsblk
```
fdisk
```
fdisk /dev/sdx
n
p
enter
enter
t
1
8e
w
```
actualizar los cambios hechos en la particion
```
udevadm settle
```
revisamos los discos fisicos(Phisical Volume o pv)
```
pvs
```
Crear volumen fisico y su verificacion
```
pvcreate /dev/sdxX
```
```
pvs
```
Crear Grupo de volumen (Volume Group o vg) y su verificacion
```
vgcreate vg_name /dev/sdxX
```
```
pvs
```
```
vgs
```
Crear Volumen Logico (Logical Volume o lv) y verificarlos
Nota1: -L Añade espacio de los pv (M,G)
Nota2: -l Añade espacio de los pv (+%free)
Nota3: -n Permite darle un nombre
```
lvcreate vg_name -L -n lv_name
```
```
lvs
```
Formatear particion logica y se monta automaticamente
```
mkfs.xfs /dev/mapper/vg_name-lv_name
```
Guardar montaje
```

```
