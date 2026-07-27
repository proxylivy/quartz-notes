Links:
- https://pve.proxmox.com/wiki/Windows_10_guest_best_practices
- https://pve.proxmox.com/wiki/Windows_VirtIO_Drivers
	- https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/

Utilizare WinterOS Rev15 - Win10 LTSC 2021, debido a que Mauro Cerqueiro hace un muy buen trabajo optimizando sistemas

PRESIONA EN UNIRTE A UN DOMINIO CONCHETUMARE
y de nombre "alumno", y de contraseña Duoc.2025

Y las 3 preguntas de seguridad, las respuestas son "Duoc"

Desactiva todas las caracteristicas de privacidad y asi estar mas limpio

La instalacion limpia pesa 14.1GB

Cuando termine de instalar, saldra una ventana de CMD que reiniciara la sesion

Luego cuando reinicie, levantara otra vez la ventana de CMD y pedira ejecutar sfc scannow y le das que "N" (No) y luego "Enter", si se queda pegado, apreta "Enter"

Desactiva los efectos visuales en "Panel de Control" > "Sistema" > "Conf. Avanzada" > "Rendimiento" y deselecciona todo menos...

Luego ser reiniciara

**Activa Windows**
- https://github.com/PowerShell/PowerShell (Probe 7.5.2.0 y funciono a la primera :D)
- https://massgrave.dev/
	- Abre una ventana de Powershell como administrador y ejecuta `irm https://get.activated.win | iex`, abrira una ventana y activas con HWID `[1]` :D

**Activa Paravirtualizacion**
Descarga la ultima version de [Virtio](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/) (0.1.285-1)

**Continua con la configuracion**

Ahora ve a "Panel de Control" y luego a "Programas" > "Activar o desactivar las caracteristicas de windows" y desactiva todo lo que no importa y le dices que no reinicie

Usa `msconfig` para desactivar programas del inicio

Ejecuta `netplwiz` y desactiva el tick en "Los usuarios deben escribir su nombre y contraseña para usar el equipo", le das en "Aceptar" y luego escribes la contraseña 2 veces para confirmar el cambio

Cambia el tema desde Configuraciones > Personalizacion y utiliza el tema basico para ahorrar RAM

Lo mejor seria probar Arch Linux sobre Virtualbox de una

Borra todos los archivos temporales con `cleanmgr`

DUMP DE COMANDOS
**Con DISM** (abre CMD como admin):
```
dism /online /English /Get-Features /Format:Table
```

> Reduce el tamaño de WinSxS y limpia
```
dism /online /Cleanup-Image /StartComponentCleanup /ResetBase /Defer
```

> Desactiva servicios del sistema
```
sc config "SysMain" start= disabled
sc config "WSearch" start= disabled
sc config "DiagTrack" start= disabled
```

> Desactiva la hibernacion
```
powercfg /h off
```

> Comprime el sistema (Tambien permite la compresion del disco C:\)
```
compact /compactos:always
```

**Optimiza con Herramientas de 3ros**
- https://github.com/ChrisTitusTech/winutil
- https://www.bcuninstaller.com/

Herramientas
- Imagen: [IrfanView](https://www.irfanview.com/)