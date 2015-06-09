# Remaster server

We have an ARM server to remaster the daily-preinstalled image on and
remove Ubuntu stuff and add Plasma stuff.  In mobster scripts this is
done by remote-remaster (run on the ARM server) and chroot-remaster
(run in the chroot on the ARM server).

## How to set up

In remaster-ubuntu-touch change SERVER variable and set up .ssh/config
to use the right ssh key.

apt install zsync apt-cacher-ng

## TODO

Ideally we wouldn't remaster from
http://cdimage.ubuntu.com/ubuntu-touch/daily-preinstalled/ but would
instead generate our own from the Ubuntu archives.
