# 部署远程下载服务
下班前，提前安排家里的树莓派下载好想看的电影和电视节目，回家后直接看岂不美哉。

## 迅雷远程下载服务
使用国内大厂的服务相对来说比较简单，当然如果你开通了会员的话，速度也会相对来说可以得到保证。
推荐镜像[https://github.com/dingguotu/rpi-xware](https://github.com/dingguotu/rpi-xware)，不过这个镜像目前不支持多盘符，这意味着如果你想区分不同的文件类型，如电影，电视剧想放不同的文件目录时只能手动填写了（如图），或者你可以启动多个docker实例来实现。
![xunlei remote download](https://raw.githubusercontent.com/latel/raspberrypi-log/master/xunlei-remote.png)

***tips***

迅雷强制下载目录包含一个二级目录`TDDOWNLOAD`，这会使得我们的硬盘目录显得很不优雅，解决这个问题的方法也很简单，就是创建容器时，调整下挂在卷对应的容器内目录，如`-v /mnt/deviceid/movies:/data/TDDOWNLOAD`，这样迅雷下载时就是实际想要存放的目录/mnt/deviceid/movies。

***tips***

如果你出现这样的错误`getting xunlei service info...the active key is not valid.`，请检查下挂载目录是否有写权限

## deluge

## Transmission

## aria2

### aria2 ui
aria2应用的是c/s架构，并且只实现了s端，c端一般通过rpc同s端通信来控制下载的各种操作，因此还需要选择一个合适的客户端，一般选择一个网页端实现就可以了。
目前的选择可以考虑:

+ [ziahamza/webui-aria2](https://github.com/ziahamza/webui-aria2)和
+ [mayswind/AriaNg](https://github.com/mayswind/AriaNg)

如果你不喜欢网页端操作，更喜欢类似迅雷之类的客户端体验，你也可以自行搜索下macOS和windows下的相应实现。

## And One More Thing
这里推荐结合合适的媒体服务器如plex、emby等等，并配置这些媒体服务器自动减速远程下载服务器下载的文件目录，
这样回家之后直接在媒体服务器里就可以直接看到并点播播放了，还会自动检索字幕，整体海报封面，演员和头像、评级等信息，非常舒服，
而不是手动去找寻下载好的文件比较麻烦。

## 参考文章

* http://ptbsare.org/2014/10/25/linux%E4%B8%8B%E7%9A%84thunder%E5%AE%9E%E7%8E%B0-%E6%8A%98%E8%85%BE%E6%89%8B%E8%AE%B0/



![plex](https://raw.githubusercontent.com/latel/raspberrypi-log/master/plex.png)
