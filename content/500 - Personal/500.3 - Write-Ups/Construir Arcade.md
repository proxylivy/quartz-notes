# Info
## Arcade Fisica
No he intentado, pero aqui hay guias interesantes
- [Autodesk Instructables - vigothecarpathian - A Super Easy Arcade Machine From 1 Sheet of Plywood](https://www.instructables.com/A-Super-Easy-Arcade-Machine-from-1-Sheet-of-Plywoo/)
- [Build a Bartop Arcade Site](https://www.buildabartoparcade.com/)
- [Xataka Blog - Como Montarte una maquina arcade desde cero](https://www.xataka.com/makers/como-montarte-una-maquina-arcade-desde-cero)

## Software
Existen diferentes Sistemas Operativos para elegir
- [Retropie](https://retropie.org.uk/)
- [Recalbox](https://www.recalbox.com/)
- [Lakka](https://www.lakka.tv/)
- [Batocera](https://batocera.org/)
- Entre muchos otros

Yo elijo [Batocera](https://batocera.org/), debido a que hay un español que hace un repack especatular llamado MultiBOB del cual facilmente puedes extraer el pack de 700GB de roms ya clasificadas y jugar juegos de la mejor calidad, puedes revisar su canal de [Telegram - Actualizaciones](t.me/BOBcera) o [Telegram - Chat](t.me/BOB_retropie_windows_dudas) o [Telegraph - MultiBob antes BOBcera 04 01](https://telegra.ph/MultiBOB-antes-BOBcera-04-01), Incluso tiene su propia [wiki](https://wiki.batocera.org)

- [Youtube - Turn into a Retro Machine](https://youtu.be/yDCNZ8MH84k?si=MC480axuxJdVLEVn)

0. Configura la Bios para que este lo maximo configurado y sea compatible con Batocera, tambien actualizala si estas con windows y formatea el disco interno a una sola particion de disco entero en EXT4.
1. Descarga [Ventoy](https://www.ventoy.net/en/download.html) y [Batocera Linux](https://batocera.org/download)
2. Mueve `batocera-x86_64-version-YYYYMMDD.img` al pendrive de 16GB previamente formateado con Ventoy
3. Inicia el PC desde el USB para que cargue Batocera
4. Siguiendo la [Guia de Instalacion](https://wiki.batocera.org/install_batocera#install_batocera_from_batocera), instala Batocera en el disco interno para no necesitas el USB | [Mirror](https://mirrors.o2switch.fr/batocera/x86_64/) y [Guia de actualizacion](https://wiki.batocera.org/upgrade_manually#upgrading_downgrading_batocera)
	- Manten `Espacio` y ve al menu `System Settings`
	- Ve a `Install Batocera on a new Disk` (Casi al final en la seccion `Storage`)
	- Selecciona el disco interno en `Target Device`
	- Selecciona `X86_64` en Target Architecture
	- Selecciona `Are you Sure?` y presiona `Install`
5. Siguiendo la [Guia de ES](https://wiki.batocera.org/emulationstation_overview) Abre el menu con `[Start]` o manteniendo el Espacio
	- Conecta a Internet desde el menu `Networks Settings`
		- Habilita el Modulo Wifi
		- Ingresa `SSID` y `WIFI KEY` y Dale a `Back`, entra al menu otra vez y revisa si tienes una IP configurada
6. Siguiendo la [Guia para agregar juegos](https://wiki.batocera.org/add_games_bios)
	- Ingresa mediante SMB a `smb://BATOCERA.local/share` o `smb://{batocera-ip}/share`
		- La configuracion de acceso
			- Nombre de usuario: `root`
			- Dominio: `WORKGROUP`
			- Contraseña: `linux`
	- Mueve los archivos de roms a `/share/roms`
	- Mueve los archivos de bios a `/share/bios`
7. Siguiendo la [Guia para acceder via ssh](https://wiki.batocera.org/access_the_batocera_via_ssh)
	- Acceder con las credenciales
		- User: `root`
		- IP: `{ip-batocera}`
		- pass: `linux`

Anotar los componentes del PC
- CPU: Intel I7-6700 | [Benchmark](https://www.cpubenchmark.net/cpu_lookup.php?cpu=Intel+Core+i7-6700+%40+3.40GHz&id=2598) -> 8.000
- Cantidad Almacenamiento: 1TB
- Motherboard:
	- Tipo de Bios y Configuracion (BIOS/UEFI): UEFI
	- Version Bios Actual: No2 Ver. 02.58 07/28/2022
	- Ultima Bios: No2 Ver. 02.60 Rev.A | [Link](https://support.hp.com/es-es/drivers/hp-prodesk-600-g2-small-form-factor-pc/7633319) | [Link2](https://support.hp.com/es-es/drivers/swdetails/hp-prodesk-600-g2-small-form-factor-pc/7633319/swItemId/vc-305248-1)
- Ram: 16GB

Scrapear contenido para que se vea bonito
- Paginas web
	- [Guia para Scrape](https://wiki.batocera.org/scrape_from)
	- [Arcadeitalia](http://adb.arcadeitalia.net/)
	- [TheGamesDB](https://thegamesdb.net/)
	- [ScreenScraper](https://www.screenscraper.fr/)
- Pasos
	1. Aprender a usar ARRM | [Pagina](http://jujuvincebros.fr/hard-soft/arrm-gamelist-roms-manager-scraper) y [Pagina2](http://jujuvincebros.fr/telechargements2/file/10-arrm-another-recalbox-roms-manager)


Modificar el Tema
- Usando [anthonycaccese/art-book-next-es](https://github.com/anthonycaccese/art-book-next-es)
- La configuracion esta en un archivo xml en `/userdata/theme-customizations/art-book-next/`
- Crear Caratulas para los sistemas Custom y Custom Full, pero el mejor es NOIR
- Debajo de `_inc>systems` modifica los valores de Consola con las fotos correspondientes
	- Por ejemplo las consolas que tiene el Benja son
		- 


Problemas
- El televisor de la arcade es de mala calidad, por lo que deberia ser cambiado a uno IPS que permita mas brillo y con una cobertura mate anti-brillo para poder ver a contraluz, no se ve nada la verdad

- Falta Scrapear Info, se necesita una cuenta y mas cosas, pero se vera bonito

- Configuracion especifica de consolas como PS2 (Fix 16:9), PS1 y FBNEO entre otras

- Faltan Bios y Roms, bueno, si las descargare :D
	- Ayuda
		- Full BIOS SET for batocera v40: Via [WebArchive](https://archive.org/details/full-pack-bios-batocera-v-40-minicaketv) or [Theminicake](https://theminicaketv.fr/PACK-BIOS-BATOCERA.htm)
		- Bios para [DSi via Myrient](https://myrient.erista.me/files/No-Intro/Nintendo%20-%20Nintendo%20DSi%20%28Decrypted%29/)


# Extra
## HandHeld R36S
Primero que nada, para comprar hay que seguir la guia de recomendacion

Lo segundo es revisar si luego de comprar la consola, es original
> Es decir, si tratas de poner de cero Arkos en una de estas consolas quedarás sólo con una luz roja parpadeando y nunca llegará a funcionar.
> - [Fuente](https://retrocool.cl/2024/09/16/consola-r36s-y-la-guerra-de-los-clones-emuelec/)

Si la consola es Clon, leer esto
- La mayoria de pasos estan en [Blog - Retrocool - Consolas R36S y la guerra](https://retrocool.cl/2024/09/16/consola-r36s-y-la-guerra-de-los-clones-emuelec/) y [Reddit Comentario - Difference between Original and Clone](https://www.reddit.com/r/R36S/comments/1fafgq0/comment/llsl7bw/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)

[Handheld - R36s Tutorials](https://handhelds.miraheze.org/wiki/R36S_Guides_%E2%80%90_Tutorials_%E2%80%90_How%E2%80%90To%27s) tienen mucha buena info
- En caso de no tener SD
	- [Github - Aeolus/R36S DTB](https://github.com/AeolusUX/R36S-DTB) - Recomiendan `rk3326-r35s-linux.dtb` y `boot.ini`
	- [Mediafire - Panel 4 (V5) Sound and FN button Fix](https://www.mediafire.com/folder/rgnojaz3229u1/Panel_4_(V5)_AmberELEC_sound_%2B_FN_button_fix)
	- [Github - tech4bot/r35s New Displays](https://github.com/tech4bot/r35s/tree/main/new_displays)

- [Handheld - R36s Custom Firmware](https://handhelds.miraheze.org/wiki/R36S_Custom_Firmware)
	- [Github AeolusUX/ArkOS-R3Xs a.k.a ArkOS Community Edition](https://github.com/AeolusUX/ArkOS-R3XS) | [Wiki](https://github.com/christianhaitian/arkos/wiki)
	- [Reddit Post - ArkOS Image](https://www.reddit.com/r/SBCGaming/comments/18kztju/r36s_arkos_image_12152023/)
	- [Github AmberELEC/AmberELEC Releases](https://github.com/AmberELEC/AmberELEC-prerelease/releases/)

Instalacion
1. Descarga ArkOS-R3Xs desde Github
2. 


