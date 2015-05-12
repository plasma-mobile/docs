```sh
curl -O http://build.maui-project.org/maui-lge-hammerhead-0.6.0-20141013.zip
mkdir xx
cd xx
unzip ../maui-lge-hammerhead-0.6.0-20141013.zip
fastboot boot hybris-boot.img -c bootmode=debug

ip -a

# You will notice a new interface (enp0s26u1u6u1 in my case) with the
# 192.168.2.20 IP address, now just telnet:

telnet 192.168.2.15

# Have fun
```
