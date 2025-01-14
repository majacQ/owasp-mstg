---
title: Xposed
platform: android
source: https://github.com/ElderDrivers/EdXposed
---

> Xposed does not work on Android 9 (API level 28). However, it was unofficially ported in 2019 under the name EdXposed, supporting Android 8-10 (API level 26 till 29). You can find the code and usage examples at [EdXposed](https://github.com/ElderDrivers/EdXposed "EdXposed") Github repo.

[Xposed](https://f-droid.org/de/packages/de.robv.android.xposed.installer/ "Xposed Installer") is a framework that allows to modify the system or application aspect and behavior at runtime, without modifying any Android application package (APK) or re-flashing. Technically, it is an extended version of Zygote that exports APIs for running Java code when a new process is started. Running Java code in the context of the newly instantiated app makes it possible to resolve, hook, and override Java methods belonging to the app. Xposed uses [reflection](https://docs.oracle.com/javase/tutorial/reflect/ "Reflection Tutorial") to examine and modify the running app. Changes are applied in memory and persist only during the process' runtime since the application binaries are not modified.

To use Xposed, you need to first install the Xposed framework on a rooted device as explained on [XDA-Developers Xposed framework hub](https://www.xda-developers.com/xposed-framework-hub/ "Xposed framework hub from XDA"). Modules can be installed through the Xposed Installer app, and they can be toggled on and off through the GUI.

Note: given that a plain installation of the Xposed framework is easily detected with SafetyNet, we recommend using Magisk to install Xposed. This way, applications with SafetyNet attestation should have a higher chance of being testable with Xposed modules.

Xposed has been compared to Frida. When you run Frida server on a rooted device, you will end up with a similarly effective setup. Both frameworks deliver a lot of value when you want to do dynamic instrumentation. When Frida crashes the app, you can try something similar with Xposed. Next, similar to the abundance of Frida scripts, you can easily use one of the many modules that come with Xposed, such as the earlier discussed module to bypass SSL pinning ([JustTrustMe](https://github.com/Fuzion24/JustTrustMe "JustTrustMe") and [SSLUnpinning](https://github.com/ac-pm/SSLUnpinning_Xposed "SSL Unpinning")). Xposed includes other modules, such as [Inspeckage](https://github.com/ac-pm/Inspeckage "Inspeckage") which allow you to do more in depth application testing as well. On top of that, you can create your own modules as well to patch often used security mechanisms of Android applications.

Xposed can also be installed on an emulator through the following script:

```bash
#!/bin/sh
echo "Start your emulator with 'emulator -avd NAMEOFX86A8.0 -writable-system -selinux permissive -wipe-data'"
adb root && adb remount
adb install SuperSU\ v2.79.apk #binary can be downloaded from http://www.supersu.com/download
adb push root_avd-master/SuperSU/x86/su /system/xbin/su
adb shell chmod 0755 /system/xbin/su
adb shell setenforce 0
adb shell su --install
adb shell su --daemon&
adb push busybox /data/busybox #binary can be downloaded from https://busybox.net/
# adb shell "mount -o remount,rw /system && mv /data/busybox /system/bin/busybox && chmod 755 /system/bin/busybox && /system/bin/busybox --install /system/bin"
adb shell chmod 755 /data/busybox
adb shell 'sh -c "./data/busybox --install /data"'
adb shell 'sh -c "mkdir /data/xposed"'
adb push xposed8.zip /data/xposed/xposed.zip #can be downloaded from https://dl-xda.xposed.info/framework/
adb shell chmod 0755 /data/xposed
adb shell 'sh -c "./data/unzip /data/xposed/xposed.zip -d /data/xposed/"'
adb shell 'sh -c "cp /data/xposed/xposed/META-INF/com/google/android/*.* /data/xposed/xposed/"'
echo "Now adb shell and do 'su', next: go to ./data/xposed/xposed, make flash-script.sh executable and run it in that directory after running SUperSU"
echo "Next, restart emulator"
echo "Next, adb install XposedInstaller_3.1.5.apk"
echo "Next, run installer and then adb reboot"
echo "Want to use it again? Start your emulator with 'emulator -avd NAMEOFX86A8.0 -writable-system -selinux permissive'"
```
