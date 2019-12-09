安装portainer,以后可以勇敢portainer管理docker，免得需要ssh到树莓派上用命令行操作的麻烦
```bash
docker run -d -p 9000:9000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
```

智能家居
```bash
mkdir hassio
sudo curl -sL https://raw.githubusercontent.com/home-assistant/hassio-installer/master/hassio_install.sh | bash -s -- -m raspberrypi4 -d /home/pi/hassio
```

samba服务，方便用电脑管理文件系统
```bash
docker run --name rpi-samba --restart always -v /home/pi:/data/share -p 445:445 -p 139:139 -p 137:137/udp -p 138:138/udp rpi-samba
```

rpi-monitor,方便从web界面实时查看树莓派的状态，比如cpu温度
```bash
docker run --device=/dev/vchiq --device=/dev/vcsm --volume=/opt/vc:/opt/vc --volume=/boot:/boot --volume=/sys:/dockerhost/sys:ro --volume=/etc:/dockerhost/etc:ro --volume=/proc:/dockerhost/proc:ro --volume=/usr/lib:/dockerhost/usr/lib:ro -p=8888:8888 --name="rpi-monitor" -d  michaelmiklis/rpi-monitor:latest
```
