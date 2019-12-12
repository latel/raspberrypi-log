## 启用SSH

sd制作好之后，默认启用ssh， ssh默认用户名为pi，密码为raspberry

```
touch SDCARD/SSH
```


## 启用vnc
raspbian已经内置了realvnc connect，但是如果你没有插入显示器，则没有创建窗口环境，你需要自己创建一个虚拟的桌面

> see: https://help.realvnc.com/hc/en-us/articles/360002249917-VNC-Connect-and-Raspberry-Pi

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

> see: https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md

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

## 更新软件源

see [https://github.com/latel/raspberrypi-log/blob/master/change-package-sourcelist.md]()

## 更新系统

要时常更新系统，防止漏洞导致被入侵，尤其是sshd.service

```bash
sudo apt upgrade
```

## 禁用密码登录，只允许证书登录

生成密钥，并加入信任

```basg
ssh-keygen -t rsa
touch ~/.ssh/authorized_keys
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

sshd的配置文件位于: `/etc/ssh/sshd_config`，注意一下几个条目

```conf
# sshd监听端口，记得改成非标准端口如2222
Port 2222

# 记录鉴权日志
SyslogFacility AUTH
LogLevel INFO

# 不允许远程登录root
PermitRootLogin no
# 严格模式，会检查使用的密钥的权限等
StrictModes yes

# 使用rsa私钥登录
PubkeyAuthentication yes
# 不允许使用密码登录
PasswordAuthentication no
```

配置文件说明请参考：[https://www.cnblogs.com/ggjucheng/archive/2012/08/19/2646032.html]()

重启sshd服务

```bash
sudo systemctl restart sshd
```

这样你就可以使用私钥来登录了，在局域网中可以通过主机名来访问

```bash
ssh -i /path/to/id_rsa pi@raspberrypi.local -p2222
```

如果嫌弃每次都输入这么多麻烦可以配置下ssh配置文件`.ssh/config`，如：

```conf
Host raspberrypi # 主机名
    HostName raspberrypi.local # 主机地址(主机名或者ip)
    User pi # 用户名
    IdentityFile /path/to/key # 私钥
    Port 2222 # 指定端口
```

这样，你就可以使用简化的命令来登录了 `ssh raspberrypi`

## 禁用pi用户提权

默认情况下，使用pi用户登录后，可以通过如下2个方式来使用root权限

1. sudo su
2. sudo xxxxx

**从安全考虑，root权限最好只保留给root用户**

设置root账户密码
```bash
sudo passwd root
```

启用root账户
```bash
sudo passwd --unlock root
```

编辑sudoer文件，把pi用户删掉，这样pi用户就不能使用sudo来调用root账户权限了

**额外的ssh安全保证**

树莓派一般作为外网接入家庭局域网的起始点，ssh是最重要的入口，因此可以使用额外的手段来提升ssh服务的安全性

1. 安装fail2ban，当一个ip产生过多错误时，自动加入黑名单

		sudo apt install -y fail2ban
