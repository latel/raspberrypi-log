prepare mounting point
```bash
sudo mkdir /mnt/timecapsule
```

add following to /etc/fstab
```
//10.0.1.1/Data /mnt/timecapsule cifs user=<USERNAME>,pass=<PASSWORD>,rw,uid=1000,iocharset=utf8,sec=ntlm,vers=1.0 0 0
```

then

```bash
sudo mount -a
```
