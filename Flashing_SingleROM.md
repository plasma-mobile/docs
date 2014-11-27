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
sudo apt-get install android-tools*
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

ACTION=="add", SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="d001" RUN+="/home/sebas/bin/connect-n5.sh"
```
replace vendor and product identifiers with yours.

The last line is only needed for systems without networkmanager. It creates the usb0 network interface for you.

connect-n5.sh (which is automatically called upon plugging in the phone) then does the ip setup:
```
#!/bin/sh
sudo ifconfig usb0 up
sudo ifconfig usb0 192.168.2.20
```
You can test this by plugging in the phone and check on the host system if usb0 shows up in ifconfig.

Now Reload uev's rules to make these changes effective:

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

Download TWRP version: http://build.maui-project.org/phone/openrecovery-twrp-2.8.0.1-hammerhead.img

We assume that at this point the phone is off.

Go to the bootloader:

* Keep volume down and power pressed together until the bootloader is shown
  (the bootloader has an android with the lid open like this: http://www.androidcentral.com/sites/androidcentral.com/files/postimages/684/android-az-bootloader.jpg)

Now from the Linx host type (make sure the img-file is in the same directory where you start the command):

```sh
sudo fastboot flash recovery openrecovery-twrp-2.8.0.1-hammerhead.img
```

Now:

* Press volume up or volume down until the "Recovery Mode" option appears
* Confirm pressing the power button

TWRP will now install (this might take a while).

Once installed, the TWRP main menu screen will appear.

## Root phone

* Tab Reboot
* Tab Recovery
* Swipe to Reboot
* If asked to "Install SuperSU", swipe to confirm and root the device


## Download images

Download the following files:

* CyanogenMod 11 M9: http://download.cyanogenmod.org/get/jenkins/78753/cm-11-20140805-SNAPSHOT-M9-hammerhead.zip
* Plasma Phone Milestone 1 image (only access via ssh): http://build.maui-project.org/phone/maui-lge-hammerhead-0.1.0.zip


## Flash images

From the TWRP main menu:

* Tap "Wipe"
* Tap "Advanced Wipe"
* Select all items
* Swipe to Wipe
* Tap "Back"
* Tap the "Home" symbol (=small house) in lower left corner

Upload the CM release running this from the Linux host (this can take a while without output on the screen):

```sh
sudo adb push cm-11-20140805-SNAPSHOT-M9-hammerhead.zip /sdcard/
```

Upload Plasma Phone running this from the Linux host (again wait to finish):

```sh
sudo adb push maui-lge-hammerhead-0.1.0.zip /sdcard/
```

From the TWRP main menu:

* Tap "Install"
* Select cm-11-20140805-SNAPSHOT-M9-hammerhead.zip from /sdcard
* Tap "Add More Zips"
* Select maui-lge-hammerhead-0.1.0.zip from /sdcard
* Swipe to confirm flash (this can take a while, watch the messages for errors)
* Tap "Reboot system"

## Connect to the device via SSH

Now the phone seems to be actually stuck just showing the Google logo and unlocked key symbol below.

That is normal, as no graphical stack has been installed with the milestone 1 kit.

But you can actually connect to the device, as the phone listen to the 192.168.2.15 IP and offers a debug telnet on port 2323 and ssh.

Connect using ssh as user: "maui" (password is also "maui"):

```sh
ssh maui@192.168.2.15
```

Congratulations! You have at this point succesfully established root access on a completely new and free basic system on your Nexus 5! 


## Milestone 2 (boot into graphical shell)

Download:

* Plasma Phone Milestone 2 image (graphical shell): http://build.maui-project.org/phone/maui-lge-hammerhead-0.2.0.zip


From the TWRP main menu:

Upload the CM release running this from the Linux host (this can take a while without output on the screen):

```sh
sudo adb push cm-11-20140805-SNAPSHOT-M9-hammerhead.zip /sdcard/
```

Upload Plasma Phone running this from the Linux host (again wait to finish):

```sh
sudo adb push maui-lge-hammerhead-0.2.0.zip /sdcard/
```

From the TWRP main menu:

* Tap "Install"
* Select cm-11-20140805-SNAPSHOT-M9-hammerhead.zip from /sdcard
* Tap "Add More Zips"
* Select maui-lge-hammerhead-0.2.0.zip from /sdcard
* Swipe to confirm flash (this can take a while, watch the messages for errors)
* Tap "Reboot system"


