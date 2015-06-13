# Development Setup

This document describes a convenient development setup for application development.



## Building apps locally

For quick development-test cycles, it's useful to be able to build applications locally on your device. For this to work, you need to cross-compile the code and produce a package suitable to install on the device. We have created a chroot environment to make this easy. You can install this chroot environment using a script, here's how:

# Setup the cross-compile chroot

The following pseudo snippet will clone the xbuilder repo, which contains the setup script for the chroot. This chroot will be created in the directory from where you're running the setup.sh script. The root directory of the chroot will be called "plasma-mobile-sdk".
```
git clone git@github.com:plasma-mobile/xbuilder.git


mkdir -p /path/to/where/your/chroot/should/end/up
cd /path/to/where/your/chroot/should/end/up

# the chroot will be set up in the current working directory!
sudo /path/to/xbuilder/setup.sh /path/to/your/src
```


