> [!TIP] Lecturas Recomendadas
> - [Quartz 4](https://quartz.jzhao.xyz/)
> 	- [Setting up your Github repository](https://quartz.jzhao.xyz/setting-up-your-GitHub-repository)
> 	- [Hosting](https://quartz.jzhao.xyz/hosting)

> Configura tu user de Github
```
git config --global user.email "Your-Mail@mail.com"

git config --global user.name "User"
```

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

> Actualiza el repo basado en el upstream
```
git fetch upstream
```

> Haz merge del branch, en este caso v4
```
git merge upstream/v4
```

> Instala los paquetes de npm
```
npm install
```

> Actualiza Quartz
```
npx quartz update
```

> Sincroniza Quartz
```
npx quartz sync
```

# Actualizar Quartz

> Ve a la carpeta base de Quartz
```
cd /home/docker/services/quartz-notes
```

> [!TIP] Sobre Syncthing
> Recuerda revisar que no tengas ningun documento suelto, que no haya ningun conflicto entre paquetes

> Hago la copia desde rsync a content como usuario root
```
rsync -druLPO --no-times --delete --exclude ".*" /home/docker/services/syncthing/sync/obsidian/ /home/docker/services/quartz-notes/content/
```

> Sincronizo tanto mis notas como del repositorio
```
npx quartz sync
```

