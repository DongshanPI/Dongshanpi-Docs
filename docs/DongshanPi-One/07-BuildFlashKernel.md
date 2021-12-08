# 编译烧写Kernel

## 编译Kernel
进入kernel根目录
选择config Nand Flash

> make infinity2m_spinand_ssc011a_s01a_minigui_defconfig;

执行make clean;make -j8（根据服务器配置，可以支持多线程编译）生成image，如需要打包需要Relase到project对应目录。

Nand Flash	 
> arch/arm/boot/uImage.xz	

需要release到project目录：

project\release\nvr\i2m\011A\glibc\8.2.1\bin\kernel\spinand	

### 检查编译环境

### 指定配置文件

### 编译系统

## 烧写Kernel
### uboot内烧写


### 文件系统内烧写
