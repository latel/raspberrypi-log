通过安装docker，来同时部署多个服务，避免污染系统，方便日后维护

```
curl -fsSL get.docker.com -o get-docker.sh
sudo su
get-docker.sh --mirror Aliyun
usermod -aG docker $USER
reboot

docker run arm32v7/hello-world
```
