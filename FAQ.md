# Frequently Asked Questions & Common Pitfalls

## What is the default user and password of the development image?




## I can't connect to the phone using SSH

If you're not using network-manager on the host system, the network might not be set up as needed. You can do the following to have that happen automatically:


Edit /etc/udev/rules.d/51-android.rules adding:
```
ACTION=="add", SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="d001" RUN+="/home/sebas/bin/connect-n5.sh"
```

Replace vendor and product identifiers with yours, change the location ofthe script. This will create the usb0 network interface for you upon plugging in the phone.

connect-n5.sh (which is automatically called upon plugging in the phone) then does the ip setup:
```
#!/bin/sh
sudo ifconfig usb0 up
sudo ifconfig usb0 192.168.2.20
```
Now Reload uev's rules to make these changes effective:
```
sudo udevadm control --reload-rules
```
You can test this by plugging in the phone and check on the host system if usb0 shows up in ifconfig.

Now you should be able to ssh to your phone, using 'ssh 192.168.2.15'.
