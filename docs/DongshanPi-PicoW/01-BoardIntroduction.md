[ [中文](https://dongshanpi.com/DongshanPi-PicoW/01-BoardIntroduction/) | [[Español]](https://dongshanpi.com/DongshanPi-PicoW/01-BoardIntroduction.ES) ]

* 开发板价格已经贴近生产价，只提供 原厂SDK的开发资料和文档，没有任何额外技术支持！
* 此开发板的任何问题都可以在我们的论坛交流讨论 https://forums.100ask.net/c/10-category/picow
# DongshanPI-PicoW开发板介绍

DongshanPI-PicoW 开发板使用 星宸科技 SSD210 + SSW101B usb WiFi组成，采用2.0mm排针+邮票孔设计，只需要5V供电输入就可以启动使用开发板系统。

## 最小板参数配置介绍

* SigMastar SSD210
  * ARM Cortex-A7 Dual Core 1 GHz with Neon and FPU
  * Max.two MIPI interfaces with 2 or 1 data lane and 2 clock lanes
  * TTL output up to 1280x800 60fps
  * MIC and DMIC inputs，lineout I2S TDM 8-channel, RX 2/4/8 channels, TX 2 channels
  * SDIO 2.0 x1
  * USB 2.0 x1
  * 64MB DDR2
  * Ethernet x1
  * Security Engines
  * SPI x2 I2C x2 UART x4 PWM x4
* SSW101B
  * 2.4GHz 1T1R USB Wifi
  * IEEE 802.11b/g/n
  * 32Bit RISC 150MHz  CPU
  * Support HT20/40MHz Bandwidth
* Winbond SPI NAND FLASH 128MB (W25Q128)
* Onseemi  USB 2.0 (480MBbps) Switch  (FSUSB30)

----

开发板外观示意图 

![](https://photos.100ask.net/dongshanpi-docs/DongshanPI-PicoW/DongshanPI-PicoW-TOP.png)



* 配套原理图：     https://forums.100ask.net/uploads/short-url/jM5L1WocV3O5xZRiRpLYRlEBVNg.pdf
* 默认系统镜像：  https://dongshanpi.cowtransfer.com/s/639100d687674c 



## 配套2.0mmTo2.54底板

如下图所示，将核心板  与 转接板 Pin脚对应，焊接至底板上，此底板可以连接至洞洞板上，通过如下方式，就可以实现7根线 加 micro usb头 + usb转串口工具实现开发。

![](https://photos.100ask.net/dongshanpi-docs/DongshanPI-PicoW/DongshanPI-PicoW-FlashUsb.png)



## 配套基础功能开发底板

预计下周上架！
