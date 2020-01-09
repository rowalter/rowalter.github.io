---
title: Running Pi-hole on Photon OS
categories: Raspberry
published: true
---
I wanted to replace my older Raspberry Pi, which is currently running Pi-hole, with a newer one. I decided to use a Raspbbery Pi 3b+ which is currently running Photon OS 3.0.
This little fellow i already use as a [Pulse IoT](https://www.vmware.com/products/pulse-iot-device-management.html) gateway.

So i logged in and downloaded the install script:

```
root@photon-rpi3 [ /opt/scripts/pihole ]# curl -L https://install.pi-hole.net > install_pihole.sh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100     5  100     5    0     0     18      0 --:--:-- --:--:-- --:--:--    18
100  111k  100  111k    0     0   190k      0 --:--:-- --:--:-- --:--:--  190k
root@photon-rpi3 [ /opt/scripts/pihole ]# chmod +x install_pihole.sh
```

But when i ran the ```install_pihole.sh``` script, i got an RPM error:

![](/images/pihole_install_error.JPG)

So i checked the install script on line 361.
There it used whiptail to create an installer box.

a short check showed that Photon OS doesn't bring whiptail with it, but the replacement, dialog, is available in the repository.
so i installed it quickly:

```
root@photon-rpi3 [ /opt/scripts/pihole ]# dialog
-bash: dialog: command not found
root@photon-rpi3 [ /opt/scripts/pihole ]# tdnf install dialog

Installing:
dialog                                           aarch64                  1.3-4.20180621.ph3               photon                     522.80k 535349

Total installed size: 522.80k 535349
Is this ok [y/N]:y

Downloading:
dialog                                  240203    100%
Testing transaction
Running transaction
Installing/Updating: dialog-1.3-4.20180621.ph3.aarch64

Complete!
```

After that, i finally got a "screen" of the install-manager of Pi-hole:

![](/images/pihole_install_1st.JPG)
