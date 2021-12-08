# 编译烧写boot
简述：SSD202D芯片的boot阶段也是我们常说的bootLoader，是基于uboot2015进行开发适配，里面增加了大量的芯片原厂相关代码，和社区版本存在很大差异，由于SSD202只支持从NANDFLASH / NorFLASH启动，所以如果您的开发板上没有boot分区或者不小心破坏了boot分区，那么系统就无法正常启动，需要通过专门的烧录工具才可以进行烧录更新。

> 烧录工具受芯片市场供货影响，价格非常高，所以我们不建议大家操作这部分。

如果您觉得看文档出现了问题 可以直接查看我们提前录制好的手把手教学视频。
<iframe src="//player.bilibili.com/player.html?aid=293906511&bvid=BV18F411a7jM&cid=434612050&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

## 编译bootloader

首先进入boot源码根目录下，在编译之前需要确认清楚交叉编译工具链等环境是否配置成功，如果不确定 可以参考 [配置交叉编译工具链](DongshanPi-One/05-GetSourceCode/#_4) 章节。

> make infinity2m_spinand_defconfig; make menuconfig;

指定完成配置文件后，继续执行如下命令开始编译（根据服务器配置，可以支持多线程编译）
> make -j8

* 参考执行示例
``` shell
book@100ask:~/DongshanPiOne-TAKOYAKI/boot$ make infinity2m_spinand_defconfig
arch/../configs/.tmp_defconfig:326:warning: override: reassigning to symbol CMD_FASTBOOT
#
# configuration written to .config
#
book@100ask:~/DongshanPiOne-TAKOYAKI/boot$ make -j16
```

最后生成的镜像文件为：
> u-boot_spinand.xz.img.bin

注意：如果需要打包 编译出来的镜像到最终的完整系统内则 需要讲生成的文件拷贝到project目录：
> cp u-boot_spinand.xz.img.bin project/board/i2m/boot/spinand/uboot/

* 参考执行示例
``` shell

book@100ask:~/DongshanPiOne-TAKOYAKI/boot$  cp u-boot_spinand.xz.img.bin  ../project/board/i2m/boot/spinand/uboot/

```

## 烧写bootloader

### 进入uboot内烧写

首先参考上述操作将编译好的镜像拷贝到您的sd卡内，之后 执行如下命令进行逐个烧写

> fatload  mmc 0:1 0x21000000 uboot_s.bin
> 
> nand erase.part UBOOT0
> 
> nand write.e 0x21000000 UBOOT0 ${filesize}
> 
> nand erase.part UBOOT1
> 
> nand write.e 0x21000000 UBOOT1 ${filesize}


* 参考操作示例
``` shell



```
### 使用专门的烧写器烧写

本方式适用于空片烧录或者板子无法进入Uboot控制台使用其它方式烧录的情况。开机进入到Uboot控制台，输入debug (如能正常进入)，此时uboot串口被禁用。
首先关闭串口调试工具，接下来打开烧写工具：烧写工具下载 [点击跳转](DongshanPi-One/10-SupportTools/)  选择合适的下载方式下载。

启动到Uboot需要必备的分区以及分区起始地址其中Nand Flash录地址如下：

|分区文件 |	分区起始地址 |
|-------- | ---------- |
|GCIS.bin |	0x000000 |
| IPL.bin | 	0x140000 |
| IPL_CUST.bin | 	0x200000 |
| u-boot_spinand.xz.img.bin | 	0x2C0000 |

打开Flash_Tool，根据以上的分区以及分区起始地址，按照以下方法依次烧录分区
点击Connect，建立连接状态（Connect必须确保关闭串口工具，否则会出现争抢串口资源问题）
选择Flash Type (Nand Flash)




选择需要烧录的分区对应的img，以u-boot_spinand.xz.img.bin为例
勾选 Base shift at，选定从基地址0开始
填写对应img分区的起始地址（u-boot_spinand.xz.img.bin对应的是0x2C0000）
点击Run，等待运行结束，直至提示Pass状态。

根据Flash Type将步骤3中对应的分区，按照步骤重复烧录即可，烧录完之后重启即可以正常启动到Uboot