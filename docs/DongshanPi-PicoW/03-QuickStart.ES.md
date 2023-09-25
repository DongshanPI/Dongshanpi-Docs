[ [中文](https://dongshanpi.com/DongshanPi-PicoW/03-QuickStart/) | [[Español]](https://dongshanpi.com/DongshanPi-PicoW/03-QuickStart.ES/) ]

# Guía de inicio rápido

## Conexión del módulo de serie

El DongshanPI-PicoW es una placa de desarrollo mínima que puede comenzar a funcionar simplemente conectándolo a una fuente de alimentación de 5V. Para ello, se puede utilizar un módulo de serie y seguir las siguientes instrucciones de conexión:

- Conectar el pin `P42 TX` a la entrada `RX` del módulo USB-Serial.
- Conectar el pin `P43 RX` a la salida `TX` del módulo USB-Serial.
- Conectar el pin `P46 GND` a la conexión `GND` del módulo USB-Serial.
- Opcionalmente, se puede conectar el pin `P31 5V` a la salida `5V` del módulo USB-Serial.

Una vez que se ha verificado la conexión, conectar el DongshanPI-PicoW al puerto USB de un ordenador. La placa se iniciará automáticamente y se puede acceder al sistema a través de un programa de terminal serie.

**Nota: Asegúrese de que la conexión de los pines `5V` y `GND` del módulo USB-Serial con los pines `P31` y `P46` de la placa esté correcta antes de encenderla, de lo contrario podría dañar la placa y no nos haremos responsables del problema.**

![](https://photos.100ask.net/dongshanpi-docs/DongshanPI-PicoW/DongshanPI-PicoW-TOPUSBLine.png)

A continuación se muestra un ejemplo de diagrama de conexión entre un DongshanPI-PicoW y un módulo USB-Serial. ¡Asegúrese de verificar los cables y las conexiones del módulo USB-Serial!

![](https://photos.100ask.net/dongshanpi-docs/DongshanPI-PicoW/DongshanPI-PicoW-UartBoot.png)


## 2. Acceso al sistema a través de un puerto serie

### 1. Conexión del cable de puerto serie

Conecte un cable TypeC que viene con la placa de desarrollo a la interfaz de puerto serie / alimentación de la placa y el otro extremo a un puerto USB de la computadora. Si la conexión es correcta, la luz roja de alimentación en la placa se encenderá automáticamente.
En la mayoría de los casos, el sistema instalará automáticamente el controlador del dispositivo de puerto serie. Si el controlador no se instala automáticamente, puede usar el controlador Genius para instalarlo automáticamente.

* Para el sistema Windows
En la sección Puertos (COM y LPT) del Administrador de dispositivos de Windows, se agregará un dispositivo de puerto serie, que generalmente comienza con `USB-Enhanced-SERIAL CH9102`. Preste atención al número de COM específico detrás de él para usarlo en la conexión posterior.

![QuickStart-01](https://photos.100ask.net/dongshanpi-docs/DongshanNezhaSTU/QuickStart-01.png)

En la imagen de arriba, el número de COM es 96, que es el número de puerto serie que usaremos a continuación.

* Para el sistema Linux
Puede verificar si se agrega un dispositivo `/dev/tty<>`, y en la mayoría de los casos el dispositivo se encuentra en `/dev/ttyACM0`.

![QuickStart-02](https://photos.100ask.net/dongshanpi-docs/DongshanNezhaSTU/QuickStart-02.png)


### 2. Abrir la consola serie
#### Obtener la herramienta serie
Utilice una herramienta serie como Putty o MobaXterm para conectarse al dispositivo de la placa.

* La herramienta Putty se puede obtener visitando la página https://www.chiark.greenend.org.uk/~sgtatham/putty/.
* Puede obtener MobaXterm visitando la página https://mobaxterm.mobatek.net/ (se recomienda utilizar).

#### Iniciar sesión en la consola serie con Putty

![QuickStart-04](https://photos.100ask.net/dongshanpi-docs/DongshanNezhaSTU/QuickStart-04.png)

#### Iniciar sesión en la consola serie con Mobaxterm
Abra MobaXterm, haga clic en "Session" en la esquina superior izquierda, seleccione "Serial" en la ventana emergente y luego seleccione el número de puerto (el número de puerto COM21 que se muestra en el administrador de dispositivos), la velocidad de bits (115200), el control de flujo (Flow Control: none). Por último, haga clic en "OK". Los pasos se muestran en la siguiente figura.
**Nota: Debe seleccionar none como control de flujo (Flow Control), de lo contrario no podrá ingresar datos al puerto serie en MobaXterm.**

![Mobaxterm_serial_set_001](https://photos.100ask.net/dongshanpi-docs/DongshanNezhaSTU/Mobaxterm_serial_set_001.png)

### 3. Ingresar al shell del sistema
Después de abrir el puerto serie con éxito utilizando la herramienta de puerto serie, puede presionar la tecla Enter para ingresar al shell. También puede presionar el botón "Reset" en la placa para ver la información completa del sistema.

``` bash
IPL gdf99011
D-06
HW Reset
00000000 00000000
Resume? N, addr 00000000
miupll_200MHz
SPI 54M
64MB
BIST0_0001-OK
SPI 54M
[BBT] Found table @ 0x00020000

Checksum OK

IPL_CUST gdf99011
Export ENV 1



U-Boot 2015.01 (Dec 10 2021 - 09:17:46), Build: jenkins-devops--UBT--AutoRelese_To_ALKAID-141

Version: P3g5414d39
I2C:   ready
DRAM:
WARNING: Caches not enabled
SPINAND_I:  [FLASH] Found SNI in block 0.
[FLASH] dev_id = 0xee
[FLASH] mfr_id = 0xef, dev_id= 0xaa id_len = 0x3
[SPINAND] RFC ues command 0x6b with 0x08 dummy clock.
[SPINAND] Program load with command 0x32.
[SPINAND] Random load with command 0x34.
[FLASH] Unlock all block.
[FLASH] Use BDMA.
128 MiB
MMC:   MStar SD/MMC: 0
ENV: offset = 0x480000 size = 0x40000
ENV1: offset = 0x4c0000 size = 0x40000
In:    serial
Out:   serial
Err:   serial
Net:   No ethernet found.

NAND read: device 0 offset 0x520000, size 0x500000
Time:383961 us, speed:13654 KB/s
 5242880 bytes read: OK
found the partition info of MISC
_BootLogoSetPq 1674, pData: 23a0ce00 u32DataSize: 2964
gpio debug MHal_GPIO_Pad_Set: pin=62
clk=12M, u16Div=0 u32Duty=0x95f u32Period=0x95f
[halPWMPadSet][106] (pwmId, padId) = (1, 63)
##  Booting kernel from Legacy Image at 22000000 ...
   Image Name:   MVX4##P3##g#######KL_LX409##[BR:
   Image Type:   ARM Linux Kernel Image (lzma compressed)
   Data Size:    2121028 Bytes = 2 MiB
   Load Address: 20008000
   Entry Point:  20008000
   Verifying Checksum ... OK
-usb_stop(USB_PORT0)
-usb_stop(USB_PORT2)
   Uncompressing Kernel Image ...
[XZ] !!!reserved 0x21000000 length=0x 1000000 for xz!!
   XZ: uncompressed size=0x43d000, ret=7
OK
atags:0x20000000

Starting kernel ...

early_atags_to_fdt() success
Booting Linux on physical CPU 0x0
Linux version 4.9.84 (ubuntu@ubuntu1804) (gcc version 9.1.0 (GCC) ) #6 SMP PREEMPT Thu Mar 16 04:59:57 EDT 2023
CPU: ARMv7 Processor [410fc075] revision 5 (ARMv7), cr=50c5387d
CPU: div instructions available: patching division code
CPU: PIPT / VIPT nonaliasing data cache, VIPT aliasing instruction cache
early_atags_to_fdt() success
OF: fdt:Machine model: PIONEER3 SSC021A-S01A-S
[ERR] LX_MEM, LX_MEM2, LX_MEM3 not 1MB aligned
LXmem is 0x3fe0000 PHYS_OFFSET is 0x20000000
Add mem start 0x20000000 size 0x3fe0000!!!!

LX_MEM  = 0x20000000, 0x3fe0000
LX_MEM2 = 0x0, 0x0
LX_MEM3 = 0x0, 0x0
EMAC_LEN= 0x0
DRAM_LEN= 0x0
deal_with_reserved_mmap memblock_reserve success mmap_reserved_config[0].reserved_start=
0x23c00000

deal_with_reserve_mma_heap memblock_reserve success mma_config[0].reserved_start=
0x21e00000

cma: Reserved 2 MiB at 0x21c00000
Memory policy: Data cache writealloc
percpu: Embedded 14 pages/cpu @c3f1c000 s25368 r8192 d23784 u57344
Built 1 zonelists in Zone order, mobility grouping on.  Total pages: 16224
Kernel command line: ubi.mtd=UBI,0x800 root=/dev/mtdblock8 rootfstype=squashfs ro init=/linuxrc LX_MEM=0x3FE0000 mma_heap=mma_heap_name0,miu=0,sz=0x1E00000 cma=2M highres=off mmap_reserved=fb,miu=0,sz=0x300000,max_start_off=0x3C00000,max_end_off=0x3F00000 mtdparts=nand0:0x140000(CIS),0x1a0000(BOOT0),0x1a0000(BOOT1),0x40000(ENV),0x40000(ENV1),0x20000(KEY_CUST),0x500000(KERNEL),0x500000(RECOVERY),0x600000(rootfs),0xa0000(MISC),-(UBI)
PID hash table entries: 256 (order: -2, 1024 bytes)
Dentry cache hash table entries: 8192 (order: 3, 32768 bytes)
Inode-cache hash table entries: 4096 (order: 2, 16384 bytes)
Memory: 24224K/65408K available (2514K kernel code, 206K rwdata, 1304K rodata, 168K init, 159K bss, 39136K reserved, 2048K cma-reserved)
Virtual kernel memory layout:
    vector  : 0xffff0000 - 0xffff1000   (   4 kB)
    fixmap  : 0xffc00000 - 0xfff00000   (3072 kB)
    vmalloc : 0xc4000000 - 0xff800000   ( 952 MB)
    lowmem  : 0xc0000000 - 0xc3fe0000   (  63 MB)
    modules : 0xbf800000 - 0xc0000000   (   8 MB)
      .text : 0xc0008000 - 0xc027ce44   (2516 kB)
      .init : 0xc03e6000 - 0xc0410000   ( 168 kB)
      .data : 0xc0410000 - 0xc0443bc8   ( 207 kB)
       .bss : 0xc0445000 - 0xc046ce90   ( 160 kB)
SLUB: HWalign=64, Order=0-3, MinObjects=0, CPUs=2, Nodes=1
Preemptible hierarchical RCU implementation.
        Build-time adjustment of leaf fanout to 32.
        RCU restricting CPUs from NR_CPUS=4 to nr_cpu_ids=2.
RCU: Adjusting geometry for rcu_fanout_leaf=32, nr_cpu_ids=2
NR_IRQS:16 nr_irqs:16 16
ms_init_main_intc: np->name=ms_main_intc, parent=gic
ms_init_pm_intc: np->name=ms_pm_intc, parent=ms_main_intc
ss_init_gpi_intc: np->name=ms_gpi_intc, parent=ms_main_intc
Find CLK_cpupll_clk, hook ms_cpuclk_ops
Find CLK_spi_synth_pll, hook ss_pllclk_ops
arm_arch_timer: Architected cp15 timer(s) running at 6.00MHz (virt).
clocksource: arch_sys_counter: mask: 0xffffffffffffff max_cycles: 0x1623fa770, max_idle_ns: 440795202238 ns
sched_clock: 56 bits at 6MHz, resolution 166ns, wraps every 4398046511055ns
Switching to timer-based delay loop, resolution 166ns
Console: colour dummy device 80x30
console [ttyS0] enabled
Calibrating delay loop (skipped), value calculated using timer frequency.. 12.00 BogoMIPS (lpj=60000)
pid_max: default: 4096 minimum: 301
Mount-cache hash table entries: 1024 (order: 0, 4096 bytes)
Mountpoint-cache hash table entries: 1024 (order: 0, 4096 bytes)
CPU: Testing write buffer coherency: ok
CPU0: update cpu_capacity 1024
CPU0: thread -1, cpu 0, socket 0, mpidr 80000000
Setting up static identity map for 0x20008280 - 0x200082cc
CPU1: update cpu_capacity 1024
CPU1: thread -1, cpu 1, socket 0, mpidr 80000001
Brought up 2 CPUs
SMP: Total of 2 processors activated (24.00 BogoMIPS).
CPU: All CPU(s) started in SVC mode.
devtmpfs: initialized
VFP support v0.3: implementor 41 architecture 2 part 30 variant 7 rev 5
clocksource: jiffies: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 19112604462750000 ns
futex hash table entries: 16 (order: -2, 1024 bytes)
NET: Registered protocol family 16
DMA: preallocated 256 KiB pool for atomic coherent allocations


Version : MVX4##P3##g#######KL_LX409##[BR:g]#XVM

GPIO: probe endhw-breakpoint: found 5 (+1 reserved) breakpoint and 4 watchpoint registers.
hw-breakpoint: maximum watchpoint size is 8 bytes.
SCSI subsystem initialized
Pinreg:0 bit:0 enable:0 speed:5
Port:0 Index=1
Enable=1
DmaReadMode:0
Speed:5
DmaEnable:0
DmaAddrMode:0
DmaMiuCh:0
DmaMiuPri:0
DmaPhyAddr:21c40000
START default delay 5(us)
STOP default delay 5(us)
HWI2C_MUTEX_CREATE!
HWI2C(0): initialized
Pinreg:0 bit:0 enable:0 speed:5
Port:1 Index=17
Enable=1
DmaReadMode:0
Speed:5
DmaEnable:1
DmaAddrMode:0
DmaMiuCh:0
DmaMiuPri:0
DmaPhyAddr:21c41000
START default delay 5(us)
STOP default delay 5(us)
HWI2C_MUTEX_CREATE!
HWI2C(1): initialized
[DrvPWMDutyQE0] grp:0
[DrvPWMDutyQE0] grp:0
[DrvPWMDutyQE0] grp:0
[DrvPWMDutyQE0] grp:0
[NOTICE]pwm-isr(60) success. If not i6e or i6b0, pls confirm it on .dtsi
clocksource: Switched to clocksource arch_sys_counter
NET: Registered protocol family 2
TCP established hash table entries: 1024 (order: 0, 4096 bytes)
TCP bind hash table entries: 1024 (order: 2, 20480 bytes)
TCP: Hash tables configured (established 1024 bind 1024)
UDP hash table entries: 128 (order: 0, 6144 bytes)
UDP-Lite hash table entries: 128 (order: 0, 6144 bytes)
NET: Registered protocol family 1
hw perfevents: enabled with armv7_cortex_a7 PMU driver, 5 counters available
workingset: timestamp_bits=30 max_order=13 bucket_order=0
squashfs: version 4.0 (2009/01/31) Phillip Lougher
jffs2: version 2.2. © 2001-2006 Red Hat, Inc.
fuse init (API version 7.26)
io scheduler noop registered
io scheduler deadline registered (default)
libphy: Fixed MDIO Bus: probed
mousedev: PS/2 mouse device common for all mice
[MHal_GPIO_Check_PE] set gpio84 PE
i2c /dev entries driver
1f221000.uart0: ttyS0 at MMIO 0x0 (irq = 34, base_baud = 10800000) is a unknown
1f221200.uart1: ttyS1 at MMIO 0x0 (irq = 35, base_baud = 10800000) is a unknown
1f220400.uart2: ttyS2 at MMIO 0x0 (irq = 37, base_baud = 10800000) is a unknown
1f221400.uart2: ttyS3 at MMIO 0x0 (irq = 38, base_baud = 10800000) is a unknown
>> [sdmmc] ms_sdmmc_probe
Fail to get pad(131074) form padmux !
>> [sdmmc] Err: Could not get SD pad group from Padmux dts!
>> [sdmmc] Err: Failed to use DTS function!

ms_sdmmc: probe of soc:sdmmc failed with error -22
MSYS: DMEM request: [emac0_buff]:0x00000812
MSYS: DMEM request: [emac0_buff]:0x00000812 success, CPU phy:@0x21C44000, virt:@0xC1C44000
libphy: mdio: probed
mdio_bus mdio-bus@emac0: /soc/emac0/mdio-bus/ethernet-phy@0 has invalid PHY address
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 0
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 1
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 2
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 3
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 4
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 5
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 6
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 7
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 8
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 9
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 10
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 11
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 12
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 13
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 14
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 15
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 16
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 17
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 18
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 19
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 20
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 21
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 22
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 23
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 24
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 25
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 26
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 27
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 28
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 29
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 30
mdio_bus mdio-bus@emac0: scan phy ethernet-phy at address 31
[emac_phy_connect][3534] connected mac emac0 to PHY at mdio-bus@emac0:00 [uid=11112222, driver=Generic PHY]
[ms_cpufreq_init] Current clk=799999872
[FLASH] Found SNI in block 0.
[FLASH] dev_id = 0xee
MSYS: DMEM request: [BDMA]:0x00000840
MSYS: DMEM request: [BDMA]:0x00000840 success, CPU phy:@0x21C45000, virt:@0xC1C45000
[FLASH] mfr_id = 0xef, dev_id= 0xaa id_len = 0x3
[SPINAND] RFC ues command 0x6b with 0x08 dummy clock.
[SPINAND] Program load with command 0x32.
[SPINAND] Random load with command 0x34.
[FLASH] Use BDMA.
nand: device found, Manufacturer ID: 0xef, Chip ID: 0xaa
nand: 128 MiB, SLC, erase size: 128 KiB, page size: 2048, OOB size: 64
11 cmdlinepart partitions found on MTD device nand0
Creating 11 MTD partitions on "nand0":
0x000000000000-0x000000140000 : "CIS"
0x000000140000-0x0000002e0000 : "BOOT0"
0x0000002e0000-0x000000480000 : "BOOT1"
0x000000480000-0x0000004c0000 : "ENV"
0x0000004c0000-0x000000500000 : "ENV1"
0x000000500000-0x000000520000 : "KEY_CUST"
0x000000520000-0x000000a20000 : "KERNEL"
0x000000a20000-0x000000f20000 : "RECOVERY"
0x000000f20000-0x000001520000 : "rootfs"
0x000001520000-0x0000015c0000 : "MISC"
0x0000015c0000-0x000008000000 : "UBI"
[wakeup source] HW gate_xtal:0 SourceNum:1
[wakeup source] WakeupSource:61

[ss_gpi_intc_domain_alloc] hw:61 -> v:65
[ss_gpi_irq_set_wake] hw:61 enable? 1
NET: Registered protocol family 17
[mstar_pm_init] resume_pbase=0x200114F5, suspend_imi_vbase=0xC4057000
ThumbEE CPU extension supported.
Registering SWP/SWPB emulation handler
ubi0: attaching mtd10
ubi0: scanning is finished
ubi0: attached mtd10 (name "UBI", size 106 MiB)
ubi0: PEB size: 131072 bytes (128 KiB), LEB size: 126976 bytes
ubi0: min./max. I/O unit sizes: 2048/2048, sub-page size 2048
ubi0: VID header offset: 2048 (aligned 2048), data offset: 4096
ubi0: good PEBs: 850, bad PEBs: 0, corrupted PEBs: 0
ubi0: user volume: 3, internal volumes: 1, max. volumes count: 128
ubi0: max/mean erase counter: 2/1, WL threshold: 4096, image sequence number: 0
ubi0: available PEBs: 25, total reserved PEBs: 825, PEBs reserved for bad PEB handling: 20
ubi0: background thread "VFS: Mounted root (squashfs filesystem) readonly on device 31:8.
devtmpfs: mounted
This architecture does not have kernel memory protection.
[DMA]: Transfer NOT Completely!
ERROR: Bus[1] in ms_i2c_xfer_write: Slave dev NAK, Addr: 0xba, Data: 0x80 0x47
Goodix-TS 1-005d: i2c test failed attempt 1: -110
[emac_phy_link_adjust] EMAC Link Down
[DMA]: Transfer NOT Completely!
ERROR: Bus[1] in ms_i2c_xfer_write: Slave dev NAK, Addr: 0xba, Data: 0x80 0x47
Goodix-TS 1-005d: i2c test failed attempt 2: -110
Goodix-TS 1-005d: I2C communication failure: -110
Goodix-TS 1-005d: touchscreen config failed!!!
mount: can't find devpts in /etc/fstab
net.core.rmem_default = 163840
net.core.rmem_max = 163840
net.core.wmem_default = 524288
net.core.wmem_max = 1048576
net.ipv4.tcp_mem = 924  1232  1848
net.ipv4.tcp_rmem = 4096  87380  325120
net.ipv4.tcp_wmem = 4096  131072  393216
mount: mounting none on /sys failed: Drandom: fast init done
evice or resource busy
mount: mounting noUBIFS (ubi0:0): background thread "ubifs_bgt0_0" started, PID 577
ne on /sys/kernel/debug/ failed: Device or resource busy
UBIFS (ubi0:0): recovery needed
UBIFS (ubi0:0): recovery completed
UBIFS (ubi0:0): UBIFS: mounted UBI device 0, volume 0, name "miservice"
UBIFS (ubi0:0): LEB size: 126976 bytes (124 KiB), min./max. I/O unit sizes: 2048 bytes/2048 bytes
UBIFS (ubi0:0): FS size: 11427840 bytes (10 MiB, 90 LEBs), journal size 1904640 bytes (1 MiB, 15 LEBs)
UBIFS (ubi0:0): reserved for root: 0 bytes (0 KiB)
UBIFS (ubi0:0): media format: w4/r0 (latest is w4/r0), UUID 17CB3D0B-DDBA-45AD-B591-2625FAE405BE, small LPT model
UBIFS (ubi0:1): background thread "ubifs_bgt0_1" started, PID 580
UBIFS (ubi0:1): recovery needed
UBIFS (ubi0:1): recovery completed
UBIFS (ubi0:1): UBIFS: mounted UBI device 0, volume 1, name "customer"
UBIFS (ubi0:1): LEB size: 126976 bytes (124 KiB), min./max. I/O unit sizes: 2048 bytes/2048 bytes
UBIFS (ubi0:1): FS size: 79613952 bytes (75 MiB, 627 LEBs), journal size 9023488 bytes (8 MiB, 72 LEBs)
UBIFS (ubi0:1): reserved for root: 0 bytes (0 KiB)
UBIFS (ubi0:1): media format: w4/r0 (latest is w4/r0), UUID 86613E8B-DAE5-49A0-9361-DE40E76B69BE, small LPT model
UBIFS (ubi0:2): background thread "ubifs_bgt0_2" started, PID 583
UBIFS (ubi0:2): recovery needed
UBIFS (ubi0:2): recovery completed
UBIFS (ubi0:2): UBIFS: mounted UBI device 0, volume 2, name "appconfigs"
UBIFS (ubi0:2): LEB size: 126976 bytes (124 KiB), min./max. I/O unit sizes: 2048 bytes/2048 bytes
UBIFS (ubi0:2): FS size: 3809280 bytes (3 MiB, 30 LEBs), journal size 1142785 bytes (1 MiB, 8 LEBs)
UBIFS (ubi0:2): reserved for root: 0 bytes (0 KiB)
UBIFS (ubi0:2): media format: w4/r0 (latest is w4/r0), UUID 6561C2CE-C61A-4F56-8E94-29512892FA00, small LPT model
firmwarefs/fwfs.c:1321:error: Corrupted dir pair at {0xa0000001, 0xa0000000}
fwfs_fuse.c:594:error: Invalid or incomplete multibyte or wide character
insmod: can't read '/config/modules/4.9.84/nfsv3.ko': No such file or directory
insmod: can't read '/config/modules/4.9.84/mmc_core.ko': No such file or directory
insmod: can't read '/config/modules/4.9.84/kdrv_sdmmc.ko': No such file or directory
insmod: can't read '/config/modules/4.9.84/ms_notify.ko': No such file or directory
jpe driver probed
[DRV_DIVP_PROC_Init]
AudioProcInit 374
module [sys] init
MI_SYSCFG_SetupMmapLoader default_config_path:/config/config_tool, argv1:/config/load_mmap,argv2:/config/mmap.ini
config...... cmdpath:/config/config_tool, argv0:/config/load_config
config...... cmdpath:/config/config_tool, argv1:/misc/config.ini
config...... cmdpath:/config/config_tool, argv2:/misc/PQConfig.ini
config...... cmdpath:/config/config_tool, argv3:(null)
mi_sys_mma_allocator_create success, heap_base_addr=20000000 length=20000
module [gfx] init
module [rgn] init
module [fb] init
[MI ERR ]: _MI_FB_GetLayerInfo[56]: no found fb info, please check fb ini config
no found fb ini param

module [ao] init
module [ai] init
module [pspi] init
module [divp] init
module [panel] init
module [disp] init
module [venc] init Dec 20 2021 14:59:44
insmod: can't read '/config/modules/4.9.84/media.ko': No such file or directory
insmod: can't read '/config/modules/4.9.84/videodev.ko': No such file or directory
insmod: can't read '/config/modules/4.9.84/v4l2-common.ko': No such file or directory
Mstar_ehc_init version:20180309
Sstar-ehci-1 H.W init
Titania3_series_start_ehc start
[USB] config miu select [70] [e8] [ef] [ef]
[USB] enable miu lower bound address subtraction
[USB] init squelch level 0x2
BC disable
insmod: can't read '/config/modules/4.9.84/scsi_mod.ko': No such file or directory
insmod: can't read '/config/modules/4.9.84/videobuf2-core.ko': No such file or directory
insmod: can't read '/config/modules/4.9.84/videobuf2-v4l2.ko': No such file or directory
insmod: can't read '/config/modules/4.9.84/videobuf2-memops.ko': No such file or directory
insmod: can't read '/config/modules/4.9.84/videobuf2-vmalloc.ko': No such file or directory
insmod: can't read '/config/modules/4.9.84/uvcvideo.ko': No such file or directory
==20180309==> hub_port_init 1 #0
Plug in USB Port1
/ #

```
**El sistema inicia sesión automáticamente sin usuario ni contraseña.**