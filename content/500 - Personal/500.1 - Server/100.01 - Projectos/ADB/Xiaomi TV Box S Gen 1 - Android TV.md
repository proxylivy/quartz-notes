# Model Info
- Name: Xiaomi MiBOX S Gen 1
- Model: MIBOX4
- Codename: 
- Plataforma: 
- Hardware Version: 
- MAC
	- Wi-fi: 
	- Bluetooth: 
- Kernel Version: 
- CPU: Cortex-A53 Quad-Core 64 Bits
- GPU: Mali-450
- RAM: 2GB DDR3
- Storage: 8GB eMMC
- OS: Android TV 8.1 | Can be Updated to Android TV 9??
- Wi-Fi: 802.11a/b/g/n/ac
- Bluetooth: 4.0


> [!IMPORTANT] Importante
> Usa [MI Product Authentication](https://www.mi.com/global/verify/#/en/tab/imei) para verificar el IMEI o SN de tu telefono

# ADB
ADB (Android Debug Bridge) is a command-line tool to install, uninstall and debug apps and access the device's shell

> [!IMPORTANT] Note
> ADB does not provide root access by default

## Enable ADB in Android
1. Go to "Settings > About > Click 7 times on the Build text"
2. Go to "Settings > Development Options > Enable Android Debugging"

## Installing ADB on your PC
- On Arch Linux: follow the [Arch Wiki](https://wiki.archlinux.org/title/Android_Debug_Bridge) and install [android-tools](https://archlinux.org/packages/?name=android-tools) package
- On Windows: Download [Platform Tools](https://developer.android.com/tools/releases/platform-tools) from [Android Developer Docs](https://developer.android.com/tools/adb)| [Direct Download (ZIP)](https://dl.google.com/android/repository/platform-tools-latest-windows.zip)

## Useful Commands

> Start the ADB Server
```
adb start-server
```

> Connect via Wi-Fi
```
adb connect device_ip_address:5555
```

> List connected devices
```
adb devices -l
```

> Get info from device
```
adb shell getprop
```

> Install APK
```
adb install -r app.apk
```

> List installed Packages
```
adb shell pm list packages
```

> Enter Shell on the device
```
adb shell
```

> List running processes
```
adb shell ps -ef
```

> Copy from device to PC
```
adb pull /remote/file /local/path
```

> Copy from PC to device
```
adb push /local/file /remote/path
```

> Reboot Device
```
adb reboot
```

> Search for Packages (Linux) | you need [rg](https://github.com/BurntSushi/ripgrep) package
```
adb shell pm list packages | sort | rg "word"
```

> Search for Packages (Windows) | Recommend [Terminal](https://learn.microsoft.com/en-us/windows/terminal/install) + [Powershell](https://github.com/PowerShell/PowerShell)
```
adb shell pm list packages | Sort-Object | Select-String "word"
```

## Debloat Commands
The recommended way to debloat a devices, is following this 3 layer options
1. Uninstall from devices, its can be installed from google play again
2. Disable Packages that cannot be unninstalled from the devices itself
3. Unninstall Package only if you cannot disable or you need to replace with other apps

> Disable Package without installing (Recommended)
```
adb shell pm disable-user --user 0 package.name
```

> Re-enable Disabled Packages
```
adb shell pm enable package.name
```

> UNINSTALL Package (keep data, user 0)
```
adb shell pm uninstall -k --user 0 package.name
```

> Reinstall if uninstall by mistake 
```
adb shell cmd package install-existing package.name
```

---
# Apps
## Recommended Apps to Install
- [Yuliskov/SmartTube](https://github.com/yuliskov/smarttube)
- [Yuliskov/LeanKeyboard](https://github.com/yuliskov/LeanKeyboard)
- [Spocky/Proyectivity Launcher](https://github.com/spocky/miproja1) | [XDA Forums](https://xdaforums.com/t/app-android-tv-projectivy-launcher.4436549/)

---
## Delete from Xiaomi TV BOX
I think none

## Disable from ADB by Package Name
### General Bloat

> Camera (Why you want a camera??)
```
com.android.camera2
```

> Xiaomi Music Player
```
com.google.android.music
```

> Xiaomi Video Player
```
com.google.android.videos
```

> Xioami TV Channel
```
com.mitv.tvhome.atv
```

> Xiaomi Updates
```
com.xiaomi.mitv.updateservice
```

> Adware/Tracking
```
tv.alphonso.alphonso_eula
```

> Xiaomi Analytics Telemetry
```
com.miui.tv.analytics
```

> MiChannel
```
com.mitv.tvhome.michannel
```

> Xiaomi Partner Customizer
```
com.xiaomi.android.tvsetup.partnercustomizer
```

> Xiaomi Services
```
mitv.service
```

> Xiaomi Link Services
```
com.mitv.milinkservice
```

> Xiaomi Telemetry 2
```
com.xiaomi.mitv.res
```

> Xiaomi Downloader
```
com.mitv.download.service
```

> Auto-install Crapware
```
android.autoinstalls.config.xioami.mibox3
```

### Google TV Junk
> [!IMPORTANT] Importante
> Remember to install other launcher before delete Android TV Launcher

> Tv Channels
```
com.google.android.tv
```

> TV Recommendations
```
com.google.android.tvrecommendations
```

> Launcher w/ ads
```
com.google.android.tvlauncher
```

### Input + Keyboard

> Latin Gboard Keyboard
```
com.google.android.inputmethod.latin
```

> Japanese Gboard Input
```
com.google.android.inputmethod.japanese
```

### Functionality Bloat

> Wallpaper changer / Backdrop
```
com.google.android.backdrop
```

> Google Text-to-Speech
```
com.google.android.tts
```

> Google Talkback Accesibility
```
com.google.android.marvin.talkback
```

> Google Voice Recognition
```
com.google.android.speech.pumpkin
```

### Apps Services

> Amazon Prime Video
```
com.amazon.amazonvideo.livingroom
```

> Youtube
```
com.google.android.youtube.tv
```

> Youtube Music
```
com.google.android.youtube.tvmusic
```

> Google App
```
com.google.android.katniss
```

> Calendar
```
com.android.providers.calendar
```

> Contacts
```
com.android.providers.contacts
```

> Dictionary
```
com.android.providers.userdictionary
```

> Feedback
```
com.google.android.feedback
```

> Partnersetup
```
com.google.android.partnersetup
```

> Sync Calendar
```
com.google.android.syncadapters.calendar
```

> Sync Contacts
```
com.google.android.syncadapters.contacts
```

> Bug sender
```
com.google.android.tv.bugreportsender
```

> Search for printers
```
com.android.printspooler
```

> HTML Viewer
```
com.android.htmlviewer
```

> Google Play Games
```
com.google.android.play.games
```

> [!IMPORTANT] Note
> When done, reboot you device with `adb reboot`


## Useful Links
Thanks to
- [Tweakradje - Xiaomi Mi TV Stick](https://sites.google.com/site/tweakradje/devices/xiaomi-mi-tv-stick)
- [XDA Devs - ADB Debloat Thread](https://xdaforums.com/t/how-to-debloat-adb-the-ultimate-adb-debloating-thread-for-the-s20-u-series.4089089/)
- [HTC Mania Forums - Lista Bloatware Samsung](https://www.htcmania.com/showthread.php?t=1604243) 