## 启用SSH

sd制作好之后，默认启用ssh， ssh默认用户名为pi，密码为raspberry

```
touch SDCARD/SSH
```


## 启用vnc
raspbian已经内置了realvnc connect，但是如果你没有插入显示器，则没有创建窗口环境，你需要自己创建一个虚拟的桌面
see https://help.realvnc.com/hc/en-us/articles/360002249917-VNC-Connect-and-Raspberry-Pi
```
vncserver -randr=1920x1080

# VNC(R) Server 6.5.0 (r41824) ARMv6 (Aug 16 2019 00:24:44)
# Copyright (C) 2002-2019 RealVNC Ltd.
# RealVNC and VNC are trademarks of RealVNC Ltd and are protected by trademark
# registrations and/or pending trademark applications in the European Union,
# United States of America and other jurisdictions.
# Protected by UK patent 2481870; US patent 8760366; EU patent 2652951.
# See https://www.realvnc.com for information on VNC.
# For third party acknowledgements see:
# https://www.realvnc.com/docs/6/foss.html
# OS: Raspbian GNU/Linux 10, Linux 4.19.75, armv7l

# On some distributions (in particular Red Hat), you may get a better experience
# by running vncserver-virtual in conjunction with the system Xorg server, rather
# than the old version built-in to Xvnc. More desktop environments and
# applications will likely be compatible. For more information on this alternative
# implementation, please see: https://www.realvnc.com/doclink/kb-546

# Running applications in /etc/vnc/xstartup

# VNC Server catchphrase: "Water trivial Agatha. Edition idea ecology."
#              signature: 30-b3-6d-a5-68-fa-10-25

# Log file is /root/.vnc/raspberrypi:1.log
# New desktop is raspberrypi:1 (10.43.108.236:1)
```
那么`10.43.108.236:1`便是vnc客户端可连接的地址

## 启用WIFI
编辑`/etc/wpa_supplicant/wpa_supplicant.conf`文件即可
```conf
# openwifi
network={
	ssid="WiFi-NAME"
	key_mgmt=NONE
	priority=2
}
# wifi with psk, scan_ssid for a hidden network
network={
	ssid="WiFi-NAME-with-PSK"
	scan_ssid=1
	psk="password"
	priority=11
}

```
