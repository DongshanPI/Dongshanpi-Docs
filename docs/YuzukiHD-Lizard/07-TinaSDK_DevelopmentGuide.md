# 使用Tina-SDK编译构建系统

## 简介

* 此套构建系统基于全志单核 Arm Cortex-A7 SoC，搭载了 RISC-V 内核的V851s  芯片，适配了Tina 5.0主线版本，是专为智能 IP 摄像机设计的，支持人体检测和穿越报警等功能。

## 获取sdk源码

待定。



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
book@virtual-machine:~/tina-v853$ source build/envsetup.sh
book@virtual-machine:~/tina-v853$ lunch 
2 #输入数字 选择v851s_perf1-tina
book@virtual-machine:~/tina-v853$ make 
book@virtual-machine:~/tina-v853$ pack
```

tina-v853/out/v851s-perf1/

### 烧写spi nand 最小系统镜像

编译完成后会在out/v851s-perf1/目录下输出 tina_v851s-perf1_uart0.img 文件，将文件拷贝到Windows系统下使用 使用 全志官方的  AllwinnertechPhoeniSuit 进行烧写。
详细烧写步骤请，请参考左侧 [快速启动](https://dongshanpi.com/DongshanNezhaSTU/03-QuickStart/#spi-nand) 页面。