>
> TODO: 安装docker时默认开启usernamepsace，以避免各种镜像的账户不统一导致的权限问题不好控制
>

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

对于使用 systemd 的系统，请在 /etc/docker/daemon.json 中写入如下内容（如果文件不存在请新建该文件），这里使用的是清华大学的镜像，请根据自己需要灵活选择。
```
{
  "registry-mirrors": [
    "https://docker.mirrors.ustc.edu.cn"
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

树莓派使用arm架构，所以dockerhub上绝大部分基于x86架构的镜像是无法使用的，你可以通过搜索时包含关键词rpi、arm32v6、arm32v7或arm64v7来搜索

## 推荐的镜像

### portainer

安装portainer,以后可以勇敢portainer管理docker，免得需要ssh到树莓派上用命令行操作的麻烦
```bash
docker run -d -p 9000:9000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
```

### homeassistant/hassio
智能家居
```bash
mkdir hassio
sudo curl -sL https://raw.githubusercontent.com/home-assistant/hassio-installer/master/hassio_install.sh | bash -s -- -m raspberrypi4 -d /home/pi/hassio
```

DEPRECATED: 官方已经不再支持这种安装方式，https://github.com/home-assistant/supervised-installer

### rpi-samba
samba服务，方便用电脑管理文件系统
```bash
docker run --name rpi-samba --restart always -v /home/pi:/data/share -p 445:445 -p 139:139 -p 137:137/udp -p 138:138/udp rpi-samba
```

### rpi-monitor
方便从web界面实时查看树莓派的状态，比如cpu温度
```bash
docker run --device=/dev/vchiq --device=/dev/vcsm --volume=/opt/vc:/opt/vc --volume=/boot:/boot --volume=/sys:/dockerhost/sys:ro --volume=/etc:/dockerhost/etc:ro --volume=/proc:/dockerhost/proc:ro --volume=/usr/lib:/dockerhost/usr/lib:ro -p=8888:8888 --name="rpi-monitor" -d  michaelmiklis/rpi-monitor:latest
```

### 其他可以考虑通过docker部署的服务
ResilioSync, seafile, emby

## 基础镜像

如果你希望构建自己的镜像来提供服务，可以考虑从如下几个基础镜像开始

- alpine # alpine本身支持arm架构（待确认）
- arm32v7/alpine
- balenalib/rpi-raspbian


## FAQ

### 如果容器里的服务需要访问被和谐的地址如何处理
可以在创建容器时指定HTTP_PROXY,HTTPS_PROXY, http_proxy和https_proxy四个环境变量，为一个可用的代理地址。
这个不能保证一定能解决问题，因为容器内的服务很有可能不会遵守这些环境变量的设计，不过值得一试。
