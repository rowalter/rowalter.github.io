---
title: Running Pihole with PhotonOS
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

![]({{site.baseurl}}/_images\pihole_install_error.JPG)
