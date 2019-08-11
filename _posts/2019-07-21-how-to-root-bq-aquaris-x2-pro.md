---
layout: post
title:  "How to root BQ Aquaris X2 Pro with Magisk"
date:   2019-07-21 20:00:00 +0200
categories: [android]
---

This is complete guide for rooting BQ Aquaris X2 Pro 4/64 zangyapro on Android 9 Pie 2.0.1_20190417-1213 based on multiple sources. 
Windows 10 x64, Android SDK Platform Tools, ConEmu.

### Prerequests:
- [ADB and Fastboot](https://forum.xda-developers.com/showthread.php?t=2588979) or Android SDK
- [BQ USB Drivers and device firmware](https://www.bq.com/en/support/aquaris-x2-pro/support-sheet)

## 1. Backup

> Unlocking bootloader will wipe all user data.

Unfortunately there is no one-click solution, especially for non-rooted devices. If your phone doesn't support TWRP, you're out of luck, there's no more reliable solution than this one. ADB Backup is obsolete by Google and may be removed in future Android releases. Make sure to use different ways to create backup of necessary data.

**<https://forum.xda-developers.com/android/help/adb-fastboot-commands-bootloader-kernel-t3597181>**  
<https://www.xda-developers.com/how-to-backup-android>  
<https://www.technobuzz.net/backup-android-apps-data-to-pc>  

### Android Wear
Every time you wipe your phone you need to wipe your watch too in order to re-pair it with the same device. There's a workaround of [How to Pair Android Wear Watch to New/Same Phone without Factory Resetting](https://www.xda-developers.com/pair-android-wear-without-factory-reset):
1. Disable bluetooth on PC and phone paired with watch.
2. Connect watch to Wifi
3. Enable developer settings and debugging over WiFi (PC and watch must be on the same network)
4. Execute the following commands (manually type them to avoid errors in shell)
```
adb connect <IP>:<port>
adb devices
adb shell "pm clear com.google.android.gms && reboot"
[device will reboot]
adb shell "am start -a android.bluetooth.adapter.action.REQUEST_DISCOVERABLE"
```
> If there are problems with connecting to ADB over WiFi check if you have WiFi debugging enabled in ADB on the PC.
5. Open Wear OS app and add the watch from there.

## 2. Create patched boot image

- Download and install Magisk Manager from [official Github releases](https://github.com/topjohnwu/Magisk/releases).
> Magisk does not have a website! Do not download Magisk from unofficial sources. The only official sources of Magisk are [Github](https://github.com/topjohnwu/Magisk) or [XDA Forum](https://forum.xda-developers.com/apps/magisk/official-magisk-v7-universal-systemless-t3473445)
- Download firmware (version installed on your device), extract boot.img and upload to your device.
- Patch boot.img with Magisk Manager and download it back to PC

## 2. Unlock Bootloader and flash patched boot image

- Enable Developer Options and enable setting `OEM unlocking` and `USB debugging`
- Connect your phone to PC
- execute the following commands:
```
adb reboot bootloader
fastboot devices
fastboot flashing unlock
[optional] fastboot flashing unlock critical
fastboot flash boot <path to boot image>.img
fastboot reboot
```

> Do not relock device. relock will wipe it again and make impossible applying OTA updates in the future.

> Unlocking [critical sections](https://source.android.com/devices/bootloader/unlock-trusty) is not needed for root but it may be necessary to unlock them later for flashing custom ROM, recovery or manully installing update. Every flashing lock / unlock will trigger wipe.

## 3. Restart the phone and check if you have root

- The phone is wiped. 
- Restart and check if the root privilidges are granted.

> In case of boot loop, try patching original boot.img file. If it doesn't work, flash the whole image downloaded from BQ (hard reset)

## 4. Configure root and phone settings

- Magisk Manager app will require installing additional libraries.
- Make sure you checked “Preserve force encryption” and “preserve AVB 2.0/dm-verity”.
- Click on Install and select Direct Install (Recommended)
- If you use Google Pay, enable Magisk Hide and use it against GPay and every banking app on your device. Also enable `Hide Magisk` setting.
- **Enable Developer Settings again and disable setting `Automatic system updates`** This is very important, otherwise phone may get automatically updaed and the root will be gone.

### [Enable Google Pay to work with Magisk](https://forum.xda-developers.com/apps/magisk/magisk-google-pay-gms-17-1-22-pie-t3929950)
0. Download a SQL database editor and terminal emulator (ie terminus). 
1. Force close Google Pay. 
2. Open `/data/data/com.google.android.gms/databases/dg.db` in SQL editor
3. Change any value that lists "attest" in the name (first column) to 0 in the third column. Mine was showing a value of 10 in the third column for each of these values. (Column c for sqlite database editor I used)
4. Execute the following commands in terminal
```
su
cd /data/data/com.google.android.gms/databases
chmod 440 dg.db
```
This makes dg.db read only (for owner and group, and no access for world.)
5. Reboot

> When GSM is updated, it may be necessary to execute `chmod 660 dg.db` to allow new keys to be written to the database and then go back and redo all these steps to reset the attestation values back to 0.

## 5. Applying OTA updates manually

[Official tutorial](https://github.com/topjohnwu/Magisk/blob/master/docs/tutorials.md)
