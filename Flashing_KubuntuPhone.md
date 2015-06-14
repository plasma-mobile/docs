How to flash Kubuntu Phone on Nexus 5
====================================

Useful information can be found here:

* https://wiki.ubuntu.com/Touch/Devices#Server_at_http:.2BAC8ALw-system-image.tasemnice.eu
* http://schier.co/post/how-to-root-nexus-5-in-ubuntu-linux

## Prerequisites

The device must be connected to the Linux host with the USB cable all the time, unless told to reconnect.

## Configure the host device

Install Android tools.

On Ubuntu do:

```sh
sudo apt-get install android-tools* ubuntu-device-flash phablet-tools
```

Connect the device and type:

```sh
lsusb
```

an output like this will come out:

```sh
Bus 004 Device 010: ID 18d1:4ee1 Google Inc. Nexus 4 / 10
```

18d1 is the vendor id, 4ee1 is the product id.

Numbers may change with other devices.

Create /etc/udev/rules.d/51-android.rules with:

```
SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", MODE="0666"
SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="4ee1", SYMLINK+="android_adb"
SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="4ee1", SYMLINK+="android_fastboot"

```
replace vendor and product identifiers with yours.

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


## Flash the device

Then connect the device to the host system via USB and boot into bootloader using volume up and the power button to switch off the phone.

ubuntu-device-flash --server="http://system-image.tasemnice.eu" touch --channel="ubuntu-touch/devel" --bootstrap

* start into TWRP
* Pick Reboot, then "Bootloader"

```ubuntu-device-flash --server="http://kubuntu.plasma-mobile.org" touch --channel="kubuntu-phone/devel" --bootstrap --developer-mode --password 1234```

or, if you want to live on the bleeding edge, flash the latest, untested image with:

```ubuntu-device-flash --server="http://kubuntu.plasma-mobile.org" touch --channel="kubuntu-phone/devel-proposed" --bootstrap --developer-mode --password 1234```

("devel-proposed" instead of just "devel")

The device will at some point (ca 5min on my system with fast connection) show the ubuntu logo spinning. This takes another 5 minutes, then it reboots, the google logo shows, and shortly thereafter our spinning ubuntu logo again. After about 1 minute, it's done, and I'm greeted with the welcome screen.


## Basic Device Setup

Install a fresh Ubuntu-phone vivid vervet image, either using multirom or by flashing directly. After installation and enabling developer mode, enable ssh:

Make sure the device is connected through your USB port, then log in to it using

```sudo adb shell```

### Writable root

By default, the root filesystem is read-only. Not too useful for developers. To make it writable, do the following:

tl;dr (on the device)
``` sudo writable-root && sudo reboot ```

The verbose version (so you know what's going on):
```
sudo touch /userdata/.writable_image
sudo touch /userdata/.adb_onlock
sudo reboot
```
### Enable SSH access

tl;dr (on the device)
``` sudo ssh-setup```

The verbose version (so you know what's going on):
```
sudo bash
echo manual > /etc/init/ssh.override
echo "exec /usr/sbin/sshd -D -o PasswordAuthentication=yes" >> /etc/init/ssh.override

sudo service ssh start
sudo setprop persist.service.ssh true
sudo reboot
```
### Connect Wifi

We've included a small script which sets up a wifi connection (WPA-PSK) for NetworkManager.

```wifi-setup SSID PASSWORD```

If you're using a different security mechanism for your wifi network, it's time to read the nmcli documentation. Look into /usr/bin/wifi-setup for inspiration.

### On and on...

Now follow the instructions for your <a href="Development_Setup.md">Development Setup</a> to get going.
