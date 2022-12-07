# 使用Tina-SDK编译构建系统

## 简介

* 此套构建系统基于全志单核 Arm Cortex-A7 SoC，搭载了 RISC-V 内核的V851s  芯片，适配了Tina 5.0主线版本，是专为智能 IP 摄像机设计的，支持人体检测和穿越报警等功能。

## 获取sdk源码

开始之前我们需要先获取 提前准备好 tina-sdk压缩包，压缩包分为国内国外两个存放位置，如下所示，大小大概11G，下载完成后，拷贝到提前配置好Host开发环境的ubuntu系统内，然后参考 下载的目录内的README.txt文档 进行解压缩。

- GoogleDriver：  https://drive.google.com/drive/folders/1_HAZRddR69hRMZAVrxFrPZXFFQiV3vE0?usp=share_link
- BaiduYun:   https://pan.baidu.com/s/115gVK-8Pt-vJi8jn2AWMYw?pwd=7n4q  提取码：7n4q 
  

解压缩命令

```bash
cat tina-v853-open.tar.gz*| tar zx
```

解压完成后，可以看到多出来一个 tina-v853-open的文件夹

```bash
book@100ask:~/tina-v853-open$ ls
brandy  build  buildroot  build.sh  device  kernel  openwrt  platform  prebuilt  README.md  target  tools
```

由于默认的sdk并未支持此开发板，所以我们需要支持此开发板的配置 单独拷贝增加到tina-v853-open sdk内，首先clone此开发板补丁仓库，然后单独覆盖。

```bas
book@100ask:~$ git clone  https://github.com/DongshanPI/Yzukilizard-v851s-TinaSDK
book@100ask:~$ cp -rfvd  Yuzukilizard-v851s-TinaSDK/* tina-v853-open/
```



## 安装必要依赖包

### ubuntu-18.04

运行环境配置： 此系统基于ubuntu18.04进行验证，在之前的基础之上还需要安装以下必要依赖

```bash
 sudo apt-get install -y  libncurses5-dev   u-boot-tools
```

安装完成后，执行如下命令进行开始编译操作。




## 最小系统编译烧写

### 编译spi nand最小系统镜像

```bash
book@100ask:~/tina-v853$ source build/envsetup.sh 

You're building on Linux

Lunch menu... pick a combo:
     1	v851s-lizard-tina
     2	v853-vision-tina
Which would you like?: 1 #输入数字1 选择v851s-lizard-tina
Jump to longan autoconfig
/home/book/workspace/tina-v853/build.sh autoconfig -o openwrt -i v851s -b lizard 		-n default 

Before using this files, please make sure that you note the following important information.
**********************************************************************

Copyright (c) 2019-2022 Allwinner Technology Co., Ltd. ALL rights reserved.

Allwinner is a trademark of Allwinner Technology Co.,Ltd., registered in
the the People's Republic of China and other countries.
All Allwinner Technology Co.,Ltd. trademarks are used with permission.

DISCLAIMER
THIRD PARTY LICENCES MAY BE REQUIRED TO IMPLEMENT THE SOLUTION/PRODUCT.
IF YOU NEED TO INTEGRATE THIRD PARTY'S TECHNOLOGY (SONY, DTS, DOLBY, AVS OR MPEGLA, ETC.)
IN ALLWINNERS'SDK OR PRODUCTS, YOU SHALL BE SOLELY RESPONSIBLE TO OBTAIN
ALL APPROPRIATELY REQUIRED THIRD PARTY LICENCES.
ALLWINNER SHALL HAVE NO WARRANTY, INDEMNITY OR OTHER OBLIGATIONS WITH RESPECT TO MATTERS
COVERED UNDER ANY REQUIRED THIRD PARTY LICENSE.
YOU ARE SOLELY RESPONSIBLE FOR YOUR USAGE OF THIRD PARTY'S TECHNOLOGY.


THIS SOFTWARE IS PROVIDED BY ALLWINNER"AS IS" AND TO THE MAXIMUM EXTENT
PERMITTED BY LAW, ALLWINNER EXPRESSLY DISCLAIMS ALL WARRANTIES OF ANY KIND,
WHETHER EXPRESS, IMPLIED OR STATUTORY, INCLUDING WITHOUT LIMITATION REGARDING
THE TITLE, NON-INFRINGEMENT, ACCURACY, CONDITION, COMPLETENESS, PERFORMANCE
OR MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE.
IN NO EVENT SHALL ALLWINNER BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT
NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
LOSS OF USE, DATA, OR PROFITS, OR BUSINESS INTERRUPTION)
HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT,
STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED
OF THE POSSIBILITY OF SUCH DAMAGE.

**********************************************************************
You can read /home/book/workspace/tina-v853/build/disclaimer/Allwinnertech_Disclaimer(Cn_En)_20181122.md for detailed information. 

You read time left 8 seconds....
I have already read, understood and accepted the above terms? [Y/N]y #输入y
You select Yes, Build continue....
========ACTION List: mk_autoconfig -o openwrt -i v851s -b lizard -n default;========
options : 
INFO: Prepare toolchain ...
INFO: kernel defconfig: generate /home/book/workspace/tina-v853/kernel/linux-4.9/.config by /home/book/workspace/tina-v853/device/config/chips/v851s/configs/lizard/linux-4.9/config-4.9
INFO: Prepare toolchain ...
make: Entering directory '/home/book/workspace/tina-v853/kernel/linux-4.9'
  HOSTCC  scripts/basic/fixdep
  HOSTCC  scripts/kconfig/conf.o
  SHIPPED scripts/kconfig/zconf.tab.c
  SHIPPED scripts/kconfig/zconf.lex.c
  SHIPPED scripts/kconfig/zconf.hash.c
  HOSTCC  scripts/kconfig/zconf.tab.o
  HOSTLD  scripts/kconfig/conf
*** Default configuration is based on '../../../../../device/config/chips/v851s/configs/lizard/linux-4.9/config-4.9'
#
# configuration written to .config
#
make: Leaving directory '/home/book/workspace/tina-v853/kernel/linux-4.9'
INFO: clean buildserver

Usage:
 kill [options] <pid> [...]

Options:
 <pid> [...]            send signal to every <pid> listed
 -<signal>, -s, --signal <signal>
                        specify the <signal> to be sent
 -l, --list=[<signal>]  list all signal names, or convert one to a name
 -L, --table            list all signal names in a nice table

 -h, --help     display this help and exit
 -V, --version  output version information and exit

For more details see kill(1).
INFO: prepare_buildserver
book@100ask:~/tina-v853$ make
...
book@100ask:~/tina-v853$ pack
...
```



### 烧写spi nand 最小系统镜像

编译完成后会在tina-v853/out/v851s/lizard/openwrt/目录下输出 v851s_linux_lizard_uart0.img 文件，将文件拷贝到Windows系统下使用 使用 全志官方的  AllwinnertechPhoeniSuit 进行烧写。
详细烧写步骤请，请参考左侧 [快速启动](https://dongshanpi.com/DongshanNezhaSTU/03-QuickStart/#spi-nand) 页面。





