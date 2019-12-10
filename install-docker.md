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
