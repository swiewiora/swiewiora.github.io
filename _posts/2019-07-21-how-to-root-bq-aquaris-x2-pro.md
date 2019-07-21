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

Unfortunately there is no one-click solution, especially for non-rooted devices. Configuration of the whole OS, settings and apps can take a lot of time. Make sure you will backup everything and use more than 1 way of making backup in case of problems. Reserve some time or be ready that your phone will not be fully functional for some time until you fully reconfigure it after wipe.

https://www.xda-developers.com/how-to-backup-android/  
https://www.technobuzz.net/backup-android-apps-data-to-pc/


## 2. Patch boot image

- Download Magisk Manager from Play Store.
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