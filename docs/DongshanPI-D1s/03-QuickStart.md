# 快速开始使用

**注意：开发板板载了 SPI-NorFLASH 发货前都会把Tina-Linux 系统提前烧录至 SPINor内，对于使用TF卡启动的同学 请单独看后续章节 `更新系统` 单独烧录系统至TF卡并启动。**

因开发板板载了CKLINK，且TF卡引脚和CKLINK冲突导致无法同时使用，需要通过拨码开关 `SW1` 来切换启动功能，由于我们的裸机/RTOS课程会用到CKLINK进行调试和输出功能，硬件默认把 PF2 PF4作为了UART0，但是当您使用DongshanPI-D1s运行Linux系统时，Linux系统默认的UART0为PE2 PE3 这时需要参考下图通过2.54mm规格的杜邦线连接右侧J2 排针的 PE2 PE3 GND 连接至 开发板 `黑色序号 5.调试与UART功能 `旁边的 **RX TX GND** 三个圆孔内，需要直接用公头杜邦线穿过圆孔。
![Dongshanpi-d1s_pe2pe3uart_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/d1s/Dongshanpi-d1s_pe2pe3uart_002.png)

如果你不想使用杜邦线这种方式，可以优先使用下面的 **使用ADB登录系统** 方式进行登录系统

## windows下使用 ADB登录系统
### 连接OTG线

将开发板配套的两根typec线，一根 直接连接至 开发板 `黑色字体序号 3.OTG烧录接口` 另一头连接至电脑的USB接口，开发板默认有系统，接通otg电源线就会通电并直接启动。

### 安装windows板ADB
点击链接下载Windows版ADB工具 [adb-tools](https://gitlab.com/dongshanpi/tools/-/raw/main/ADB.7z)
下载完成后解压，可以看到如下目录，

![adb-tools-dir](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/d1s/adb-tools-dir.png)

然后 我们单独 拷贝 上一层的 **platform-tools** 文件夹到任意 目录，拷贝完成后，记住这个 目录位置，我们接下来要把这个 路径添加至 Windows系统环境变量里。

![adb-tools-dir](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/d1s/adb-tools-dir-001.png)

我这里是把它单独拷贝到了 D盘，我的目录是 `D:\platform-tools` 接下来 我需要把它单独添加到Windows系统环境变量里面才可以在任意位置使用adb命令。

![adb-tools-windows_config_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/d1s/adb-tools-windows_config_001.png)

添加到 Windows系统环境变量里面
![adb-tools-windows_config_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/d1s/adb-tools-windows_config_002.png)

### 打开cmd连接开发板
打开CMD Windows 命令提示符方式有两种<br>
方式1：直接在Windows10/11搜索对话框中输入  cmd 在弹出的软件中点击  `命令提示符`<br>
方式2：同时按下 wind + r 键，输入 cmd 命令，按下确认 就可以自动打开 `命令提示符`<br>
![adb-tools-windows_config_003](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/d1s/adb-tools-windows_config_003.png)

打开命令提示符，输出 adb命令可以直接看到我们的adb已经配置成功<br>
![adb-tools-windows_config_004](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/d1s/adb-tools-windows_config_004.png)

连接好开发板的 OTG 并将其连接至电脑上，然后 输入 adb shell就可以自动登录系统<br>
``` shell

C:\System> adb shell
* daemon not running. starting it now on port 5037 *
* daemon started successfully *

 _____  _              __     _
|_   _||_| ___  _ _   |  |   |_| ___  _ _  _ _
  | |   _ |   ||   |  |  |__ | ||   || | ||_'_|
  | |  | || | || _ |  |_____||_||_|_||___||_,_|
  |_|  |_||_|_||_|_|  Tina is Based on OpenWrt!
 ----------------------------------------------
 Tina Linux
 ----------------------------------------------
root@TinaLinux:/#

```
ADB 也可以作为文件传输使用，例如：
``` shell
C:\System> adb push badapple.mp4 /mnt/UDISK   # 将 badapple.mp4 上传到开发板 /mnt/UDISK 目录内
```
``` shell
C:\System> adb pull /mnt/UDISK/badapple.mp4   # 将 /mnt/UDISK/badapple.mp4 下拉到当前目录内
```

**注意： 此方法目前只适用于 使用全志Tina-SDK 构建出来的系统。**


## 使用串口登录系统
### 1. 连接串口线
将配套的TypeC线一段连接至开发板的串口/供电接口，另一端连接至电脑USB接口，连接成功后板载的红色电源灯会亮起。
默认情况下系统会自动安装串口设备驱动，如果没有自动安装，可以使用驱动精灵来自动安装。
* 对于Windows系统
此时Windows设备管理器 在 端口(COM和LPT) 处会多出一个串口设备，一般是以 `USB-Enhanced-SERIAL CH9102`开头，您需要留意一下后面的具体COM编号，用于后续连接使用。

![QuickStart-01](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/QuickStart-01.png)

如上图，COM号是96，我们接下来连接所使用的串口号就是96。

* 对于Linux系统
可以查看是否多出一个/dev/tty<> 设备,一般情况设备节点为 /dev/ttyACM0  。

![QuickStart-02](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/QuickStart-02.png)

### 2. 打开串口控制台
#### 获取串口工具
使用Putty或者MobaXterm等串口工具来开发板设备。

* 其中putty工具可以访问页面  https://www.chiark.greenend.org.uk/~sgtatham/putty/  来获取。
* MobaXterm可以通过访问页面 https://mobaxterm.mobatek.net/ 获取 (推荐使用)。

#### 使用putty登录串口

![QuickStart-04](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/QuickStart-04.png)

#### 使用Mobaxterm登录串口
打开MobaXterm，点击左上角的“Session”，在弹出的界面选中“Serial”，如下图所示选择端口号（前面设备管理器显示的端口号COM21）、波特率（Speed 115200）、流控（Flow Control: none）,最后点击“OK”即可。步骤如下图所示。
**注意：流控（Flow Control）一定要选择none，否则你将无法在MobaXterm中向串口输入数据**

![Mobaxterm_serial_set_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/Mobaxterm_serial_set_001.png)


### 3. 进入系统shell
使用串口工具成功打开串口后，可以直接按下 Enter 键 进入shell，当然您也可以按下板子上的 `Reset`复位键，来查看完整的系统信息。

``` bash
[34]HELLO! BOOT0 is starting!
[37]BOOT0 commit : 88480af
[40]set pll start
[42]periph0 has been enabled
[44]set pll end
[46][pmu]: bus read error
[48]board init ok
[50]ZQ value = 0x2f
[52]get_pmu_exist() = -1
[54]DRAM BOOT DRIVE INFO: V0.33
[57]DRAM CLK = 528 MHz
[59]DRAM Type = 2 (2:DDR2,3:DDR3)
[62]DRAMC read ODT  off.
[65]DRAM ODT off.
[67]ddr_efuse_type: 0xa
[69]DRAM SIZE =64 M
[71]dram_tpr4:0x0
[73]PLL_DDR_CTRL_REG:0xf8002b00
[76]DRAM_CLK_REG:0xc0000000
[78][TIMING DEBUG] MR2= 0x0
[83]DRAM simple test OK.
[85]dram size =64
[87]spinor id is: ef 40 18, read cmd: 6b
[90]Succeed in reading toc file head.
[94]The size of toc is 100000.
[139]Entry_name        = opensbi
[142]Entry_name        = u-boot
[146]Entry_name        = dtb
▒149]Jump to second Boot.

U-Boot 2018.05-g24521d6 (Feb 11 2022 - 08:52:39 +0000) Allwinner Technology

[00.158]DRAM:  64 MiB
[00.160]Relocation Offset is: 01ee7000
[00.165]secure enable bit: 0
[00.167]CPU=1008 MHz,PLL6=600 Mhz,AHB=200 Mhz, APB1=100Mhz  MBus=300Mhz
[00.174]flash init start
[00.176]workmode = 0,storage type = 3
individual lock is enable
[00.185]spi sunxi_slave->max_hz:100000000
SF: Detected w25q128 with page size 256 Bytes, erase size 64 KiB, total 16 MiB
[00.195]sunxi flash init ok
[00.198]line:703 init_clocks
[00.201]drv_disp_init
request pwm success, pwm7:pwm7:0x2000c00.
[00.218]drv_disp_init finish
[00.220]boot_gui_init:start
[00.223]set disp.dev2_output_type fail. using defval=0
[00.250]boot_gui_init:finish
partno erro : can't find partition bootloader
54 bytes read in 0 ms
[00.259]bmp_name=bootlogo.bmp size 38454
38454 bytes read in 1 ms (36.7 MiB/s)
[00.434]Loading Environment from SUNXI_FLASH... OK
[00.448]out of usb burn from boot: not need burn key
[00.453]get secure storage map err
partno erro : can't find partition private
root_partition is rootfs
set root to /dev/mtdblock5
[00.464]update part info
[00.467]update bootcmd
[00.469]change working_fdt 0x42aa6da0 to 0x42a86da0
No reserved memory region found in source FDT
FDT ERROR:fdt_get_all_pin:get property handle pinctrl-0 error:FDT_ERR_INTERNAL
sunxi_pwm_pin_set_state, fdt_set_all_pin, ret=-1
[00.494]LCD open finish
[00.510]update dts
noncached_alloc(): addr = 0x42ebb200
noncached_alloc(): addr = 0x42ebb240
noncached_alloc(): addr = 0x42ebb280
noncached_alloc(): addr = 0x42ebbac0
geth_sys_init:634: get node 'gmac0' error
geth_sys_init fail!
[00.530]Board Net Initialization Failed
[00.533]No ethernet found.
Hit any key to stop autoboot:  0
[01.686]no vendor_boot partition is found
Android's image name: d1s-nezha_nor
Detect comp none
[01.704]
Starting kernel ...

** 9 printk messages dropped **
[    0.000000] On node 0 totalpages: 16384
[    0.000000]   DMA32 zone: 224 pages used for memmap
[    0.000000]   DMA32 zone: 0 pages reserved
[    0.000000]   DMA32 zone: 16384 pages, LIFO batch:3
[    0.000000] elf_hwcap is 0x20112d
[    0.000000] pcpu-alloc: s0 r0 d32768 u32768 alloc=1*32768
[    0.000000] pcpu-alloc: [0] 0
[    0.000000] Built 1 zonelists, mobility grouping on.  Total pages: 16160
[    0.000000] Kernel command line: earlyprintk=sunxi-uart,0x02500000 clk_ignore_unused initcall_debug=0 console=ttyS0,115200 loglevel=8 root=/dev/mtdblock5 rootfstype=squashfs init=/sbin/init partitions=boot-resource@mtdblock1:env@mtdblock2:env-redund@mtdblock3:boot@mtdblock4:rootfs@mtdblock5:UDISK@mtdblock6 cma=8M snum= mac_addr= wifi_mac= bt_mac= specialstr= gpt=1 androidboot.hardware=sun20iw1p1 boot_type=3 androidboot.boot_type=3 gpt=1 uboot_message=2018.05-g24521d6(02/11/2022-08:52:39) mbr_offset=1556480 disp_reserve=4096000,0x000000004
[    0.000000] Dentry cache hash table entries: 8192 (order: 4, 65536 bytes, linear)
[    0.000000] Inode-cache hash table entries: 4096 (order: 3, 32768 bytes, linear)
[    0.000000] Sorting __ex_table...
[    0.000000] mem auto-init: stack:off, heap alloc:off, heap free:off
[    0.000000] Memory: 44932K/65536K available (4353K kernel code, 404K rwdata, 1736K rodata, 144K init, 229K bss, 12412K reserved, 8192K cma-reserved)
[    0.000000] SLUB: HWalign=64, Order=0-3, MinObjects=0, CPUs=1, Nodes=1
[    0.000000] rcu: Preemptible hierarchical RCU implementation.
[    0.000000]  Tasks RCU enabled.
[    0.000000] rcu: RCU calculated value of scheduler-enlistment delay is 10 jiffies.
[    0.000000] NR_IRQS: 0, nr_irqs: 0, preallocated irqs: 0
[    0.000000] plic: mapped 200 interrupts with 1 handlers for 2 contexts.
[    0.000000] riscv_timer_init_dt: Registering clocksource cpuid [0] hartid [0]
[    0.000000] clocksource: riscv_clocksource: mask: 0xffffffffffffffff max_cycles: 0x588fe9dc0, max_idle_ns: 440795202592 ns
[    0.000006] sched_clock: 64 bits at 24MHz, resolution 41ns, wraps every 4398046511097ns
[    0.000026] riscv_timer_clockevent depends on broadcast, but no broadcast function available
[    0.000375] clocksource: timer: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 79635851949 ns
[    0.001012] Calibrating delay loop (skipped), value calculated using timer frequency.. 48.00 BogoMIPS (lpj=240000)
[    0.001032] pid_max: default: 32768 minimum: 301
[    0.001214] Mount-cache hash table entries: 512 (order: 0, 4096 bytes, linear)
[    0.001232] Mountpoint-cache hash table entries: 512 (order: 0, 4096 bytes, linear)
[    0.003389] ASID allocator initialised with 65536 entries
[    0.003591] rcu: Hierarchical SRCU implementation.
[    0.004352] devtmpfs: initialized
[    0.020017] random: get_random_u32 called from bucket_table_alloc.isra.27+0xfe/0x120 with crng_init=0
[    0.021117] clocksource: jiffies: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 19112604462750000 ns
[    0.021148] futex hash table entries: 256 (order: 0, 6144 bytes, linear)
[    0.021756] pinctrl core: initialized pinctrl subsystem
[    0.023320] NET: Registered protocol family 16
[    0.025492] DMA: preallocated 256 KiB pool for atomic allocations
[    0.026044] cpuidle: using governor menu
[    0.074053] rtc_ccu: sunxi ccu init OK
[    0.082956] clock: sunxi ccu init OK
[    0.084024] clock: sunxi ccu init OK
[    0.126116] iommu: Default domain type: Translated
[    0.126314] sunxi iommu: irq = 4
[    0.127641] SCSI subsystem initialized
[    0.127857] usbcore: registered new interface driver usbfs
[    0.127972] usbcore: registered new interface driver hub
[    0.128097] usbcore: registered new device driver usb
[    0.128239] videodev: Linux video capture interface: v2.00
[    0.129274] Advanced Linux Sound Architecture Driver Initialized.
[    0.130018] Bluetooth: Core ver 2.22
[    0.130111] NET: Registered protocol family 31
[    0.130122] Bluetooth: HCI device and connection manager initialized
[    0.130146] Bluetooth: HCI socket layer initialized
[    0.130164] Bluetooth: ▒A▒socket layer initialized
[    0.130206] Bluetooth: SCO socket layer initialized
[    0.130489] pwm module init!
[    0.132410] g2d 5410000.g2d: Adding to iommu group 0
[    0.132978] G2D: rcq version initialized.major:252
[    0.133921] clocksource: Switched to clocksource riscv_clocksource
[    0.149198] sun8iw20-pinctrl 2000000.pinctrl: initialized sunXi PIO driver
[    0.152820] NET: Registered protocol family 2
[    0.153764] tcp_listen_portaddr_hash hash table entries: 256 (order: 0, 4096 bytes, linear)
[    0.153818] TCP established hash table entries: 512 (order: 0, 4096 bytes, linear)
[    0.153838] TCP bind hash table entries: 512 (order: 0, 4096 bytes, linear)
[    0.153853] TCP: Hash tables configured (established 512 bind 512)
[    0.154134] UDP hash table entries: 256 (order: 1, 8192 bytes, linear)
[    0.154180] UDP-Lite hash table entries: 256 (order: 1, 8192 bytes, linear)
[    0.154478] NET: Registered protocol family 1
[    0.155843] sun8iw20-pinctrl 2000000.pinctrl: 2000000.pinctrl supply vcc-pc not found, using dummy regulator
[    0.156478] spi spi0: spi0 supply spi not found, using dummy regulator
[    0.156806] sunxi_spi_resource_get()2151 - [spi0] SPI MASTER MODE
[    0.156874] sunxi_spi_resource_get()2189 - Failed to get sample mode
[    0.156884] sunxi_spi_resource_get()2194 - Failed to get sample delay
[    0.156893] sunxi_spi_resource_get()2198 - sample_mode:-1431633921 sample_delay:-1431633921
[    0.157022] sunxi_spi_clk_init()2240 - [spi0] mclk 80000000
[    0.157876] sunxi_spi_probe()2653 - [spi0]: driver probe succeed, base ffffffd004058000, irq 31
[    0.160241] workingset: timestamp_bits=62 max_order=14 bucket_order=0
[    0.167662] squashfs: version 4.0 (2009/01/31) Phillip Lougher
[    0.167917] ntfs: driver 2.1.32 [Flags: R/W].
[    0.168281] jffs2: version 2.2. (NAND) (SUMMARY)  © 2001-2006 Red Hat, Inc.
[    0.189248] io scheduler mq-deadline registered
[    0.189265] io scheduler kyber registered
[    0.190529] [DISP]disp_module_init
[    0.191182] disp 5000000.disp: Adding to iommu group 0
[    0.191838] [DISP] disp_init,line:2386:
[    0.191844] smooth display screen:0 type:1 mode:4
[    0.234898] display_fb_request,fb_id:0
[    0.278054] Freeing logo buffer memory: 4000K
[    0.278577] disp_al_manager_apply ouput_type:1
[    0.278710] [DISP] lcd_clk_config,line:732:
[    0.278724] disp 0, clk: pll(420000000),clk(420000000),dclk(70000000) dsi_rate(70000000)
[    0.278724]      clk real:pll(420000000),clk(420000000),dclk(105000000) dsi_rate(150000000)
[    0.279046] sun8iw20-pinctrl 2000000.pinctrl: 2000000.pinctrl supply vcc-pd not found, using dummy regulator
[    0.279905] [DISP]disp_module_init finish
[    0.280877] sunxi_sid_init()551 - insmod ok
[    0.281502] pwm-regulator: supplied by regulator-dummy
[    0.283365] sun8iw20-pinctrl 2000000.pinctrl: 2000000.pinctrl supply vcc-pe not found, using dummy regulator
[    0.283900] uart uart0: get regulator failed
[    0.283996] uart uart0: uart0 supply uart not found, using dummy regulator
[    0.284411] uart0: ttyS0 at MMIO 0x2500000 (irq = 18, base_baud = 1500000) is a SUNXI
[    0.284441] sw_console_setup()1808 - console setup baud 115200 parity n bits 8, flow n
[    1.008841] printk: console [ttyS0] enabled
[    1.014546] sun8iw20-pinctrl 2000000.pinctrl: 2000000.pinctrl supply vcc-pg not found, using dummy regulator
[    1.026112] uart uart1: get regulator failed
[    1.030889] uart uart1: uart1 supply uart not found, using dummy regulator
[    1.039026] uart1: ttyS1 at MMIO 0x2500400 (irq = 19, base_baud = 1500000) is a SUNXI
[    1.049155] misc dump reg init
[    1.053653] sunxi-rfkill soc@3000000:rfkill@0: module version: v1.0.9
[    1.061001] sunxi-rfkill soc@3000000:rfkill@0: get gpio chip_en failed
[    1.068336] sunxi-rfkill soc@3000000:rfkill@0: get gpio power_en failed
[    1.075806] sunxi-rfkill soc@3000000:rfkill@0: wlan_busnum (1)
[    1.082308] sunxi-rfkill soc@3000000:rfkill@0: Missing wlan_power.
[    1.089275] sunxi-rfkill soc@3000000:rfkill@0: wlan_regon gpio=137 assert=1
[    1.097209] sunxi-rfkill soc@3000000:rfkill@0: wlan_hostwake gpio=202 assert=1
[    1.105358] sunxi-rfkill soc@3000000:rfkill@0: wakeup source is enabled
[    1.113084] sunxi-rfkill soc@3000000:rfkill@0: Missing bt_power.
[    1.119888] sunxi-rfkill soc@3000000:rfkill@0: bt_rst gpio=133 assert=0
[    1.128096] [ADDR_MGT] addr_mgt_probe: module version: v1.0.10
[    1.135878] [ADDR_MGT] addr_mgt_probe: success.
[    1.142271] spi-nor spi0.0: w25q128 (16384 Kbytes)
[    1.150219] 7 sunxipart partitions found on MTD device spi0.0
[    1.156761] Creating 7 MTD partitions on "spi0.0":
[    1.162111] 0x000000000000-0x000000180000 : "uboot"
[    1.174910] 0x000000180000-0x0000001a0000 : "boot-resource"
[    1.194894] 0x0000001a0000-0x0000001c0000 : "env"
[    1.214859] 0x0000001c0000-0x0000001e0000 : "env-redund"
[    1.234825] 0x0000001e0000-0x000000960000 : "boot"
[    1.254839] 0x000000960000-0x000000e60000 : "rootfs"
[    1.274882] 0x000000e60000-0x000001000000 : "UDISK"
[    1.295094] ehci_hcd: USB 2.0 'Enhanced' Host Controller (EHCI) Driver
[    1.302406] sunxi-ehci: EHCI SUNXI driver
[    1.307584] get ehci1-controller wakeup-source is fail.
[    1.313516] sunxi ehci1-controller don't init wakeup source
[    1.319843] [sunxi-ehci1]: probe, pdev->name: 4200000.ehci1-controller, sunxi_ehci: 0xffffffe0006a5968, 0x:ffffffd004075000, irq_no:31
[    1.333440] sunxi-ehci 4200000.ehci1-controller: 4200000.ehci1-controller supply drvvbus not found, using dummy regulator
[    1.346016] sunxi-ehci 4200000.ehci1-controller: 4200000.ehci1-controller supply hci not found, using dummy regulator
[    1.358354] sunxi-ehci 4200000.ehci1-controller: EHCI Host Controller
[    1.365653] sunxi-ehci 4200000.ehci1-controller: new USB bus registered, assigned bus number 1
[    1.375584] sunxi-ehci 4200000.ehci1-controller: irq 49, io mem 0x04200000
[    1.403980] sunxi-ehci 4200000.ehci1-controller: USB 2.0 started, EHCI 1.00
[    1.412967] hub 1-0:1.0: USB hub found
[    1.417344] hub 1-0:1.0: 1 port detected
[    1.422706] ohci_hcd: USB 1.1 'Open' Host Controller (OHCI) Driver
[    1.429725] sunxi-ohci: OHCI SUNXI driver
[    1.434948] get ohci1-controller wakeup-source is fail.
[    1.440916] sunxi ohci1-controller don't init wakeup source
[    1.447262] [sunxi-ohci1]: probe, pdev->name: 4200400.ohci1-controller, sunxi_ohci: 0xffffffe0006a5d30
[    1.457705] sunxi-ohci 4200400.ohci1-controller: 4200400.ohci1-controller supply drvvbus not found, using dummy regulator
[    1.470355] sunxi-ohci 4200400.ohci1-controller: 4200400.ohci1-controller supply hci not found, using dummy regulator
[    1.482691] sunxi-ohci 4200400.ohci1-controller: OHCI Host Controller
[    1.489976] sunxi-ohci 4200400.ohci1-controller: new USB bus registered, assigned bus number 2
[    1.499896] sunxi-ohci 4200400.ohci1-controller: irq 50, io mem 0x04200400
[    1.579129] hub 2-0:1.0: USB hub found
[    1.583375] hub 2-0:1.0: 1 port detected
[    1.590973] sunxi-rtc 7090000.rtc: registered as rtc0
[    1.596865] sunxi-rtc 7090000.rtc: setting system clock to 1970-01-01T02:04:44 UTC (7484)
[    1.606081] sunxi-rtc 7090000.rtc: sunxi rtc probed
[    1.612051] i2c /dev entries driver
[    1.616055] IR NEC protocol handler initialized
[    1.624271] usbcore: registered new interface driver uvcvideo
[    1.630710] USB Video Class driver (1.1.1)
[    1.635323] sunxi cedar version 1.1
[    1.639546] sunxi-cedar 1c0e000.ve: Adding to iommu group 0
[    1.645914] VE: install start!!!
[    1.645914]
[    1.651455] VE: cedar-ve the get irq is 6
[    1.651455]
[    1.657869] VE: ve_debug_proc_info:(____ptrval____), data:(____ptrval____), lock:(____ptrval____)
[    1.657869]
[    1.669509] VE: install end!!!
[    1.669509]
[    1.674642] VE: sunxi_cedar_probe
[    1.678617] Bluetooth: HCI UART driver ver 2.3
[    1.683571] Bluetooth: HCI UART protocol H4 registered
[    1.689347] Bluetooth: HCI UART protocol BCSP registered
[    1.695514] Bluetooth: XRadio Bluetooth LPM Mode Driver Ver 1.0.10
[    1.702813] [XR_BT_LPM] bluesleep_probe: bt_wake polarity: 1
[    1.709312] [XR_BT_LPM] bluesleep_probe: host_wake polarity: 1
[    1.715883] [XR_BT_LPM] bluesleep_probe: wakeup source is disabled!
[    1.715883]
[    1.724595] [XR_BT_LPM] bluesleep_probe: uart_index(1)
[    1.733393] sunxi-mmc 4020000.sdmmc: SD/MMC/SDIO Host Controller Driver(v4.21 2021-11-18 10:02)
[    1.743488] sunxi-mmc 4020000.sdmmc: ***ctl-spec-caps*** 8
[    1.749717] sunxi-mmc 4020000.sdmmc: No vmmc regulator found
[    1.756106] sunxi-mmc 4020000.sdmmc: No vqmmc regulator found
[    1.762506] sunxi-mmc 4020000.sdmmc: No vdmmc regulator found
[    1.768940] sunxi-mmc 4020000.sdmmc: No vd33sw regulator found
[    1.775504] sunxi-mmc 4020000.sdmmc: No vd18sw regulator found
[    1.782001] sunxi-mmc 4020000.sdmmc: No vq33sw regulator found
[    1.788570] sunxi-mmc 4020000.sdmmc: No vq18sw regulator found
[    1.795653] sunxi-mmc 4020000.sdmmc: Got CD GPIO
[    1.801151] sunxi-mmc 4020000.sdmmc: set cd-gpios as 24M fail
[    1.807911] sunxi-mmc 4020000.sdmmc: sdc set ios:clk 0Hz bm PP pm UP vdd 21 width 1 timing LEGACY(SDR12) dt B
[    1.819057] sunxi-mmc 4020000.sdmmc: no vqmmc,Check if there is regulator
[    1.839264] sunxi-mmc 4020000.sdmmc: sdc set ios:clk 400000Hz bm PP pm ON vdd 21 width 1 timing LEGACY(SDR12) dt B
[    1.863788] sunxi-mmc 4020000.sdmmc: sdc set ios:clk 0Hz bm PP pm OFF vdd 0 width 1 timing LEGACY(SDR12) dt B
[    1.874947] sunxi-mmc 4020000.sdmmc: detmode:gpio irq
[    1.881360] sunxi-mmc 4021000.sdmmc: SD/MMC/SDIO Host Controller Driver(v4.21 2021-11-18 10:02)
[    1.891565] sunxi-mmc 4021000.sdmmc: ***ctl-spec-caps*** 8
[    1.897828] sunxi-mmc 4021000.sdmmc: No vmmc regulator found
[    1.904224] sunxi-mmc 4021000.sdmmc: No vqmmc regulator found
[    1.910624] sunxi-mmc 4021000.sdmmc: No vdmmc regulator found
[    1.917058] sunxi-mmc 4021000.sdmmc: No vd33sw regulator found
[    1.923580] sunxi-mmc 4021000.sdmmc: No vd18sw regulator found
[    1.930113] sunxi-mmc 4021000.sdmmc: No vq33sw regulator found
[    1.936671] sunxi-mmc 4021000.sdmmc: No vq18sw regulator found
[    1.943200] sunxi-mmc 4021000.sdmmc: Cann't get pin bias hs pinstate,check if needed
[    1.952830] sunxi-mmc 4021000.sdmmc: sdc set ios:clk 0Hz bm PP pm UP vdd 21 width 1 timing LEGACY(SDR12) dt B
[    1.963988] sunxi-mmc 4021000.sdmmc: no vqmmc,Check if there is regulator
[    1.984144] sunxi-mmc 4021000.sdmmc: sdc set ios:clk 400000Hz bm PP pm ON vdd 21 width 1 timing LEGACY(SDR12) dt B
[    2.008376] sunxi-mmc 4021000.sdmmc: detmode:manually by software
[    2.016052] sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 52, RTO !!
[    2.023246] ashmem: initialized
[    2.026772] sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 52, RTO !!
[    2.033685] sunxi-mmc 4021000.sdmmc: sdc set ios:clk 400000Hz bm PP pm ON vdd 21 width 1 timing LEGACY(SDR12) dt B
[    2.050339] [AUDIOCODEC][sunxi_codec_parse_params][2412]:digital_vol:0, lineout_vol:26, mic1gain:31, mic2gain:31 pa_msleep:120, pa_level:1, pa_pwr_level:1
[    2.050339]
[    2.067665] [AUDIOCODEC][sunxi_codec_parse_params][2448]:adcdrc_cfg:0, adchpf_cfg:1, dacdrc_cfg:0, dachpf:0
[    2.079194] [AUDIOCODEC][sunxi_internal_codec_probe][2609]:codec probe finished
[    2.087466] sunxi-mmc 4021000.sdmmc: sdc set ios:clk 400000Hz bm PP pm ON vdd 21 width 1 timing LEGACY(SDR12) dt B
[    2.100621] debugfs: Directory '203034c.dummy_cpudai' with parent 'audiocodec' already present!
[    2.110578] [SNDCODEC][sunxi_card_init][583]:card init finished
[    2.118102] sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 5, RTO !!
[    2.125696] sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 5, RTO !!
[    2.133257] sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 5, RTO !!
[    2.140740] sunxi-codec-machine 2030340.sound: 2030000.codec <-> 203034c.dummy_cpudai mapping ok
[    2.150542] sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 5, RTO !!
[    2.157349] sunxi-mmc 4021000.sdmmc: sdc set ios:clk 0Hz bm PP pm OFF vdd 0 width 1 timing LEGACY(SDR12) dt B
[    2.170213] input: audiocodec sunxi Audio Jack as /devices/platform/soc@3000000/2030340.sound/sound/card0/input0
[    2.182642] [SNDCODEC][sunxi_card_dev_probe][836]:register card finished
[    2.192438] NET: Registered protocol family 10
[    2.199198] Segment Routing with IPv6
[    2.203505] NET: Registered protocol family 17
[    2.208780] Bluetooth: RFCOMM TTY layer initialized
[    2.214457] Bluetooth: RFCOMM socket layer initialized
[    2.220251] Bluetooth: RFCOMM ver 1.11
[    2.254092] sunxi-i2c sunxi-i2c2: sunxi-i2c2 supply twi not found, using dummy regulator
[    2.269939] sunxi-i2c sunxi-i2c2: probe success
[    2.277215] sun8iw20-pinctrl 2000000.pinctrl: 2000000.pinctrl supply vcc-pb not found, using dummy regulator
[    2.291717] get ehci0-controller wakeup-source is fail.
[    2.297794] sunxi ehci0-controller don't init wakeup source
[    2.304082] [sunxi-ehci0]: probe, pdev->name: 4101000.ehci0-controller, sunxi_ehci: 0xffffffe0006a51d8, 0x:ffffffd0040fd000, irq_no:2e
[    2.317660] [sunxi-ehci0]: Not init ehci0
[    2.322699] get ohci0-controller wakeup-source is fail.
[    2.328758] sunxi ohci0-controller don't init wakeup source
[    2.335012] [sunxi-ohci0]: probe, pdev->name: 4101400.ohci0-controller, sunxi_ohci: 0xffffffe0006a55a0
[    2.345455] [sunxi-ohci0]: Not init ohci0
[    2.354475] clk: Not disabling unused clocks
[    2.359279] ALSA device list:
[    2.362581]   #0: audiocodec
[    2.367078] platform regulatory.0: Direct firmware load for regulatory.db failed with error -2
[    2.376830] cfg80211: failed to load regulatory.db
[    2.382241] alloc_fd: slot 0 not NULL!
[    2.391463] VFS: Mounted root (squashfs filesystem) readonly on device 31:5.
[    2.407785] devtmpfs: mounted
[    2.411302] Freeing unused kernel memory: 144K
[    2.416354] This architecture does not have kernel memory protection.
[    2.423565] Run /sbin/init as init process
[    2.437570] random: fast init done
[    2.833256] [SNDCODEC][sunxi_check_hs_detect_status][191]:plugin --> switch:1
[    3.444188]
[    3.444188] insmod_device_driver
[    3.444188]
[    3.451429] sunxi_usb_udc 4100000.udc-controller: 4100000.udc-controller supply udc not found, using dummy regulator
[    3.469630] device_chose finished 139!
[    3.606030] init: Console is alive
[    3.610285] init: - preinit -
/dev/by-name/UDISK already format by jffs2
[    5.339399] mount_root: mounting /dev/root
[    5.344741] mount_root: loading kmods from internal overlay
[    5.491810] random: crng init done
[    5.765812] block: attempting to load /etc/config/fstab
[    5.807607] block: check_filesystem: jffs2 is not supported
[    5.838954] jffs2: notice: (91) jffs2_build_xattr_subsystem: complete building xattr subsystem, 10 of xdatum (0 unchecked, 3 orphan) and 12 of xref (3 dead, 0 orphan) found.
[    5.860743] block: extroot: UUID match (root: bc2f88ab-06eba037-7590c3e8-6fa728e7, overlay: bc2f88ab-06eba037-7590c3e8-6fa728e7)
[    5.882754] overlayfs: upper fs does not support tmpfile.
[    5.894649] mount_root: switched to extroot
[    5.919200] procd: - early -
[    6.166648] procd: - ubus -
[    6.170648] procd (1): /proc/100/oom_adj is deprecated, please use /proc/100/oom_score_adj instead.
[    6.476406] procd: - init -
Please press Enter to activate this console.
[    8.502814] file system registered
[    8.785556] configfs-gadget 4100000.udc-controller: failed to start g1: -19
[    9.204177] read descriptors
[    9.207403] read strings
[    9.366159] sunxi_set_cur_vol_work()397 WARN: get power supply failed
[    9.452309] android_work: sent uevent USB_STATE=CONNECTED
[    9.750396] sunxi_set_cur_vol_work()397 WARN: get power supply failed
[    9.844049] sunxi_vbus_det_work()3356 WARN: get power supply failed
[    9.874070] android_work: sent uevent USB_STATE=DISCONNECTED
[    9.884058] android_work: sent uevent USB_STATE=CONNECTED
[    9.890766] configfs-gadget gadget: high-speed config #1: c
[    9.904247] android_work: sent uevent USB_STATE=CONFIGURED
[   10.998925] ======== XRADIO WIFI OPEN ========
[   11.004696] [XRADIO] Driver Label:XR_V02.16.85_P2P_HT40_01.31
[   11.011460] [XRADIO] Allocated hw_priv @ 0000000093cf579f
[   11.024060] sunxi-rfkill soc@3000000:rfkill@0: bus_index: 1
[   11.050057] sunxi-rfkill soc@3000000:rfkill@0: wlan power on success
[   11.260453] sunxi-mmc 4021000.sdmmc: sdc set ios:clk 0Hz bm PP pm UP vdd 21 width 1 timing LEGACY(SDR12) dt B
[   11.271601] [XRADIO] Detect SDIO card 1
[   11.284074] sunxi-mmc 4021000.sdmmc: no vqmmc,Check if there is regulator
[   11.304205] sunxi-mmc 4021000.sdmmc: sdc set ios:clk 400000Hz bm PP pm ON vdd 21 width 1 timing LEGACY(SDR12) dt B
[   11.329295] sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 52, RTO !!
[   11.336962] sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 52, RTO !!
[   11.347857] sunxi-mmc 4021000.sdmmc: sdc set ios:clk 400000Hz bm PP pm ON vdd 21 width 1 timing LEGACY(SDR12) dt B
[   11.377223] sunxi-mmc 4021000.sdmmc: sdc set ios:clk 400000Hz bm PP pm ON vdd 21 width 1 timing LEGACY(SDR12) dt B
[   11.396443] sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 5, RTO !!
[   11.403978] sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 5, RTO !!
[   11.411576] sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 5, RTO !!
[   11.419230] sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 5, RTO !!
[   11.426072] sunxi-mmc 4021000.sdmmc: sdc set ios:clk 0Hz bm PP pm OFF vdd 0 width 1 timing LEGACY(SDR12) dt B
[   13.364108] sunxi-rfkill soc@3000000:rfkill@0: wlan power off success
[   13.471353] sunxi-mmc 4021000.sdmmc: sdc set ios:clk 0Hz bm PP pm UP vdd 21 width 1 timing LEGACY(SDR12) dt B
[   13.482458] [XRADIO] Remove SDIO card 1
[   13.494072] sunxi-mmc 4021000.sdmmc: no vqmmc,Check if there is regulator
[   13.503664] [SBUS_ERR] sdio probe timeout!
[   13.508321] [XRADIO_ERR] sbus_sdio_init failed
[   13.514634] sunxi-mmc 4021000.sdmmc: sdc set ios:clk 400000Hz bm PP pm ON vdd 21 width 1 timing LEGACY(SDR12) dt B
[   13.526581] xradio_core_init failed (-110)!
[   13.557678] sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 52, RTO !!
[   13.565382] sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 52, RTO !!
[   13.572229] sunxi-mmc 4021000.sdmmc: sdc set ios:clk 400000Hz bm PP pm ON vdd 21 width 1 timing LEGACY(SDR12) dt B
[   13.587126] sunxi-mmc 4021000.sdmmc: sdc set ios:clk 400000Hz bm PP pm ON vdd 21 width 1 timing LEGACY(SDR12) dt B
[   13.600976] sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 5, RTO !!
[   13.608589] sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 5, RTO !!
[   13.616646] sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 5, RTO !!
[   13.624186] sunxi-mmc 4021000.sdmmc: smc 1 p1 err, cmd 5, RTO !!
[   13.630916] sunxi-mmc 4021000.sdmmc: sdc set ios:clk 0Hz bm PP pm OFF vdd 0 width 1 timing LEGACY(SDR12) dt B
kmodloader done

```
**系统默认会自己登录 没有用户名 没有密码。**
**系统默认会自己登录 没有用户名 没有密码。**
**系统默认会自己登录 没有用户名 没有密码。**