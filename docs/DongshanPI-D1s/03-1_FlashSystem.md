# 快速开始使用

## 烧写固件至SPINor
### 准备工作
* 硬件：DongshanPI-D1s主板 x1
* 硬件：TypeC线 X2
* 软件：全志线刷工具：[AllwinnertechPhoeniSuit](https://gitlab.com/dongshanpi/tools/-/raw/main/AllwinnertechPhoeniSuit.zip)
* 软件：SPI Nor系统镜像：[tina_d1s-nezha_nor_uart0_nor](https://gitlab.com/dongshanpi/tools/-/raw/main/tina_d1s-nezha_nor_uart0_nor.zip)
* 软件：全志USB烧录驱动：[AllwinnerUSBFlashDeviceDriver](https://gitlab.com/dongshanpi/tools/-/raw/main/AllwinnerUSBFlashDeviceDriver.zip)

### 连接开发板
参考下图所示，

![DongshanPI-D1s-V2TopFuction](https://photos.100ask.net/dongshanpi-docs/d1s/DongshanPI-D1s-V2TopFuction.png){ width="600" }

将两个TypeC线分别连至DongshanPI-D1s开发板 `黑色仔细序号 3.OTG烧录接口 ` `黑色字体序号 4.调试&串口接口`  Typec线另一端 连接至 电脑USB接口，连接成功后，

可以先获取软件 `全志线刷工具` `SPI Nor系统镜像`  `全志USB烧录驱动`安装包 进行解压缩操作。

### 安装usb驱动
在我们连接好开发板以后，先按住 DongshanPI-D1s开发板 `黑色序号为 2.烧录模式按键` 也称为**FEL** 烧写模式按键，之后按一下 `黑色序号为 5.系统复位按键`也称 **RESET** 复位键，就可以自动进入烧写模式。

这时我们可以看到电脑设备管理器  **通用串行总线控制器** 部分弹出一个  未知设备 ，这个时候我们就需要把我们提前下载好的 **全志USB烧录驱动** 进行修改，然后将解压缩过的 **全志USB烧录驱动**  压缩包，解压缩，可以看到里面有这么几个文件。

```bash
InstallUSBDrv.exe
drvinstaller_IA64.exe
drvinstaller_X86.exe
UsbDriver/          
drvinstaller_X64.exe   
install.bat
```

对于wind7系统的同学，只需要以管理员 打开   `install.bat` 脚本，等待安装，在弹出的 是否安装驱动的对话框里面，点击安装即可。

对于wind10/wind11系统的同学，需要在设备管理器里面进行手动安装驱动。

如下图所示，在第一次插入OTG设备，进入烧写模式设备管理器会弹出一个未知设备。

![Windows_FlashDevice_001](https://photos.100ask.net/dongshanpi-docs/DongshanNezhaSTU/Windows_FlashDevice_001.png){ width="600" }

接下来鼠标右键点击这个未知设备，在弹出的对话框里， 点击浏览我计算机以查找驱动程序软件。

![Windows_FlashDevice_001](https://photos.100ask.net/dongshanpi-docs/DongshanNezhaSTU/Windows_FlashDevice_002.png){ width="600" }

之后在弹出新的对话框里，点击浏览找到我们之前下载好的 usb烧录驱动文件夹内，找到 `UsbDriver/` 这个目录，并进入，之后点击确定即可。

![Windows_FlashDevice_001](https://photos.100ask.net/dongshanpi-docs/DongshanNezhaSTU/Windows_FlashDevice_007.png){ width="600" }

注意进入到  `UsbDriver/`  文件夹，然后点击确定，如下图所示。

![Windows_FlashDevice_001](https://photos.100ask.net/dongshanpi-docs/DongshanNezhaSTU/Windows_FlashDevice_003.png){ width="600" }

此时，我们继续点击 **下一页** 按钮，这时系统就会提示安装一个驱动程序。 

在弹出的对话框里，我们点击 始终安装此驱动程序软件 等待安装完成。

![Windows_FlashDevice_001](https://photos.100ask.net/dongshanpi-docs/DongshanNezhaSTU/Windows_FlashDevice_004.png){ width="600" }

安装完成后，会提示，Windows已成功更新你的驱动程序。

![Windows_FlashDevice_001](https://photos.100ask.net/dongshanpi-docs/DongshanNezhaSTU/Windows_FlashDevice_005.png){ width="600" }


最后我们可以看到，设备管理器 里面的未知设备 变成了一个 `USB Device(VID_1f3a_efe8)`的设备，这时就表明设备驱动已经安装成功。

![Windows_FlashDevice_001](https://photos.100ask.net/dongshanpi-docs/DongshanNezhaSTU/Windows_FlashDevice_006.png){ width="600" }


### 运行软件烧写
将下载下来的全志线刷工具 **AllwinnertechPhoeniSuit** 解压缩，同时将**SPI Nor系统镜像**下载下来也进行解压缩。

解压后，得到一个 **tina_d1s-nezha_nor_uart0_nor.img** 镜像，是用于烧录到SPI NAND镜像得。另一个是**AllwinnertechPhoeniSuit**文件夹。

首先我们进入到 **AllwinnertechPhoeniSuit\AllwinnertechPhoeniSuitRelease20201225** 目录下 找到 **PhoenixSuit.exe** 双击运行。

打开软件后 软件主界面如下图所示

![PhoenixSuit_001](https://photos.100ask.net/dongshanpi-docs/DongshanNezhaSTU/PhoenixSuit_001.png){ width="600" }


​	接下来 我们需要切换到 **一键刷机**窗口，如下图所示，点击红框标号1，在弹出的新窗口内，我们点击 红框2 **浏览** 找到我们刚才解压过的 SPI Nor 最小系统镜像  **tina_d1s-nezha_nor_uart0_nor.img** ，选中镜像后，点击红框3 **全盘擦除升级** ，最后点击红框4  **立即升级**。

​	点击完成后，不需要理会 弹出的信息，这时 我们拿起已经连接好的开发板，先按住 **FEL** 烧写模式按键，之后按一下 **RESET** 系统复位键，就可以自动进入烧写模式并开始烧写。

![PhoenixSuit_002](https://photos.100ask.net/dongshanpi-docs/DongshanNezhaSTU/PhoenixSuit_002.png){ width="600" }


​	烧写时会提示烧写进度条，烧写完成后 开发板会自己重启。

![PhoenixSuit_003](https://photos.100ask.net/dongshanpi-docs/DongshanNezhaSTU/PhoenixSuit_003.png){ width="600" }


### 启动系统

一般情况下，烧写成功后 都会自动重启 启动系统，此时我们进入到 串口终端，可以看到它的启动信息，等所有启动信息加载完成，输入 root 用户名即可登录烧写好的系统内。



## 烧写固件至TF卡

### 准备工作
* 硬件： DongshanPI-D1s主板 x1
* 硬件：USB Type-C线 x2
* 硬件：TF卡读卡器  x1
* 硬件：8GB以上的 Micro TF卡 x1
* 软件：Tina系统TF卡烧录工具: [PhoenixCard-V2.8](https://gitlab.com/dongshanpi/tools/-/raw/main/PhoenixCard-V2.8.zip)
* 软件：SDcard格式化工具：[SDCardFormatter5](https://gitlab.com/dongshanpi/tools/-/raw/main/SDCardFormatter5.0.1Setup.exe)
* 软件：TinaTF卡最小系统镜像：[tina_d1s-nezha_sd_uart0](https://gitlab.com/dongshanpi/tools/-/raw/main/tina_d1s-nezha_sd_uart0.zip）


### 运行烧写软件烧写

首先需要下载  **win32diskimage  SDcard专用格式化** 这两个烧写TF卡的工具，然后获取到TF卡系统镜像文件**tina_d1s-nezha_sd_uart0.zip**，获取到以后，先安装 **SDcard专用格式化 SDCardFormatter5**  这个工具，同时可以解压 一下TF卡系统的镜像文件 **tina_d1s-nezha_sd_uart0.zip**，可以得到一个  **tina_d1s-nezha_sd_uart0.img**文件，这个文件就是我们要烧写的镜像。  同时解压缩 **Tina系统TF卡烧录工具 PhoenixCard-V2.8**，解压完成后，进入到烧写工具目录内，双击运行 `PhoenixCard.exe`烧录工具。

![DongshanPI-D1s-V2TopFuction](https://photos.100ask.net/dongshanpi-docs/d1s/DongshanPI-D1s-V2TopFuction.png){ width="600" }


步骤一： 将TF卡插进读卡器内，同时将读卡器插到电脑USB接口，使用SD CatFormat格式化TF卡，注意备份卡内数据。参考下图所示，点击刷新找到TF卡，然后点击 Format 在弹出的 对话框 点击 **是(Yes)**等待格式完成即可。

![SDCardFormat_001](https://photos.100ask.net/dongshanpi-docs/DongshanNezhaSTU/SDCardFormat_001.png){ width="400" }


步骤二：格式化完成后，使用**PhoenixCard.exe**工具来烧录镜像，参考下图步骤，找到自己的TF卡盘符，点击 `左上角红框1` 固件，选择已经解压过的 `tina_d1s-nezha_sd_uart0.img` 镜像，然后点击 `红框2 启动卡`，最后点击`红框3 烧录` 等待烧录完成即可。

![PhoenixCard_Config_002](https://photos.100ask.net/dongshanpi-docs/d1s/PhoenixCard_Config_001.png){ width="600" }

如下图为烧录成功示意图。

![PhoenixCard_Config_002](https://photos.100ask.net/dongshanpi-docs/d1s/PhoenixCard_Config_002.png){ width="600" }


烧录完成以后，就可以弹出TF卡，并将其插到开发板正面 `黑色字体序号 11.TF卡卡槽`位置处，此时可以使用 杜邦线 连接 `PE2 PE3 GND`使用串口进行登录，也可以使用 adb shell 直接连接 ADB进行登录访问。

**注意：D1s因TF卡和CKlink引脚存在复用关系，需将拨码开关 `SW1` 拨至数字方向，才可以支持TF卡启动**

### 启动系统

如下启动信息 为 使用杜邦线 将PE2 PE3 GND连接至 CKlink接口旁 RX TX GND 引脚通孔显示。
``` shell
[71]HELLO! BOOT0 is starting!
[74]BOOT0 commit : 88480af
[77]set pll start
[79]periph0 has been enabled
[81]set pll end
[83][pmu]: bus read error
[85]board init ok
[87]ZQ value = 0x2f
[89]get_pmu_exist() = -1
[91]ddr_efuse_type: 0xa
[94]trefi:7.8ms
[96][AUTO DEBUG] single rank and full DQ!
[100]ddr_efuse_type: 0xa
[102]trefi:7.8ms
[104][AUTO DEBUG] rank 0 row = 13
[107][AUTO DEBUG] rank 0 bank = 4
[110][AUTO DEBUG] rank 0 page size = 2 KB
[114]DRAM BOOT DRIVE INFO: V0.33
[117]DRAM CLK = 528 MHz
[119]DRAM Type = 2 (2:DDR2,3:DDR3)
[123]DRAMC read ODT  off.
[125]DRAM ODT off.
[127]ddr_efuse_type: 0xa
[130]DRAM SIZE =64 M
[132]dram_tpr4:0x0
[133]PLL_DDR_CTRL_REG:0xf8002b00
[136]DRAM_CLK_REG:0xc0000000
[139][TIMING DEBUG] MR2= 0x0
[144]DRAM simple test OK.
[146]dram size =64
[148]card no is 0
[149]sdcard 0 line count 4
[152][mmc]: mmc driver ver 2021-04-2 16:45
[161][mmc]: Wrong media type 0x0
[164][mmc]: ***Try SD card 0***
[173][mmc]: HSSDR52/SDR25 4 bit
[176][mmc]: 50000000 Hz
[178][mmc]: 30448 MB
[180][mmc]: ***SD/MMC 0 init OK!!!***
[230]Loading boot-pkg Succeed(index=0).
[234]Entry_name        = opensbi
[237]Entry_name        = u-boot
[240]Entry_name        = dtb
[243]mmc not para
▒245]Jump to second Boot.

U-Boot 2018.05-g24521d6 (Feb 11 2022 - 08:52:39 +0000) Allwinner Technology

[00.254]DRAM:  64 MiB
[00.256]Relocation Offset is: 01ee7000
[00.261]secure enable bit: 0
[00.263]CPU=1008 MHz,PLL6=600 Mhz,AHB=200 Mhz, APB1=100Mhz  MBus=300Mhz
[00.270]flash init start
[00.272]workmode = 0,storage type = 1
[00.275][mmc]: mmc driver ver uboot2018:2021-11-19 15:38:00
[00.281][mmc]: get sdc_type fail and use default host:tm1.
[00.287][mmc]: can't find node "mmc0",will add new node
[00.292][mmc]: fdt err returned <no error>
[00.296][mmc]: Using default timing para
[00.299][mmc]: SUNXI SDMMC Controller Version:0x50310
[00.317][mmc]: card_caps:0x3000000a
[00.320][mmc]: host_caps:0x3000003f
[00.324]sunxi flash init ok
[00.327]line:703 init_clocks
[00.330]drv_disp_init
request pwm success, pwm7:pwm7:0x2000c00.
[00.347]drv_disp_init finish
[00.349]boot_gui_init:start
[00.352]set disp.dev2_output_type fail. using defval=0
[00.379]boot_gui_init:finish
partno erro : can't find partition bootloader
54 bytes read in 1 ms (52.7 KiB/s)
[00.561]bmp_name=bootlogo.bmp size 38454
38454 bytes read in 3 ms (12.2 MiB/s)
[00.582]Loading Environment from SUNXI_FLASH... OK
[00.602]out of usb burn from boot: not need burn key
[00.606][mmc]: memalign dst_align is NULL!
read first [00.612]LCD open finish
backup failed in fun sunxi_flash_mmc_secread line 358
[00.619][mmc]: memalign dst_align is NULL!
read first backup failed in fun sunxi_flash_mmc_secread line 358
[00.628]unknown error happen in item 0 read
[00.632]get secure storage map err
partno erro : can't find partition private
root_partition is rootfs
set root to /dev/mmcblk0p5
[00.646]update part info
[00.649]update bootcmd
[00.652]change working_fdt 0x42aa6da0 to 0x42a86da0
disable nand error: FDT_ERR_BADPATH
No reserved memory region found in source FDT
[00.682]update dts
noncached_alloc(): addr = 0x42efb080
noncached_alloc(): addr = 0x42efb0c0
noncached_alloc(): addr = 0x42efb100
noncached_alloc(): addr = 0x42efb940
geth_sys_init:634: get node 'gmac0' error
geth_sys_init fail!
[00.702]Board Net Initialization Failed
[00.706]No ethernet found.
Hit any key to stop autoboot:  0
[02.017]no vendor_boot partition is found
Android's image name: d1s-nezha_sd
Detect comp none
[02.035]
Starting kernel ...

[02.038][mmc]: MMC Device 2 not found
[02.041][mmc]: mmc 2 not find, so not exit
** 6 printk messages dropped **
  node   0: [mem 0x0000000040000000-0x0000000043ffffff]
Initmem setup node 0 [mem 0x0000000040000000-0x0000000043ffffff]
On node 0 totalpages: 16384
  DMA32 zone: 224 pages used for memmap
  DMA32 zone: 0 pages reserved
  DMA32 zone: 16384 pages, LIFO batch:3
elf_hwcap is 0x20112d
pcpu-alloc: s0 r0 d32768 u32768 alloc=1*32768
pcpu-alloc: [0] 0
Built 1 zonelists, mobility grouping on.  Total pages: 16160
Kernel command line: earlyprintk=sunxi-uart,0x02500000 clk_ignore_unused initcall_debug=0 console=ttyS0,115200 loglevel=8 root=/dev/mmcblk0p5 init=/pseudo_init partitions=boot-resource@mmcblk0p1:env@mmcblk0p2:env-redund@mmcblk0p3:boot@mmcblk0p4:rootfs@mmcblk0p5:recovery@mmcblk0p6:rootfs_data@mmcblk0p7:UDISK@mmcblk0p8 cma=0M snum= mac_addr= wifi_mac= bt_mac= specialstr= gpt=1 androidboot.mode=normal androidboot.hardware=sun20iw1p1 boot_type=1 androidboot.boot_type=1 gpt=1 uboot_message=2018.05-g24521d6(02/11/2022-08:52:39) mbr_
Dentry cache hash table entries: 8192 (order: 4, 65536 bytes, linear)
Inode-cache hash table entries: 4096 (order: 3, 32768 bytes, linear)
Sorting __ex_table...
mem auto-init: stack:off, heap alloc:off, heap free:off
Memory: 53012K/65536K available (4486K kernel code, 402K rwdata, 1712K rodata, 144K init, 230K bss, 12524K reserved, 0K cma-reserved)
SLUB: HWalign=64, Order=0-3, MinObjects=0, CPUs=1, Nodes=1
rcu: Preemptible hierarchical RCU implementation.
        Tasks RCU enabled.
rcu: RCU calculated value of scheduler-enlistment delay is 10 jiffies.
NR_IRQS: 0, nr_irqs: 0, preallocated irqs: 0
plic: mapped 200 interrupts with 1 handlers for 2 contexts.
riscv_timer_init_dt: Registering clocksource cpuid [0] hartid [0]
clocksource: riscv_clocksource: mask: 0xffffffffffffffff max_cycles: 0x588fe9dc0, max_idle_ns: 440795202592 ns
sched_clock: 64 bits at 24MHz, resolution 41ns, wraps every 4398046511097ns
riscv_timer_clockevent depends on broadcast, but no broadcast function available
clocksource: timer: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 79635851949 ns
Calibrating delay loop (skipped), value calculated using timer frequency.. 48.00 BogoMIPS (lpj=240000)
pid_max: default: 32768 minimum: 301
Mount-cache hash table entries: 512 (order: 0, 4096 bytes, linear)
Mountpoint-cache hash table entries: 512 (order: 0, 4096 bytes, linear)
ASID allocator initialised with 65536 entries
rcu: Hierarchical SRCU implementation.
devtmpfs: initialized
random: get_random_u32 called from bucket_table_alloc.isra.27+0xfe/0x120 with crng_init=0
clocksource: jiffies: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 19112604462750000 ns
futex hash table entries: 256 (order: 0, 6144 bytes, linear)
pinctrl core: initialized pinctrl subsystem
NET: Registered protocol family 16
DMA: preallocated 256 KiB pool for atomic allocations
cpuidle: using governor menu
rtc_ccu: sunxi ccu init OK
clock: sunxi ccu init OK
clock: sunxi ccu init OK
iommu: Default domain type: Translated
sunxi iommu: irq = 4
SCSI subsystem initialized
usbcore: registered new interface driver usbfs
usbcore: registered new interface driver hub
usbcore: registered new device driver usb
videodev: Linux video capture interface: v2.00
Advanced Linux Sound Architecture Driver Initialized.
Bluetooth: Core ver 2.22
NET: Registered protocol family 31
Bluetooth: HCI device and connection manager initialized
Bluetooth: HCI socket layer initialized
Bluetooth: L2CAP socket layer initialized
pwm module init!
g2d 5410000.g2d: Adding to iommu group 0
G2D: rcq version initialized.major:252
clocksource: Switched to clocksource riscv_clocksource
sun8iw20-pinctrl 2000000.pinctrl: initialized sunXi PIO driver
NET: Registered protocol family 2
tcp_listen_portaddr_hash hash table entries: 256 (order: 0, 4096 bytes, linear)
TCP established hash table entries: 512 (order: 0, 4096 bytes, linear)
TCP bind hash table entries: 512 (order: 0, 4096 bytes, linear)
TCP: Hash tables configured (established 512 bind 512)
UDP hash table entries: 256 (order: 1, 8192 bytes, linear)
UDP-Lite hash table entries: 256 (order: 1, 8192 bytes, linear)
NET: Registered protocol family 1
sun8iw20-pinctrl 2000000.pinctrl: 2000000.pinctrl supply vcc-pc not found, using dummy regulator
spi spi0: spi0 supply spi not found, using dummy regulator
sunxi_spi_resource_get()2151 - [spi0] SPI MASTER MODE
sunxi_spi_resource_get()2189 - Failed to get sample mode
sunxi_spi_resource_get()2194 - Failed to get sample delay
sunxi_spi_resource_get()2198 - sample_mode:-1431633921 sample_delay:-1431633921
sunxi_spi_clk_init()2240 - [spi0] mclk 100000000
sunxi_spi_probe()2653 - [spi0]: driver probe succeed, base ffffffd004058000, irq 31
workingset: timestamp_bits=62 max_order=14 bucket_order=0
squashfs: version 4.0 (2009/01/31) Phillip Lougher
ntfs: driver 2.1.32 [Flags: R/W].
io scheduler mq-deadline registered
io scheduler kyber registered
[DISP]disp_module_init
disp 5000000.disp: Adding to iommu group 0
[DISP] disp_init,line:2386:
smooth display screen:0 type:1 mode:4
display_fb_request,fb_id:0
Freeing logo buffer memory: 4000K
disp_al_manager_apply ouput_type:1
[DISP] lcd_clk_config,line:732:
disp 0, clk: pll(420000000),clk(420000000),dclk(70000000) dsi_rate(70000000)
     clk real:pll(420000000),clk(420000000),dclk(105000000) dsi_rate(150000000)
sun8iw20-pinctrl 2000000.pinctrl: 2000000.pinctrl supply vcc-pd not found, using dummy regulator
[DISP]disp_module_init finish
sunxi_sid_init()551 - insmod ok
pwm-regulator: supplied by regulator-dummy
sun8iw20-pinctrl 2000000.pinctrl: 2000000.pinctrl supply vcc-pe not found, using dummy regulator
uart uart0: get regulator failed
uart uart0: uart0 supply uart not found, using dummy regulator
uart0: ttyS0 at MMIO 0x2500000 (irq = 18, base_baud = 1500000) is a SUNXI
sw_console_setup()1808 - console setup baud 115200 parity n bits 8, flow n
printk: console [ttyS0] enabled
sun8iw20-pinctrl 2000000.pinctrl: 2000000.pinctrl supply vcc-pg not found, using dummy regulator
uart uart1: get regulator failed
uart uart1: uart1 supply uart not found, using dummy regulator
uart1: ttyS1 at MMIO 0x2500400 (irq = 19, base_baud = 1500000) is a SUNXI
misc dump reg init
sunxi-rfkill soc@3000000:rfkill@0: module version: v1.0.9
sunxi-rfkill soc@3000000:rfkill@0: get gpio chip_en failed
sunxi-rfkill soc@3000000:rfkill@0: get gpio power_en failed
sunxi-rfkill soc@3000000:rfkill@0: wlan_busnum (1)
sunxi-rfkill soc@3000000:rfkill@0: Missing wlan_power.
sunxi-rfkill soc@3000000:rfkill@0: wlan_regon gpio=137 assert=1
sunxi-rfkill soc@3000000:rfkill@0: wlan_hostwake gpio=202 assert=1
sunxi-rfkill soc@3000000:rfkill@0: wakeup source is enabled
sunxi-rfkill soc@3000000:rfkill@0: Missing bt_power.
sunxi-rfkill soc@3000000:rfkill@0: bt_rst gpio=133 assert=0
[ADDR_MGT] addr_mgt_probe: module version: v1.0.10
[ADDR_MGT] addr_mgt_probe: success.
sunxi-spinand: AW SPINand MTD Layer Version: 2.0 20201228
sunxi-spinand-phy: AW SPINand Phy Layer Version: 1.10 20200306
random: fast init done
random: crng init done
sunxi-spinand-phy: read id failed : -110
spi-nand: probe of spi0.0 failed with error -110
ehci_hcd: USB 2.0 'Enhanced' Host Controller (EHCI) Driver
sunxi-ehci: EHCI SUNXI driver
get ehci1-controller wakeup-source is fail.
sunxi ehci1-controller don't init wakeup source
[sunxi-ehci1]: probe, pdev->name: 4200000.ehci1-controller, sunxi_ehci: 0xffffffe0006c0870, 0x:ffffffd004075000, irq_no:31
sunxi-ehci 4200000.ehci1-controller: 4200000.ehci1-controller supply drvvbus not found, using dummy regulator
sunxi-ehci 4200000.ehci1-controller: 4200000.ehci1-controller supply hci not found, using dummy regulator
sunxi-ehci 4200000.ehci1-controller: EHCI Host Controller
sunxi-ehci 4200000.ehci1-controller: new USB bus registered, assigned bus number 1
sunxi-ehci 4200000.ehci1-controller: irq 49, io mem 0x04200000
sunxi-ehci 4200000.ehci1-controller: USB 2.0 started, EHCI 1.00
hub 1-0:1.0: USB hub found
hub 1-0:1.0: 1 port detected
ohci_hcd: USB 1.1 'Open' Host Controller (OHCI) Driver
sunxi-ohci: OHCI SUNXI driver
get ohci1-controller wakeup-source is fail.
sunxi ohci1-controller don't init wakeup source
[sunxi-ohci1]: probe, pdev->name: 4200400.ohci1-controller, sunxi_ohci: 0xffffffe0006c0c38
sunxi-ohci 4200400.ohci1-controller: 4200400.ohci1-controller supply drvvbus not found, using dummy regulator
sunxi-ohci 4200400.ohci1-controller: 4200400.ohci1-controller supply hci not found, using dummy regulator
sunxi-ohci 4200400.ohci1-controller: OHCI Host Controller
sunxi-ohci 4200400.ohci1-controller: new USB bus registered, assigned bus number 2
sunxi-ohci 4200400.ohci1-controller: irq 50, io mem 0x04200400
hub 2-0:1.0: USB hub found
hub 2-0:1.0: 1 port detected
sunxi-rtc 7090000.rtc: registered as rtc0
sunxi-rtc 7090000.rtc: setting system clock to 1970-01-01T02:07:25 UTC (7645)
sunxi-rtc 7090000.rtc: sunxi rtc probed
i2c /dev entries driver
IR NEC protocol handler initialized
uvcvideo: Unable to create debugfs directory
usbcore: registered new interface driver uvcvideo
USB Video Class driver (1.1.1)
sunxi cedar version 1.1
sunxi-cedar 1c0e000.ve: Adding to iommu group 0
VE: install start!!!

VE: cedar-ve the get irq is 6

VE: install end!!!

VE: sunxi_cedar_probe
Bluetooth: HCI UART driver ver 2.3
Bluetooth: HCI UART protocol H4 registered
Bluetooth: HCI UART protocol BCSP registered
Bluetooth: XRadio Bluetooth LPM Mode Driver Ver 1.0.10
[XR_BT_LPM] bluesleep_probe: bt_wake polarity: 1
[XR_BT_LPM] bluesleep_probe: host_wake polarity: 1
[XR_BT_LPM] bluesleep_probe: wakeup source is disabled!

[XR_BT_LPM] bluesleep_probe: uart_index(1)
sunxi-mmc 4020000.sdmmc: SD/MMC/SDIO Host Controller Driver(v4.21 2021-11-18 10:02)
sunxi-mmc 4020000.sdmmc: ***ctl-spec-caps*** 8
sunxi-mmc 4020000.sdmmc: No vmmc regulator found
sunxi-mmc 4020000.sdmmc: No vqmmc regulator found
sunxi-mmc 4020000.sdmmc: No vdmmc regulator found
sunxi-mmc 4020000.sdmmc: No vd33sw regulator found
sunxi-mmc 4020000.sdmmc: No vd18sw regulator found
sunxi-mmc 4020000.sdmmc: No vq33sw regulator found
sunxi-mmc 4020000.sdmmc: No vq18sw regulator found
sunxi-mmc 4020000.sdmmc: sdc set ios:clk 0Hz bm PP pm UP vdd 21 width 1 timing LEGACY(SDR12) dt B
sunxi-mmc 4020000.sdmmc: no vqmmc,Check if there is regulator
sunxi-mmc 4020000.sdmmc: sdc set ios:clk 400000Hz bm PP pm ON vdd 21 width 1 timing LEGACY(SDR12) dt B
sunxi-mmc 4020000.sdmmc: detmode:gpio polling
sunxi-mmc 4020000.sdmmc: sdc set ios:clk 400000Hz bm PP pm ON vdd 21 width 1 timing LEGACY(SDR12) dt B
sunxi-mmc 4021000.sdmmc: SD/MMC/SDIO Host Controller Driver(v4.21 2021-11-18 10:02)
sunxi-mmc 4021000.sdmmc: ***ctl-spec-caps*** 8
sunxi-mmc 4021000.sdmmc: No vmmc regulator found
sunxi-mmc 4021000.sdmmc: No vqmmc regulator found
sunxi-mmc 4021000.sdmmc: No vdmmc regulator found
sunxi-mmc 4020000.sdmmc: sdc set ios:clk 400000Hz bm PP pm ON vdd 21 width 1 timing LEGACY(SDR12) dt B
sunxi-mmc 4021000.sdmmc: No vd33sw regulator found
sunxi-mmc 4021000.sdmmc: No vd18sw regulator found
sunxi-mmc 4021000.sdmmc: No vq33sw regulator found
sunxi-mmc 4021000.sdmmc: No vq18sw regulator found
sunxi-mmc 4021000.sdmmc: Cann't get pin bias hs pinstate,check if needed
sunxi-mmc 4020000.sdmmc: sdc set ios:clk 400000Hz bm PP pm ON vdd 21 width 1 timing LEGACY(SDR12) dt B
sunxi-mmc 4021000.sdmmc: sdc set ios:clk 0Hz bm PP pm UP vdd 21 width 1 timing LEGACY(SDR12) dt B
sunxi-mmc 4021000.sdmmc: no vqmmc,Check if there is regulator
sunxi-mmc 4020000.sdmmc: sdc set ios:clk 400000Hz bm PP pm ON vdd 21 width 1 timing LEGACY(SDR12) dt B
sunxi-mmc 4021000.sdmmc: sdc set ios:clk 400000Hz bm PP pm ON vdd 21 width 1 timing LEGACY(SDR12) dt B
sunxi-mmc 4021000.sdmmc: detmode:manually by software
sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 52, RTO !!
ashmem: initialized
sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 52, RTO !!
sunxi-mmc 4021000.sdmmc: sdc set ios:clk 400000Hz bm PP pm ON vdd 21 width 1 timing LEGACY(SDR12) dt B
exFAT: Version 1.3.0
[AUDIOCODEC][sunxi_codec_parse_params][2412]:digital_vol:0, lineout_vol:26, mic1gain:31, mic2gain:31 pa_msleep:120, pa_level:1, pa_pwr_level:1

mmc0: host does not support reading read-only switch, assuming write-enable
[AUDIOCODEC][sunxi_codec_parse_params][2448]:adcdrc_cfg:0, adchpf_cfg:1, dacdrc_cfg:0, dachpf:0
sunxi-mmc 4020000.sdmmc: sdc set ios:clk 400000Hz bm PP pm ON vdd 21 width 1 timing SD-HS(SDR25) dt B
[AUDIOCODEC][sunxi_internal_codec_probe][2609]:codec probe finished
sunxi-mmc 4020000.sdmmc: sdc set ios:clk 50000000Hz bm PP pm ON vdd 21 width 1 timing SD-HS(SDR25) dt B
sunxi-mmc 4021000.sdmmc: sdc set ios:clk 400000Hz bm PP pm ON vdd 21 width 1 timing LEGACY(SDR12) dt B
[SNDCODEC][sunxi_card_init][583]:card init finished
sunxi-codec-machine 2030340.sound: 2030000.codec <-> 203034c.dummy_cpudai mapping ok
sunxi-mmc 4020000.sdmmc: sdc set ios:clk 50000000Hz bm PP pm ON vdd 21 width 4 timing SD-HS(SDR25) dt B
mmc0: new high speed SDHC card at address 5048
sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 5, RTO !!
sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 5, RTO !!
input: audiocodec sunxi Audio Jack as /devices/platform/soc@3000000/2030340.sound/sound/card0/input0
sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 5, RTO !!
sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 5, RTO !!
sunxi-mmc 4021000.sdmmc: sdc set ios:clk 0Hz bm PP pm OFF vdd 0 width 1 timing LEGACY(SDR12) dt B
mmcblk0: mmc0:5048 SD32G 29.7 GiB
[SNDCODEC][sunxi_card_dev_probe][836]:register card finished
NET: Registered protocol family 10
Segment Routing with IPv6
NET: Registered protocol family 17
 mmcblk0: p1 p2 p3 p4 p5 p6 p7 p8
sunxi-i2c sunxi-i2c2: sunxi-i2c2 supply twi not found, using dummy regulator
sunxi-i2c sunxi-i2c2: probe success
sun8iw20-pinctrl 2000000.pinctrl: 2000000.pinctrl supply vcc-pb not found, using dummy regulator
get ehci0-controller wakeup-source is fail.
sunxi ehci0-controller don't init wakeup source
[sunxi-ehci0]: probe, pdev->name: 4101000.ehci0-controller, sunxi_ehci: 0xffffffe0006c00e0, 0x:ffffffd0040fd000, irq_no:2e
[sunxi-ehci0]: Not init ehci0
get ohci0-controller wakeup-source is fail.
sunxi ohci0-controller don't init wakeup source
[sunxi-ohci0]: probe, pdev->name: 4101400.ohci0-controller, sunxi_ohci: 0xffffffe0006c04a8
[sunxi-ohci0]: Not init ohci0
clk: Not disabling unused clocks
ALSA device list:
  #0: audiocodec
platform regulatory.0: Direct firmware load for regulatory.db failed with error -2
cfg80211: failed to load regulatory.db
alloc_fd: slot 0 not NULL!
VFS: Mounted root (squashfs filesystem) readonly on device 179:5.
devtmpfs: mounted
Freeing unused kernel memory: 144K
This architecture does not have kernel memory protection.
Run /pseudo_init as init process
mount: mounting none on /dev failed: Device or resource busy
mount: mounting /dev/by-name/rootfs_data on /overlay failed: No such device
Mount Failed: formating /dev/by-name/rootfs_data to ext4 ...
/pseudo_init: line 395: mkfs.ext4: not found
can't run '/etc/preinit': No such file or directory
mount: mounting proc on /proc failed: Device or resource busy
mount: mounting tmpfs on /run failed: No such file or directory
[SNDCODEC][sunxi_check_hs_detect_status][191]:plugin --> switch:1
hostname: can't open '/etc/hostname': No such file or directory
------run rc.preboot file-----
/etc/init.d/rcS: line 136: mkfs.ext4: not found
------run rc.modules file-----
usbcore: registered new interface driver usb-storage
sunxi_gpadc_init,2151, success
sunxi_gpadc_setup: get channel scan data failed
input: sunxi-gpadc0 as /devices/virtual/input/input1
insmod: can't insert '/lib/modules/5.4.61/xr829.ko': No such file or directory
Successfully initialized wpa_supplicant
Could not read interface wlan0 flags: No such device
nl80211: Driver does not support authentication/association or connect commands
nl80211: deinit ifname=wlan0 disabled_11b_rates=0
Could not read interface wlan0 flags: No such device
wlan0: Failed to initialize driver interface
------run rc.final file-----

insmod_host_driver

[ehci0-controller]: sunxi_usb_enable_ehci
[sunxi-ehci0]: probe, pdev->name: 4101000.ehci0-controller, sunxi_ehci: 0xffffffe0006c00e0, 0x:ffffffd0040fd000, irq_no:2e
sunxi-ehci 4101000.ehci0-controller: 4101000.ehci0-controller supply hci not found, using dummy regulator
sunxi-ehci 4101000.ehci0-controller: EHCI Host Controller
sunxi-ehci 4101000.ehci0-controller: new USB bus registered, assigned bus number 3
sunxi-ehci 4101000.ehci0-controller: irq 46, io mem 0x04101000
sunxi-ehci 4101000.ehci0-controller: USB 2.0 started, EHCI 1.00
device_chose finished 139!
hub 3-0:1.0: USB hub found
hub 3-0:1.0: 1 port detected
[ohci0-controller]: sunxi_usb_enable_ohci
[sunxi-ohci0]: probe, pdev->name: 4101400.ohci0-controller, sunxi_ohci: 0xffffffe0006c04a8
sunxi-ohci 4101400.ohci0-controller: 4101400.ohci0-controller supply hci not found, using dummy regulator
sunxi-ohci 4101400.ohci0-controller: OHCI Host Controller
sunxi-ohci 4101400.ohci0-controller: new USB bus registered, assigned bus number 4
sunxi-ohci 4101400.ohci0-controller: irq 47, io mem 0x04101400
file system registered
hub 4-0:1.0: USB hub found
hub 4-0:1.0: 1 port detected
host_chose finished!
configfs-gadget 4100000.udc-controller: failed to start g1: -19
sh: write error: No such device

rmmod_host_driver

[ehci0-controller]: sunxi_usb_disable_ehci
[sunxi-ehci0]: remove, pdev->name: 4101000.ehci0-controller, sunxi_ehci: 0xffffffe0006c00e0
nice: can't execute '/usr/bin/story_ota_bin': No such file or directory
read descriptors
read strings
sunxi-ehci 4101000.ehci0-controller: remove, state 4
usb usb3: USB disconnect, device number 1
sunxi-ehci 4101000.ehci0-controller: USB bus 3 deregistered
[ohci0-controller]: sunxi_usb_disable_ohci
[sunxi-ohci0]: remove, pdev->name: 4101400.ohci0-controller, sunxi_ohci: 0xffffffe0006c04a8
sunxi-ohci 4101400.ohci0-controller: remove, state 4
usb usb4: USB disconnect, device number 1
sunxi-ohci 4101400.ohci0-controller: USB bus 4 deregistered

insmod_device_driver

sunxi_usb_udc 4100000.udc-controller: 4100000.udc-controller supply udc not found, using dummy regulator
device_chose finished!
numid=30,iface=MIXER,name='Headphone Switch'
  ; type=BOOLEAN,access=rw------,values=1
  : values=on


BusyBox v1.27.2 () built-in shell (ash)

------run profile file-----
 _____  _              __     _
|_   _||_| ___  _ _   |  |   |_| ___  _ _  _ _
  | |   _ |   ||   |  |  |__ | ||   || | ||_'_|
  | |  | || | || _ |  |_____||_||_|_||___||_,_|
  |_|  |_||_|_||_|_|  Tina is Based on OpenWrt!
 --------------------------sunxi_set_cur_vol_work()397 WARN: get power supply failed
--------------------
 Tina Linux (Neptune, 61CC0487)
 ----------------------------------------------
root@TinaLinux:/# android_work: sent uevent USB_STATE=CONNECTED
sunxi_set_cur_vol_work()397 WARN: get power supply failed
sunxi_vbus_det_work()3356 WARN: get power supply failed
android_work: sent uevent USB_STATE=DISCONNECTED
android_work: sent uevent USB_STATE=CONNECTED
configfs-gadget gadget: high-speed config #1: c
android_work: sent uevent USB_STATE=CONFIGURED

```



**系统默认会自己登录 没有用户名 没有密码。**
**系统默认会自己登录 没有用户名 没有密码。**
**系统默认会自己登录 没有用户名 没有密码。**

