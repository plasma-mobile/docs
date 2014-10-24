How to flash Plasma Phone on Nexus 5
====================================

Useful information can be found here:

* https://wiki.merproject.org/wiki/Building_Sailfish_OS_for_Nexus_5
* https://wiki.merproject.org/wiki/Adaptations/libhybris/Install_SailfishOS_for_hammerhead
* http://releases.sailfishos.org/sfa-ea/2014-07-21_SailfishOSHardwareAdaptationDevelopmentKit.pdf
* http://schier.co/post/how-to-root-nexus-5-in-ubuntu-linux

## Prerequisites

The device must be connected to the Linux host with the USB cable all the time, unless told to reconnect.

## Configure Linux

Install Android tools.

On Ubuntu do:

```sh
sudo apt-get install android-tools
```

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

## Unlock the device

Go to the bootloader:

* Power off the phone
* Keep volume down and power pressed together until the bootloader is shown
  (the bootloader has an android with the lid open like this: http://www.androidcentral.com/sites/androidcentral.com/files/postimages/684/android-az-bootloader.jpg)

From the Linux host type:

```sh
sudo fastboot oem unlock
```

Select "Yes" on the phone using volume keys, confirm pressing the power key once.

Now reboot:

```
sudo fastboot reboot
```

Since unlocking wipes data, Android will restart the first time wizard. Let the wizard come up, then press Power button to just power off the phone, you will not need to complete the wizard.

## Install TWRP

TWRP is an alternative recovery image that offers a lot of flexibility
and will be used to install Plasma Mobile.

Download the latest version: http://techerrata.com/file/twrp2/hammerhead/openrecovery-twrp-2.8.0.1-hammerhead.img

We assume that at this point the phone is off.

Go to the bootloader:

* Keep volume down and power pressed together until the bootloader is shown
  (the bootloader has an android with the lid open like this: http://www.androidcentral.com/sites/androidcentral.com/files/postimages/684/android-az-bootloader.jpg)

Now from the Linx host type (make sure the img-file is in the same directory where you start the command):

```sh
sudo fastboot flash recovery openrecovery-twrp-2.8.0.1-hammerhead.img
```

Now:

* Press volume up or volume down until the "Restart to recovery" option appears
* Confirm pressing the power button

TWRP will now install (this might take a while), please note that it will reboot into stock Android on its own.

## Download images

Download the following files:

* CyanogenMod 11 M9: http://download.cyanogenmod.org/get/jenkins/78753/cm-11-20140805-SNAPSHOT-M9-hammerhead.zip
* Latest Plasma Phone image: http://build.maui-project.org/phone/maui-armv7hl-lge-hammerhead-milestone1/maui-lge-hammerhead-0.6.0.zip

## Flash images

From the TWRP main menu:

* Tap "Wipe"
* Tap "Advanced Wipe"
* Select all items
* Swipe to Wipe
* Tap "Back"
* Tap home

Upload the CM release running this from the Linux host:

```sh
sudo adb push cm-11-20140805-SNAPSHOT-M9-hammerhead.zip /sdcard/
```

Upload Plasma Phone running this from the Linux host:

```sh
sudo adb push maui-lge-hammerhead-0.6.0.zip /sdcard/
```

From the TWRP main menu:

* Tap "Install"
* Select cm-11-20140805-SNAPSHOT-M9-hammerhead.zip from /sdcard
* Tap "Add More Zips"
* Select maui-lge-hammerhead-0.6.0.zip from /sdcard
* Swipe to confirm flash
* Tap "Reboot system"

## Connect to the device via SSH

The phone listen to the 192.168.2.15 IP and offers a debug telnet on port 2323 and ssh.

Connect using ssh as maui user (password is maui):

```sh
ssh maui@192.168.2.15
```
