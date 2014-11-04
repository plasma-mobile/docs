# Convenience Setup

This file outlines a few tricks and practices to ease your development and testing workflows, as well as providing a bunch of pointers for common problems.


## Bringing up Wifi

See this page https://wiki.merproject.org/wiki/Minimal/Networking

In short:

```
vim /var/lib/connman/home-wifi.config
```
Write something like this into it:
```
[service_home_wifi]
Type = wifi
Name = my_home_wifi
Passphrase = secret
```
The enable wifi and restart connman:
```bash
/usr/lib/connman/test/test-connman enable wifi
sudo systemctl stop connman.service 
sudo systemctl start connman.service
```


## SSHing into the device

* Add a line like this to your /etc/hosts file
``` 
192.168.2.15 n5
```
This means you can reach the device under the name "n5", and you don't have to type the IP all the time.

* Now add this to your ~/.ssh/config
```
host = n5
user = maui
```
This automatically picks "maui" as user if you don't specify one, so you can reach your device just with the command "ssh n5".

* Add your public key to the maui user's authorized keys, so you don't need a password to ssh into your device.

On your host:
```bash
scp ~/.ssh/id_rsa.pub n5:/tmp
```
On the device:
```bash
mkdir ~/.ssh
cat /tmp/rd_rsa.pub >> ~/.ssh/authorized_keys
chmod -R 700 ~/.sshme/
```
Now you should be able to ssh into the device just issueing "ssh n5", no password asked. This can be very handy for copying packages, files or anything onto the device.


 