# 编译运行Helloword驱动

## 配置开发环境

首先我们需要获取 柚木PI-蜥蜴 开发板 配套的交叉编译工具链。

由于目前工具链没有提供windows版本，所以只能在 Linux下进行操作，请先参考上述章节 配置ubuntu 虚拟机章节，进行配置，并配置好。




## 配置内核编译环境
```bash
export PATH=$PATH:/home/book/tina-v853/prebuilt/gcc/linux-x86/arm/toolchain-sunxi-musl/toolchain/bin
export ARCH=arm
export CROSS_COMPILE=arm-openwrt-linux-
```
```bash
book@100ask:~/workspace/V851sTest/hello_drv$ export PATH=$PATH:/home/book/tina-v853/prebuilt/gcc/linux-x86/arm/toolchain-sunxi-musl/toolchain/bin
book@100ask:~/workspace/V851sTest/hello_drv$ export ARCH=arm
book@100ask:~/workspace/V851sTest/hello_drv$ export CROSS_COMPILE=arm-openwrt-linux-
```



## 编写 helloword驱动

hello_drv.c

```c
#include <linux/module.h>

#include <linux/fs.h>
#include <linux/errno.h>
#include <linux/miscdevice.h>
#include <linux/kernel.h>
#include <linux/major.h>
#include <linux/mutex.h>
#include <linux/proc_fs.h>
#include <linux/seq_file.h>
#include <linux/stat.h>
#include <linux/init.h>
#include <linux/device.h>
#include <linux/tty.h>
#include <linux/kmod.h>
#include <linux/gfp.h>

/* 1. 确定主设备号                                                                 */
static int major = 0;
static char kernel_buf[1024];
static struct class *hello_class;


#define MIN(a, b) (a < b ? a : b)

/* 3. 实现对应的open/read/write等函数，填入file_operations结构体                   */
static ssize_t hello_drv_read (struct file *file, char __user *buf, size_t size, loff_t *offset)
{
	int err;
	printk("%s %s line %d\n", __FILE__, __FUNCTION__, __LINE__);
	err = copy_to_user(buf, kernel_buf, MIN(1024, size));
	return MIN(1024, size);
}

static ssize_t hello_drv_write (struct file *file, const char __user *buf, size_t size, loff_t *offset)
{
	int err;
	printk("%s %s line %d\n", __FILE__, __FUNCTION__, __LINE__);
	err = copy_from_user(kernel_buf, buf, MIN(1024, size));
	return MIN(1024, size);
}

static int hello_drv_open (struct inode *node, struct file *file)
{
	printk("%s %s line %d\n", __FILE__, __FUNCTION__, __LINE__);
	return 0;
}

static int hello_drv_close (struct inode *node, struct file *file)
{
	printk("%s %s line %d\n", __FILE__, __FUNCTION__, __LINE__);
	return 0;
}

/* 2. 定义自己的file_operations结构体                                              */
static struct file_operations hello_drv = {
	.owner	 = THIS_MODULE,
	.open    = hello_drv_open,
	.read    = hello_drv_read,
	.write   = hello_drv_write,
	.release = hello_drv_close,
};

/* 4. 把file_operations结构体告诉内核：注册驱动程序                                */
/* 5. 谁来注册驱动程序啊？得有一个入口函数：安装驱动程序时，就会去调用这个入口函数 */
static int __init hello_init(void)
{
	int err;
	
	printk("%s %s line %d\n", __FILE__, __FUNCTION__, __LINE__);
	major = register_chrdev(0, "hello", &hello_drv);  /* /dev/hello */


	hello_class = class_create(THIS_MODULE, "hello_class");
	err = PTR_ERR(hello_class);
	if (IS_ERR(hello_class)) {
		printk("%s %s line %d\n", __FILE__, __FUNCTION__, __LINE__);
		unregister_chrdev(major, "hello");
		return -1;
	}
	
	device_create(hello_class, NULL, MKDEV(major, 0), NULL, "hello"); /* /dev/hello */
	
	return 0;
}

/* 6. 有入口函数就应该有出口函数：卸载驱动程序时，就会去调用这个出口函数           */
static void __exit hello_exit(void)
{
	printk("%s %s line %d\n", __FILE__, __FUNCTION__, __LINE__);
	device_destroy(hello_class, MKDEV(major, 0));
	class_destroy(hello_class);
	unregister_chrdev(major, "hello");
}


/* 7. 其他完善：提供设备信息，自动创建设备节点                                     */

module_init(hello_init);
module_exit(hello_exit);

MODULE_LICENSE("GPL");
```

```c

#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

/*
 * ./hello_drv_test -w abc
 * ./hello_drv_test -r
 */
int main(int argc, char **argv)
{
	int fd;
	char buf[1024];
	int len;
	
	/* 1. 判断参数 */
	if (argc < 2) 
	{
		printf("Usage: %s -w <string>\n", argv[0]);
		printf("       %s -r\n", argv[0]);
		return -1;
	}

	/* 2. 打开文件 */
	fd = open("/dev/hello", O_RDWR);
	if (fd == -1)
	{
		printf("can not open file /dev/hello\n");
		return -1;
	}

	/* 3. 写文件或读文件 */
	if ((0 == strcmp(argv[1], "-w")) && (argc == 3))
	{
		len = strlen(argv[2]) + 1;
		len = len < 1024 ? len : 1024;
		write(fd, argv[2], len);
	}
	else
	{
		len = read(fd, buf, 1024);		
		buf[1023] = '\0';
		printf("APP read : %s\n", buf);
	}
	
	close(fd);
	
	return 0;
}

```



Makefile:

```makefile

# 1. 使用不同的开发板内核时, 一定要修改KERN_DIR
# 2. KERN_DIR中的内核要事先配置、编译, 为了能编译内核, 要先设置下列环境变量:
# 2.1 ARCH,          比如: export ARCH=arm64
# 2.2 CROSS_COMPILE, 比如: export CROSS_COMPILE=aarch64-linux-gnu-
# 2.3 PATH,          比如: export PATH=$PATH:/home/book/100ask_roc-rk3399-pc/ToolChain-6.3.1/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu/bin 
# 注意: 不同的开发板不同的编译器上述3个环境变量不一定相同,
#       请参考各开发板的高级用户使用手册

KERN_DIR = /home/book/tina-v853/lichee/linux-4.9

all:
	make -C $(KERN_DIR) M=`pwd` modules 
	$(CROSS_COMPILE)gcc -o hello_drv_test hello_drv_test.c 

clean:
	make -C $(KERN_DIR) M=`pwd` modules clean
	rm -rf modules.order
	rm -f hello_drv_test

obj-m	+= hello_drv.o

```

### 编译

```bash
book@100ask:~/workspace/V851sTest/hello_drv$ make
make -C /home/book/tina-v853/lichee/linux-4.9 M=`pwd` modules 
make[1]: Entering directory '/home/book/tina-v853/lichee/linux-4.9'
  CC [M]  /home/book/workspace/V851sTest/hello_drv/hello_drv.o
  Building modules, stage 2.
  MODPOST 1 modules
  CC      /home/book/workspace/V851sTest/hello_drv/hello_drv.mod.o
  LD [M]  /home/book/workspace/V851sTest/hello_drv/hello_drv.ko
make[1]: Leaving directory '/home/book/tina-v853/lichee/linux-4.9'
arm-openwrt-linux-gcc -o hello_drv_test hello_drv_test.c 
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
book@100ask:~/workspace/V851sTest/hello_drv$ adb push hello_drv.ko /
hello_drv.ko: 1 file pushed. 1.2 MB/s (5616 bytes in 0.005s)
book@100ask:~/workspace/V851sTest/hello_drv$ adb push hello_drv_test /
hello_drv_test: 1 file pushed. 1.7 MB/s (26024 bytes in 0.014s)
```



### 使用TF卡方式

将文件拷贝到TF卡中，将TF卡插入 柚木PI-蜥蜴 开发板中，正接至开发板中，启动系统后使用如下命令将TF卡挂载至tina系统上。我这里使用的是4G的内存卡，所以为/dev/mmcblk0p1，用户可以根据自己的设备号挂载对应的设备。

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
root@TinaLinux:/mnt# cp hello_drv.ko /
root@TinaLinux:/mnt# cp hello_drv_test /
```



## 运行

```bash
root@TinaLinux:~# cd /
root@TinaLinux:/# ls
bin             hello_drv_test  proc            run             var
config          helloworld      pseudo_init     sbin            www
data            home            ramparser       squashfs
dev             lib             rdinit          sys
etc             mnt             rom             tmp
hello_drv.ko    overlay         root            usr
root@TinaLinux:/# insmod hello_drv.ko
[  131.474038] hello_drv: loading out-of-tree module taints kernel.
[  131.481625] /home/book/workspace/V851sTest/hello_drv/hello_drv.c hello_init line 70
root@TinaLinux:/# chmod +x hello_drv_test
root@TinaLinux:/# ls /dev/hello
/dev/hello
root@TinaLinux:/# ./hello_drv_test  -w abc
[  172.500652] /home/book/workspace/V851sTest/hello_drv/hello_drv.c hello_drv_open line 45
[  172.509956] /home/book/workspace/V851sTest/hello_drv/hello_drv.c hello_drv_write line 38
[  172.519237] /home/book/workspace/V851sTest/hello_drv/hello_drv.c hello_drv_close line 51
root@TinaLinux:/# ./hello_drv_test  -r
[  180.906833] /home/book/workspace/V851sTest/hello_drv/hello_drv.c hello_drv_open line 45
[  180.916024] /home/book/workspace/V851sTest/hello_drv/hello_drv.c hello_drv_read line 30
APP read : abc[  180.925324] /home/book/workspace/V851sTest/hello_drv/hello_drv.c hello_drv_close line 51
```

