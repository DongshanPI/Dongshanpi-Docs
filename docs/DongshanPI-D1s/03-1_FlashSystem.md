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
![DongshanPI-D1s-V2TopFuction](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/d1s/DongshanPI-D1s-V2TopFuction.png)
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
![Windows_FlashDevice_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/Windows_FlashDevice_001.png)

接下来鼠标右键点击这个未知设备，在弹出的对话框里， 点击浏览我计算机以查找驱动程序软件。
![Windows_FlashDevice_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/Windows_FlashDevice_002.png)

之后在弹出新的对话框里，点击浏览找到我们之前下载好的 usb烧录驱动文件夹内，找到 `UsbDriver/` 这个目录，并进入，之后点击确定即可。
![Windows_FlashDevice_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/Windows_FlashDevice_007.png)

注意进入到  `UsbDriver/`  文件夹，然后点击确定，如下图所示。

![Windows_FlashDevice_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/Windows_FlashDevice_003.png)

此时，我们继续点击 **下一页** 按钮，这时系统就会提示安装一个驱动程序。 

在弹出的对话框里，我们点击 始终安装此驱动程序软件 等待安装完成。

![Windows_FlashDevice_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/Windows_FlashDevice_004.png)

安装完成后，会提示，Windows已成功更新你的驱动程序。

![Windows_FlashDevice_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/Windows_FlashDevice_005.png)


最后我们可以看到，设备管理器 里面的未知设备 变成了一个 `USB Device(VID_1f3a_efe8)`的设备，这时就表明设备驱动已经安装成功。

![Windows_FlashDevice_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/Windows_FlashDevice_006.png)


### 运行软件烧写
将下载下来的全志线刷工具 **AllwinnertechPhoeniSuit** 解压缩，同时将**SPI Nor系统镜像**下载下来也进行解压缩。

解压后，得到一个 **tina_d1s-nezha_nor_uart0_nor.img** 镜像，是用于烧录到SPI NAND镜像得。另一个是**AllwinnertechPhoeniSuit**文件夹。

首先我们进入到 **AllwinnertechPhoeniSuit\AllwinnertechPhoeniSuitRelease20201225** 目录下 找到 **PhoenixSuit.exe** 双击运行。

打开软件后 软件主界面如下图所示

![PhoenixSuit_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/PhoenixSuit_001.png)


​	接下来 我们需要切换到 **一键刷机**窗口，如下图所示，点击红框标号1，在弹出的新窗口内，我们点击 红框2 **浏览** 找到我们刚才解压过的 SPI Nor 最小系统镜像  **tina_d1s-nezha_nor_uart0_nor.img** ，选中镜像后，点击红框3 **全盘擦除升级** ，最后点击红框4  **立即升级**。

​	点击完成后，不需要理会 弹出的信息，这时 我们拿起已经连接好的开发板，先按住 **FEL** 烧写模式按键，之后按一下 **RESET** 系统复位键，就可以自动进入烧写模式并开始烧写。

![PhoenixSuit_002](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/PhoenixSuit_002.png)


​	烧写时会提示烧写进度条，烧写完成后 开发板会自己重启。

![PhoenixSuit_003](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/PhoenixSuit_003.png)


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
* 软件：TinaTF卡最小系统镜像：[tina_d1s-nezha_sd_uart0]https://gitlab.com/dongshanpi/tools/-/raw/main/tina_d1s-nezha_sd_uart0.zip）


### 运行烧写软件烧写

首先需要下载  **win32diskimage  SDcard专用格式化** 这两个烧写TF卡的工具，然后获取到TF卡系统镜像文件**tina_d1s-nezha_sd_uart0.zip**，获取到以后，先安装 **SDcard专用格式化 SDCardFormatter5**  这个工具，同时可以解压 一下TF卡系统的镜像文件 **tina_d1s-nezha_sd_uart0.zip**，可以得到一个  **tina_d1s-nezha_sd_uart0.img**文件，这个文件就是我们要烧写的镜像。  同时解压缩 **Tina系统TF卡烧录工具 PhoenixCard-V2.8**，解压完成后，进入到烧写工具目录内，双击运行 `PhoenixCard.exe`烧录工具。

![DongshanPI-D1s-V2TopFuction](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/d1s/DongshanPI-D1s-V2TopFuction.png)


步骤一： 将TF卡插进读卡器内，同时将读卡器插到电脑USB接口，使用SD CatFormat格式化TF卡，注意备份卡内数据。参考下图所示，点击刷新找到TF卡，然后点击 Format 在弹出的 对话框 点击 **是(Yes)**等待格式完成即可。
![SDCardFormat_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/SDCardFormat_001.png)


步骤二：格式化完成后，使用**PhoenixCard.exe**工具来烧录镜像，参考下图步骤，找到自己的TF卡盘符，点击 `左上角红框1` 固件，选择已经解压过的 `tina_d1s-nezha_sd_uart0.img` 镜像，然后点击 `红框2 启动卡`，最后点击`红框3 烧录` 等待烧录完成即可。

![PhoenixCard_Config](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/d1s/PhoenixCard_Config.png)

烧录完成以后，就可以弹出TF卡，并将其插到开发板正面 `黑色字体序号 11.TF卡卡槽`位置处，此时可以使用 杜邦线 连接 `PE2 PE3 GND`使用串口进行登录，也可以使用 adb shell 直接连接 ADB进行登录访问。

**注意：D1s因TF卡和CKlink引脚存在复用关系，需将拨码开关 `SW1` 拨至数字方向，才可以支持TF卡启动**
### 启动系统





**系统默认会自己登录 没有用户名 没有密码。**
**系统默认会自己登录 没有用户名 没有密码。**
**系统默认会自己登录 没有用户名 没有密码。**

