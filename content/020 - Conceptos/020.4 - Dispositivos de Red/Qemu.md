# Info
[KVM/Qemu](https://www.qemu.org/) es un emulador y virtualizador de maquinas Open-Source con un rendimiento casi nativo, usa librerias asociadas como [XEN](https://xenproject.org/) y [libvirt](https://libvirt.org/)


Tiene una [wiki](https://wiki.qemu.org) con mas informacion

Convertir discos de Virtualbox o VmWare(VDI, VHD, VHDX) a qemu(Raw o Qcow2)
- [KevinDiaz - Running VMware Images in QEMU](https://www.kevindiaz.dev/blog/running-vmware-images-in-qemu.html)
- [Mario Fischer - Convert VM from OVA to QCOW2 and run on QEMU/KVM](https://blog.mcfisch.com/virtualization/Convert-VM-from-OVA-to-QCOW2-and-run-on-QEMU-KVM/)
- [Dannyda - How to use QEMU img command to convert between vmdk, raw, qcow2, vdi...](https://dannyda.com/2020/06/25/how-to-use-qemu-img-command-to-convert-between-vmdk-raw-qcow2-vdi-vhd-vhdx-formats-disk-images-qemu-img-create-snapshot-resize-etc/)

1. Extrae el .OVA
```
tar -xvf VM.ova
```

2. Convierte el archivo desde vmdk a QCOW2 y comprime la imagen
```
qemu-img convert -c -f vmdk -O qcow2 vm-disk.vmdk vm-disk.qcow2
```

3. Puedes comprimirlo aun mas con
```
virt-sparsify vm-disk.qcow2 --compress vm-disk.qcow2
```


Host Only
[Source1](https://kevrocks67.github.io/blog/qemu-host-only-networking.html)

Dejo como nota aqui que la contraseña de Win7 ENSP es "12345"