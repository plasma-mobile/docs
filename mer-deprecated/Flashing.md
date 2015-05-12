How to flash Plasma Phone on Nexus 5
====================================

Useful information can be found here:

* https://wiki.merproject.org/wiki/Building_Sailfish_OS_for_Nexus_5
* http://releases.sailfishos.org/sfa-ea/2014-07-21_SailfishOSHardwareAdaptationDevelopmentKit.pdf

Download images
---------------

Download the following files:

* Latest Plasma Phone image: http://dev.plasma-mobile.org/phone/
* CyanogenMod 11 M9: http://download.cyanogenmod.org/get/jenkins/78753/cm-11-20140805-SNAPSHOT-M9-hammerhead.zip

Flash
-----

* Boot into Android Recovery

* Upload the CM release:

```sh
adb push cm-11-20140805-SNAPSHOT-M9-hammerhead.zip /sdcard/
```

* Upload Plasma Phone:

```sh
adb push maui-lge-hammerhead-0.6.0-20141007.zip /sdcard/
```

* In the Recovery on the device:
  - Clear data and cache (factory reset)
  - Install the CM release by picking the CM image
  - Install Plasma Phone by picking its image
  - Reboot the device
