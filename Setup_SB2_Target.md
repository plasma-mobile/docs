Setting up scratchbox2 target
=============================

Maui prepares sb2 target for armv7hl using maui-kickstarter-configs and the tarball is
published on http://dev.plasma-mobile.org/

Here is a Maui revised version of the instructions in page 17 from HADK:

```sh
MERSDK $

hadk

cd $HOME

SFFE_SB2_TARGET=/parentroot/$MER_ROOT/targets/$VENDOR-$DEVICE-armv7hl
TARBALL=http://dev.plasma-mobile.org/nightly/20141017/maui-sdk-target-armv7hl/maui-sdk-target-armv7hl.tar.bz2
curl -O $TARBALL

sudo mkdir -p $SFFE_SB2_TARGET
sudo tar --numeric-owner -pxjf $(basename $TARBALL) -C $SFFE_SB2_TARGET

sudo chown -R $USER $SFFE_SB2_TARGET

cd $SFFE_SB2_TARGET
grep :$(id -u): /etc/passwd >> etc/passwd
grep :$(id -g): /etc/group >> etc/group

sb2-init -d -L "--sysroot=/" -C "--sysroot=/" \
  -c /usr/bin/qemu-arm-dynamic -m sdk-build \
  -n -N -t / $VENDOR-$DEVICE-armv7hl \
  /opt/cross/bin/armv7hl-meego-linux-gnueabi-gcc

sb2 -t $VENDOR-$DEVICE-armv7hl -m sdk-install -R rpm --rebuilddb

sb2 -t $VENDOR-$DEVICE-armv7hl -m sdk-install -R zypper ar \
    -G http://repo.merproject.org/obs/home:/plfiorini:/maui:/devel:/hw:/droid:/tools/latest_armv7hl/ \
    maui-hw-droid-tools

sb2 -t $VENDOR-$DEVICE-armv7hl -m sdk-install -R zypper ref --force
```
