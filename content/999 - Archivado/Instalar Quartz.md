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

> Haz merge del branch, en este caso v5
```
git merge upstream/v5
```

> Instala los paquetes de npm
```
npm i
```

> Crea Quartz, selecciona Obsidian
```
npx quartz create
```

> Si un plugin fallo, reinstentalo (Bueno, hasta que funcione eso si...)
```
npx quartz plugin install
```

> En caso de que haya fallado varias veces, puedes instalar desde el config
```
npx quartz plugin install --from-config
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

# Version 5

> [!TIP] Lecturas Recomendadas
> - https://quartz.jzhao.xyz/plugins/
> - https://quartz.jzhao.xyz/cli/plugin
> - https://quartz.jzhao.xyz/troubleshooting
> - https://quartz.jzhao.xyz/getting-started/upgrading

27/Jul/2026: Tuve que cambiar los `@` por `github:` para que pudiera descargar los plugins correctamente

> En serio???
```
git fetch upstream v5

git checkout -b v5 upstream/v5

git reset --hard upstream/v5
```

> FIX TEMPORAL: Mueve de `@` a `github`
```
sed -i 's|"@quartz/|"github:quartz/|g' quartz.config.yaml
```

> FIX TEMPORAL: Modifica el repositorio de `quartz-fonts` a `fonts` | [Fuente](https://github.com/quartz-community/fonts)
```
micro /quartz.config.yaml
```

> FIX TEMPORAL: Agrega `#main` al final de `quartz-themes/core`

> Instala los plugins
```
npx quartz plugin install --from-config
```

> Actualiza los archivos
```
npx quartz upgrade
```

> Actualiza los plugins
```
npx quartz plugin install --latest
```

> Revisa los que fallaron, en mi caso
> - syntax-hightlight
> - created-modified-date
> - obsidian-flavored-markdown

> Ve a cada carpeta dentro de `.quartz/plugins/` y ejecuta dentro de la carpeta `npm run build`

> Recuerda volver a `quartz` como raiz base

> Modifica `quartz.config.yaml` con tus valores
```
pageTitle: Proxylivy Notes
analytics:
  provider: null
priority:
  - git
  - frontmatter
  - filesystem
options:
  repo: proxylivy/quartz-notes
  repoId: R_kgDONQhebA
  category: Announcements
  categoryId: DIC_kwDONQhebM4CoJNB
  lang: en
options:
  links:
    Github: https://github.com/proxylivy/quartz-notes
    GitHub Jacky Source: https://github.com/jackyzha0/quartz
    Linktree: https://littlelink.proxylivy.work
```

> Elimina el Index por defecto
```
rm content/index.md
```

> Sincroniza las notas
```
rsync -druLPO --no-times --delete --exclude ".*" /home/docker/services/syncthing/sync/obsidian/ /home/docker/services/quartz-notes/content/
```

- https://github.com/saberzero1/quartz-themes
	- https://github.com/saberzero1/quartz-themes#supported-themes

> Instala temas
```
npm install @quartz-themes/catppuccin @quartz-themes/rose-pine-minimal @quartz-themes/minimal @quartz-themes/half-life @quartz-themes/material-gruvbox @quartz-themes/monokai-filtersun-spectrum @quartz-themes/muted-blue @quartz-themes/neovim @quartz-themes/nightfox @quartz-themes/notation @quartz-themes/obsidian-nord @quartz-themes/poimandres @quartz-themes/pomme-notes @quartz-themes/praxis @quartz-themes/royal-velvet @quartz-themes/serika @quartz-themes/termina
```

> Modifica el config para cambiar el theme de core de default a minimal? o rose-pine

> Sincroniza con Github
```
npx quartz sync
```

XD
```
git clone https://github.com/quartz-themes/core.git
Clonando en 'core'...
remote: Enumerating objects: 79, done.
remote: Counting objects: 100% (79/79), done.
remote: Compressing objects: 100% (58/58), done.
remote: Total 79 (delta 32), reused 61 (delta 14), pack-reused 0 (from 0)
Recibiendo objetos: 100% (79/79), 95.27 KiB | 348.00 KiB/s, listo.
Resolviendo deltas: 100% (32/32), listo.
```