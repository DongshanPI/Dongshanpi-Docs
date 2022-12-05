# 使用Tina-SDK编译构建Bootloader

* 柚木PI-蜥蜴开发板，Bootloader由4部分组成， 第一部分是 boot0 阶段，用于初始化CPU DDR UART 时钟等一些必要外设和引脚分配，之后进入第二部分，第二部分是 optee  uboot  board.dtb 这三部分组成，为一个 boot_package.fex 文件。
* 所以Bootloader的整体的启动流程是，boot0-->optee-->u-boot-->board.dtb。
* Bootloader在内核运行之前运行，可以初始化硬件设备、建立内存空间映 射图，从而将系统的软硬件环境带到一个合适状态，为最终调用 linux 内核准备好正确的环境。

## 单独编译打包第一部分


## 单独编译打包第二部分
## 单独编译打包第三部分

### 单独编译 uboot

* 单独编译 uboot阶段
``` shell
book@100ask:~/tina-v853$ source build/envsetup.sh
book@100ask:~/tina-v853$ lunch
2 #输入数字选择对应开发板
book@100ask:~/tina-v853$muboot
```

编译uboot，编译完成后自动更新uboot binary到TinaSDK/target/allwinner/$(BOARD)-common/bin/

### 单独编译 board.dtb

```shell
mboot
```



