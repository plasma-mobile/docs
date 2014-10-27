How to manually run the UI
==========================

Status:

* systemd user session is finished but has not been tested
* Might contains bug which can only be discovered with tests
* Test is blocked by droid-hal-init

## How to run

Log in via ssh as usual.

Become root and run droid-hal-init:

```sh
sudo su -
/bin/sh /usr/bin/droid/droid-hal-startup.sh
```

Now open another ssh session.

Create ~/run-comp:

```sh
#!/bin/sh
export QT_QPA_PLATFORM=hwcomposer
export EGL_PLATFORM=hwcomposer
export KSCREEN_BACKEND=QScreen
export QT_COMPOSITOR_NEGATE_INVERTED_Y=0
export QT_QPA_EGLFS_DEPTH=32
export QT_QPA_EGLFS_HIDECURSOR=1

greenisland -plugin evdevtouch:/dev/input/event1 -plugin evdevkeyboard:keymap=/usr/share/qt5/keymaps/droid.qmap -p org.kde.satellite.compositor.phone
```

Make it executable:

```sh
chmod 755 ~/run-comp
```

Create ~/run-shell:

```sh
#!/bin/bash

set -x

export $(dbus-launch)

rm -rf ~/.config ~/.local ~/.cache

mkdir -p ~/.config

cat > ~/.config/kded5rc <<EOF
[General]
CheckSycoca=false
EOF

QT_PLUGIN_PATH=${QT_PLUGIN_PATH+$QT_PLUGIN_PATH:}`qtpaths --plugin-dir`
# TODO: Do we really need this?
QT_PLUGIN_PATH=$QT_PLUGIN_PATH:/usr/lib/kde5/plugins/
export QT_PLUGIN_PATH

export QT_QPA_PLATFORMTHEME=KDE

export XDG_DATA_DIRS=/usr/local/share:/usr/share
export LIBEXEC_PATH=/usr/libexec:/usr/lib/libexec:/usr/lib/libexec/kf5
export QT_QPA_PLATFORM=wayland
export QT_WAYLAND_DISABLE_WINDOWDECORATION=1
export XDG_CURRENT_DESKTOP=KDE
export KSCREEN_BACKEND=QScreen
export KDE_FULL_SESSION=true
export KDE_SESSION_VERSION=5
#export XDG_MENU_PREFIX=kde-

#export QT_DEBUG_PLUGINS=1

/usr/lib/libexec/ksyncdbusenv
/usr/bin/kbuildsycoca5
#LD_BIND_NOW=true /usr/lib/libexec/kf5/start_kdeinit_wrapper --kded +kcminit_startup
LD_BIND_NOW=true /usr/lib/libexec/kf5/start_kdeinit_wrapper --kded --suicide

/usr/bin/plasmashell -p org.kde.satellite.phone -n
```

Make it executable:

```sh
chmod 755 ~/run/shell
```

Run the compositor:

```sh
~/run-comp
```

Open another ssh session and run the shell:

```sh
~/run-shell
```
