#### Problem

linux内核版本在4.14.9及以上的系统，如ubuntu 16.04，在使用nvidia-384及以上版本驱动（包括按照cuda时安装的驱动）可能导致黑屏、图形界面打不开、循环登陆等问题，下面是一个可能解决问题的方法：

编辑/usr/src/nvidia/\<version\>/kernel/nvidia-uvm/uvm8_va_block.c<sup>[1]</sup>，
也可能此文件在位置/var/lib/dkms/nvidia-\<version\>/\<version\>/build/nvidia-uvm/uvm8_va_block.c<sup>[2]</sup>下，
增加如下一行代码

```c++
include <linux/sched/task_stack.h>
```

重新编译安装此模块
```bash
# 文件在[1]中
sudo dkms build nvidia/<version>
sudo dkms install nvidia/<version>
# 文件在[2]中
sudo dkms build nvidia-<version>/<version>
sudo dkms install nvidia-<version>/<version>
```
