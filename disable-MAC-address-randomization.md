Found a post on the rpi forums. Apparently NetworkManager can randomize your mac to make it harder to people to snoop on traffic.

Here is a blog post on how to turn it off.
[https://blog.muench-johannes.de/networkmanager-disable-mac-randomization-314]()
in case it goes down:

To disable the MAC address randomization create the file

```
/etc/NetworkManager/conf.d/100-disable-wifi-mac-randomization.conf
```

with the content:

```
[connection]
wifi.mac-address-randomization=1

[device]
wifi.scan-rand-mac-address=no
```
