# Info
## Descargar
[Onedrive](https://duoccl0-my.sharepoint.com/:w:/g/personal/ga_zunigam_duocuc_cl/Ef4Z9RnTiFxMl5vKxt_5mrQBgZDfm8nezBIsezHbkysH1Q?e=gmLmeh)
## Pasos
[[extras/Adjuntos/1_5_1_Pauta_de_Evaluacion_Sumativa.pdf|Pauta Evaluativa]]
[[extras/Adjuntos/EVALUACIÓN 1 ASY3132.pdf|Evaluacion 1 ASY3132]]

Apoyo:
[[002 - Ordenar/3er Semestre/ASY3132 (ASOE)/Actividades/1.2.2- Uso De Herramientas basicas en Linux (ASOE)|Herramientas Esenciales]]
[[../Apuntes/24-03-2023 (ASOE)|A24-03]]
[[extras/Adjuntos/Procedimientos linux ASY3132.docx.pdf|Mega Guia Profe.pdf]]
[[002 - Ordenar/3er Semestre/ASY3132 (ASOE)/Apuntes/Instalar CentOS (ASOE)|Ayuda Instalacion CentOS]]

Configurar Nombre Hostname
```
hostnamectl set-hostname Server01-Apellido1-Apellido2
```

```
hostname
```

---

*Requerimiento 2: Repo Local*
NOTA: Ir a la configuracion de la maquina virtual, luego en Almacenamiento
agrega en `Controlador: IDE`: el disco de instalacion de centos y darle en `[] CD en vivo`
verificar con `lsblk`, la particion `sr0`
NOTA: Si `/dev/sr0` esta montado en `run/media/blablabla`, desmontar con `umount /dev/sr0`
```
mkdir /repositorio
```

```
mount -t iso9660 -o loop /dev/sr0 /repositorio
```

```
ls -l /repositorio
```

```
mkdir /tmp/repo/
```

```
mv /etc/yum.repos.d/*.repo /tmp/repo/
```

```
yum repolist
```

```
nano /etc/yum.repos.d/local.repo
```
Modifica `local.repo`:
```
[LocalRepo]
name=LocalRepo
baseurl=file:///repositorio
enabled=1
gpgcheck=0
```

```
yum clean all
```

```
yum repolist
```

Instalar desde repositorio local
```
yum install vsftpd
```

```
rm -f /etc/yum.repos.d/local.repo
```

```
mv /tmp/repo/*.repo /etc/yum.repos.d/
```

```
ls -l /etc/yum.repos.d/
```

---

Requerimiento 3: Gestion de usuarios y contraseñas

Crear Grupos
```
groupadd Redes
```

```
groupadd Soporte
```

```
groupadd Administracion -g 1020
```

```
groupadd Desarrollo -g 1030
```

---

Crear Usuarios
```
useradd Rvaldes -c "Ramon Valdes" -s /bin/bash -g Redes -G Soporte
```

```
useradd Frojas -c "Francisca Rojas" -s /bin/bash -g Soporte -G Desarrollo,Redes
```

```
useradd Amontes -c "Alejandro Montes" -s /bin/sh -u 1020 -g Administracion -G Soporte
```

```
useradd Flopez -c "Federico Lopez" -s /bin/sh -u 1030 -g Desarrollo -G Redes
```

---

Configurar Caducidad Cuenta Usuario
```
chage -E 2024-04-30 -I 60 -W 10 Rvaldes
```

```
chage -E 2024-04-30 -I 60 -W 10 Frojas
```

```
chage -E 2024-04-30 -I 60 -W 10 Amontes
```

```
chage -E 2024-04-30 -I 60 -W 10 Flopez
```

---

Requerimiento 4: Gestion de archivos, Carpetas, Permisos

Configurar indicaciones
```
echo "NombreApellido1 NombreApellido2" > /tmp/fileApellido1-Apellido2
```

```
chmod 640 /tmp/fileApellido1-Apellido2
```

```
chown frojas:redes /tmp/fileApellido1-Apellido2
```

```
ls -l /tmp/ | grep file
```

---

Informacion del sistema
```
echo "Tiempo de actividad del servidor:" > InfoSistemaApellido1-Apellido2
```

```
uptime >> InfoSistemaApellido1-Apellido2
```

```
echo "" >> InfoSistemaApellido1-Apellido2
```

```
echo "Version detallada del sistema operativo" >> InfoSistemaApellido1-Apellido2
```

```
uname -a >> InfoSistemaApellido1-Apellido2
```

```
echo "" >> InfoSistemaApellido1-Apellido2
```

```
echo "Particiones y sus capacidades" >> InfoSistemaApellido1-Apellido2
```

```
df -h >> InfoSistemaApellido1-Apellido2
```
---

Requerimiento 5: Configuracion por consola de IP del servidor

Acceder a `Configuracion De la maquina virtual > Red` y poner `Red NAT`
```
IP: 172.16.10/27 (255.255.255.224)
Gateway: 172.16.10.31
DNS 1: 10.20.40.11
DNS 2: 10.20.1.35
```

Se puede hacer desde el `nmtui` :D

```
nmtui
```

Solucion
[[extras/Adjuntos/Documento.pdf]]