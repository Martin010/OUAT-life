# Android and Windows Automation

![Bash](https://img.shields.io/badge/Windows-Bash-black.svg) ![Android](https://img.shields.io/badge/Mobile-Android-brightgreen.svg)

This document presents Windows commands to automate the communication between Android devices, Windows and Unity. The language used is Batch and ADB (Android Debug Bridge) commands. To complete the automation, a shortcut is created and linked to a keyboard combination.

## Table of contents
* [Prerequisite](#prerequisite)
* [Syntax](#syntax)
* [Connecting an Android Devices to Windows](#connecting-an-android-devices-to-windows)
* [Installing a file in Android devices from Windows](#installing-a-file-in-android-devices-from-windows)
  * [Step by step with shell](#step-by-step-with-shell)
  * [The ULTIMATE command](#the-ultimate-command)
* [Launching an application in Android devices from Windows](#launching-an-application-in-android-devices-from-windows)
  * [Find the package and the activity](#find-the-package-and-the-activity)
  * [Launch and stop an application](#launch-and-stop-an-application)

## Prerequisite

* [Android SDK](https://android-sdk.fr.uptodown.com/windows) installed
* [USB debugging](https://developer.android.com/studio/debug/dev-options) or [Wireless debugging (Android 11+) or ADB over network enable (Android 10 and lower)](https://developer.android.com/studio/command-line/adb) enabled on the device

## Syntax
  
| Syntax              | Description                         |
| ------------------- | ----------------------------------- |
| *ipadd*               | IP adress                         |
| *local*               | file name to copy                         |
| *remote*              | directory destination             |
| *path*                | path to the apk                   |
| *name.apk*            | your application                  |
| *com.package.name*    | name of the package to process    |

## Connecting Android Devices to Windows

If all prerequisites are completed, Windows should **automatically** detect all Android devices **connected via USB**.

* To display all the devices connected via USB or TCP/IP

```shell
adb devices
```

* To pair a device with **Android 10 or lower** (requires the device setting **ADB over network** enabled)  

```shell
adb connect ipadd
```

* To pair a device with **Android 11 or upper** (requires the device setting **Wireless Debugging** enabled and its **password**)

```shell
adb pair ipadd:port
```

</br>

## Installing an application on Android devices from Windows

### Step by step with shell

* To copy a file **from a specific directory** to a device

```bash
adb -s ipadd push local remote
```

* To copy a file **from a device** to a specific directory

```bash
adb -s ipadd pull local remote
```

* To **install** an .apk in a device from the **shell**

```bash
adb -s ipadd shell pm install path
```

### The ULTIMATE command

* **Push** and **install** at the same time

```bash
adb -s ipadd install -r name.apk
```

```bash
Options
-l: Install the package with forward lock.
-r: Reinstall an existing app, keeping its data.
-t: Allow test APKs to be installed.
-i <INSTALLER_PACKAGE_NAME>: Specify the installer package name.
-s: Install package on the shared mass storage (such as sdcard).
-f: Install package on the internal system memory.
-d: Allow version code downgrade.

uninstall [options] <PACKAGE>   Removes a package from the system.

Options:
-k: Keep the data and cache directories around after package removal.
```

</br>

## Launching an application on Android devices from Windows

### Find the package and the activity

If you already know the name of the package and of the activity to [Launch and stop an application](#launch-and-stop-an-application).

* First step : to find the **package** installed on the device list all the packages present in the device

```bash
adb -s ipadd shell pm list packages
```

*Example*

```bash
> adb -s RF8M828XJEZ shell pm list packages
```

```diff
package:com.linkedin.android
package:com.android.settings
package:com.samsung.app.newtrim
package:com.samsung.android.dsms
package:com.samsung.android.fast
package:com.samsung.android.lool
package:com.samsung.android.app.notes
package:com.sec.android.app.bluetoothtest
package:com.sec.android.sdhms
package:com.samsung.android.app.spage
+ package:com.qiwy.com.ouat.life
package:com.samsung.android.wifi.softap.resources
package:com.samsung.android.samsungpositioning
package:com.android.statementservice
package:com.google.android.as
package:com.google.android.gm
```
> My package is named `com.qiwy.com.ouat.life` in the device.

</br>

* Second step : to find the **package activity** to launch list all the activities published by the package

```bash
adb -s ipadd shell #press Enter
dumpsys package | grep -Eo "^[[:space:]]+[0-9a-f]+[[:space:]]+com.package.name/[^[:space:]]+" | grep -oE "[^[:space:]]+$"
```

*Example*

```bash
> adb -s RF8M828XJEZ shell
> d2s:/ $ dumpsys package | grep -Eo "^[[:space:]]+[0-9a-f]+[[:space:]]+com.qiwy.com.ouat.life/[^[:space:]]+" | grep -oE "[^[:space:]]+$"
```

```diff
com.qiwy.com.ouat.life/com.facebook.CustomTabActivity
com.qiwy.com.ouat.life/com.google.firebase.auth.internal.GenericIdpActivity
com.qiwy.com.ouat.life/com.google.firebase.auth.internal.RecaptchaActivity
+ com.qiwy.com.ouat.life/com.unity3d.player.UnityPlayerActivity
com.qiwy.com.ouat.life/com.facebook.CurrentAccessTokenExpirationBroadcastReceiver
com.qiwy.com.ouat.life/com.unity.androidnotifications.UnityNotificationRestartOnBootReceiver
com.qiwy.com.ouat.life/com.google.firebase.auth.api.fallback.service.FirebaseAuthFallbackService
d2s:/ $
```
> The package `com.qiwy.com.ouat.life` contains the activity `com.qiwy.com.ouat.life/com.unity3d.player.UnityPlayerActivity`. This activity allows to start the game.

### Launch and stop an application

* To launch an application

```bash
adb -s ipadd shell am start -n com.package.name/com.activity.name
```

*Example*

```bash
> adb -s RF8M828XJEZ shell am start -n com.qiwy.com.ouat.life/com.unity3d.player.UnityPlayerActivity
```

</br>

* To stop an application

```bash
adb -s ipadd shell am force-stop com.package.name
```

*Example*

```bash
> adb -s RF8M828XJEZ shell am force-stop com.qiwy.com.ouat.life
```
