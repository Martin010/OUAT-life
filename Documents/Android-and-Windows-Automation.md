# Android and Windows Automation

![Bash](https://img.shields.io/badge/Windows-Bash-black.svg) ![Android](https://img.shields.io/badge/Mobile-Android-brightgreen.svg)

This document presents Windows commands to automate the communication between Android devices, Windows and Unity. The language used is Batch and ADB (Android Debug Bridge) commands. To complete the automatisation, a shortcut is created and linked to a keyboard combination.

## Prerequisite

* [Android SDK](https://android-sdk.fr.uptodown.com/windows) installed
* [USB debugging](https://developer.android.com/studio/debug/dev-options) or [Wireless debugging (Android 11+) or ADB over network enable (Android 10 and lower)](https://developer.android.com/studio/command-line/adb) enabled on the device

## Syntax

| Syntax              | Description                         |
| ------------------- | ----------------------------------- |
| *ipadd*               | IP adress                         |
| *local*               | file name                         |
| *remote*              | directory destination             |
| *path*                | path to the apk                   |
| *name.apk*            | your application                  |
| *com.package.name*    | name of the package to process    |


## ADB command to connect an Android Devices with Windows

If all the prerequisite are completed, Windows should **automatically** detect all Android devices **connected via USB**.

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

## ADB command to install a file in Android Devices from Windows

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

## ADB command for launch an application in Android devices from Windows

### The package and the activity

* First step : find the package in the device**Push** and **install** at the same time

```bash
adb -s ipadd install -r name.apk
```
