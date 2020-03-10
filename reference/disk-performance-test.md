# 参考文献 - 硬盘速写能力测试

以下的测试均建立在磁盘设备是`/dev/sda1`，并且挂载在`/mnt/disk`的假设下。

## 性能测试工具

### hdparm
```bash
$ sudo hdparm -Tt /dev/sda1

/dev/sda1:
 Timing cached reads:   1638 MB in  2.00 seconds = 818.75 MB/sec
 Timing buffered disk reads: 650 MB in  3.00 seconds = 216.55 MB/sec
```
上面的命令意思是检测/dev/sda1这个设备的读取速度，区分直接读取和缓存读取，
可以发现，有缓存的数据读取速度会快得多

### dd

```bash
$ dd count=50000 bs=1M if=/dev/zero of=/mnt/ironwolf/test.img

记录了50000+0 的读入
记录了50000+0 的写出
52428800000 bytes (52 GB, 49 GiB) copied, 1541.39 s, 34.0 MB/s
```
上面的意思是向/mnt/ironwolf/test.img写入50G数据，写入完成后会返回写入速度

## 不同文件系统性能测试

下面的测试基于以下假设:

- 设备型号为树莓派4b
- 操作系统为rasbian sketch32位
- 存储设备位购买的[希捷ironwolf硬盘](https://item.jd.com/54994027565.html)
- 读取测试使用hdparm，写入测试使用dd

| 文件系统 | 读取    | 读取（缓存） | 写入    |
| -------- | ------- | ------------ | ------- |
| ntfs     | 216MB/s | 818MB/s      | 34MB/s  |
| ext4     | 200Mb/s | 881MB/s      | 169MB/s |
| exfat    | 217MB/s | 806MB/s      | 133MB/s |

经过上面的测试，可以发现ext4的读写性能最高，
但是需要考虑的是，如果硬盘损坏的话，相对来说ntfs的修复工具生态更成熟一些，毕竟数据的安全也是很重要的考虑项（别问我为什么知道，惨痛的教训）。···


## FAQ 

### dd时很慢，如何显示dd进度？
可以借助程序`pv`来统计

```bash
$dd count=5000 bs=1M if=/dev/zero | pv | dd of=/test.img
```

### dd错误提示dd: bs: illegal numeric value
这是因为某些 SD 卡接受的 bs（Block Size）單位必須是小寫（某些則是大寫）。所以你試試看把指令中bs的单位改为小写的m或者反之。

### 使用dd备份系统
```bash
$dd if=/dev/<系统盘符> of=/dev/<存储位置>/system.img
```

 Timing buffered disk reads: 650 MB in  3.00 seconds = 216.55 MB/secdd
 Timing buffered disk reads: 650 MB in  3.00 seconds = 216.55 MB/sec
