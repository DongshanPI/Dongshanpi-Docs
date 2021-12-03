# 东山Pi壹号-开发板

> 东山Pi壹号开发板是联合芯片原厂星辰科技一起推的最小Linux开发板，使用的是SSD202D主控芯片，最小开发板接口只保留了几个常见接口 包含LED灯 按键 FPCRGB显示接口 以及一路TYPE C USBHOST接口 还有一个专门用来供电和调试二合一的 TypeC接口，背面同时增加了SD卡卡槽 也保留了专门的烧录接口。

## 硬件描述

* 最小开发板规格

    开发板大小参考了标准的MINI PCI-E规格，在此基础上针对芯片和器件做了相应的调整，大概尺寸是宽3.8cm x 长5.1cm。

* 最小开发板器件布局

![image-20211202174358899](http://photos.100ask.net/dongshanpi/one/BoardIntroduction-01.png)

* 最小开发板规格参数
  * 主控芯片： 星辰科技 SSD202D 内置128MB DDR 支持H264/H265解码 支持MJPG编码。
  * 存储：板载128MB SPI NAND FLASH芯片 以及专门的SD card接口
  * LED灯：红色x1 表示pwr  蓝色 绿色 均为用户灯。
  * Key：硬件复位按键x1  用户按键x1
  * 显示：50Pin FPC RGB888显示输出
  * 供电&调试：板载专用USB转TTL芯片同时给整个板子供电。
  * usbHost:  TypeC接口的USB HOST 支持连接支持USB协议的设备。
  * 扩展接口： 使用MINI-PCI-E接口 用于连接底板。

## 开发资源描述

### 芯片原厂配套SDK
> 系统版本介绍,目前整套SDK基于星辰科技芯片原厂释放出来的 TAKOYAKI_DLC00V030 版本进行适配讲解。

| 名称        | 版本     |
| :----------- | :------- |
|  bootloader版本: | 深度定制化uboot |
| kernel版本: | 深度定制化的 LinuxKernel 4.9|
 busybox  | 1.24 |
 C库：| 提供了musl glibc库支持|
 toolchain版本：| arm-linux-gnueabihf- 8.2 |

* 芯片原厂SDK的优势: 

    这套框架是星辰科技针对于产品方案 项目方案等专门进行调整过的sdk 里面包含了许多实际产品中的demo示例，如果我们想快速落地产品 可以使用此套sdk。

* 芯片原厂SDK的劣势

    由于这套SDK是为了做产品，所以在此基础上做了很多框架性的改动，和我们常见的Linux开发有一定的区别，需要配合着原厂的文档才可以使用。

* 详细的获取及开发方式请参考后续 **原厂SDK开发入门** 章节。

### Linux社区版本SDK
> 社区版本使用的是基于 主线LinuxKernel Uboot 以及Buildroot进行开发。

* 详细的获取及开发方式请参考 **社区版本开发** 章节。

  



