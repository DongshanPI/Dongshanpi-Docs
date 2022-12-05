# 运行输出hello word



## 配置开发环境

首先我们需要获取 柚木PI-蜥蜴 开发板 配套的交叉编译工具链。

由于目前工具链没有提供windows版本，所以只能在 Linux下进行，操作，请先参考上述章节 配置ubuntu 虚拟机章节，进行配置，并配置好。



获取完成源码后，需要将交叉编译工具链的路径加入到 系统的 PATH环境变量内。

首先 需要获取 交叉编译工具链 所在的绝对路径，进入到  <code>tina-v853/prebuilt/gcc/linux-x86/arm/toolchain-sunxi-musl/toolchain/arm-openwrt-linux-muslgnueabi</code>目录下执行 **pwd** 命令，即可得到绝对路径 ` /home/book/tina-v853/prebuilt/gcc/linux-x86/arm/toolchain-sunxi-musl/toolchain/arm-openwrt-linux-muslgnueabi</code>` 。

```bash
book@100ask:~/tina-v853/prebuilt/gcc/linux-x86/arm/toolchain-sunxi-musl/toolchain/bin$ pwd
/home/book/tina-v853/prebuilt/gcc/linux-x86/arm/toolchain-sunxi-musl/toolchain/bin
```

接下来，可以在终端下执行如下命令，讲这个加入到系统 环境变量内，这样就可以在任意位置执行  交叉编译工具链了。

```bash
export STAGING_DIR=/home/book/tina-v853/prebuilt/gcc/linux-x86/arm/toolchain-sunxi-musl/toolchain/bin
```

注意：此方式只针对当前的终端有效，如果你关闭了这个终端，再次开启终端 需要重新执行才可以。

还有另一种永久生效的方式 就是写入到 系统环境变量里面，需要修改  **/etc/environment** 在末尾加上 你获取到的交叉编译工具链绝对路径,注意修改需要使用 sudo 命令。

```bash
book@100ask:~$ cat /etc/environment
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/book/tina-v853/prebuilt/gcc/linux-x86/arm/toolchain-sunxi-musl/toolchain/bin"
book@100ask:~$ source /etc/environment
```



## 编写Hello word程序

* 配置好交叉编译工具链以后，就可以开始编写我们的应用程序了，如下为一个最简单的 hello word打印示例程序。

```c
#include <stdio.h>
int main (void)
{
    printf("hello word!\n");
    return 0;
}    
```

编写完成后，保存到 helloword.c

之后我们执行 如下编译命令进行编译 

```
book@100ask:~/workspace/V851sTest/helloword$ vim helloword.c 
book@100ask:~/workspace/V851sTest/helloword$ arm-openwrt-linux-gcc -o helloword helloword.c
book@100ask:~/workspace/V851sTest/helloword$ file helloword
helloword: ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /lib/ld-musl-armhf.so.1, with debug_info, not stripped
```

## 拷贝到开发板

怎么拷贝文件到开发板上？ 有U盘  ADB 网络 串口等等。

### 使用usb adb方式

typeC线反接至开发板，点击VMware菜单栏中的虚拟机->可移动设备->Google Tina ADB ->连接（断开与 主机 的连接），使虚拟机连接上柚木PI-蜥蜴 开发板。

![image-20221205091647473](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/YuzukiHD-Lizard/YuzukiHD-Lizard-05-1_ADB.png)

之后我们执行如下命令查询虚拟机是否能连接到开发板，如果出现设备号即为连接成功。

```
book@100ask:~$ adb devices
List of devices attached
* daemon not running; starting now at tcp:5037
* daemon started successfully
20080411	device
```

此时可以通过下面命令将生成的helloword使用adb传输到开发板的根目录下。

```
book@100ask:~/workspace/V851sTest/helloworld$ adb push helloword /
helloword: 1 file pushed. 0.2 MB/s (25024 bytes in 0.126s)
```



### 使用TF卡方式

将文件拷贝到TF卡中，将TF卡插入 柚木PI-蜥蜴 开发板中，**正接**至开发板中，启动系统后使用如下命令将TF卡挂载至tina系统上。我这里使用的是4G的内存卡，所以为/dev/mmcblk0p1，用户可以根据自己的设备号挂载对应的设备。

```
root@TinaLinux:/# df -h
Filesystem                Size      Used Available Use% Mounted on
/dev/root                16.3M     16.3M         0 100% /rom
devtmpfs                 26.0M         0     26.0M   0% /dev
tmpfs                    27.2M         0     27.2M   0% /tmp
/dev/by-name/rootfs_data
                         43.5M     48.0K     41.2M   0% /overlay
overlayfs:/overlay       43.5M     48.0K     41.2M   0% /
tmpfs                    27.2M         0     27.2M   0% /run
/dev/ubi0_6              29.9M     24.0K     28.3M   0% /mnt/UDISK
/dev/mmcblk0p1            3.7G    160.0K      3.7G   0% /mnt/extsd
root@TinaLinux:/# mount /dev/mmcblk0p1 /mnt/
```

将helloword可执行程序拷贝到根目录下备用。

```
root@TinaLinux:/# cd /mnt/
root@TinaLinux:/mnt# ls
System Volume Information  helloword
root@TinaLinux:/mnt# cp helloword /
```



## 运行

下载好程序以后，需要使用chmod +x 命令来给程序添加可执行权限，之后 我们就可以执行 这个helloword应用了。

```bash
root@TinaLinux:~# cd /
root@TinaLinux:/# chmod +x helloword
root@TinaLinux:/# ./helloword
hello word!
```

