# 快速开始使用
如果您感觉跟着文档不知道怎么操作，可以直接查看我们提前录制好的手把手教学视频。

<iframe src="//player.bilibili.com/player.html?aid=293906511&bvid=BV18F411a7jM&cid=434612050&page=1"scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true">  </iframe>

## 上电启动系统
### 1. 连接串口线
将配套的TypeC线一段连接至开发板的串口/供电接口，另一端连接至电脑USB接口，连接成功后板载的红色电源灯会亮起。
默认情况下系统会自动安装串口设备驱动，如果没有自动安装，可以使用驱动精灵来自动安装。

连接截图

* 对于Windows系统
此时Windows设备管理器 在 端口(COM和LPT) 处会多出一个串口设备，一般是以 `USB-Enhanced-SERIAL CH9102`开头，您需要留意一下后面的具体COM编号，用于后续连接使用。

设备管理器截图

* 对于Linux系统
可以查看是否多出一个/dev/ttyUSB 设备,一般情况为/dev/ttyUSB0 。

设备节点截图

### 2. 打开串口控制台
#### 获取串口工具
使用Putty或者MobaXterm等串口工具来开发板设备。
其中putty工具可以访问页面  https://www.chiark.greenend.org.uk/~sgtatham/putty/  来获取。
MobaXterm可以通过访问页面 https://mobaxterm.mobatek.net/ 获取 (推荐使用)。

#### 使用putty登录串口
登录截图

#### 使用putty登录串口
登录截图

### 3. 进入系统shell
使用串口工具成功打开串口后，可以直接按下 Enter 键 进入shell，当然您也可以按下板子上的 `Reset`复位键，来查看完整的系统信息。

## 扩展硬件

### 连接模块

#### 连接7寸屏模块
* 视频教程
 
* 模块介绍 

### 连接底板
#### 直播课配套底板
* 视频教程
  
* 底板介绍


## 常见开发板异常问题
### 无串口输出信息
* 检查是否连接成功 电脑是否安装串口驱动
* 检查串口波特率是否设置正确
* 检查串口是否被其它工具拦截
  
### 无法烧写系统
* 检查开发板是否进入uboot模式
* 是否可以加载SD卡内镜像到系统内
* 检查是否可以烧写
  
###  屏幕无任何显示
* 检查连接是否稳定。
* 查看屏幕背光是否亮起。
* 检查是否有操作屏幕进行显示。


## 简述开发流程

### 调试环境概述 