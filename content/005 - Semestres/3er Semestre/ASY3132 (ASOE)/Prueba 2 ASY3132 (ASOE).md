# Info
## Descargar
- [Onedrive](https://duoccl0-my.sharepoint.com/:w:/g/personal/ga_zunigam_duocuc_cl/EUamS_P1HVFKmXgbVCahrR0Bnadf-kvw-xpTH8X-LiYS8Q?e=0qkX5C)
## Pasos
Agregue 03 Disco duro SCSI de 10 GB a la Máquina Virtual
Ir a configuracion de la maquina virtual > Configuracion > Almacenamiento > Controlador: Xxxx

Crear carpetas
```
mkdir -p /montados/disco1 /montados/disco2 /montados/LV/Fotos /montados/LV/Musica
```

Crear particion de 5GB
```
fdisk /dev/sdb
```
```
m
n
p
enter
+5G
w
```
Configurar particion ext4 5GB
```
mkfs.ext4 /dev/sdb1
```
Montar disco
```
mount dev/sdb1 /montados/disco1
```
revisar discos (Extraer UUID)
```
lsblk
blkid
```
Modificar /etc/fstab
```
EDITOR /etc/fstab
	dev/sdb1 /montados/disco1 ext4 defaults 0 0
```
Hacer Particion 3GB
```
fdisk /dev/sdc
```
Teclas para Fdisk
```
m
n
p
enter
+3G
w
```
Crear fs
```
mkfs.ext4 /dev/sdc1
```
Montar disco
```
mount /dev/sdc1 /montados/disco2
```
revisar discos (Extraer UUID)
```
EDITOR /etc/fstab
	/dev/sdc1 /montados/disco2 ext4 defaults 0 0
```

```
fdisk /dev/sdd
n
p
enter
enter
t
1
8e
w
```

```
udevadm settle
```

LVM
```
pvcreate /dev/sdd1
```

```
vgcreate VG_FINAL /dev/sdd1
```

```
lvcreate VG_FINAL -L 15G -n LV_Fotos
```

```
lvcreate VG_FINAL -l +100%free -n LV_Musica
```

```
mkfs.ext4 /dev/mapper/VG_FINAL-LV_Fotos
```

```
mkfs.ext4 /dev/mapper/VG_FINAL-LV_Musica
```

```
mount /dev/mapper/VG_FINAL-LV_Fotos /montados/LVFotos
```

```
mount /dev/mapper/VG_FINAL_LV_Musica /montados/LVMusica
```

Configurar fstab (UUID)
```

```