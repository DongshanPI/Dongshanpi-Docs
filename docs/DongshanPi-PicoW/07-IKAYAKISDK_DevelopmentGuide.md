# 使用IKAYAKI-SDK编译构建系统

## 1. IKAYAKI-SDK架构简介

### 1. 模块介绍

| **简称** | **全称**                              | **职责**                                                     |
| :------- | :------------------------------------ | :----------------------------------------------------------- |
| SYS      | System                                | 实现 MI 系统初始化、内存缓冲池管理、各个模块之间数据流的管理 |
| VDEC     | Video Decoder                         | H264/H265/JPEG 视频解码器                                    |
| DIVP     | Deinterlace&Video Post Process Engine | DIVP Engine有两个主要的功能 ①对解码后数据进行格式转换 ②解码后数据进行缩放 |
| VDISP    | Vitrual Display                       | 软件拼图                                                     |
| DISP     | Display Engine                        | DISP对VDEC/DIVP 处理单元输出的图像做硬件拼图，并连同AO输出音频信号一起编码成HDMI/VGA/CVBS 输出信号的单元 |
| VENC     | Video Encoder                         | H264/H265/MotionJpeg编码器                                   |
| AI       | Audio Input Interface                 | I2S audio input采集单元                                      |
| AO       | Audio Output Interface                | 音频输出                                                     |
| GFX      | Graphics Engine                       | Graphic Engine 提供对2D画图的基本硬件加速支援，降低CPU的负荷 |
| FB       | FrameBuffer                           | UI显示                                                       |
| HDMI     | High Definition Multimedia Interface  | HDMI/VGA标准输出                                             |

### 2. 软件架构

![](https://jsd.cdn.zzko.cn/gh/DongshanPI/Docs-Photos@master/DongshanPI-PicoW/IKAYAKI-SDK_sw_arch.png)

-   功能实现函数从上到下，分为MI API层，MI实现层，Hal硬件抽象层，Driver层和芯片硬件层。
-   SDK功能代码在Kernel层实现，减少从kernel到User mode来回调度，提高逻辑函数实现的效率。
-   对上层客户提供MI API的User Mode接口，用户层APP直接调用MI接口，即可调用到对应的MI功能。



### 3. SDK目录结构

| 目录    | 模块名         | 功能                                                 |
| :------ | :------------- | :--------------------------------------------------- |
| project | board          | PCB板信息存放路径                                    |
| project | configs        | 预配置文件存放路径                                   |
| project | image          | 产生镜像文件的材料库和镜像文件存放处                 |
| project | kbuild         | kernel编译环境                                       |
| project | release        | 目标池，存放对外头文件，库文件和内核模块以及第三方库 |
| SDK     | Verify/feature | 验证文件夹，里面存放模块单元测试和特性测试文件       |
| SDK     | Verify/demo    | 整体功能测试demo                                     |



### 4. 内存管理

详细参考[Memory Layout介绍](../reference/memory_layout.html)。

-   通过mmap.ini预留的内存
-   支持mma管理module内存，各个module从mma预留的size中分配

![](https://jsd.cdn.zzko.cn/gh/DongshanPI/Docs-Photos@master/DongshanPI-PicoW/IKAYAKI-SDK_memory_layout.png)



### 5. 基本概念

-   数据流：各个MI Module 可以看成是一个纯数据处理单元，数据流推送由MI SYS内部统一调度。输入数据流表示该数据单元的input数据，输出数据流表示该处理单元处理过的output数据。
-   控制流：APP 对各个MI Module 数据处理过程进行参数控制的过程，比如设置MI_VDEC解码参数，启动停止MI_VDEC 通道，设置MI_VDEC通道输出端口之分辨率及format等
-   Channel（通道）
    -   对于需要处理或者输出stream的MI模组，一个channel代表该MI 模组处理或者输出一路stream的分时复用的上下文（context）及相关控制流设定
    -   对于可分时复用之模组如MI_VDEC, MI_DIVP, MI_DISP，可支援多个channel
-   Port（端口）
    -   Port分为2种，input port和output port。input port为channel输入数据流的位置，而output port则是channel输出数据流的位置。
    -   一个channel可以有多个input port及多个output port.

![](https://jsd.cdn.zzko.cn/gh/DongshanPI/Docs-Photos@master/DongshanPI-PicoW/IKAYAKI-SDK_mi_flow.png)



## 2. 获取SDK工程

###   代码获取

​	使用浏览器打开链接  https://dongshanpi.cowtransfer.com/s/5c3c05deef7247 下载整个文件夹 IKAYAKI_SDK ，下载完成后可以看到里面包含了如下 6个 压缩包文件。

```bash
arm-sigmastar-linux-uclibcgnueabihf-9.1.0.tar.xz
gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf.tar.xz
boot-IKAYAKI_DLM00V015.tar.gz    
project-IKAYAKI_DLM00V015.tar.gz
kernel-IKAYAKI_DLM00V015.tar.gz  
sdk-IKAYAKI_DLM00V015.tar.gz
```
使用Filezila 或者samba 或拖拽等方式把这些文件拷贝到 Ubuntu系统家目录下。

拷贝之前 可以先 使用 mkdir DongshanPI-PicoW 创建一个板子的目录，之后再次创建一个  IKAYAKI_SDK  文件夹,如下命令中 $ 符号后的操作示例。

```shell
book@100ask.org:~$ mkdir DongshanPI-PicoW
book@100ask.org:~$ cd DongshanPI-PicoW/
book@100ask.org:~/DongshanPI-PicoW$ mkdir IKAYAKI_SDK
book@100ask.org:~/DongshanPI-PicoW$ cd IKAYAKI_SDK/
book@100ask.org:~/DongshanPI-PicoW/IKAYAKI_SDK$
```

最后 我们将下载好的 6个压缩包文件 拷贝至 创建好的 IKAYAKI_SDK 文件夹。拷贝完成后如下所示

```shell
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK$ ls -lh
total 1.3G
-rwxr--r-- 1 book book  40M Sep  9  2022 arm-sigmastar-linux-uclibcgnueabihf-9.1.0.tar.xz
-rwxr--r-- 1 book book  16M Sep  9  2022 boot-IKAYAKI_DLM00V015.tar.gz
-rwxr--r-- 1 book book 341M Sep  9  2022 gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf.tar.xz
-rwxr--r-- 1 book book 158M Sep  9  2022 kernel-IKAYAKI_DLM00V015.tar.gz
-rwxr--r-- 1 book book 452M Sep  9  2022 project-IKAYAKI_DLM00V015.tar.gz
-rwxr--r-- 1 book book 310M Sep  9  2022 sdk-IKAYAKI_DLM00V015.tar.gz
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK$
```

### 解压源码

接下来我们需要使用 tar 命令进行依次解压缩操作。

* 解压glibc工具链

```shell
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK$ tar -xvf  gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf.tar.xz
```

* 解压bootloader
```shell
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK$ tar -xvf  boot-IKAYAKI_DLM00V015.tar.gz
```


* 解压kernel
```shell
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK$ tar -xvf  kernel-IKAYAKI_DLM00V015.tar.gz
```

* 解压sdk
```shell
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK$ tar -xvf  project-IKAYAKI_DLM00V015.tar.gz
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK$ tar -xvf  sdk-IKAYAKI_DLM00V015.tar.gz
```

解压后的所有目录,这里会发现 boot kernel project sdk 目录都没有了 后面的 **-IKAYAKI_DLM00V015** 字样，这里请留意。

```bash
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK$ ls -lh
total 1.3G
dr-xr-xr-x  8 book book 4.0K Sep 16  2020 arm-sigmastar-linux-uclibcgnueabihf-9.1.0
-rwxr--r--  1 book book  40M Sep  9  2022 arm-sigmastar-linux-uclibcgnueabihf-9.1.0.tar.xz
drwxr-xr-x 21 book book 4.0K Dec 20  2021 boot
-rwxr--r--  1 book book  16M Sep  9  2022 boot-IKAYAKI_DLM00V015.tar.gz
drwxr-xr-x  8 book book 4.0K Jul 24  2020 gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf
-rwxr--r--  1 book book 341M Sep  9  2022 gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf.tar.xz
drwxr-xr-x 25 book book 4.0K Dec 20  2021 kernel
-rwxr--r--  1 book book 158M Sep  9  2022 kernel-IKAYAKI_DLM00V015.tar.gz
drwxr-xr-x 10 book book 4.0K Dec 20  2021 project
-rwxr--r--  1 book book 452M Sep  9  2022 project-IKAYAKI_DLM00V015.tar.gz
drwxr-xr-x  4 book book 4.0K Dec 20  2021 sdk
-rwxr--r--  1 book book 310M Sep  9  2022 sdk-IKAYAKI_DLM00V015.tar.gz
```



### 设置工具链

在上一步骤我们解压了 glibc交叉编译工具链，接下来需要设置一下环境变量，如下所示，首先进入到 `gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf`交叉编译工具链路径，之后继续进入到 bin路径，使用pwd命令显示当前的完整路径，`/home/book/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/bin`之后 通过设置 ~/.bashrc 来配置环境变量参数

```shell
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK$ cd gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf$ cd bin/
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/bin$ ls
arm-linux-gnueabihf-addr2line      arm-linux-gnueabihf-ld.bfd                      arm-linux-gnueabihf-sigmastar-9.1.0-gcov-dump
arm-linux-gnueabihf-ar             arm-linux-gnueabihf-ld.gold                     arm-linux-gnueabihf-sigmastar-9.1.0-gcov-tool
arm-linux-gnueabihf-as             arm-linux-gnueabihf-nm                          arm-linux-gnueabihf-sigmastar-9.1.0-gdb
arm-linux-gnueabihf-c++            arm-linux-gnueabihf-objcopy                     arm-linux-gnueabihf-sigmastar-9.1.0-gdb-add-index
arm-linux-gnueabihf-c++filt        arm-linux-gnueabihf-objdump                     arm-linux-gnueabihf-sigmastar-9.1.0-gfortran
arm-linux-gnueabihf-cpp            arm-linux-gnueabihf-ranlib                      arm-linux-gnueabihf-sigmastar-9.1.0-gprof
arm-linux-gnueabihf-dwp            arm-linux-gnueabihf-readelf                     arm-linux-gnueabihf-sigmastar-9.1.0-ld
arm-linux-gnueabihf-elfedit        arm-linux-gnueabihf-sigmastar-9.1.0-addr2line   arm-linux-gnueabihf-sigmastar-9.1.0-ld.bfd
arm-linux-gnueabihf-g++            arm-linux-gnueabihf-sigmastar-9.1.0-ar          arm-linux-gnueabihf-sigmastar-9.1.0-ld.gold
arm-linux-gnueabihf-gcc            arm-linux-gnueabihf-sigmastar-9.1.0-as          arm-linux-gnueabihf-sigmastar-9.1.0-nm
arm-linux-gnueabihf-gcc-9.1.0      arm-linux-gnueabihf-sigmastar-9.1.0-c++         arm-linux-gnueabihf-sigmastar-9.1.0-objcopy
arm-linux-gnueabihf-gcc-ar         arm-linux-gnueabihf-sigmastar-9.1.0-c++filt     arm-linux-gnueabihf-sigmastar-9.1.0-objdump
arm-linux-gnueabihf-gcc-nm         arm-linux-gnueabihf-sigmastar-9.1.0-cpp         arm-linux-gnueabihf-sigmastar-9.1.0-ranlib
arm-linux-gnueabihf-gcc-ranlib     arm-linux-gnueabihf-sigmastar-9.1.0-dwp         arm-linux-gnueabihf-sigmastar-9.1.0-readelf
arm-linux-gnueabihf-gcov           arm-linux-gnueabihf-sigmastar-9.1.0-elfedit     arm-linux-gnueabihf-sigmastar-9.1.0-size
arm-linux-gnueabihf-gcov-dump      arm-linux-gnueabihf-sigmastar-9.1.0-g++         arm-linux-gnueabihf-sigmastar-9.1.0-strings
arm-linux-gnueabihf-gcov-tool      arm-linux-gnueabihf-sigmastar-9.1.0-gcc         arm-linux-gnueabihf-sigmastar-9.1.0-strip
arm-linux-gnueabihf-gdb            arm-linux-gnueabihf-sigmastar-9.1.0-gcc-9.1.0   arm-linux-gnueabihf-size
arm-linux-gnueabihf-gdb-add-index  arm-linux-gnueabihf-sigmastar-9.1.0-gcc-ar      arm-linux-gnueabihf-strings
arm-linux-gnueabihf-gfortran       arm-linux-gnueabihf-sigmastar-9.1.0-gcc-nm      arm-linux-gnueabihf-strip
arm-linux-gnueabihf-gprof          arm-linux-gnueabihf-sigmastar-9.1.0-gcc-ranlib
arm-linux-gnueabihf-ld             arm-linux-gnueabihf-sigmastar-9.1.0-gcov
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/bin$ pwd
/home/book/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/bin
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/bin$
```

* 设置临时环境变量

​	此方式只在当前终端下有效，如果重新打开一个终端，需要重新设置执行这些配置

```bahs
export ARCH=arm
export CROSS_COMPILE=arm-linux-gnueabihf-
export PATH=$PATH:/home/book/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/bin
```

```bash
book@virtual-machine:~$ export ARCH=arm
book@virtual-machine:~$ export CROSS_COMPILE=arm-linux-gnueabihf-
book@virtual-machine:~$ export PATH=$PATH:/home/book/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/bin
```



* 设置永久环境变量

使用vim gedit nano等文本编辑器工具 打开 ~/.bashrc 在最底部添加 如下三行环境配置，即可永久生效。

```bash
export ARCH=arm
export CROSS_COMPILE=arm-linux-gnueabihf-
export PATH=$PATH:/home/book/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/bin
```



###  修改默认sh

如果默认是sh，需要将sh改成 **bash** 。

```
book@virtual-machine:~$ sudo rm /bin/sh
book@virtual-machine:~$ sudo ln –s /bin/bash /bin/sh
```

## 3. SDK开发包编译步骤

如下分别提到了“编译boot”，“编译kernel”，“编译sdk”，boot已经默认编译好放到project路径，因此刚开始验证的时候，可以直接跳过“编译boot”和“编译kernel”，直接从“编译sdk”（kernel的编译已经集成到编译sdk里面，默认会编译kernel）开始编译出来整个image，就可以烧写、验证了。

注意：不同的芯片原厂都有不同的开发框架和方式，请使用的同学不要纠结这个问题，按照文档操作就可以。
注意：不同的芯片原厂都有不同的开发框架和方式，请使用的同学不要纠结这个问题，按照文档操作就可以。
注意：不同的芯片原厂都有不同的开发框架和方式，请使用的同学不要纠结这个问题，按照文档操作就可以。

### 编译bootloader

1. 检查环境变量是否正确

在终端下输入  arm-linux-gnueabihf-gcc -v来验证是否正确，如果正常 会 打印出 gcc的版本信息等,如下所示，如果没有，请回到上一步配置。

```bash
book@virtual-machine:~$ arm-linux-gnueabihf-gcc -v
Using built-in specs.
COLLECT_GCC=arm-linux-gnueabihf-gcc
COLLECT_LTO_WRAPPER=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/bin/../libexec/gcc/arm-linux-gnueabihf/9.1.0/lto-wrapper
Target: arm-linux-gnueabihf
Configured with: '/home/meng/toolchain/build/snapshots/gcc.git~gcc-9_1_0-release/configure' SHELL=/bin/bash --with-mpc=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu --with-mpfr=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu --with-gmp=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu --with-gnu-as --with-gnu-ld --disable-libmudflap --enable-lto --enable-shared --without-included-gettext --enable-nls --with-system-zlib --disable-sjlj-exceptions --enable-gnu-unique-object --enable-linker-build-id --disable-libstdcxx-pch --enable-c99 --enable-clocale=gnu --enable-libstdcxx-debug --enable-long-long --with-cloog=no --with-ppl=no --with-isl=no --disable-multilib --with-float=hard --with-fpu=vfpv3-d16 --with-mode=thumb --with-arch=armv7-a --enable-threads=posix --enable-multiarch --enable-libstdcxx-time=yes --enable-gnu-indirect-function --with-build-sysroot=/home/meng/toolchain/build/sysroots/arm-linux-gnueabihf --with-sysroot=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu/arm-linux-gnueabihf/libc --enable-checking=yes --disable-bootstrap --enable-languages=c,c++,fortran,lto --build=x86_64-unknown-linux-gnu --host=x86_64-unknown-linux-gnu --target=arm-linux-gnueabihf --prefix=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu
Thread model: posix
gcc version 9.1.0 (GCC)
```

之后再重新在终端配置一下 ARCH CROSS_COMPILE 两个环境变量,设置完成后 就可以继续进行后续的开发操作了

```bash
export ARCH=arm
export CROSS_COMPILE=arm-linux-gnueabihf-
```



2. 进入bootloader目录

接下来进入到 boot 目录内，这个就是bootloader源码目录，进入目录后，我们就可以在这里面编译 bootloader 镜像了

```bash
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK$ cd boot/
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/boot$ ls
api     config.mk      doc       fs       lib             MAKEALL           mz           README                tatus
arch    configs        drivers   include  Licenses        Makefile          net          release_to_alkaid.sh  test
board   create_img.sh  dts       Kbuild   list_config.sh  mkimage           pad_file.py  scripts               tools
common  disk           examples  Kconfig  MAINTAINERS     ms_gen_mvxv_h.py  post         snapshot.commit
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/boot$
```



3. 编译bootloader

在开始编译之前 我们要先确认 配置文件名称，如下所示 DongshanPI-PicoW 默认的镜像配置 文件名称 为 pioneer3_spinand_defconfig 


| Flash Type | Other                         | Defconfig                          |
| :--------- | :---------------------------- | :--------------------------------- |
| SPI-NAND   | 基本                          | pioneer3_spinand_defconfig         |

接下来 使用 make  pioneer3_spinand_defconfig  来指定配置文件并开始编译，配置完成后 输入 make 命令即可开始编译，操作步骤如下所示，请留意 $ 符号后面才是命令。

```bash
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/boot$ make  pioneer3_spinand_defconfig
  HOSTCC  scripts/basic/fixdep
  HOSTCC  scripts/kconfig/conf.o
  SHIPPED scripts/kconfig/zconf.tab.c
  SHIPPED scripts/kconfig/zconf.lex.c
  SHIPPED scripts/kconfig/zconf.hash.c
  HOSTCC  scripts/kconfig/zconf.tab.o
  HOSTLD  scripts/kconfig/conf
#
# configuration written to .config
#
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/boot$ make -j8
......

{standard input}: Assembler messages:
{standard input}:1404: Warning: setting incorrect section attributes for .rodata
  CC      net/net.o
  CC      fs/fat/fat_write.o
  CC      fs/firmwarefs/fwfs_util.o
  CC      fs/firmwarefs/fwfs.o
  LD      common/built-in.o
  CC      net/ping.o
  CC      lib/xz/xz_dec_lzma2.o
  CC      fs/fat/file.o
  CC      fs/firmwarefs/fwfs_tftl.o
  CC      fs/firmwarefs/firmwarefs.o
  CC      net/tftp.o
  CC      lib/zlib/zlib.o
  CC      lib/crc7.o
  CC      lib/crc8.o
  CC      lib/crc16.o
  CC      lib/gunzip.o
  LD      net/built-in.o
  CC      lib/initcall.o
  CC      lib/lmb.o
  CC      lib/ldiv.o
  CC      lib/net_utils.o
  CC      lib/qsort.o
  CC      lib/strmhz.o
  CC      lib/rbtree.o
  CC      lib/list_sort.o
  CC      lib/hashtable.o
  LD      lib/xz/built-in.o
  CC      lib/errno.o
  CC      lib/display_options.o
  CC      lib/crc32.o
  CC      lib/ctype.o
  CC      lib/div64.o
  CC      lib/hang.o
  CC      lib/linux_compat.o
  CC      lib/linux_string.o
  CC      lib/string.o
  CC      lib/time.o
  CC      lib/vsprintf.o
  CC      lib/uminiz.o
  LD      fs/fat/built-in.o
  LD      lib/zlib/built-in.o
  LD      lib/built-in.o
  LD      fs/firmwarefs/built-in.o
  LD      fs/built-in.o
  CC      examples/standalone/stubs.o
  CC      examples/standalone/hello_world.o
  LD      examples/standalone/libstubs.o
  LD      examples/standalone/hello_world
  OBJCOPY examples/standalone/hello_world.srec
  OBJCOPY examples/standalone/hello_world.bin
  LD      u-boot
  OBJCOPY u-boot.srec
  OBJCOPY u-boot.bin
Mode: c, Level: 4
Input File: "u-boot.bin"
Output File: "u-boot.bin.mz"
Input file size: 731100
Total input bytes: 731100
Total output bytes: 343142
Success.
xz: u-boot.bin.xz: File already has `.xz' suffix, skipping

u-boot_spinand.mz.img.bin
Image Name:   MVX4##P3##g#######CM_UBT1501#XVM
Created:      Wed Apr 12 09:47:31 2023
Image Type:   ARM U-Boot Kernel Image (mz compressed)
Data Size:    343142 Bytes = 335.10 kB = 0.33 MB
Load Address: 23d00000
Entry Point:  23e00000


u-boot_spinand.xz.img.bin
Image Name:   MVX4##P3##g#######CM_UBT1501#XVM
Created:      Wed Apr 12 09:47:31 2023
Image Type:   ARM U-Boot Kernel Image (lzma compressed)
Data Size:    255572 Bytes = 249.58 kB = 0.24 MB
Load Address: 23d00000
Entry Point:  23e00000


u-boot_spinand.img.bin
Image Name:   MVX4##P3##g#######CM_UBT1501#XVM
Created:      Wed Apr 12 09:47:31 2023
Image Type:   ARM U-Boot Kernel Image (uncompressed)
Data Size:    731100 Bytes = 713.96 kB = 0.70 MB
Load Address: 23d00000
Entry Point:  23e00000

book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/boot$
```

编译完成的镜像 是 `u-boot_spinand.xz.img.bin` 这个是最终可以用来烧录的镜像文件。

一般情况下，没法单独只烧写一个 u-boot镜像，所以最好是 参考下面步骤 拷贝到 project 目录，做成一个完整的镜像 使用烧写工具烧写。




4. 拷贝打包使用

把编译出来的  `u-boot_spinand.xz.img.bin`，替换到`project/board/p3/boot/spinand/uboot/u-boot_spinand.xz.img.bin`，重新编译project即可生产一个完整系统镜像

```bash
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/boot$ cp u-boot_spinand.xz.img.bin ../project/board/p3/boot/spinand/uboot/
```




###   编译kernel

编译SDK会默认编译kernel，和SSD201/SSD202不同。因此建议修改相关kernel之后，直接在project下编译，会默认编译kernel的内容，并且不需要手动release到project下的路径。

1. 检查环境变量是否正确

在终端下输入  arm-linux-gnueabihf-gcc -v来验证是否正确，如果正常 会 打印出 gcc的版本信息等,如下所示，如果没有，请回到上一步配置。

```bash
book@virtual-machine:~$ arm-linux-gnueabihf-gcc -v
Using built-in specs.
COLLECT_GCC=arm-linux-gnueabihf-gcc
COLLECT_LTO_WRAPPER=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/bin/../libexec/gcc/arm-linux-gnueabihf/9.1.0/lto-wrapper
Target: arm-linux-gnueabihf
Configured with: '/home/meng/toolchain/build/snapshots/gcc.git~gcc-9_1_0-release/configure' SHELL=/bin/bash --with-mpc=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu --with-mpfr=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu --with-gmp=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu --with-gnu-as --with-gnu-ld --disable-libmudflap --enable-lto --enable-shared --without-included-gettext --enable-nls --with-system-zlib --disable-sjlj-exceptions --enable-gnu-unique-object --enable-linker-build-id --disable-libstdcxx-pch --enable-c99 --enable-clocale=gnu --enable-libstdcxx-debug --enable-long-long --with-cloog=no --with-ppl=no --with-isl=no --disable-multilib --with-float=hard --with-fpu=vfpv3-d16 --with-mode=thumb --with-arch=armv7-a --enable-threads=posix --enable-multiarch --enable-libstdcxx-time=yes --enable-gnu-indirect-function --with-build-sysroot=/home/meng/toolchain/build/sysroots/arm-linux-gnueabihf --with-sysroot=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu/arm-linux-gnueabihf/libc --enable-checking=yes --disable-bootstrap --enable-languages=c,c++,fortran,lto --build=x86_64-unknown-linux-gnu --host=x86_64-unknown-linux-gnu --target=arm-linux-gnueabihf --prefix=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu
Thread model: posix
gcc version 9.1.0 (GCC)
```

之后再重新在终端配置一下 ARCH CROSS_COMPILE 两个环境变量,设置完成后 就可以继续进行后续的开发操作了

```bash
export ARCH=arm
export CROSS_COMPILE=arm-linux-gnueabihf-
```



2. 进入Linux Kernel目录

接下来进入到 boot 目录内，这个就是bootloader源码目录，进入目录后，我们就可以在这里面编译 bootloader 镜像了

```bash
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK$ cd kernel/
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/kernel$ ls
arch     CREDITS        firmware  ipc      lib             Makefile        ms_pack_modules.sh           REPORTING-BUGS  sound
block    crypto         fs        Kbuild   list_config.sh  mm              net                          samples         tools
certs    Documentation  include   Kconfig  MAINTAINERS     modules         README                       scripts         usr
COPYING  drivers        init      kernel   makefile        Module.symvers  release_kernel_to_alkaid.sh  security        virt
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/kernel$

```



3. 编译LinuxKernel

在开始编译之前 我们要先确认 配置文件名称，如下所示 DongshanPI-PicoW 默认的镜像配置 文件名称 为 pioneer3_ssc021a_s01a_spinand_demo_defconfig 


| **Chip** | **Packaging** | **Memory** | **Flash Type** | **Toolchain** | **Other**  | **Defconfig**                                     |
| :------- | :------------ | :--------- | :------------- | :------------ | :--------- | :------------------------------------------------ |
| SSD210   | QFN68         | 64M        | SPI-NAND       | glibc         | 不带sensor | make pioneer3_ssc021a_s01a_spinand_demo_defconfig |


接下来 使用 make  pioneer3_ssc021a_s01a_spinand_demo_defconfig  来指定配置文件并开始编译，配置完成后 输入 make 命令即可开始编译，操作步骤如下所示，请留意 $ 符号后面才是命令。

```bash
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/kernel$ make pioneer3_ssc021a_s01a_spinand_demo_defconfig
Extract CHIP NAME (pioneer3) to '.sstar_chip.txt'
make[1]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/kernel'
  HOSTCC  scripts/basic/fixdep
  HOSTCC  scripts/kconfig/conf.o
  HOSTCC  scripts/kconfig/zconf.tab.o
  HOSTLD  scripts/kconfig/conf
#
# configuration written to .config
#
make[1]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/kernel'
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/kernel$ make -j8
make[1]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/kernel'
.....
changelist g
  BRANCHID   g
  CHK     include/generated/uapi/linux/version.h
  MS_PLATFORM_ID: P3
  CHK     include/generated/utsrelease.h
  CHK     include/generated/timeconst.h
  CHK     include/generated/bounds.h
  CHK     include/generated/asm-offsets.h
  CALL    scripts/checksyscalls.sh
  CHK     include/generated/compile.h
  CC      arch/arm/mach-sstar/ms_chip.o
  LD      arch/arm/mach-sstar/built-in.o
  LD      vmlinux.o
  MODPOST vmlinux.o
WARNING: modpost: Found 5 section mismatch(es).
To see full details build your kernel with:
'make CONFIG_DEBUG_SECTION_MISMATCH=y'
  GEN     .version
  CHK     include/generated/compile.h
  UPD     include/generated/compile.h
  CC      init/version.o
  LD      init/built-in.o
  KSYM    .tmp_kallsyms1.o
  KSYM    .tmp_kallsyms2.o
  LD      vmlinux
  SORTEX  vmlinux
  SYSMAP  System.map
  OBJCOPY arch/arm/boot/Image
  Building modules, stage 2.
  Kernel: arch/arm/boot/Image is ready
  MODPOST 35 modules
  DTC     arch/arm/boot/dts/pioneer3-ssc021a-s01a-demo.dtb
  DTC     arch/arm/boot/dts/pioneer3-fpga.dtb
  XZKERN  arch/arm/boot/compressed/piggy_data


  Packing modules to '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/kernel/modules'


  AS      arch/arm/boot/compressed/piggy.o
  LD      arch/arm/boot/compressed/vmlinux
  OBJCOPY arch/arm/boot/zImage
#update builtin DTB
  IMAGE   arch/arm/boot/Image
  BNDTB arch/arm/boot/dts/pioneer3-ssc021a-s01a-demo.dtb
offset:0x004103B0
  size:0x00009545

#update Image-fpga DTB
  IMAGE   arch/arm/boot/Image-fpga
  BNDTB   pioneer3-fpga.dtb
offset:0x004103B0
  size:0x00008904

  The configuration of GPIO & PADMUX is correct!
#build uImage
Image Name:   MVX4##P3##g#######KL_LX409##[BR:
Created:      Wed Apr 12 10:07:07 2023
Image Type:   ARM Linux Kernel Image (uncompressed)
Data Size:    4444160 Bytes = 4340.00 kB = 4.24 MB
Load Address: 20008000
Entry Point:  20008000

Compress Kernel Image
Image Name:   MVX4##P3##g#######KL_LX409##[BR:
Created:      Wed Apr 12 10:07:10 2023
Image Type:   ARM Linux Kernel Image (lzma compressed)
Data Size:    2120676 Bytes = 2070.97 kB = 2.02 MB
Load Address: 20008000
Entry Point:  20008000
Mode: c, Level: 4
Input File: "arch/arm/boot/Image"
Output File: "arch/arm/boot/Image.mz"
Input file size: 4444160
Total input bytes: 4444160
Total output bytes: 2577043
Success.

Image Name:   MVX4##P3##g#######KL_LX409##[BR:
Created:      Wed Apr 12 10:07:10 2023
Image Type:   ARM Linux Kernel Image (mz compressed)
Data Size:    2577043 Bytes = 2516.64 kB = 2.46 MB
Load Address: 20008000
Entry Point:  20008000


  Kernel: arch/arm/boot/zImage is ready
make[1]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/kernel'
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/kernel$

```

编译完成的镜像 是 `arch/arm/boot/uImage.xz` 这个是最终可以用来启动的内核镜像。
这个是设备树文件 arch/arm/boot/dts/pioneer3-ssc021a-s01a-demo.dtb

如果kernel 有新增kernel modules需要将相应的module添加到`kernel_mod_list/kernel_mod_list_late`（kernel_mod_list_late里的ko会在mi module之后装载）
修改路径：`project/kbuild/customize/$(KERNEL_VERSION)/$(CHIP)/$(PRODUCT)/kernel_mod_list`

注意： 如果编译kernel时提示 dtb打包错误，则需要参考 https://github.com/DongshanPI/Sigmastar-Linux/commit/dfbfa30b5dd564a05812d5ecd967a53e51749304 修改对应的源码，这个问题是SDK不兼容问题导致。

### 编译SDK

默认编译sdk时，会先`build kernel`，第一次编译通过`make clean;make image -j8`命令全部编译，后续如果不需要`build kernel`可以用`make image-fast`代替`make image`。

1. 检查环境变量是否正确

在终端下输入  arm-linux-gnueabihf-gcc -v来验证是否正确，如果正常 会 打印出 gcc的版本信息等,如下所示，如果没有，请回到上一步配置。

```bash
book@virtual-machine:~$ arm-linux-gnueabihf-gcc -v
Using built-in specs.
COLLECT_GCC=arm-linux-gnueabihf-gcc
COLLECT_LTO_WRAPPER=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/bin/../libexec/gcc/arm-linux-gnueabihf/9.1.0/lto-wrapper
Target: arm-linux-gnueabihf
Configured with: '/home/meng/toolchain/build/snapshots/gcc.git~gcc-9_1_0-release/configure' SHELL=/bin/bash --with-mpc=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu --with-mpfr=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu --with-gmp=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu --with-gnu-as --with-gnu-ld --disable-libmudflap --enable-lto --enable-shared --without-included-gettext --enable-nls --with-system-zlib --disable-sjlj-exceptions --enable-gnu-unique-object --enable-linker-build-id --disable-libstdcxx-pch --enable-c99 --enable-clocale=gnu --enable-libstdcxx-debug --enable-long-long --with-cloog=no --with-ppl=no --with-isl=no --disable-multilib --with-float=hard --with-fpu=vfpv3-d16 --with-mode=thumb --with-arch=armv7-a --enable-threads=posix --enable-multiarch --enable-libstdcxx-time=yes --enable-gnu-indirect-function --with-build-sysroot=/home/meng/toolchain/build/sysroots/arm-linux-gnueabihf --with-sysroot=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu/arm-linux-gnueabihf/libc --enable-checking=yes --disable-bootstrap --enable-languages=c,c++,fortran,lto --build=x86_64-unknown-linux-gnu --host=x86_64-unknown-linux-gnu --target=arm-linux-gnueabihf --prefix=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu
Thread model: posix
gcc version 9.1.0 (GCC)
```

之后再重新在终端配置一下 ARCH CROSS_COMPILE 两个环境变量,设置完成后 就可以继续进行后续的开发操作了

```bash
export ARCH=arm
export CROSS_COMPILE=arm-linux-gnueabihf-
```



2. 指定配置文件编译

进入到 project 目录下
```
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/project$ ls
board                                                      include                         make_sd_upgrade_sigmastar.sh   scripts
change_config_into_defconfig.sh                            kbuild                          make_usb_factory_sigmastar.sh  setup_config.sh
configs                                                    Kconfig                         make_usb_upgrade_sigmastar.sh  setup_defconfig.sh
dispcam_p3_nor.glibc-9.1.0-ramfs.s01a.64.qfn128_defconfig  make_emmc_upgrade_sigmastar.sh  parser_IPL.sh                  split_partion.sh
image                                                      makefile                        release                        tools
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/project$
```
执行 make dispcam_p3_spinand.glibc-9.1.0-s01a.64.qfn68.demo_defconfig 命令来开始配置

| **Chip** | **Packaging** | **Memory** | **Flash Type** | **Toolchain** | **Other**  | **Defconfig**                                                |
| -------- | ------------- | ---------- | -------------- | ------------- | ---------- | ------------------------------------------------------------ |
| SSD210   | QFN68         | 64M        | SPI-NAND       | glibc         | 不带sensor | make dispcam_p3_spinand.glibc-9.1.0-s01a.64.qfn68.demo_defconfig |

整个配置过程比较快。
```bash
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/project$ make dispcam_p3_spinand.glibc-9.1.0-s01a.64.qfn68.demo_defconfig
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
make -f /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/scripts/Makefile.build obj=scripts/basic
make[1]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project'
  gcc -Wp,-MD,scripts/basic/.fixdep.d -Wall -Wmissing-prototypes -Wstrict-prototypes -O2 -fomit-frame-pointer -std=gnu89     -o scripts/basic/fixdep scripts/basic/fixdep.c
make[1]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project'
make -f /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/scripts/Makefile.build obj=scripts/kconfig dispcam_p3_spinand.glibc-9.1.0-s01a.64.qfn68.demo_defconfig
make[1]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project'
  gcc -Wp,-MD,scripts/kconfig/.conf.o.d -Wall -Wmissing-prototypes -Wstrict-prototypes -O2 -fomit-frame-pointer -std=gnu89   -D_GNU_SOURCE -D_DEFAULT_SOURCE -I/usr/include/ncursesw -DCURSES_LOC="<ncurses.h>" -DNCURSES_WIDECHAR=1 -DLOCALE   -c -o scripts/kconfig/conf.o scripts/kconfig/conf.c
  gcc -Wp,-MD,scripts/kconfig/.zconf.tab.o.d -Wall -Wmissing-prototypes -Wstrict-prototypes -O2 -fomit-frame-pointer -std=gnu89   -D_GNU_SOURCE -D_DEFAULT_SOURCE -I/usr/include/ncursesw -DCURSES_LOC="<ncurses.h>" -DNCURSES_WIDECHAR=1 -DLOCALE  -Iscripts/kconfig -c -o scripts/kconfig/zconf.tab.o scripts/kconfig/zconf.tab.c
  gcc  -o scripts/kconfig/conf scripts/kconfig/conf.o scripts/kconfig/zconf.tab.o
#scripts/kconfig/conf  --defconfig=arch//configs/dispcam_p3_spinand.glibc-9.1.0-s01a.64.qfn68.demo_defconfig Kconfig
scripts/kconfig/conf  --defconfig=configs/defconfigs/dispcam_p3_spinand.glibc-9.1.0-s01a.64.qfn68.demo_defconfig Kconfig
#
# configuration written to .config
#
make[1]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project'
make silentoldconfig
make[1]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project'
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
make -f /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/scripts/Makefile.build obj=scripts/kconfig silentoldconfig
make[2]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project'
mkdir -p include/config include/generated
test -e include/generated/autoksyms.h || \
    touch   include/generated/autoksyms.h
scripts/kconfig/conf  --silentoldconfig Kconfig
make[2]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project'
make[1]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project'
PROJ_ROOT = /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project
CONFIG_NAME = config_module_list.mk
KBUILD_MK = kbuild/kbuild.mk
SOURCE_MK = ../sdk/sdk.mk
KERNEL_MEMADR = $(shell /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/makefiletools/bin/mmapparser /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/board/$(CHIP)/mmap/$(MMAP) $(CHIP) E_LX_MEM phyaddr)
KERNEL_MEMLEN = $(shell /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/makefiletools/bin/mmapparser /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/board/$(CHIP)/mmap/$(MMAP) $(CHIP) E_LX_MEM size)
KERNEL_MEMADR2 = $(shell /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/makefiletools/bin/mmapparser /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/board/$(CHIP)/mmap/$(MMAP) $(CHIP) E_LX_MEM2 phyaddr)
KERNEL_MEMLEN2 = $(shell /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/makefiletools/bin/mmapparser /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/board/$(CHIP)/mmap/$(MMAP) $(CHIP) E_LX_MEM2 size)
KERNEL_MEMADR3 = $(shell /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/makefiletools/bin/mmapparser /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/board/$(CHIP)/mmap/$(MMAP) $(CHIP) E_LX_MEM3 phyaddr)
KERNEL_MEMLEN3 = $(shell /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/makefiletools/bin/mmapparser /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/board/$(CHIP)/mmap/$(MMAP) $(CHIP) E_LX_MEM3 size)
LOGO_ADDR = Not support chip
P3 = y
CHIP = p3
DISP = y
PRODUCT = disp
NO_WIFI = y
SIGMA_WIFI = no_wifi
BOARD = 020A
BOARD_NAME = SSC021A-S01B
TOOLCHAIN_GLIBC = y
TOOLCHAIN = glibc
TOOLCHAIN_VERSION_9.1.0 = y
TOOLCHAIN_VERSION = 9.1.0
TOOLCHAIN_REL_ARM-LINUX-GNUEABIHF-SIGMASTAR-9.1.0 = y
TOOLCHAIN_REL = arm-linux-gnueabihf-sigmastar-9.1.0
BUSYBOX = busybox-1.20.2-arm-linux-gnueabihf-glibc-sigmastar-9.1.0-dynamic
KERNEL_VERSION_4.9.84 = y
KERNEL_VERSION = 4.9.84
KERNEL_CONFIG = pioneer3_ssc021a_s01a_spinand_demo_defconfig
KERNEL_BOOT_ENV = LX_MEM=0x3FE0000 mma_heap=mma_heap_name0,miu=0,sz=0x1E00000 cma=2M highres=off $(KERNEL_RESERVED_ENV)
EXBOOTARGS =
IMAGE_CONFIG = spinand.ubifs.partition.dualenv.dispcam.config
FLASH_SIZE_NONE = y
FLASH_SIZE =
MMAP = MMAP_P3_64.h
IQ0 = gc1054/gc1054_iqfile.bin
IQ1 = gc1054/gc1054_iqfile.bin
IQ2 = gc1054/gc1054_iqfile.bin
IQ3 = gc1054/gc1054_iqfile.bin
IQ_API0 = gc1054/gc1054_api.bin
IQ_API1 = gc1054/gc1054_api.bin
SENSOR_LIST =
SENSOR0 =
SENSOR0_OPT =
SENSOR1 =
SENSOR1_OPT =
SENSOR2 =
SENSOR2_OPT =
mi_dbg = y
MI_DBG = 1
SUPPORT_SMART_DISPLAY_VDEC = 0
support_msos_mpool_add_pa2varange = y
SUPPORT_MsOS_MPool_Add_PA2VARange = 1
support_disp_align_up_offset32 = y
SUPPORT_DISP_ALIGN_UP_OFFSET32 = 1
SUPPORT_HDMI_VGA_DIRECT_MODE = 0
SUPPORT_DIVP_USE_GE_SCALING_UP = 0
SUPPORT_VIF_USE_GE_FILL_BUF = 0
support_stub_device_for_test_sys = y
SUPPORT_STUB_DEVICE_FOR_TEST_SYS = 1
SUPPORT_VDEC_MULTI_RES = 0
SUPPORT_DIVP_P2_MODE = 0
FB_VIDEO = 0
CONFIG_MI_ENABLE_VB_POOL = 0
config_mi_enable_meta_pool = y
CONFIG_MI_ENABLE_META_POOL = 1
config_mi_enable_sys_cfg = y
CONFIG_MI_ENABLE_SYS_CFG = 1
CONFIG_MI_ENABLE_SHRINKABLE_POOL = 0
config_mi_enable_ringheap_pool = y
CONFIG_MI_ENABLE_RINGHEAP_POOL = 1
is_demo_board = y
IS_DEMO_BOARD = 3
config_mi_venc_enable_jpeg = y
CONFIG_MI_VENC_ENABLE_JPEG = 1
config_mi_venc_enable_h26x = y
CONFIG_MI_VENC_ENABLE_H26X = 1
FPGA = 0
BENCH = no
MHAL =
MERGE_BOOT =
BOOTLOGO_FILE =
LOGO_ADDR =
BOOTLOGO_BUFSIZE =
UPGRADE_FILE =
DISP_OUT_NAME = SAT070CP50
FBDEV =
WIFI_MODULE =
SPEECH_VENDOR =
ALINK =
DUAL_OS = off
FAST_DEMO = off
SENSOR_TYPE0 =
SENSOR_TYPE1 =
LINUX_MEM_SIZE =
RTOS_BIN =
RTOS_LOAD_ADDR =
MMA_BASE =
MMA_SIZE =
verify_ai = disable
verify_ao = disable
verify_cipher = disable
verify_disp = disable
verify_divp = disable
verify_dla = disable
verify_fb = disable
verify_gfx = disable
verify_hdmi = disable
verify_ipu = disable
verify_jpd = disable
verify_shadow = disable
verify_sys = disable
verify_uvc_uac = disable
verify_uac = disable
verify_vdec = disable
verify_vdisp = disable
verify_venc = disable
verify_vif = disable
verify_vpe = disable
verify_warp = disable
verify_wlan = disable
verify_mdb = disable
verify_mi_demo = disable
verify_mixer = disable
verify_py_ipu = disable
verify_zk_bootup = disable
verify_zk_mini = disable
verify_zk_full = disable
verify_zk_mini_nosensor = disable
verify_zk_mini_fastboot = disable
verify_zk_full_fastboot = disable
verify_zk_mini_nosensor_fastboot = disable
verify_qfn68_sensor_panel = disable
verify_barcode = disable
verify_disp_pic_fastboot = disable
verify_usbcamera_fastboot = disable
verify_usbcamera = disable
verify_usbcam_fastboot = disable
verify_barcodeYuyan = disable
verify_jpeg2disp = disable
INTERFACE_AI = y
interface_ai = enable
interface_alsa = disable
INTERFACE_AO = y
interface_ao = enable
interface_bar = disable
interface_cipher = disable
INTERFACE_COMMON = y
interface_common = enable
interface_cus3a = disable
INTERFACE_DISP = y
interface_disp = enable
INTERFACE_DIVP = y
interface_divp = enable
INTERFACE_PSPI = y
interface_pspi = enable
interface_foo = disable
INTERFACE_GFX = y
interface_gfx = enable
interface_gyro = disable
interface_hdmi = disable
interface_ipu = disable
interface_iqserver = disable
interface_isp = disable
interface_ispalgo = disable
interface_jpd = disable
interface_ldc = disable
interface_mipitx = disable
INTERFACE_PANEL = y
interface_panel = enable
INTERFACE_RGN = y
interface_rgn = enable
interface_sed = disable
interface_sensor = disable
interface_shadow = disable
INTERFACE_SYS = y
interface_sys = enable
interface_vdec = disable
interface_vdf = disable
interface_vdisp = disable
INTERFACE_VENC = y
interface_venc = enable
interface_vif = disable
interface_vpe = disable
interface_warp = disable
INTERFACE_WLAN = y
interface_wlan = enable
INTERFACE_FB = y
interface_fb = enable
misc_fbdev = disable
MISC_CONFIG_TOOL = y
misc_config_tool = enable
MHAL_AIO = y
mhal_aio = enable
MHAL_CMDQ_SERVICE = y
mhal_cmdq_service = enable
mhal_csi = disable
MHAL_DISP = y
mhal_disp = enable
MHAL_DIVP = y
mhal_divp = enable
mhal_earlyinit = disable
MHAL_GE = y
mhal_ge = enable
mhal_hdmitx = disable
MHAL_IMI_HEAP = y
mhal_imi_heap = enable
MHAL_INIT = y
mhal_init = enable
mhal_ipu = disable
mhal_isp = disable
mhal_ispalgo = disable
mhal_ispmid = disable
mhal_ispscl = disable
mhal_jpd = disable
mhal_ldc = disable
mhal_mipitx = disable
mhal_mload = disable
MHAL_NEON = y
mhal_neon = enable
MHAL_PANEL = y
mhal_panel = enable
MHAL_RGN = y
mhal_rgn = enable
mhal_sensorif = disable
MHAL_VCODEC = y
mhal_vcodec = enable
mhal_vif = disable
mhal_vpe = disable
ARCH=arm
CROSS_COMPILE=arm-linux-gnueabihf-sigmastar-9.1.0-
CHIP_ALIAS = ikayaki
CHIP_FULL_NAME = pioneer3
PREFIX =$(TOOLCHAIN_REL)-
AS = $(PREFIX)as
CC = $(PREFIX)gcc
CXX = $(PREFIX)g++
CPP = $(PREFIX)cpp
AR = $(PREFIX)ar
LD = $(PREFIX)ld
STRIP = $(PREFIX)strip
export ARCH CROSS_COMPILE
KERNEL_RESERVED_ENV = mmap_reserved=fb,miu=0,sz=0x300000,max_start_off=0x3C00000,max_end_off=0x3F00000
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/project$

```

3. 编译最终镜像
配置成功以后，在 project 目录下执行 make image命令可以开始编译操作。 等待编译完成，就会生成完整的可烧录镜像。
```bash
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/project$ make image
ln -sf /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/../kernel/modules /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/4.9.84/
ln -sf /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/../kernel/arch /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/4.9.84/
ln -sf /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/../kernel/drivers /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/4.9.84/
ln -sf /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/../kernel/include /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/4.9.84/
ln -sf /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/../kernel/scripts /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/4.9.84/
ln -sf /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/../kernel/Makefile /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/4.9.84/
ln -sf /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/../kernel/Module.symvers /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/4.9.84/
ln -sf /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/../kernel/.sstar_chip.txt /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/4.9.84/
ln -sf /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/../kernel/.config /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/4.9.84/
ln -sf /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/tools/usr /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/4.9.84/
ln -sf /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/customize/4.9.84/p3/disp /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/4.9.84/customize
make headfile_link
make[1]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project'
'/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/release/include/mi_isp_general.h' -> '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/release/include/isp/mi_isp_general.h'
......
Brief USAGE:
   /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/makefiletools/scri
                                        pt/../../build/mkfwfs  {-c
                                        <pack_dir>|-u <dest_dir>|-l} [-d
                                        <0-5>] [-a] [-m <number>] [-C
                                        <number>] [-D <number>] [-P
                                        <number>] [-e <number>] [-y
                                        <number>] [-X <number>] [-x
                                        <number>] [-w <number>] [-r
                                        <number>] [-B <number>] [-b
                                        <number>] [-p <number>] [-s
                                        <number>] [--] [--version] [-h]
                                        <image_file>

For complete USAGE and HELP type:
   /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/makefiletools/script/../../build/mkfwfs --help

image size 605253
'/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/board/p3/boot/spinand/partition/flash.sni' -> '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/flash.sni'
/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/makefiletools/bin/pnigenerator -c 1024 -s 0x20000 -a "0x140000(CIS),0x60000(IPL),0x60000(IPL_CUST),0xe0000(UBOOT)" -b "0x60000(IPL),0x60000(IPL_CUST),0xe0000(UBOOT)" -t "0x40000(ENV),0x40000(ENV1),0x20000(KEY_CUST),0x500000(KERNEL),0x500000(RECOVERY),0x600000(rootfs),0xa0000(MISC),-(UBI)" -o /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/partinfo.pni      \
                 > /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/partition_layout.txt
[[boot_nofsimage]]
make boot_images
make[4]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
cat /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/partition_layout.txt
BOOT0: 0x140000(CIS),0x60000(IPL),0x60000(IPL_CUST),0xe0000(UBOOT)
BOOT1: 0x60000(IPL),0x60000(IPL_CUST),0xe0000(UBOOT)
SYS: 0x40000(ENV),0x40000(ENV1),0x20000(KEY_CUST),0x500000(KERNEL),0x500000(RECOVERY),0x600000(rootfs),0xa0000(MISC),-(UBI)
FLASH HAS USED 0x8000000KB
ChkSum       : 8081
Magic        : SSTARSEMICIS0001
Checksum ok!!
IDX:         StartBlk:           BlkCnt:     Trunk/BkTrunk:     Active:     BBM:      Name:
  0:    0,(0000000000)   10,(0X00140000)                0/1           1      OFF        CIS
  1:   10,(0X00140000)    3,(0X00060000)                0/1           1      OFF        IPL
  2:   13,(0X001A0000)    3,(0X00060000)                0/1           1      OFF   IPL_CUST
  3:   16,(0X00200000)    7,(0X000E0000)                0/1           1      OFF      UBOOT
  4:   23,(0X002E0000)    3,(0X00060000)                1/0           0      OFF        IPL
  5:   26,(0X00340000)    3,(0X00060000)                1/0           0      OFF   IPL_CUST
  6:   29,(0X003A0000)    7,(0X000E0000)                1/0           0      OFF      UBOOT
  7:   36,(0X00480000)    2,(0X00040000)                0/0           1      OFF        ENV
  8:   38,(0X004C0000)    2,(0X00040000)                0/0           1      OFF       ENV1
  9:   40,(0X00500000)    1,(0X00020000)                0/0           1      OFF   KEY_CUST
 10:   41,(0X00520000)   40,(0X00500000)                0/0           1      OFF     KERNEL
 11:   81,(0X00A20000)   40,(0X00500000)                0/0           1      OFF   RECOVERY
 12:  121,(0X00F20000)   48,(0X00600000)                0/0           1      OFF     rootfs
 13:  169,(0X01520000)    5,(0X000A0000)                0/0           1      OFF       MISC
 14:  174,(0X015C0000)  850,(0X06A40000)                0/0           1      OFF        UBI
if [ "spinand" = "spinand" ]; then      \
                flash_page_offset=$[0x23]; flash_page_size=8; printf "%x: %02x" ${flash_page_offset} ${flash_page_size} | xxd -r - /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/flash.sni;     \
        loop=$[$(stat -c "%s" /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/flash.sni)/512-1]; for i in `seq 0 ${loop}`;do blk_pb0_off=$[${i}*512+0x2F]; blk_pb1_off=$[${i}*512+0x30]; printf "%x: %02x" ${blk_pb0_off} 10 | xxd -r - /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/flash.sni; printf "%x: %02x" ${blk_pb1_off} 23 | xxd -r - /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/flash.sni; done;    \
        loop=$[$(stat -c "%s" /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/flash_list.sni)/512-1]; for i in `seq 0 ${loop}`;do blk_pb0_off=$[${i}*512+0x2F]; blk_pb1_off=$[${i}*512+0x30]; printf "%x: %02x" ${blk_pb0_off} 10 | xxd -r - /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/flash_list.sni; printf "%x: %02x" ${blk_pb1_off} 23 | xxd -r - /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/flash_list.sni; done;     \
fi;
[================================================================/                                                                    ] 24/49  48%make ipl_mkboot ipl_cust_mkboot uboot_mkboot
make[5]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
[====================================================================================================================================/] 49/49 100%

Exportable Squashfs 4.0 filesystem, xz compressed, data block size 131072
        compressed data, compressed metadata, compressed fragments, compressed xattrs
        duplicates are removed
Filesystem size 1800.93 Kbytes (1.76 Mbytes)
        47.58% of uncompressed filesystem size (3784.79 Kbytes)
Inode table size 1492 bytes (1.46 Kbytes)
        9.64% of uncompressed inode table size (15477 bytes)
Directory table size 3458 bytes (3.38 Kbytes)
        57.20% of uncompressed directory table size (6045 bytes)
Number of duplicate files found 0
Number of inodes 414
Number of files 25
Number of fragments 5
Number of symbolic links  366
Number of device nodes 0
Number of fifo nodes 0
Number of socket nodes 0
Number of directories 23
Number of ids (unique uids + gids) 1
Number of uids 1
        root (0)
Number of gids 1
        root (0)
dd if=/dev/zero bs=131072 count=1 | tr '\000' '\377' > /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_mkboot;
dd if=/dev/zero bs=131072 count=1 | tr '\000' '\377' > /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_cust_mkboot;
dd if=/dev/zero bs=393216 count=1 | tr '\000' '\377' > /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/uboot_mkboot;
1+0 records in
1+0 records out
131072 bytes (131 kB, 128 KiB) copied, 0.000785679 s, 167 MB/s
1+0 records in
1+0 records out
131072 bytes (131 kB, 128 KiB) copied, 0.00157433 s, 83.3 MB/s
dd if=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/board/p3/boot/spinand/ipl/IPL_CUST.bin of=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_cust_mkboot bs=131072 count=1 conv=notrunc seek=0;
dd if=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/board/p3/boot/spinand/ipl/IPL.bin of=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_mkboot bs=131072 count=1 conv=notrunc seek=0;
1+0 records in
1+0 records out
393216 bytes (393 kB, 384 KiB) copied, 0.00284586 s, 138 MB/s
dd if=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/board/p3/boot/spinand/uboot/u-boot_dualenv_spinand.xz.img.bin of=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/uboot_mkboot bs=393216 count=1 conv=notrunc seek=0;
0+1 records in
0+1 records out
31760 bytes (32 kB, 31 KiB) copied, 0.000403474 s, 78.7 MB/s
if [ "3" != "" ]; then \
        for((Row=1;Row<3;Row++));do \
                dd if=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_cust_mkboot of=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_cust_mkboot bs=131072 count=1 conv=notrunc seek=${Row}; \
        done; \
fi;
0+1 records in
0+1 records out
29200 bytes (29 kB, 29 KiB) copied, 0.000393671 s, 74.2 MB/s
if [ "3" != "" ]; then \
        for((Row=1;Row<3;Row++));do \
                dd if=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_mkboot of=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_mkboot bs=131072 count=1 conv=notrunc seek=${Row}; \
        done; \
fi;
0+1 records in
0+1 records out
271312 bytes (271 kB, 265 KiB) copied, 0.000833338 s, 326 MB/s
if [ "1" != "" ]; then \
        for((Row=1;Row<1;Row++));do \
                dd if=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/uboot_mkboot of=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/uboot_mkboot bs=393216 count=1 conv=notrunc seek=${Row}; \
        done; \
fi;
1+0 records in
1+0 records out
131072 bytes (131 kB, 128 KiB) copied, 0.000951708 s, 138 MB/s
1+0 records in
1+0 records out
131072 bytes (131 kB, 128 KiB) copied, 0.000823161 s, 159 MB/s
1+0 records in
1+0 records out
131072 bytes (131 kB, 128 KiB) copied, 0.000792032 s, 165 MB/s
1+0 records in
1+0 records out
131072 bytes (131 kB, 128 KiB) copied, 0.000828601 s, 158 MB/s
make[5]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
cat /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_mkboot /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_cust_mkboot /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/uboot_mkboot > /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot.bin
rm -rfv /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_mkboot /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_cust_mkboot /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/uboot_mkboot
removed '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_mkboot'
removed '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_cust_mkboot'
removed '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/uboot_mkboot'
make[4]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
dd if=/dev/zero bs=2048 count=2 | tr '\000' '\377' > /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/cis.bin
2+0 records in
2+0 records out
4096 bytes (4.1 kB, 4.0 KiB) copied, 7.7572e-05 s, 52.8 MB/s
dd if=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/flash.sni of=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/cis.bin bs=2048 count=1 conv=notrunc seek=0
1+0 records in
1+0 records out
2048 bytes (2.0 kB, 2.0 KiB) copied, 0.000287638 s, 7.1 MB/s
dd if=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/partinfo.pni of=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/cis.bin bs=2048 count=1 conv=notrunc seek=1
0+1 records in
0+1 records out
2008 bytes (2.0 kB, 2.0 KiB) copied, 0.000260181 s, 7.7 MB/s
cat /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/flash_list.sni >> /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/cis.bin
make[3]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
make scripts
make[3]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
mkdir -p /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/scripts
make set_partition
make[4]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
make[4]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
make cis_spinand__script boot_spinand__script kernel_spinand__script rootfs_spinand_squashfs_script misc_spinand_fwfs_script miservice_spinand_ubifs_script customer_spinand_ubifs_script appconfigs_spinand_ubifs_script spinand_config_script
make[4]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
#@echo -e ubi create rootfs 0x600000\\n ubi create misc 0xa0000\\n ubi create miservice 0xC00000\\n ubi create customer 0x5000000\\n ubi create appconfigs 0x4C0000\\n >> /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/scripts/[[ipl.es
#@echo -e ubi create rootfs 0x600000\\n ubi create misc 0xa0000\\n ubi create miservice 0xC00000\\n ubi create customer 0x5000000\\n ubi create appconfigs 0x4C0000\\n >> /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/scripts/[[ipl.es
#@echo ubi create appconfigs 0x4C0000 >> /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/scripts/[[appconfigs.es
#@echo ubi create miservice 0xC00000 >> /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/scripts/[[miservice.es
#@echo -e ubi create rootfs 0x600000\\n ubi create misc 0xa0000\\n ubi create miservice 0xC00000\\n ubi create customer 0x5000000\\n ubi create appconfigs 0x4C0000\\n >> /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/scripts/[[ipl.es
#@echo ubi create customer 0x5000000 >> /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/scripts/[[customer.es
if [ -a ../parser_IPL.sh ] ; \
then \
        sh ../parser_IPL.sh /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/board/p3/boot/spinand/ipl/IPL.bin /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/scripts ;\
fi;
kernel-image done!!!
../parser_IPL.sh: line 1: warning: command substitution: ignored null byte in input
make[4]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
make[3]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
make[2]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
./split_partion.sh
split customer image
customer.es is not over size,do nothing!!
make[1]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project'
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/project$

```

打包错误解决
```
vim image/makefiletools/script/fwfs_pack.py
修改 120行 str(image_size) 为 str(int(image_size))
```

4.拷贝镜像烧写
编译完成后生成的image在`project/image/output/images`目录，我们把这些所有的镜像文件拷贝到 默认的系统镜像文件夹内，替换掉原来的，重启开发板进入烧录模式 就可以更新系统为你最新编译出来的系统镜像了。
```bash
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/project$ ls image/output/images/
appconfigs.ubifs     auto_update.txt  boot.bin  customer.ubifs  misc.fwfs        partition_layout.txt  scripts      usb_updater_boot.bin
auto_update_bin.txt  boot             cis.bin   kernel          miservice.ubifs  rootfs.sqfs           scripts_bin  usb_updater_ipl.bin
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/project$

```

