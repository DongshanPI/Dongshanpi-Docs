# DongshanPI-D1s开发板介绍
* 此开发板的任何问题都可以在我们的论坛交流讨论 https://forums.100ask.net/c/aw/d1s/
## 硬件简述
### 1.1 主芯片介绍

D1s 是全志针对智能解码市场推出的高性价比 AIoT 芯片。它使用 64bit RISC-V 架构的阿里平头哥 C906 处理器，内置了64M DDR2，支持 Linux 系统，同时集成了大量自研的音视频编解码相关 IP，可以支持 H.265,、H.264、MPEG-1/2/4、JPEG 等全格式视频解码，支持ADC、DAC、I2S、PCM、DMIC、OWA 等多种音频接口，可以广泛应用于智能家居面板、智能商显、工业控制、车载等产品。
#### 芯片框图

![D1s芯片框图](https://d1s.docs.aw-ol.com/assets/img/index/D1sSoC.png){ width="600" }


####  参数规格

* CPU

```
Alibaba T-Head C906 RISC CPU, 720MHz
32 KB I-cache + 32 KB D-cache
```
*  Memory

```
SIP 64 MB DDR2
SD3.0/eMMC 5.0, SPI Nor/NAND Flash
```

* Video Engine

```
Video decoding
    H.265 up to 1080p@60fps 
    H.264 up to 1080p@60fps 
    MPEG-1/2/4, JPEG, VC1 up to 1080p@60fps

Video encoding
    JPEG/MJPEG up to 1080p@60fps
    Supports input picture scaler up/down
```

*  Display Engine

```
CVBS OUT interface, supporting NTSC and PAL format
RGB LCD output interface up to 1920 x 1080@60fps
Dual link LVDS interface up to 1920 x 1080@60fps
4-lane MIPI DSI interface up to 1920 x 1200@60fps
```

*  Video IN

```
8-bit parallel CSI interface
```

*  Audio

```
2 DACs and 3 ADCs
Analog audio interfaces: MICIN1P/N, MICIN2P/N, MICIN3P/N, FMINL/R, LINEINL/R, LIEOUTLP/N, LINEOUTRP/N, HPOUTL/R
Digital audio interfaces: I2S/PCM, DMIC, OWA IN/OUT
```

*  Connectivity

```
USB2.0 DRD, USB2.0 Host
SDIO 3.0, SPI x 2, UART x 6, TWI x 4
PWM (8-ch), GPADC (2-ch), LRADC (1-ch), TPADC (4-ch), IR TX&RX 
10/100/1000M EMAC with RMII and RGMII interfaces
```

* Package

```
eLQFP128, 14 mm x 14 mm
```

* Chip process

```
22nm
```

### 1.2 外围器件介绍

![DongshanPI-D1s-V2TopFuction](https://photos.100ask.net/dongshanpi-docs/d1s/DongshanPI-D1s-V2TopFuction.png){ width="600" }

* 开发板原理图 [DongshanPI-D1s_SCH-V2.pdf](/DongshanPI-D1s/DongshanPI-D1s_SCH-V2.pdf)

### 1.3 配套模块介绍
* 7寸 1024x600 RGB显示屏(带电容触摸)
* 4寸 400x800双LINE MIPI DSI显示屏(带电容触摸)
* 2.1寸圆屏 （带电容触摸）
* 4寸方屏 （带电容触摸）