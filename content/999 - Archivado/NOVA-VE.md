
Me llama mucho la atencion, sobre todo por la implementacion limpia

Fuente: https://github.com/fahadysf/nova-ve
Docs: https://docs.nova-ve.com/

Debes hacer una instalacion limpia en un VM con [Ubuntu Server 26.04 LTS](https://ubuntu.com/download/server), luego ejecutar un script como root para instalar NOVA-VE

> Como root
```
curl -fsSL https://raw.githubusercontent.com/fahadysf/nova-ve/main/install.sh | sudo bash
```

Creo que como review, no me gusta las aplicaciones escritas en javascript, si su idea es que no sean modulares, no tiene sentido

El lugar base es `/var/lib/nova-ve`

Luego de la instalacion, se reiniciara, y podras ver tus credenciales en `/home/proxylivy/nova-ve-install-summary.md` y `/etc/nova-ve/backend.env`

**IOURC**

`/var/lib/nova-ve/iourc`


