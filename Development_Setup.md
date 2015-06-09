# Development Setup

This document describes a convenient development setup for application development.

## Device

Install a fresh Ubuntu-phone vivid vervet image, either using multirom or by flashing directly. After installation and enabling developer mode, enable ssh:

### To make logging in possible without unlocking first

        sudo touch /userdata/.writable_image
        sudo touch /userdata/.adb_onlock
        sudo reboot

### edit /etc/init/ssh.override

        sudo bash
        echo manual > /etc/init/ssh.override
        echo "exec /usr/sbin/sshd -D -o PasswordAuthentication=yes" >> /etc/init/ssh.override
        
### To make ssh start persistently. The ssh password is 1234 or whatever you setup for unlocking the phone

        sudo service ssh start
        sudo setprop persist.service.ssh true
        sudo reboot

### To start network with nmcli

        nmcli c add con-name CONNECTIONNAME ifname wlan0 type wifi ssid SSIDGOESHERE
        nmcli c modify CONNECTIONNAME wifi-sec.key-mgmt wpa-psk
        nmcli c modify CONNECTIONNAME wifi-sec.psk PASSWORDGOESHERE
        nmcli c up CONNECTIONNAME
	sudo bash
	echo "nameserver 8.8.8.8" > /etc/resolv.conf

### To install our stuff

First remove a bunch of existing packages to make space. See the end of this document for my list. then add our own repo:
    
        sudo apt-add-repository ppa:plasma-phone/ppa 
        sudo apt-get update
        
        sudo apt-get install libepoxy0 libkdecorations2-5 xwayland qtwayland5 libkf5waylandclient5 libkf5waylandserver5 kwin simplelogin plasma-phone-components qml-module-org-kde*

and untar in the home dir when plasmashell and kactivitymanagerd are not running.

Then start plasma:
    
Plasma-shell now starts automatically on boot! You can kill the shell and the restart from the terminal after making changes. You need to export a number of environment variables:

         export QT_QPA_PLATFORM=wayland
         export QT_QPA_PLATFORMTHEME=KDE
         export QT_WAYLAND_DISABLE_WINDOWDECORATION=1
         export XDG_CURRENT_DESKTOP=KDE
         export KSCREEN_BACKEND=QScreen

         export KDE_FULL_SESSION=1
         export KDE_SESSION_VERSION=5

         export $(dbus-launch)
         exec /usr/bin/plasmashell -p org.kde.satellite.phone

If kded5 has crashed, you may have to start it manually:

         /usr/bin/kded5&
        
Note: You need to edit ~/.config/plasma-org.kde.satellite.phone-appletsrc to contain:

         [Containments][1]
         activityId=//leave the existing value unchanged
         formfactor=0
         immutability=1
         lastScreen=0
         location=0
         plugin=org.kde.phone.homescreen
         wallpaperplugin=org.kde.image

         [Containments][1][Applets][3]
         immutability=1
         plugin=org.kde.phone.notifications

         [Containments][2]
         activityId=
         formfactor=2
         immutability=1
         lastScreen=0
         location=3
         plugin=org.kde.phone.panel
         wallpaperplugin=org.kde.image

         [Containments][2][Applets][4]
         immutability=1
         plugin=org.kde.phone.quicksettings
        
        
## Building apps locally

You can build anything on the phone, if you've got enough space on the root partition to install the development tools and dev packages you need. You can also build on the arm-phone server.

          IP Address: 212.47.241.146
          
If you want to create packages locally, you need to add a debian directory and use debuild: there is nothing special about it.          

## Installation and Device-Deployment

## Building on the arm server

IP Address: 212.47.241.146
Put all data in /mnt/data, not in /

## Removed packages from a default vivid proposed ubuntu-touch image:

    rc  account-plugin-facebook                              0.12+15.04.20150417-0ubuntu1                all          GNOME Control Center account plugin for single signon - facebook
    rc  account-plugin-flickr                                0.12+15.04.20150417-0ubuntu1                all          GNOME Control Center account plugin for single signon - flickr
    rc  account-plugin-google                                0.12+15.04.20150417-0ubuntu1                all          GNOME Control Center account plugin for single signon
    rc  account-plugin-twitter                               0.12+15.04.20150417-0ubuntu1                all          GNOME Control Center account plugin for single signon - twitter
    rc  apg                                                  2.2.3.dfsg.1-2ubuntu1                       armhf        Automated Password Generator - Standalone version
    rc  desktop-file-utils                                   0.22-1ubuntu3                               armhf        Utilities for .desktop files
    rc  gnome-bluetooth                                      3.8.2.1-0ubuntu12                           armhf        GNOME Bluetooth tools
    rc  gnome-menus                                          3.10.1-0ubuntu5                             armhf        GNOME implementation of the freedesktop menu specification
    rc  ibus                                                 1.5.9-1ubuntu3                              armhf        Intelligent Input Bus - core
    rc  indicator-bluetooth                                  0.0.6+14.10.20141006-0ubuntu1               armhf        System bluetooth indicator.
    rc  indicator-keyboard                                   0.0.0+15.04.20150310-0ubuntu1               armhf        Keyboard indicator
    rc  indicator-location                                   13.10.0+15.04.20150417-0ubuntu1             armhf        Indicator to show when the system is using your
    rc  indicator-network                                    0.5.1+15.04.20150412-0ubuntu1               armhf        Systems settings menu service - Network indicator
    rc  indicator-transfer                                   0.1+15.04.20150112-0ubuntu1                 armhf        Shows Transfers
    rc  language-pack-touch-ast                              1:15.04+20150413                            all          Ubuntu Touch translations for language Asturian
    rc  language-pack-touch-bg                               1:15.04+20150413                            all          Ubuntu Touch translations for language Bulgarian
    rc  language-pack-touch-bs                               1:15.04+20150413                            all          Ubuntu Touch translations for language Bosnian
    rc  language-pack-touch-ca                               1:15.04+20150413                            all          Ubuntu Touch translations for language Catalan; Valencian
    rc  language-pack-touch-cs                               1:15.04+20150413                            all          Ubuntu Touch translations for language Czech
    rc  language-pack-touch-da                               1:15.04+20150413                            all          Ubuntu Touch translations for language Danish
    rc  language-pack-touch-de                               1:15.04+20150413                            all          Ubuntu Touch translations for language German
    rc  language-pack-touch-el                               1:15.04+20150413                            all          Ubuntu Touch translations for language Greek, Modern (1453-)
    rc  language-pack-touch-en                               1:15.04+20150413                            all          Ubuntu Touch translations for language English
    rc  language-pack-touch-es                               1:15.04+20150413                            all          Ubuntu Touch translations for language Spanish; Castilian
    rc  language-pack-touch-eu                               1:15.04+20150413                            all          Ubuntu Touch translations for language Basque
    rc  language-pack-touch-fi                               1:15.04+20150413                            all          Ubuntu Touch translations for language Finnish
    rc  language-pack-touch-fr                               1:15.04+20150413                            all          Ubuntu Touch translations for language French
    rc  language-pack-touch-gd                               14.10+20140610.3                            all          Ubuntu Touch translations for language Gaelic; Scottish Gaelic
    rc  language-pack-touch-gl                               1:15.04+20150413                            all          Ubuntu Touch translations for language Galician
    rc  language-pack-touch-he                               1:15.04+20150413                            all          Ubuntu Touch translations for language Hebrew
    rc  language-pack-touch-hu                               1:15.04+20150413                            all          Ubuntu Touch translations for language Hungarian
    rc  language-pack-touch-id                               1:15.04+20150413                            all          Ubuntu Touch translations for language Indonesian
    rc  language-pack-touch-it                               1:15.04+20150413                            all          Ubuntu Touch translations for language Italian
    rc  language-pack-touch-ja                               1:15.04+20150413                            all          Ubuntu Touch translations for language Japanese
    rc  language-pack-touch-ko                               1:15.04+20150413                            all          Ubuntu Touch translations for language Korean
    rc  language-pack-touch-lt                               1:15.04+20150413                            all          Ubuntu Touch translations for language Lithuanian
    rc  language-pack-touch-lv                               1:15.04+20150413                            all          Ubuntu Touch translations for language Latvian
    rc  language-pack-touch-ms                               1:15.04+20150413                            all          Ubuntu Touch translations for language Malay
    rc  language-pack-touch-nb                               1:15.04+20150413                            all          Ubuntu Touch translations for language Bokmål, Norwegian; Norwegian Bokmål
    rc  language-pack-touch-nl                               1:15.04+20150413                            all          Ubuntu Touch translations for language Dutch; Flemish
    rc  language-pack-touch-oc                               1:15.04+20150413                            all          Ubuntu Touch translations for language Occitan (post 1500)
    rc  language-pack-touch-pl                               1:15.04+20150413                            all          Ubuntu Touch translations for language Polish
    rc  language-pack-touch-pt                               1:15.04+20150413                            all          Ubuntu Touch translations for language Portuguese
    rc  language-pack-touch-ro                               1:15.04+20150413                            all          Ubuntu Touch translations for language Romanian
    rc  language-pack-touch-ru                               1:15.04+20150413                            all          Ubuntu Touch translations for language Russian
    rc  language-pack-touch-sk                               1:15.04+20150413                            all          Ubuntu Touch translations for language Slovak
    rc  language-pack-touch-sl                               1:15.04+20150413                            all          Ubuntu Touch translations for language Slovenian
    rc  language-pack-touch-sr                               1:15.04+20150413                            all          Ubuntu Touch translations for language Serbian
    rc  language-pack-touch-sv                               1:15.04+20150413                            all          Ubuntu Touch translations for language Swedish
    rc  language-pack-touch-tr                               1:15.04+20150413                            all          Ubuntu Touch translations for language Turkish
    rc  language-pack-touch-ug                               1:15.04+20150413                            all          Ubuntu Touch translations for language Uighur; Uyghur
    rc  language-pack-touch-uk                               1:15.04+20150413                            all          Ubuntu Touch translations for language Ukrainian
    rc  language-pack-touch-zh-hans                          1:15.04+20150413                            all          Ubuntu Touch translations for language Simplified Chinese
    rc  language-pack-touch-zh-hant                          1:15.04+20150413                            all          Ubuntu Touch translations for language Traditional Chinese
    rc  libaspell15:armhf                                    0.60.7~20110707-1.3                         armhf        GNU Aspell spell-checker runtime library
    rc  libcanberra-gtk3-0:armhf                             0.30-2ubuntu2                               armhf        GTK+ 3.0 helper for playing widget event sounds with libcanberra
    rc  libcheese-gtk23:armhf                                3.14.1-2ubuntu4                             armhf        tool to take pictures and videos from your webcam - widgets
    rc  libcheese7:armhf                                     3.14.1-2ubuntu4                             armhf        tool to take pictures and videos from your webcam - base library
    rc  libclutter-1.0-0:armhf                               1.20.0-1ubuntu1                             armhf        Open GL based interactive canvas library
    rc  libclutter-gst-2.0-0:armhf                           2.0.12-1build1                              armhf        Open GL based interactive canvas library GStreamer elements
    rc  libclutter-gtk-1.0-0:armhf                           1.6.0-1                                     armhf        Open GL based interactive canvas library GTK+ widget
    rc  libcogl-pango20:armhf                                1.18.2-3                                    armhf        Object oriented GL/GLES Abstraction/Utility Layer
    rc  libcogl-path20:armhf                                 1.18.2-3                                    armhf        Object oriented GL/GLES Abstraction/Utility Layer
    rc  libcogl20:armhf                                      1.18.2-3                                    armhf        Object oriented GL/GLES Abstraction/Utility Layer
    rc  libcontent-hub0:armhf                                0.0+15.04.20150422-0ubuntu1                 armhf        content sharing/picking library
    rc  libcrack2:armhf                                      2.9.2-1                                     armhf        pro-active password checker library
    rc  libenchant1c2a:armhf                                 1.6.0-10.1                                  armhf        Wrapper library for various spell checker engines (runtime libs)
    rc  libfcitx-config4:armhf                               1:4.2.8.5-6ubuntu3                          armhf        Flexible Input Method Framework - configuration support library
    rc  libfcitx-gclient0:armhf                              1:4.2.8.5-6ubuntu3                          armhf        Flexible Input Method Framework - D-Bus client library for Glib
    rc  libfcitx-utils0:armhf                                1:4.2.8.5-6ubuntu3                          armhf        Flexible Input Method Framework - utility support library
    rc  libgnome-bluetooth11                                 3.8.2.1-0ubuntu12                           armhf        GNOME Bluetooth tools - support library
    rc  libgnome-desktop-3-10                                3.14.1-1ubuntu2                             armhf        Utility library for loading .desktop files - runtime files
    rc  libgnome-menu-3-0                                    3.10.1-0ubuntu5                             armhf        GNOME implementation of the freedesktop menu specification
    rc  libgnomekbd8                                         3.6.0-1ubuntu1                              armhf        GNOME library to manage keyboard configuration - shared library
    rc  libgtop2-10                                          2.30.0.is.2.30.0-0ubuntu1                   armhf        gtop system monitoring library (shared)
    rc  libharfbuzz-icu0:armhf                               0.9.37-1                                    armhf        OpenType text shaping engine ICU backend
    rc  libibus-1.0-5:armhf                                  1.5.9-1ubuntu3                              armhf        Intelligent Input Bus - shared library
    rc  libjavascriptcoregtk-3.0-0:armhf                     2.4.8-1ubuntu2                              armhf        JavaScript engine library from WebKitGTK+
    rc  liblightdm-gobject-1-0                               1.14.0-0ubuntu2                             armhf        LightDM GObject client library
    rc  libnm-gtk0:armhf                                     0.9.10.1-0ubuntu4                           armhf        network management framework (GNOME dialogs for wifi and mobile)
    rc  libpwquality-common                                  1.2.3-1ubuntu2                              all          library for password quality checking and generation (data files)
    rc  libpwquality1:armhf                                  1.2.3-1ubuntu2                              armhf        library for password quality checking and generation
    rc  libubuntu-app-launch2:armhf                          0.4+15.04.20150410-0ubuntu1                 armhf        library for sending requests to the ubuntu app launch
    rc  libubuntu-download-manager-client0                   0.9+15.04.20150203-0ubuntu1                 armhf        Ubuntu Download Manager - shared public library
    rc  libubuntu-download-manager-common0                   0.9+15.04.20150203-0ubuntu1                 armhf        Ubuntu Download Manager - shared common library
    rc  libubuntu-location-service2:armhf                    2.1+15.04.20150427.1-0ubuntu1               armhf        location service aggregating position/velocity/heading
    rc  libubuntu-upload-manager-common0                     0.9+15.04.20150203-0ubuntu1                 armhf        Ubuntu Upload Manager - shared common library
    rc  libubuntuoneauth-2.0-0:armhf                         14.04+15.04.20150401                        armhf        Ubuntu One authentication library
    rc  libunity-action-qt1:armhf                            1.1.0+14.04.20140304-0ubuntu1               armhf        Unity Action Qt API
    rc  libunity-api0:armhf                                  7.96+15.04.20150410.2-0ubuntu1              armhf        API for Unity shell integration
    rc  libunity-control-center1                             15.04.0+15.04.20150410-0ubuntu1             armhf        utilities to configure the GNOME desktop
    rc  libunity-protocol-private0:armhf                     7.1.4+14.10.20140808-0ubuntu1               armhf        binding to get places into the launcher - private library
    rc  libunity-scopes3:armhf                               0.6.17+15.04.20150423.2-0ubuntu1            armhf        API for Unity scopes integration
    rc  libunity-settings-daemon1                            15.04.1+15.04.20150408-0ubuntu1             armhf        Helper library for accessing settings
    rc  libunity9:armhf                                      7.1.4+14.10.20140808-0ubuntu1               armhf        binding to get places into the launcher - shared library
    rc  libwacom2:armhf                                      0.11+dfsg-0ubuntu1                          armhf        Wacom model feature query library
    rc  libwebkitgtk-3.0-0:armhf                             2.4.8-1ubuntu2                              armhf        Web content engine library for GTK+
    rc  libwebp5:armhf                                       0.4.1-1.2                                   armhf        Lossy compression of digital photographic images.
    rc  libxklavier16                                        5.4-0ubuntu1                                armhf        X Keyboard Extension high-level API
    rc  lightdm                                              1.14.0-0ubuntu2                             armhf        Display Manager
    rc  myspell-ca                                           0.20111230b-7                               all          Catalan dictionary for myspell
    rc  myspell-es                                           1.11-9                                      all          Spanish dictionary for myspell
    rc  myspell-it                                           1:4.2.1-0ubuntu3                            all          Italian dictionary for myspell
    rc  myspell-pl                                           20140830-2                                  all          Polish dictionary for myspell
    rc  myspell-pt-br                                        20131030-4                                  all          Brazilian Portuguese dictionary for myspell
    rc  myspell-pt-pt                                        20091013-7                                  all          European Portuguese dictionary for myspell
    rc  nano                                                 2.2.6-3                                     armhf        small, friendly text editor inspired by Pico
    rc  obex-data-server                                     0.4.6-0ubuntu2                              armhf        D-Bus service for OBEX client and server side functionality
    rc  openbox                                              3.5.2-8                                     armhf        standards-compliant, fast, light-weight and extensible window manager
    rc  qtdeclarative5-ubuntu-contacts0.1                    0.2+15.04.20150402-0ubuntu1                 armhf        Qt Ubuntu Contacts Components - QML plugin
    rc  qtdeclarative5-ubuntu-content1                       0.0+15.04.20150422-0ubuntu1                 armhf        QML binding for libcontent-hub
    rc  qtdeclarative5-ubuntu-telephony-phonenumber0.1:armhf 0.1+15.04.20150430.1-0ubuntu1               armhf        Telephony service components for Ubuntu - QML plugin
    rc  qtdeclarative5-ubuntu-telephony0.1:armhf             0.1+15.04.20150430.1-0ubuntu1               armhf        Telephony service components for Ubuntu - QML plugin
    rc  qtdeclarative5-ubuntu-ui-extras-browser-plugin:armhf 0.23+15.04.20150430.1-0ubuntu1              armhf        Ubuntu web QML plugin
    rc  qtdeclarative5-ubuntu-ui-extras0.2                   0.2+15.04.20150311-0ubuntu1                 armhf        Ubuntu UI Extra Components
    rc  qtdeclarative5-ubuntu-web-plugin:armhf               0.23+15.04.20150430.1-0ubuntu1              armhf        Ubuntu web QML plugin
    rc  qtdeclarative5-ubuntuone1.0:armhf                    14.04+15.04.20150401                        armhf        Ubuntu One authentication library - Qt declarative plug-in
    rc  qtmir-android:armhf                                  0.4.4+15.04.20150511.1-0ubuntu1             armhf        Qt platform abstraction (QPA) plugin for a Mir server (mobile)
    rc  qtubuntu-sensors:armhf                               0.6+15.04.20150226-0ubuntu1                 armhf        QtSensors plugins for Android sensors
    rc  system-image-common                                  2.5-0ubuntu1                                all          Ubuntu system image updater
    rc  system-image-dbus                                    2.5-0ubuntu1                                all          Ubuntu system image updater command line client
    rc  telephony-service                                    0.1+15.04.20150430.1-0ubuntu1               armhf        Telephony service components for Ubuntu - backend
    rc  ubuntu-application-api2-touch:armhf                  2.9.0+15.04.20150326-0ubuntu1               armhf        Implementation of the Platform API for Ubuntu Touch
    rc  ubuntu-download-manager                              0.9+15.04.20150203-0ubuntu1                 armhf        Ubuntu Download Manager - daemon
    rc  ubuntu-keyboard                                      0.99.trunk.phablet2+15.04.20150429-0ubuntu1 armhf        Ubuntu on-screen keyboard
    rc  ubuntu-location-provider-here                        0.1+15.04.20141110-0ubuntu1                 armhf        HERE provider for the Ubuntu Location Service
    rc  ubuntu-location-service-bin                          2.1+15.04.20150427.1-0ubuntu1               armhf        location service aggregating position/velocity/heading
    rc  ubuntu-push-client                                   0.68+15.04.20150430.1-0ubuntu1              armhf        Ubuntu Push Notifications client-side daemon
    rc  ubuntu-system-settings                               0.3+15.04.20150422.1-0ubuntu1               armhf        System Settings application for Ubuntu Touch
    rc  ubuntu-system-settings-online-accounts               0.6+15.04.20150417-0ubuntu1                 armhf        Online Accounts setup for Ubuntu Touch
    rc  ubuntu-touch-customization-hooks                     0.6+14.10.20140922.1-0ubuntu1               all          Enables customization of images
    rc  ubuntu-touch-session                                 0.108+15.04.20150422-0ubuntu1               all          Ubuntu Session Manager for Ubuntu Touch Devices
    rc  ubuntu-upload-manager                                0.9+15.04.20150203-0ubuntu1                 armhf        Ubuntu Upload Manager - daemon
    rc  unity-control-center                                 15.04.0+15.04.20150410-0ubuntu1             armhf        utilities to configure the GNOME desktop
    rc  unity-plugin-scopes:armhf                            0.5.4+15.04.20150410.2-0ubuntu1             armhf        QML plugin for Scopes
    rc  unity-scope-mediascanner2                            0.2+15.04.20150411-0ubuntu1                 armhf        Media scanner scope for Unity
    rc  unity-settings-daemon                                15.04.1+15.04.20150408-0ubuntu1             armhf        daemon handling the Unity session settings
    rc  unity-system-compositor                              0.0.5+15.04.20150430-0ubuntu1               armhf        System compositor for Ubuntu
    rc  unity8                                               8.02+15.04.20150423-0ubuntu1                armhf        Unity 8 shell
    rc  url-dispatcher:armhf                                 0.1+15.04.20150123-0ubuntu1                 armhf        service to allow sending of URLs and get handlers started


in one go:
apt-get remove account-plugin-facebook account-plugin-flickr account-plugin-google account-plugin-twitter apg desktop-file-utils gnome-bluetooth gnome-menus ibus indicator-bluetooth indicator-keyboard indicator-location indicator-network indicator-transfer language-pack-touch-ast language-pack-touch-bg language-pack-touch-bs language-pack-touch-ca language-pack-touch-cs language-pack-touch-da language-pack-touch-de language-pack-touch-el language-pack-touch-en language-pack-touch-es language-pack-touch-eu language-pack-touch-fi language-pack-touch-fr language-pack-touch-gd language-pack-touch-gl language-pack-touch-he language-pack-touch-hu language-pack-touch-id language-pack-touch-it language-pack-touch-ja language-pack-touch-ko language-pack-touch-lt language-pack-touch-lv language-pack-touch-ms language-pack-touch-nb language-pack-touch-nl language-pack-touch-oc language-pack-touch-pl language-pack-touch-pt language-pack-touch-ro language-pack-touch-ru language-pack-touch-sk language-pack-touch-sl language-pack-touch-sr language-pack-touch-sv language-pack-touch-tr language-pack-touch-ug language-pack-touch-uk language-pack-touch-zh-hans language-pack-touch-zh-hant libaspell15 libcanberra-gtk3-0 libcheese-gtk23 libcheese7 libclutter-1.0-0 libclutter-gst-2.0-0 libclutter-gtk-1.0-0 libcogl-pango20 libcogl-path20 libcogl20 libcontent-hub0 libcrack2 libenchant1c2a libfcitx-config4 libfcitx-gclient0 libfcitx-utils0 libgnome-bluetooth11 libgnome-desktop-3-10 libgnome-menu-3-0 libgnomekbd8 libgtop2-10 libharfbuzz-icu0 libibus-1.0-5 libjavascriptcoregtk-3.0-0 liblightdm-gobject-1-0 libnm-gtk0 libpwquality-common libpwquality1 libubuntu-app-launch2 libubuntu-download-manager-client0 libubuntu-download-manager-common0 libubuntu-location-service2 libubuntu-upload-manager-common0 libubuntuoneauth-2.0-0 libunity-action-qt1 libunity-api0 libunity-control-center1 libunity-protocol-private0 libunity-scopes3 libunity-settings-daemon1 libunity9 libwacom2 libwebkitgtk-3.0-0 libwebp5 libxklavier16 lightdm myspell-ca myspell-es myspell-it myspell-pl myspell-pt-br myspell-pt-pt nano obex-data-server openbox qtdeclarative5-ubuntu-contacts0.1 qtdeclarative5-ubuntu-content1 qtdeclarative5-ubuntu-telephony-phonenumber0.1 qtdeclarative5-ubuntu-telephony0.1 qtdeclarative5-ubuntu-ui-extras-browser-plugin qtdeclarative5-ubuntu-ui-extras0.2 qtdeclarative5-ubuntu-web-plugin qtdeclarative5-ubuntuone1.0 qtmir-android qtubuntu-sensors system-image-common system-image-dbus telephony-service ubuntu-application-api2-touch ubuntu-download-manager ubuntu-keyboard ubuntu-location-provider-here ubuntu-location-service-bin ubuntu-push-client ubuntu-system-settings ubuntu-system-settings-online-accounts ubuntu-touch-customization-hooks ubuntu-touch-session ubuntu-upload-manager unity-control-center unity-plugin-scopes unity-scope-mediascanner2 unity-settings-daemon unity-system-compositor unity8 url-dispatcher 
