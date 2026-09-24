# Info

LVM (Logical Volume Manager) y MD (Multiple Devices) (RAID software), son cosas utiles, pero para uso general un poco molestas, y no se borran simplemente, esta nota temporal es para tener una referencia para eliminarlos jajaja

Comandos desordenados para LVM
```
sudo lvs
sudo vgs
sudo pvs

sudo umount /dev/nombre_vg/nombre_lv

sudo lvremove /dev/nombre_vg/nombre_lv

sudo vgremove nombre_vg

sudo pvremove /dev/sdXn
```

Comandos desordenados para MD
```
cat /proc/mdstat

sudo mdadm --detail --scan

sudo mdadm --stop /dev/mdX

sudo mdadm --zero-superblock /dev/sdXn

sudo mdadm --remove /dev/mdX
```