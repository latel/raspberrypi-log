# 安装docker

## why docker

## 安装docker

通过安装docker，来同时部署多个服务，避免污染系统，方便日后维护

```
curl -fsSL get.docker.com -o get-docker.sh
sudo su
get-docker.sh --mirror Aliyun
groupadd docker
usermod -aG docker $USER
reboot

docker run arm32v7/hello-world
```

### 更换dockerhub源

对于使用 systemd 的系统，请在 /etc/docker/daemon.json 中写入如下内容（如果文件不存在请新建该文件）
```
{
  "registry-mirrors": [
    "https://registry.docker-cn.com"
    "https://dockerhub.azk8s.cn",
    "https://hub-mirror.c.163.com"
  ]
}
```
注意，一定要保证该文件符合 json 规范，否则 Docker 将不能启动。之后重新启动服务。

```
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```

验证新的源是否生效
```
docker info
```

## 安装docker-compose

```bash
sudo pip3 install docker-compose
```

## 常用的docker命令

## 推荐的镜像

树莓派使用arm架构，所以dockerhub上绝大部分基于x86架构的镜像是无法使用的，你可以通过搜索时包含关键词rpi、arm32v6、arm32v7或arm64v7来搜索

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

## 基础镜像

如果你希望构建自己的镜像来提供服务，可以考虑从如下几个基础镜像开始

- alpine # alpine本身支持arm架构（待确认）
- arm32v7/alpine
- balenalib/rpi-raspbian
