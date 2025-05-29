# Device Info
- Name: Xiaomi Redmi 12C

> Borrar desde el dispositivo
```
- Billetera de Google
- Block Bast
- Candy Crush Saga
- Encontrar Dispositivo
- Linkedin
- Mi Home
- Google Drive
- Google Files
- Google TV
- Shein
- Tiktok
- Youtube Music
- Xiaomi Community
```

> Forzar Eliminacion
```
adb shell pm uninstall -k --user 0 com.miui.android.fashiongallery
adb shell pm uninstall -k --user 0 com.miui.mediaeditor
adb shell pm uninstall -k --user 0 com.google.android.apps.chromecast.app
adb shell pm uninstall -k --user 0 com.google.android.apps.tachyon
adb shell pm uninstall -k --user 0 cn.wps.xiaomi.abroad.lite
adb shell pm uninstall -k --user 0 com.mi.global.shop
adb shell pm uninstall -k --user 0 com.miui.weather2
adb shell pm uninstall -k --user 0 com.android.chrome
adb shell pm uninstall -k --user 0 com.android.mms
adb shell pm uninstall -k --user 0 com.android.providers.downloads.ui
adb shell pm uninstall -k --user 0 com.google.android.apps.photos
adb shell pm uninstall -k --user 0 com.google.android.apps.safetyhub
adb shell pm uninstall -k --user 0 com.google.android.apps.subscriptions.red
adb shell pm uninstall -k --user 0 com.google.android.gm
adb shell pm uninstall -k --user 0 com.google.android.googlequicksearchbox
adb shell pm uninstall -k --user 0 com.google.android.youtube
adb shell pm uninstall -k --user 0 com.mi.globalbrowser
adb shell pm uninstall -k --user 0 com.mi.globalminusscreen
adb shell pm uninstall -k --user 0 com.miui.analytics
adb shell pm uninstall -k --user 0 com.miui.audiomonitor
adb shell pm uninstall -k --user 0 com.miui.bugreport
adb shell pm uninstall -k --user 0 com.miui.gallery
adb shell pm uninstall -k --user 0 com.miui.miservice
adb shell pm uninstall -k --user 0 com.miui.misound
adb shell pm uninstall -k --user 0 com.miui.miwallpaper
adb shell pm uninstall -k --user 0 com.miui.phrase
adb shell pm uninstall -k --user 0 com.miui.player
adb shell pm uninstall -k --user 0 com.miui.qr
adb shell pm uninstall -k --user 0 com.miui.videoplayer
adb shell pm uninstall -k --user 0 com.xiaomi.aicr
adb shell pm uninstall -k --user 0 com.xiaomi.calendar
adb shell pm uninstall -k --user 0 com.xiaomi.glgm
adb shell pm uninstall -k --user 0 com.xiaomi.mipicks
adb shell pm uninstall -k --user 0 com.miui.fm
adb shell pm uninstall -k --user 0 com.miui.screenshot
```

> Experimental Sistema, Podria fallar
```
adb shell pm uninstall -k --user 0 com.mi.android.globalFileexplorer #Primero instala Zarchiver o Archivos de Google
adb shell pm uninstall -k --user 0 com.miui.home #Instala Otro Launcher como Nova Launcher
```

> Necesitas una SIM para borrar esto?? `Failure [-1000]`
> Cuando tenga sim, simplemente se borran
```
adb shell pm uninstall -k --user 0 com.miui.calculator
adb shell pm uninstall -k --user 0 com.xiaomi.scanner
adb shell pm uninstall -k --user 0 com.android.soundrecorder
adb shell pm uninstall -k --user 0 com.miui.screenrecorder
adb shell pm uninstall -k --user 0 com.miui.notes
adb shell pm uninstall -k --user 0 com.xiaomi.midrop
adb shell pm uninstall -k --user 0 com.miui.weather2
```