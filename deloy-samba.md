# 搭建Samba服务
搭建samba服务可以很好的在windows和mac平台，或者移动端如ios/Android，使用系统自带的文件管理器（如mac的finder或windows的资源管理器）查看和编辑文件，
而不是每次都只能远程到树莓派上通过树莓派来存取文件，非常麻烦

当然也可以用sshfs来实现类似的目的，但是存在诸多限制，samba才是最为标准、可靠和原生化的跨平台文件分享方案。

## 查看samba状态 

```bash
# smbstatus

Samba version 4.5.12-Debian
PID     Username     Group        Machine                                   Protocol Version  Encryption           Signing     
----------------------------------------------------------------------------------------------------------------------------------------
301     pi           pi           10.0.1.8 (ipv4:10.0.1.8:12581)            SMB3_11           -                    partial(AES-128-CMAC)

Service      pid     Machine       Connected at                     Encryption   Signing
---------------------------------------------------------------------------------------------
mi           301     10.0.1.8      Mon Mar  2 12:31:26 2020 UTC     -            -

Locked files:
Pid          Uid        DenyMode   Access      R/W        Oplock           SharePath   Name   Time
--------------------------------------------------------------------------------------------------
301          1000       DENY_NONE  0x100081    RDONLY     NONE             /share/mi   .   Mon Mar  2 12:31:26 2020
```
