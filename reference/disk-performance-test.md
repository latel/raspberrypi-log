# 参考文献 - 硬盘速写能力测试

以下的测试均建立在磁盘设备是`/dev/sda1`，并且挂载在`/mnt/disk`的假设下。

## hdparm
```bash
$ sudo hdparm -Tt /dev/sda2

/dev/sda2:
 Timing cached reads:   1638 MB in  2.00 seconds = 818.75 MB/sec
 Timing buffered disk reads: 650 MB in  3.00 seconds = 216.55 MB/sec
```
上面的命令意思是检测/dev/sda1这个设备的读取速度，区分直接读取和缓存读取，
可以发现，有缓存的数据读取速度会快得多

## dd

```bash
$ dd count=50000 bs=1M if=/dev/zero of=~/test.img

记录了50000+0 的读入
记录了50000+0 的写出
52428800000 bytes (52 GB, 49 GiB) copied, 1541.39 s, 34.0 MB/s
```
上面的意思是向~/test.img写入50G数据，写入完成后会返回写入速度

下面的表哥显示了在树莓派4b中使用rasbian sketch操作系统，分别对各种文件操作系统做了读写测试，以便为nas选择一个理想的
文件系统格式

经过上面的测试，可以发现ext4的读写性能最高，
但是需要考虑的是，如果硬盘损坏的话，相对来说ntfs的修复工具生态更成熟一些，毕竟数据的安全也是很重要的考虑项（别问我为什么知道，惨痛的教训）。···