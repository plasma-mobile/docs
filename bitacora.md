Daily Captains Log
-------------------

29.04.2015:
Short summary:

* greenisland always crashes when compiling shaders
* qmlscene helloworld.qml runs
* qml-compositor sometimes runs with our own libhybris, but mostly crashes. With ubuntu's libhybris it starts and hangs.
* weston without the --with-gl option finds the wrong hybris backend plugin and complains about missing symbols, with, it crashes or doesn't do anything
* weston-launch needs to be launched from logind: d__ed and mgraesslin are looking into that.

My current opinion is that starting weston (or kwin) through logind on Ubuntu's libhybris and then running plasmashell from there is probably the best bet, because it cuts down on the number of components.


------------------

Running Wayland on Ubuntu Phone

Starting situation
==================

* LG Nexus 5 Hammerhead device
* Ubuntu Phone 15.04 Vivid Vervet, Linux ubuntu-phablet 3.4.0-1-hammerhead #9-Ubuntu SMP PREEMPT Sun Jul 20 08:27:48 UTC 2014 armv7l armv7l armv7l GNU/Linux
* Qt 5.4

Preparation
===========

Modifications:

    * To make changing the system and logging in possible without unlocking first

        sudo touch /userdata/.writable_image
        sudo touch /userdata/.adb_onlock

    * To make ssh start persistently

        sudo service ssh start
        sudo setprop persist.service.ssh true

    * To make PasswordAuthentication possible.

        change /etc/init/ssh.override:

        manual
        exec /usr/sbin/sshd -D -o PasswordAuthentication=yes

      (instead of 'no')


    * removed lightdm (except for one weston test, where weston-launch was launched by lightdm)
      

Libhybris
=========

Tried with 3 versions of libhybris:

1. ubuntu: git://phablet.ubuntu.com/ubuntu/libhybris.git
2. Mer: git@github.com:libhybris/libhybris.git
3. Our own, which merges the above two: https://github.com/plasma-mobile/libhybris

All three were recompiled to enable tracing and debugging and packages as debs.

Ubuntu hybris was configured with

dh_auto_configure --sourcedirectory=hybris --  --enable-alinker=jb --enable-debug --enable-trace --enable-wayland --enable-arch=arm --with-android-headers=/usr/include/android

Mer and our own with 

dh_auto_configure --sourcedirectory=hybris --  --enable-alinker=jb --enable-wayland --enable-arch=arm --enable-debug --enable-trace --with-android-headers=/usr/include/android


Wayland
=======

We've tried the following setups:

1.  qt5_qpa_hwcomposer, qtwayland, greenisland
1.a qt5_qpa_hwcomposer, qmlscene
2.  qt5_qpa_hwcomposer, qt5/examples/wayland/qml-compositor/qml-compositor
3.  weston

repos:

qt5_qpa_hwcomposer: https://github.com/plasma-mobile/qt5-qpa-hwcomposer-plugin
qtwayland: https://github.com/plasma-mobile/qtwayland
greenisland: https://github.com/plasma-mobile/greenisland

In all cases, qt5_qpa_hwcomposer had been recompiled to provide extra tracing.

Weston was taking directly from Ubuntu's package repo

Combinations
============

We have tried the following combinations:

1. Hybrid libhybris + greenisland
2. Hybrid libhybris + qml-compositor
3. Mer libhybris + qml-compositor
4. Ubuntu libhybris + qml-compositor
5. Hybrid libhybris + weston
6. Hybrid libhybris + weston started from lightdm

1. Hybrid libhybris + greenisland
=================================

Fails to compile the greenisland shaders: see greenisland.txt for a full log.

QOpenGLShader::compile(Vertex): failed
QOpenGLShader: could not create shader
shader compilation failed: 
""
QOpenGLShaderProgram::uniformLocation( matrix ): shader program is not linked
QOpenGLShaderProgram::uniformLocation( opacity ): shader program is not linked
QOpenGLShaderProgram: could not create shader program
QOpenGLShader: could not create shader
QOpenGLShader: could not create shader
shader compilation failed: 
""
QOpenGLShaderProgram::uniformLocation( qt_Matrix ): shader program is not linked
QOpenGLShaderProgram: could not create shader program
QOpenGLShader: could not create shader
QOpenGLShader: could not create shader
shader compilation failed: 
""
QOpenGLShaderProgram::uniformLocation( qt_Matrix ): shader program is not linked
QOpenGLShaderProgram::uniformLocation( opacity ): shader program is not linked
QOpenGLShaderProgram: could not create shader program
QOpenGLShader: could not create shader
QOpenGLShader: could not create shader
shader compilation failed: 
""
QOpenGLShaderProgram::uniformLocation( qt_Matrix ): shader program is not linked
time in renderer: total=10ms, preprocess=0, updates=0, binding=0, rendering=10

1.a qmlscene 
============

Originally, we thought that these shaders were part of the QtCore shaders, but it turned out that running a simple qml file against qt5_qpa_hwcomposer works:

  phablet@ubuntu-phablet:~$ EGL_PLATFORM=hwcomposer qmlscene helloworld.qml -platform hwcomposer

And displays the output on screen. So Qt's basic shaders needed for drawing e.g. text work against qt5_qpa_hwcomposer. This always works.


2. Hybrid libhybris + qml-compositor
====================================

Next, we tried with qml-compositor, using the command

  sudo XDG_RUNTIME_DIR=/run/user/0/ EGL_PLATFORM=hwcomposer HYBRIS_TRACE=1 HYBRIS_LOGGING_LEVEL=debug /usr/lib/arm-linux-gnueabihf/qt5/examples/wayland/qml-compositor/qml-compositor -platform hwcomposer
  
This crashes most of the time (see qml-compositor-strace.log), but once in a while it will start up and start spinning one mer logo.

Next, we tried to add tracing information to qt5_qpa_hwcomposer. The tracing point to calls in hwcomposer_backend_v11.cpp, in void HWComposer::present(HWComposerNativeWindowBuffer *buffer):

  err = hwcdevice->prepare(hwcdevice, num_displays, mlist)

and 

  err = hwcdevice->set(hwcdevice, num_displays, mlist);


3. Mer libhybris + qml-compositor
=================================

When replacing the hybrid libhybris with the mer one, we get the same crashes. If we copy the code from hwcdevice->set and hwcdevice->prepare into hwcomposer_backend_v11, we will be able to reach the end of the present() function, but still no result. There is no crash, but nothing is drawn to the screen.

4. Ubuntu libhybris + qml-compositor
====================================

This uses the libhybris deb packages from http://www.valdyas.org/~boud/deb: this is plain Ubuntu libhybris but with trace and debug enabled.

Copied /usr/lib/arm-linux-gnueabihf/qt5/examples/wayland/qml-compositor to Phablet's home.

Start qml-compositor with:

sudo XDG_RUNTIME_DIR=/run/user/0/ EGL_PLATFORM=hwcomposer HYBRIS_TRACE=1 HYBRIS_LOGGING_LEVEL=debug  ./qml-compositor  -platform hwcomposer

This presents the following output, after which nothing happens. qml-compositor doesn't exit, attaching gdb does not give a useful backtrace.

5. Hybrid libhybris + weston
============================

Weston runs directly against libhybris. We used Weston from Ubuntu's repository.

* Start weston with:

XDG_RUNTIME_DIR=/run/user/0/ weston --backend=fbdev-backend.so --tty=1 --device=/dev/fb0

Returns:
