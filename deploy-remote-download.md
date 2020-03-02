# 部署远程下载服务
下班前，提前安排家里的树莓派下载好想看的电影和电视节目，回家后直接看岂不美哉。

## 迅雷远程下载服务
使用国内大厂的服务相对来说比较简单，当然如果你开通了会员的话，速度也会相对来说可以得到保证。
推荐镜像[https://github.com/dingguotu/rpi-xware](https://github.com/dingguotu/rpi-xware)，不过这个镜像目前不支持多盘符，这意味着如果你想区分不同的文件类型，如电影，电视剧想放不同的文件目录时只能手动填写了（如图），[有时间我来改下dockerfile来支持多盘符，或者启动多了docker实例来区分](http://ptbsare.org/2014/10/25/linux%E4%B8%8B%E7%9A%84thunder%E5%AE%9E%E7%8E%B0-%E6%8A%98%E8%85%BE%E6%89%8B%E8%AE%B0/)。
![xunlei remote download](https://raw.githubusercontent.com/latel/raspberrypi-log/master/xunlei-remote.png)

## deluge

## And One More Thing
这里推荐结合合适的媒体服务器如plex、kodi等等，并配置这些媒体服务器自动减速远程下载服务器下载的文件目录，
这样回家之后直接在媒体服务器里就可以直接看到并点播播放了，还会自动检索字幕，整体海报封面，演员和头像、评级等信息，非常舒服，
而不是手动去找寻下载好的文件比较麻烦。

![plex](https://raw.githubusercontent.com/latel/raspberrypi-log/master/plex.png)
