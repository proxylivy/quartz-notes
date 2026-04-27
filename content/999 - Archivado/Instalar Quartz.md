> [!TIP] Lecturas Recomendadas
> - [Quartz 4](https://quartz.jzhao.xyz/)
> 	- [Setting up your Github repository](https://quartz.jzhao.xyz/setting-up-your-GitHub-repository)
> 	- [Hosting](https://quartz.jzhao.xyz/hosting)

> Clona tu repositorio
```
git clone https://github.com/proxylivy/quartz-notes.git
```

> Agrega el upstream
```
git remote add upstream https://github.com/jackyzha0/quartz.git
```

> Ejecutar `git remote -v` deberia mostrar
```
origin	https://github.com/proxylivy/quartz-notes.git (fetch)
origin	https://github.com/proxylivy/quartz-notes.git (push)
upstream	https://github.com/jackyzha0/quartz.git (fetch)
upstream	https://github.com/jackyzha0/quartz.git (push)
```

> 
```
npx quartz update
```

```
npx quartz sync --push
```

```

```


# Actualizar Quartz

> Ve a la carpeta base de Quartz
```
cd /docker/quartz
```

> [!TIP] Sobre Syncthing
> Recuerda revisar que no tengas ningun documento suelto, que no haya ningun conflicto entre paquetes

> Hago la copia desde rsync a content como usuario root
```
rsync -druLPO --no-times --delete --exclude ".*" /home/containers/syncthing/config/obsidian/ /docker/quartz/content/
```

> Sincronizo tanto mis notas como del repositorio
```
npx quartz sync --push
```

/home/docker/services/syncthing/sync/obsidian/ /home/docker