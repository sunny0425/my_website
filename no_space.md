# Ubuntu - Error: No space left on device

一台服务器出错了，ssh 连接上
先看下是不是空间不够

```
$ df -h
```

果然不够，清理空间吧。

搜索一下，据说服务器一直更新会有很多images，占用大量磁盘空间，那就来看看

```
dpkg -l linux-headers-\* linux-image-\* | grep ^ii
```
然后发现确实有大量的image，写个脚本
remove_linux_images.sh

```
#!/bin/bash
# check old images
# http://askubuntu.com/questions/317763/apt-get-no-space-left-on-device-12-04
# dpkg -l linux-headers-\* linux-image-\* | grep ^ii

for n in 100 101 103 105 106; do
  echo "sudo apt-get remove linux-image-3.13.0-${n}"
  sudo apt-get remove linux-image-3.13.0-"$n" -y
  sudo apt-get remove linux-headers-3.13.0-"$n" -y
done
```

数字手动添加的，没有自动获取，将除去最新版本的images都删除。
运行：

```
$ chmod +x remove_linux_images.sh
$ ./remove_linux_images.sh
```

DONE!
