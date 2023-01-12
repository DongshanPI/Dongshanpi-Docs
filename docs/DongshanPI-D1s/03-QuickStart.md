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

**系统默认会自己登录 没有用户名 没有密码。**
**系统默认会自己登录 没有用户名 没有密码。**
**系统默认会自己登录 没有用户名 没有密码。**