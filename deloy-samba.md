# 搭建Samba服务
搭建samba服务可以很好的在windows和mac平台，或者移动端如ios/Android，使用系统自带的文件管理器（如mac的finder或windows的资源管理器）查看和编辑文件，
而不是每次都只能远程到树莓派上通过树莓派来存取文件，非常麻烦

当然也可以用sshfs来实现类似的目的，但是存在诸多限制，samba才是最为标准、可靠和原生化的跨平台文件分享方案。

## samba服务搭建
你可以直接在网上搜索一些samba搭建教程，过程不是很复杂。但是处于不污染系统全局环境的考虑，还是推荐使用[docker][]来部署，推荐的镜像为[trnape/rpi-samba][]。

### 准备挂载目录

```bash
# docker run -d -p <docker_host_ip>:445:445 \
  -v /mnt/data:/share/data \
  -v /mnt/backups:/share/backups \
  --name <container name> trnape/rpi-samba \
  -u "alice:abc123" \
  -u "bob:secret" \
  -u "guest:guest" \
  -s "Backup directory:/share/backups:rw:alice,bob" \
  -s "Alice (private):/share/data/alice:rw:alice" \
  -s "Bob (private):/share/data/bob:rw:bob" \
  -s "Documents (readonly):/share/data/documents:ro:guest,alice,bob"
```

参数解释：
首先通过-v来定义2个数据卷以便允许docker内的samba服务可以访问宿主系统已挂载的存储，通过-u来配置3个samba服务的用户名和相应用户密码，最后通过-s来定义3个samba分享挂载点并指定挂载点名称、读写权限(ro,rw)和对应的可访问用户。如果需要允许匿名访问，用户加上guest即可。

*这里不推荐使用exfat文件格式的存储器，本人尝试过会存在权限问题，估计和exfat-fuse的实现有关系，如果确认要使用extfat可以考虑通过privileged方式启动容器*

*搭建好后，综合体验了一下，发现速度比较慢，还需要具体定位，一是据说arm架构上smbv3协议速度比较慢，可以限制为smbv2，而是smb的参数还有调教空间([https://www.arm-blog.com/samba-finetuning-for-better-transfer-speeds/](https://www.arm-blog.com/samba-finetuning-for-better-transfer-speeds/)*

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

## 周边知识

### 给一个磁盘增加label
使用相应的文件格式软件包提供的程序来给磁盘添加label，如extfs的e2label, ntfs的ntfslabel
注意要先取消挂载相应的磁盘，系统中已经挂载的磁盘可以同lsblk来查看
```bash
#e2label /dev/<deviceid> <label>
```


[docker]: https://github.com/latel/raspberrypi-log/blob/master/docker.md
[trnape/rpi-samba]: https://hub.docker.com/r/trnape/rpi-samba
