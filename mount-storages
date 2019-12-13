# 挂载存储设备

## 挂载apple time capsule

```bash
# 安装cifs文件协议，新版树莓派默认已经包含
sudo apt install cifs
# 建立挂载点
sudo mkdir /mnt/timecapsule
# 编写/etc/fstab，//10.0.1.1/Data为atp设备的ip和存储挂载点，user为atp设备用户名(默认应该是admin)，pass为atp设备用户密码
# 需要根据自己的情况做相应改动
sudo echo "//10.0.1.1/Data /mnt/timecapsule cifs user=<USERNAME>,pass=<PASSWORD>,rw,uid=1000,iocharset=utf8,sec=ntlm,vers=1.0 0 0" >> /etc/fstab

# 需要挂载时执行
sudo mount -a
```

## 挂载ntfs设备

需要安装ntfs文件协议的开源实现`ntfs-3g`，新版的树莓派默认已经包含 

```bash
sudo apt install ntfs-3g
```
