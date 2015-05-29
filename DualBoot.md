* root your phone: you have to have superSu installed and working
* get http://humpolec.ubuntu.com/latest/dualboot.sh on your pc
* run it as root
* it will reboot some times, after this an "Ubuntu" app will be available
* log in the phone with adb shell
* execute su to be root (supersu should ask for permission)
* cd /data/data/com.canonical.ubuntu.installer/files/
* echo http://kubuntu.plasma-mobile.org > custom_server &&  chmod 777 custom_server
* run the ubuntu app and do install ubuntu
