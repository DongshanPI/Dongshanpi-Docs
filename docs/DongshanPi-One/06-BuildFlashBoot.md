# 编译烧写boot

## 编译bootloader

进入boot根目录
根据Flash选择config
Nand Flash
make infinity2m_spinand_defconfig; make menuconfig;
执行make clean;make -j8 （根据服务器配置，可以支持多线程编译）

生成image，如需要打包需要Relase到project对应目录
Nand Flash u-boot_spinand.xz.img.bin

需要release到project目录：
cp u-boot_spinand.xz.img.bin project\board\i2m\boot\spinand\uboot	需要release到project目录：

## 烧写bootloader

### 进入uboot内烧写


### 使用专门的烧写器烧写

本方式适用于空片烧录或者板子无法进入Uboot控制台使用其它方式烧录的情况。

开机进入到Uboot控制台，输入debug (如能正常进入)，此时uboot串口被禁用

关闭串口调试工具

启动到Uboot需要必备的分区以及分区起始地址

Nand/Nor区别如下：

Nand Flash

|分区文件 |	分区起始地址 |
|-------- | ---------- |
|GCIS.bin |	0x000000 |
| IPL.bin | 	0x140000 |
| IPL_CUST.bin | 	0x200000 |
| u-boot_spinand.xz.img.bin | 	0x2C0000 |


打开Flash_Tool，根据以上的分区以及分区起始地址，按照以下方法依次烧录分区
点击Connect，建立连接状态（Connect必须确保关闭串口工具，否则会出现争抢串口资源问题）
选择Flash Type (Nand Flash/Nor Flash)
选择需要烧录的分区对应的img，以u-boot_spinand.xz.img.bin为例
勾选 Base shift at，选定从基地址0开始
填写对应img分区的起始地址（u-boot_spinand.xz.img.bin对应的是0x2C0000）
点击Run，等待运行结束，直至提示Pass状态。



根据Flash Type将步骤3中对应的分区，按照步骤重复烧录即可，烧录完之后重启即可以正常启动到Uboot