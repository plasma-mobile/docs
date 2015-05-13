# Development Setup

This document describes a convenient development setup for application development.

## Device

Install a fresh Ubuntu-phone vivid vervet image, either using multirom or by flashing directly. After installation and enabling developer mode, enable ssh:

    * To make logging in possible without unlocking first

        sudo touch /userdata/.writable_image
        sudo touch /userdata/.adb_onlock
        sudo reboot

    * edit /etc/init/ssh.override
    
        manual
        exec /usr/sbin/sshd -D -o PasswordAuthentication=yes
        
    * To make ssh start persistently. The ssh password is 1234 or whatever you setup for unlocking the phone

        sudo service ssh start
        sudo setprop persist.service.ssh true
        sudo reboot

  



## Building apps locally


## Installation and Device-Deployment

