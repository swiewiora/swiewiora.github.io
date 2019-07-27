---
layout: post
title:  "How to root BQ Aquaris X2 Pro with Magisk"
date:   2019-07-21 20:00:00 +0200
categories: [android]
---

Here is how I rooted my phone BQ Aquaris X2 Pro 4/64 zangyapro on Android 9 Pie 2.0.1_20190417-1213.  
Windows 10 x64, Android SDK Platform Tools, ConEmu.

### Prerequests:
- [ADB and Fastboot](https://forum.xda-developers.com/showthread.php?t=2588979)
- [BQ USB Drivers and device firmware](https://www.bq.com/en/support/aquaris-x2-pro/support-sheet)

## 1. Backup

> Unlocking bootloader will wipe all user data.

Unfortunately there is no one-click solution, especially for non-rooted devices. If your phone doesn't support TWRP, you're out of luck, there's no more reliable solution than this one. I tried ADB backup but it didn't work well for me. Make sure to create backup of necessary data in many ways in case one will not work.

https://www.xda-developers.com/how-to-backup-android/  
https://www.technobuzz.net/backup-android-apps-data-to-pc/  
**https://forum.xda-developers.com/android/help/adb-fastboot-commands-bootloader-kernel-t3597181**

### Android Wear
Every time you wipe your phone you need to wipe your watch too in order to re-pair it with the same device. There's a workaround of [How to Pair Android Wear Watch to New/Same Phone without Factory Resetting](https://www.xda-developers.com/pair-android-wear-without-factory-reset).

## 2. Create patched boot image

- Download Magisk Manager from Play Store.
- Download firmware (version installed on your device), extract boot.img and copy to your device.
- Patch boot.img with Magisk Manager and download it back to PC

## 2. Unlock Bootloader and flash patched boot image

- Enable Developer Options and enable setting `OEM unlocking` and `USB debugging`
- Connect your phone to PC
- execute the following commands:
```
adb reboot bootloader
fastboot devices
fastboot flashing unlock
<<optional>> fastboot flashing unlock critical
fastboot flash boot <<path to boot image>>.img
fastboot reboot
```

> Do not relock device. relock will wipe it again and make impossible applying OTA updates in the future.

> Unlocking [critical sections](https://source.android.com/devices/bootloader/unlock-trusty) is not needed for root but it may be necessary to unlock them later for flashing custom ROM, recovery or manully installing update.

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

## 5. Applying OTA updates manually

[Official tutorial](https://github.com/topjohnwu/Magisk/blob/master/docs/tutorials.md)
