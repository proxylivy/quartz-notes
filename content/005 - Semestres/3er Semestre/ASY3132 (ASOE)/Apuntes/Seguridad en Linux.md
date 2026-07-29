# Info

Apuntes de [[005 - Semestres/3er Semestre/ASY3132 (ASOE)/ASY3132 (ASOE)|ASY3132 (ASOE)]]

SELinux

ruta de configuracion
```
/etc/selinux/config
```
getenforce
setenforce (0,1)

Confinados -> Recurso del sistema aislado
Desconfinado -> Recurso del sistema libre

---

`firewall-cmd --zone=dmz --add-port=443/tcp --permanent`