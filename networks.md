# 网络

## 反向代理
为了尽可能的向外部网络减少暴露的端口，很有必要部署一个反向代理，对外部网络只暴露反向代理的端口，减少被探测和入侵的几率，同时所有的请求集中出入可以很方便的实施各种网络策略，如防火墙或者记录进出流量等。

## 查询网络设备
```bash
#ifconfig
```

## 无线网
请注意，树莓派自带的wifi模块是使用陶瓷技术，信号很差，所以一般速度都不可靠。建议使用有线网或者单独购买一个wifi增益天线模块。

### 断开wifi连接
```bash
#ifconfig wlan0 down
```

## FAQ

### 部署反向代理的nginx占用了大量的磁盘io
试着把关闭proxy_buffering参数，
定位磁盘负载高的问题：https://www.cnblogs.com/felixzh/p/9002143.html
定位nginx反向代理占用大量磁盘io的问题：https://blog.csdn.net/thor_qm/article/details/21548137。
