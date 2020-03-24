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

Then i replaced whiptail with dialog in the install script.

After that, i finally got a "screen" of the install-manager of Pi-hole:

![](/images/pihole_install_1st.JPG)


Download lighttpd RPM for Fedora
```
root@photon-rpi3 [ /tmp ]# curl https://rpmfind.net/linux/fedora/linux/development/rawhide/Everything/aarch64/os/Packages/l/lighttpd-1.4.54-4.fc32.aarch64.rpm -o /tmp/lighttpd-1.4.54-4.fc32.aarch64.rpm
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  443k  100  443k    0     0   773k      0 --:--:-- --:--:-- --:--:--  774k
```

Adding Arch Linux Repo
```
root@photon-rpi3 [ ~ ]# cat > /etc/yum.repos.d/arch.repo << "EOF"
> [ArchLinux]
> name=ArchLinux Extra Repo(aarch64)
> baseurl=http://ftp.hosteurope.de/mirror/ftp.archlinux.org/extra/os/$arch
> enabled=1
> skip_if_unavailable=True
> EOF
```

