# 东山Pi柒号-开发板
> 东山PI柒号开发板是基于 STM32MP157DAC 设计的一款最小开发板。其中核心板使用的是米尔的DDR512MB  EMMC4GB配置，最小底板引出了一些常用接口，比如typec 调试供电，typec OTG系统烧写接口，Tf卡接口 TypeA 立式usb接口，千兆以太网接口，同时还专门引出了 RGB888 以及MIPI DSI显示接口，最后，依旧在最小板上保留了M4调试接口。

## 硬件描述
> 如下图所示， DongshanPISeven 开发板功能位置示例图。
![DongshanPi-Seven01_BoardIntroduction-001](https://cdn.jsdelivr.net/gh/DongshanPI/Docs-Photos@master/DongshanPi-Seven01_BoardIntroduction-001.webp)

* 开发板规格
 开发板大小长宽约100mm x 70mm，为了方便连接使用了SODIMM接口。

* 核心板规格：核心板所有的芯片都在屏蔽罩下面，可以取下屏蔽罩看到。
  SOC主控: STM32MP157DAC （双核CorteX A7 800Mhz  + 209Mhz M4 + 3D　GPU ）
  DDR：  内置512MB DDR3
  存储:  EMMC 4GB FLASH
  网络： 千兆PHY芯片。