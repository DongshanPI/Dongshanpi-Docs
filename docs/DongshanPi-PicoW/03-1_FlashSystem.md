# 更新系统镜像

## 使用USB烧录

DongshanPI-PicoW属于最小开发板，可以直接使用 5V电压输入 即可开始工作,可以参考下图右侧  蓝 黄 黑 红 四个箭头所示，使用一个 Micro USB模块 分别 连接至

* `P36 USB_DM`连接至 MicroUSB模块 `D-`信号上
* `P35 USB_DP`连接至 MicroUSB模块 `D+`信号上
* `P34 GND`连接至 MicroUSB模块 `GND` 信号上
* `P31 5V`连接至 MicroUSB模块 VBUS信号上

**注意：如果你之前有 使用串口的 5V供电，在这一章节，一定要把 串口的 5V供电先拔掉，再来接 MicroUSB的5V VBUS**

**注意：如果你之前有 使用串口的 5V供电，在这一章节，一定要把 串口的 5V供电先拔掉，再来接 MicroUSB的5V VBUS**

**注意：如果你之前有 使用串口的 5V供电，在这一章节，一定要把 串口的 5V供电先拔掉，再来接 MicroUSB的5V VBUS**

**注意： 一定要确保开发板与Micro模块 VBUS  GND与开发板的P31 P34 连接无误，才能通电，否则 会导致硬件损坏，我们不予处理。**



![](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanPI-PicoW/DongshanPI-PicoW-TOPUSBLine.png)



### 1. 连接MicroUSB至开发板

如下示意图，为实物连接图，请注意看下图 左下角 四根 杜邦线 与 绿色 MicroUSB 模块的连接顺序。

**注意： 一定要确保开发板与Micro模块 VBUS  GND与开发板的P31 P34 连接无误，才能通电，否则 会导致硬件与电脑短路损坏，此问题我们不予处理。**

**注意： 一定要确保开发板与Micro模块 VBUS  GND与开发板的P31 P34 连接无误，才能通电，否则 会导致硬件与电脑短路损坏，此问题我们不予处理。**

**注意： 一定要确保开发板与Micro模块 VBUS  GND与开发板的P31 P34 连接无误，才能通电，否则 会导致硬件与电脑短路损坏，此问题我们不予处理。**

![](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanPI-PicoW/DongshanPI-PicoW-FlashUsb.png)



### 2. 运行烧录工具

确保上述步骤开发板和Micro USB模块连接无误后，获取烧录工具和配套的镜像文件。

1. 下载默认的镜像和usb烧写工具，使用浏览器访问  https://dongshanpi.cowtransfer.com/s/639100d687674c 下载 DongshanPI-PicoW_DefaultImage.zip 下载完成后进行解压缩，解压完成后 可以看到文件夹内有如下文件,其中 **USBDownloadTool.exe** 是烧写工具，其他文件为系统所需镜像文件等。

``` bash
 AitUVCExtApi.dll
 USBDownloadTool.exe
 appconfigs.ubifs
 auto_update.txt
 auto_update_bin.txt
 boot/
 boot.bin
 cis.bin
 customer.ubifs
 kernel
 misc.fwfs
 miservice.ubifs
 partition_layout.txt
 rootfs.sqfs
 scripts/
 scripts_bin/
 u-boot.bin
 usb_updater.bin
 usb_updater_boot.bin
 usb_updater_ipl.bin
```

2. 双击 **USBDownloadTool.exe** 软件，打开软件，会弹出一个对话框,如下图所示，接下来需要参考后续步骤 将板子通过短接 `GND M4` 进入USB 烧录模式 来开始进行烧录操作。

![](https://jsd.cdn.zzko.cn/gh/DongshanPI/Docs-Photos@master/DongshanPI-PicoW/DongshanPI-PicoW-UsbDownTools.png)

### 3. 开发板进入烧录模式

参考下图所示步骤，

1. 使用镊子塞入 板子箭头所示 的`GND M4`孔内，然后捏紧，不要松开

2. 另一个手 按一下 板子中间的白色按钮，
3. 此时烧录工具会自动提示  MSDC Connected 就表示设备已经连接
4.  最后，点击 **Upgrade Firmware** 来烧录固件

![](H:\DongshanPI-PicoW\2023-04-10_在线资料整理\在线文档\DongshanPI-D1s\03-1_FlashSystem.assets\DongshanPI-PicoW-UsbDown2.png)

参考下图操作示意图

![](https://jsd.cdn.zzko.cn/gh/DongshanPI/Docs-Photos@master/DongshanPI-PicoW/DongshanPI-PicoW-UsbDown3.png)



### 4. 开始烧录

点击 USBDonwloadTool 烧写软件的 **Upgrade Firmware**  即可开始烧录，等待烧录完成即可。烧录完成会弹出 RESET 提示对话框，此时按下 开发板中间的 白色按钮重启开发板即可。

![](https://jsd.cdn.zzko.cn/gh/DongshanPI/Docs-Photos@master/DongshanPI-PicoW/DongshanPI-PicoW-UsbDown4.png)



### 5. 烧录完成后设置

烧录完成后，如果无法启动则需要在uboot 命令行中增加如下env参数，目前初步认为是烧写工具的bug导致的 具体原因未知。 开机后长按 enter 进入 uboot 命令行，输入如下配置，重启开发板即可。

``` bash
setenv mtdids nand0=nand0
setenv mtdparts ' mtdparts=nand0:0x140000(CIS),0x1a0000(BOOT0),0x1a0000(BOOT1),0x40000(ENV),0x40000(ENV1),0x20000(KEY_CUST),0x500000(KERNEL),0x500000(RECOVERY),0x600000(rootfs),0xa0000(MISC),-(UBI)
setenv bootargs ubi.mtd=UBI,0x800 root=/dev/mtdblock8 rootfstype=squashfs ro init=/linuxrc LX_MEM=0x3FE0000 mma_heap=mma_heap_name0,miu=0,sz=0x1E00000 cma=2M highres=off mmap_reserved=fb,miu=0,sz=0x300000,max_start_off=0x3C00000,max_end_off=0x3F00000 ${mtdparts}
setenv bootcmd ' nand read.e 0x22000000 KERNEL ${kernel_file_size}; dcache on ; bootlogo 0 0 0 0; bootm 0x22000000;nand read.e 0x22000000 RECOVERY ${recovery_file_size}; dcache on ; bootm 0x22000000
setenv autoestart 0
setenv sstar_bbm off
setenv ipl_version "##p3##gdf99011IPL_##########setenv ipl_version "DUALENV=1 SILENT_CONSOLE=1 CFG_SDMMC_DISABLE=n ALK=1 SPINAND=1 CHIP=pioneer3""
saveenv
```



