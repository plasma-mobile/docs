# Running Apps

This document describes how to run and debug applications on the device.

## Environment Setup

In order to run applications on the device, you need to set up a few environment variables:

```
export QT_QPA_PLATFORM=wayland
export QT_QPA_PLATFORMTHEME=KDE
export QT_WAYLAND_DISABLE_WINDOWDECORATION=1
export XDG_CURRENT_DESKTOP=KDE
export KSCREEN_BACKEND=QScreen

export KDE_FULL_SESSION=1
export KDE_SESSION_VERSION=5
```

### Manually starting the Plasma shell (or applications)

If you want to run the Plasma phone shell, do

```
export $(dbus-launch)
exec /usr/bin/plasmashell -p org.kde.satellite.phone
```

If kded5 has crashed, you may have to start it manually:

```
/usr/bin/kded5&
```


## Debugging Applications

* installation of debug packages and gdb

