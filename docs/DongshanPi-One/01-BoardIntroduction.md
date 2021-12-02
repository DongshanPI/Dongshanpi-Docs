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

## 软件资源描述

### 芯片原厂配套SDK

* 系统版本
  * bootloader uboot 2015 
  * kernel 4.9
  * busybox 1.24
  * toolchain gcc 8.2

详细的获取使用方式 请参考后续 开发入门章节内

### Linux社区版本SDK



