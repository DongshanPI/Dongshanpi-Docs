# 使用Tina-SDK编译构建系统

## 简介

## 获取sdk源码

开始之前我们需要先获取 提前准备好 tina-sdk压缩包，压缩包分为国内国外两个存放位置，如下所示，大小大概9G，下载完成后，拷贝到提前配置好Host开发环境的ubuntu系统内，然后参考 下载的目录内的README.txt文档 进行解压缩。

获取Tina-sdk V2.0源码 百度网盘获取地址 链接：https://pan.baidu.com/s/13uKlqDXImmMl9cgKc41tZg?pwd=qcw7 提取码：qcw7 压缩包路径在 Tina-SDK_DevelopLearningKits-V1/DongshanNezhaSTU-TinaV2.0-SDK
拷贝进Ubuntu系统内，并进行解压缩,解压命令在README里面


解压缩命令

```bash
cat tina-d1-h.tar.gz*| tar zx
```
解压完成后，可以看到多出来一个 tina-d1-h的文件夹

```bash
book@100ask:~/tina-d1-h$ ls  
build  config  Config.in  device  dl  lichee  Makefile  out  package  prebuilt  README.md  rules.mk  scripts  target  TinaAddons  tmp  toolchain  tools

```

由于默认的sdk并未支持此开发板，所以我们需要支持此开发板的配置 单独拷贝增加到DongshanPI-D1s sdk内，首先clone此开发板补丁仓库，然后单独覆盖。

```bash
book@100ask:~$ git clone  https://gitee.com/weidongshan/DongshanPI-D1s_TinaSDK.git
book@100ask:~$ cp -rfvd  DongshanPI-D1s_TinaSDK/* tina-d1-h/
```

## 安装必要依赖包

### ubuntu-18.04

运行环境配置： 此系统基于ubuntu18.04进行验证，在之前的基础之上还需要安装以下必要依赖

```bash
 sudo apt-get install -y  libncurses5-dev   u-boot-tools
```

安装完成后，执行如下命令进行开始编译操作。


## 最小系统编译烧写

### 编译spi nor最小系统镜像

```bash
book@ubuntu1804:~/tina-d1-h$ source build/envsetup.sh 
Setup env done! Please run lunch next.
book@ubuntu1804:~/tina-d1-h$ lunch 

You're building on Linux

Lunch menu... pick a combo:
     1. d1-h_nezha_min-tina
     2. d1-h_nezha-tina
     3. d1s_nezha_nand-tina
     4. d1s_nezha_nor-tina
     5. d1s_nezha_sd-tina
     6. d1s_nezha-tina

Which would you like? [Default d1s_nezha]: 4
============================================
TINA_BUILD_TOP=/home/book/tina-d1-h
TINA_TARGET_ARCH=riscv
TARGET_PRODUCT=d1s_nezha_nor
TARGET_PLATFORM=d1s
TARGET_BOARD=d1s-nezha_nor
TARGET_PLAN=nezha_nor
TARGET_BUILD_VARIANT=tina
TARGET_BUILD_TYPE=release
TARGET_KERNEL_VERSION=5.4
TARGET_UBOOT=u-boot-2018
TARGET_CHIP=sun20iw1p1
============================================
no buildserver to clean
[1] 21161
book@ubuntu1804:~/tina-d1-h$ 

```

### 烧写SPINor最小系统镜像

编译完成后会在 out/d1s-nezha_nor/目录下输出 tina_d1s-nezha_nor_uart0_nor.img 文件，将文件拷贝到Windows系统下使用 使用 全志官方的  AllwinnertechPhoeniSuit 进行烧写。
详细烧写步骤请，请参考左侧 [更新系统](https://dongshanpi.com/DongshanPI-D1s/03-1_FlashSystem/#spinor) 页面。


### 编译TFCard最小系统镜像
``` bash
book@ubuntu1804:~/tina-d1-h$ source build/envsetup.sh 
Setup env done! Please run lunch next.
book@ubuntu1804:~/tina-d1-h$ lunch 

You're building on Linux

Lunch menu... pick a combo:
     1. d1-h_nezha_min-tina
     2. d1-h_nezha-tina
     3. d1s_nezha_nand-tina
     4. d1s_nezha_nor-tina
     5. d1s_nezha_sd-tina
     6. d1s_nezha-tina

Which would you like? [Default d1s_nezha]: 5
============================================
TINA_BUILD_TOP=/home/book/tina-d1-h
TINA_TARGET_ARCH=riscv
TARGET_PRODUCT=d1s_nezha_sd
TARGET_PLATFORM=d1s
TARGET_BOARD=d1s-nezha_sd
TARGET_PLAN=nezha_sd
TARGET_BUILD_VARIANT=tina
TARGET_BUILD_TYPE=release
TARGET_KERNEL_VERSION=5.4
TARGET_UBOOT=u-boot-2018
TARGET_CHIP=sun20iw1p1
============================================
clean buildserver
[1] 3278
book@ubuntu1804:~/tina-d1-h$ 

```

### 烧写TF Card最小系统镜像

编译完成后会在 out/d1s-nezha_sd/目录下输出 tina_d1s-nezha_sd_uart0.img 文件，将文件拷贝到Windows系统下使用 PhoenixCard烧写，烧写成功之后插到开发板上，设置好拨码开关即可启动。 请参考左侧  [更新系统](https://dongshanpi.com/DongshanPI-D1s/03-1_FlashSystem/#tf) 页面。
