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

Unfortunately there is no one-click solution, especially for non-rooted devices. If your phone doesn't support TWRP, you're out of luck, there's no more reliable solution than this one. I tried ADB backup but it didn't work well for me. Make sure to create backup of necessary data in many ways in case one will not work.

<https://www.xda-developers.com/how-to-backup-android>  
<https://www.technobuzz.net/backup-android-apps-data-to-pc>

### Android Wear
Every time you wipe your phone you need to wipe your watch too in order to re-pair it with the same device. There's a workaround of [How to Pair Android Wear Watch to New/Same Phone without Factory Resetting](https://www.xda-developers.com/pair-android-wear-without-factory-reset).

## 2. Patch boot image

- Download and install Magisk Manager from [official Github releases](https://github.com/topjohnwu/Magisk/releases).
> Magisk does not have a website! Do not download Magisk from unofficial sources. The only official sources of Magisk are [Github](https://github.com/topjohnwu/Magisk) or [XDA Forum](https://forum.xda-developers.com/apps/magisk/official-magisk-v7-universal-systemless-t3473445)
- Download firmware (version installed on your device), extract boot.img and copy to your device.
- Patch boot.img with Magisk Manager and download it back to PC

## 2. Unlock Bootloader

- Enable Developer Options and enable setting `OEM unlocking` and `USB debugging`
- Connect your phone to PC
- execute the following commands:
```
adb reboot bootloader
fastboot devices
fastboot flashing unlock
fastboot flash boot <<path to boot image>>.img
fastboot reboot
```

> Do not relock device. relock will wipe it again and make impossible applying OTA updates in the future.

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