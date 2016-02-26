resize-root-filesystem script is really slow.. takes like 15-20 mins.
Here is quicker way for those who uses MultiROM..

-----------------------------------------------------------------------------------------------------------------------
NOTE: STRICTLY FOR PEOPLE USING MULTIROM, FOR UBUNTU-DEVICE-FLASH DON'T DO THIS
-----------------------------------------------------------------------------------------------------------------------

* ssh into your phone, and then as root..

```
root@ubuntu-phablet:/userdata# resize2fs system.img
resize2fs 1.42.13 (17-May-2015)
The filesystem is already 512000 (4k) blocks long.  Nothing to do!
```
This will tell filesystem is 512000 blocks long, note that number. Now,

* fsck the filesystem

```
root@ubuntu-phablet:/userdata# e2fsck -yf system.img
e2fsck 1.42.13 (17-May-2015)
Pass 1: Checking inodes, blocks, and sizes
Deleted inode 32055 has zero dtime.  Fix? yes
...
lots of output...
```

* once done.. increase the number we noted in step number 1.. (512000, I
usually double that..) and with that number

```
root@ubuntu-phablet:/userdata# resize2fs system.img 1024000
resize2fs 1.42.13 (17-May-2015)
Resizing the filesystem on system.img to 1024000 (4k) blocks.
The filesystem on system.img is now 1024000 (4k) blocks long.
```

And once that is done, once again...

```
root@ubuntu-phablet:/userdata# e2fsck -yf system.img
```

* Tada...

This takes just 1 minute depending upon your typing speed. much better
lovestory then resize-root-partition.. ;-)
