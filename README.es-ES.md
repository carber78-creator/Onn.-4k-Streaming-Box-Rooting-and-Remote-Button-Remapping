

# [Gen 1 - 2021] Rooting y Remapeo de Botones Remotos de la Onn. 4k Streaming Box + Scripts Útiles para Android TV

#### Descargo de responsabilidad

<b>El contenido de esta publicación es solo para fines educativos. Utilice el contenido de acuerdo con las regulaciones.</b>  


<hr>
</br>
En esta publicación veremos cómo configurar el acceso root y remapear los botones del control remoto en la `Onn. 4k Streaming Box`. 

Utilizaremos los siguientes pasos para configurar la `Onn. 4k Streaming box`. Todos los scripts mencionados en esta publicación pueden encontrarse en mi repositorio de GitHub -> [Rooting y Remapeo de Botones Remotos de la Onn. 4k Streaming Box](https://github.com/amit-raut/Onn.-4k-Streaming-Box-Rooting-and-Remote-Button-Remapping)

### Paso 1: Realice la configuración inicial con la actualización e inicio de sesión de Google

Configure el dispositivo de streaming Onn. 4k de forma normal para configurar el control remoto y el inicio de sesión inicial de Google. 

<hr>

### Paso 2: Habilitar Opciones de desarrollador y depuración USB

Vaya a `Settings -> Device Preferences -> About -> Build` y presione la opción `Build` 7 veces y debería ver un mensaje emergente que dice "You are a developer". Después de habilitar la opción de desarrollador, vaya a `Settings -> Device Preferences -> Developer Options` y habilite `USB debugging`.

Esto debería generar un mensaje emergente para permitir la depuración USB cuando la caja de streaming Onn. 4k esté conectada a un sistema Linux. Seleccione la opción de siempre permitir la depuración para el sistema Linux.

<hr>

### Paso 3: Desbloquear el cargador de arranque

Para desbloquear el bootloader utilice el script [Unblock-Bootloader.sh](https://github.com/amit-raut/Onn.-4k-Streaming-Box-Rooting-and-Remote-Button-Remapping/blob/main/Unlock-Bootloader.sh). Cuando se ejecute el script `Unlock-Bootloader.sh`, realizará las siguientes acciones:

Desbloquea el bootloader con comandos adb

```bash
adb reboot bootloader
fastboot devices
fastboot flashing unlock
fastboot reboot
```

<hr>


### Paso 4: Repita los pasos 1 y 2

Ahora que el bootloader está desbloqueado, repita los pasos 1 y 2 para configurar la caja de streaming Onn. 4k nuevamente. Luego trabajaremos en obtener el acceso root.

<hr>

### Paso 5: Configurar root 

Utilice el script [Set-Root.sh](https://github.com/amit-raut/Onn.-4k-Streaming-Box-Rooting-and-Remote-Button-Remapping/blob/main/Set-Root.sh). Cuando se ejecute el script, realizará las siguientes acciones:

- Descarga el firmware de la caja de streaming Onn. 4k desde ~~[https://download.ods.ninja/Android/firmware/dopinder/dopinder-qt.20210519-factory.zip](https://download.ods.ninja/Android/firmware/dopinder/dopinder-qt.20210519-factory.zip)~~ [https://u.pcloud.link/publink/show?code=XZvdNJ0ZpUYLpHQ4H986VavfRg5RnkphLuIk](https://u.pcloud.link/publink/show?code=XZvdNJ0ZpUYLpHQ4H986VavfRg5RnkphLuIk)
- Valida el md5sum del firmware con `08fa86ced3da6c50aafe8395fce27fe9`
- Extrae el firmware al directorio de trabajo actual
- Descarga la última APK de Magisk
- Envía `boot.img` y `Magisk.apk` a la caja de streaming Onn. 4k mediante `adb push`

<hr>


### Paso 6: Crear un boot.img personalizado con Magisk (Pasos a realizar en la caja de streaming Onn. 4k)

Para crear un archivo boot.img personalizado y obtener root, realice los siguientes pasos en la caja de streaming Onn. 4k:

1. Instale un explorador de archivos desde `Play Store`
2. Abra el explorador de archivos y navegue a `/sdcard/Downloads/`
3. Instale `Magisk.apk`
4. Abra la aplicación Magisk 
   <img src="https://github.com/amit-raut/Onn.-4k-Streaming-Box-Rooting-and-Remote-Button-Remapping/raw/main/Images/m1.png" width="100%">
5. Haga clic en el botón `Install` junto al encabezado Magisk
   <img src="https://github.com/amit-raut/Onn.-4k-Streaming-Box-Rooting-and-Remote-Button-Remapping/raw/main/Images/m2.png" width="100%">
6. Seleccione `/sdcard/Download/boot.img` en el menú `Method` -> `Select and Patch a File`
   <img src="https://github.com/amit-raut/Onn.-4k-Streaming-Box-Rooting-and-Remote-Button-Remapping/raw/main/Images/m3.png" width="100%">
7. Haga clic en el botón `LET'S GO`

Ahora tenemos el archivo `boot.img` parcheado almacenado en `/sdcard/Download/magisk_patched_*****.img`; lo utilizaremos en el siguiente paso para obtener acceso root en la caja de streaming Onn. 4k.

   <img src="https://github.com/amit-raut/Onn.-4k-Streaming-Box-Rooting-and-Remote-Button-Remapping/raw/main/Images/m4.png" width="100%">

<hr>


### Paso 7: Obtener acceso root 

Utilice el script [Get-Root.sh](https://github.com/amit-raut/Onn.-4k-Streaming-Box-Rooting-and-Remote-Button-Remapping/blob/main/Get-Root.sh). Cuando se ejecute el script, realizará las siguientes acciones:

- Extrae el archivo `boot.img` parcheado desde la caja de streaming Onn. 4k
- Escribe la imagen parcheada en la caja de streaming Onn. 4k utilizando los siguientes comandos:

```bash
adb pull /sdcard/Download/$(adb shell ls /sdcard/Download/ | grep magisk_patch) ./magisk-boot.img
adb reboot bootloader
fastboot flash boot magisk-boot.img
fastboot reboot
```

A continuación, el script imprimirá una salida para informarle si es posible obtener acceso root o no. 


<hr>


### Paso 8: Remapear el control remoto de la caja de streaming Onn. 4k 

Utilice el script [Remap-Remote-Buttons.sh](https://github.com/amit-raut/Onn.-4k-Streaming-Box-Rooting-and-Remote-Button-Remapping/blob/main/Remap-Remote-Buttons.sh) para remapear los botones del control remoto. Cuando se ejecute el script, realizará las siguientes acciones:

- Copia la carpeta `onntvremap` a `/sdcard/Download`
- Mueve la carpeta `onntvremap` al directorio `/data/adb/modules/` como usuario root
- Reinicia el dispositivo

Una vez que el dispositivo se vuelva a conectar. No habrá asignación de botones para botones como `YouTube`, `Netflix`, `Disney+`, `HBOMAX`. Instale la aplicación `Button Mapper` desde `Play Store`, conceda acceso superusuario a `Button Mapper` y remapee los botones del control remoto según sus preferencias.


<hr>


### Paso 9: Eliminar bloatware de la caja de streaming Onn. 4k

La caja de streaming Onn. 4k contiene las siguientes aplicaciones preinstaladas:

1. YouTube TV
2. Disney Plus
3. HBO MAX
4. YouTube Music
5. HULU
6. Google Play Games
7. Default TV Launcher
8. Netflix (No se puede eliminar por alguna razón)

Ejecute el script [Remove-Bloatware.sh](https://github.com/amit-raut/Onn.-4k-Streaming-Box-Rooting-and-Remote-Button-Remapping/blob/main/Remove-Bloatware.sh) para eliminar las aplicaciones preinstaladas que no necesite.

<hr>


### Paso 10: Instalar algunas aplicaciones útiles

Utilice [Install-Useful-Apk.sh](https://github.com/amit-raut/Onn.-4k-Streaming-Box-Rooting-and-Remote-Button-Remapping/blob/main/Install-Useful-Apk.sh) para instalar las aplicaciones que le gusten en su Android TV. Aquí hay algunas aplicaciones útiles para Android TV:

1. [SmartTubeNext](https://github.com/yuliskov/SmartTubeNext/releases/tag/14.96)


Espero que haya encontrado esta información útil. 

¡Disfrútenlo~ :)

~ Amit Raut
