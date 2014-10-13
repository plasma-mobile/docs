How to flash Plasma Phone on Nexus 5
====================================

Useful information can be found here:

* https://wiki.merproject.org/wiki/Building_Sailfish_OS_for_Nexus_5
* https://wiki.merproject.org/wiki/Adaptations/libhybris/Install_SailfishOS_for_hammerhead
* http://releases.sailfishos.org/sfa-ea/2014-07-21_SailfishOSHardwareAdaptationDevelopmentKit.pdf
* http://schier.co/post/how-to-root-nexus-5-in-ubuntu-linux

## Update Android

Update to the latest version.

## Enable developer mode and USB debugging

* Go to settings on your phone
* Go to "About Phone"
* Tap "Built Number" a bunch of times
* Go back
* Go to "Developer Options"
* Check "USB Debugging"
* Click "OK"

## Configure Linux

Install Android tools.

Connect the device and type:

```sh
lsusb
```

an output like this will come out:

```sh
Bus 004 Device 010: ID 18d1:4ee1 Google Inc. Nexus 4 / 10
```

18d1 is the vendor id, 4ee1 is the produce id.

Numbers may change with other devices.

Create /etc/udev/rules.d/51-android.rules with:

```
SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", MODE="0666"
SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="4ee1", SYMLINK+="android_adb"
SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="4ee1", SYMLINK+="android_fastboot"
```

replace vendor and product identifiers with yours.

Reload rules:

```sh
sudo udevadm control --reload-rules
```

Reconnect your device.

Type:

```sh
sudo adb devices
```

your device will be listed.

## Root device

Reboot to bootloader:

```sh
sudo adb reboot booloader
```

Then unlock the bootloader:

```sh
sudo fastboot oem unlock
```

Select "Yes" on the phone using volume keys, confirm pressing the power key once.

Now reboot:

```
sudo fastboot reboot
```

Download [CF-Auto_root](http://download.chainfire.eu/363/CF-Root/CF-Auto-Root/CF-Auto-Root-hammerhead-hammerhead-nexus5.zip)
on your PC then:

```sh
unzip CF-Auto-Root-hammerhead-hammerhead-nexus5.zip
cd image/
sudo fastboot boot CF-Auto-Root-hammerhead-hammerhead-nexus5.img
```

This will take a few minutes and reboot on its own.

## Install MultiROM Manager

Go to the store, install MultiROM Manager and run it.

Grant permission when asked.

Install update, recovery and "CM 11 (4.4.3 - 4.4.4)" kernel.

Reboot when asked.

## Download images

Download the following files:

* Latest Plasma Phone image: http://build.maui-project.org/maui-lge-hammerhead-0.6.0-20141007.zip
* CyanogenMod 11 M9: http://download.cyanogenmod.org/get/jenkins/78753/cm-11-20140805-SNAPSHOT-M9-hammerhead.zip

Upload the CM release:

```sh
adb push cm-11-20140805-SNAPSHOT-M9-hammerhead.zip /sdcard/
```

Upload Plasma Phone:

```sh
adb push maui-lge-hammerhead-0.6.0-20141007.zip /sdcard/
```

* In the Recovery on the device:
  - Clear data and cache (factory reset)
  - Install the CM release by picking the CM image
  - Install Plasma Phone by picking its image
  - Reboot the device
