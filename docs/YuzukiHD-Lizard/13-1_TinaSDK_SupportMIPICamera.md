# 使用Tina-SDK支持MIPI摄像头

使用Tina-SDK生成的镜像已经默认装载了GC2053的MIPI摄像头驱动，可以通过如下命令查看

```
root@TinaLinux:/# lsmod
Module                  Size  Used by
vin_v4l2              156643  0
gc2053_mipi             8567  0
vin_io                 21042  2 vin_v4l2,gc2053_mipi
videobuf2_v4l2          9304  1 vin_v4l2
videobuf2_dma_contig     8734  1 vin_v4l2
videobuf2_memops         948  1 videobuf2_dma_contig
videobuf2_core         21976  2 vin_v4l2,videobuf2_v4l2
xradio_wlan              598  0
xradio_core           430533  1 xradio_wlan
xradio_mac            221572  1 xradio_core
```



**断电**后，如下图所示将GC2053的MIPI摄像头连接到开发板上。注意一定需要断电后才能连接，否则会将摄像头烧坏。

![YuzukiHD-Lizard-13-1-camera.png](https://photos.100ask.net/dongshanpi-docs/YuzukiHD-Lizard/YuzukiHD-Lizard-13-1-camera.png)



## 使用camerademo测试MIPI摄像头驱动

获取camerademo的帮助，用户可以根据自己的需求选择对应参数

```
root@TinaLinux:/# camerademo -h
[CAMERA]**********************************************************
[CAMERA]*                                                        *
[CAMERA]*              this is camera test.                      *
[CAMERA]*                                                        *
[CAMERA]**********************************************************
[CAMERA]******************** camerademo help *********************
[CAMERA] This program is a test camera.
[CAMERA] It will query the sensor to support the resolution, output format and test frame rate.
[CAMERA] At the same time you can modify the data to save the path and get the number of photos.
[CAMERA] When the last parameter is debug, the output will be more detailed information
[CAMERA] There are eight ways to run:
[CAMERA]    1.camerademo --- use the default parameters.
[CAMERA]    2.camerademo debug --- use the default parameters and output debug information.
[CAMERA]    3.camerademo setting --- can choose the resolution and data format.
[CAMERA]    4.camerademo setting debug --- setting and output debug information.
[CAMERA]    5.camerademo NV21 640 480 30 bmp /tmp 5 --- param input mode,can save bmp or yuv.
[CAMERA]    6.camerademo NV21 640 480 30 bmp /tmp 5 debug --- output debug information.
[CAMERA]    7.camerademo NV21 640 480 30 bmp /tmp 5 Num --- /dev/videoNum param input mode,can save bmp or yuv.
[CAMERA]    8.camerademo NV21 640 480 30 bmp /tmp 5 Num debug --- /dev/videoNum output debug information.
[CAMERA]    8.camerademo NV21 640 480 30 bmp /tmp 5 Num 1 --- 1/2: chose memory: V4L2_MEMORY_MMAP/USERPTR
[CAMERA]**********************************************************
```

使用debug确定摄像头支持的参数配置

```
root@TinaLinux:/# camerademo debug
[CAMERA]**********************************************************
[CAMERA]*                                                        *
[CAMERA]*              this is camera test.                      *
[CAMERA]*                                                        *
[CAMERA]**********************************************************
[CAMERA]**********************************************************
[CAMERA] open /dev/video0!
[CAMERA]**********************************************************
[CAMERA_DEBUG] Querey device capabilities succeed
[CAMERA_DEBUG] cap.driver=sunxi-vin
[CAMERA_DEBUG] cap.card=sunxi-vin
[CAMERA_DEBUG] cap.bus_info=
[CAMERA_DEBUG] cap.version=0x00010000
[CAMERA_DEBUG] cap.capabilities=0x85201000
[CAMERA]**********************************************************
[CAMERA] The path to data saving is /tmp.
[CAMERA] The number of captured photos is 5.
[CAMERA] save bmp format
[CAMERA_DEBUG]******************[   63.469141] [VIN_ERR]vin is not support this pixelformat
********************************[   63.476141] [VIN_ERR]vin is not support this pixelformat
********
[CAMERA_DEBUG] enumera[   63.484878] [VIN_ERR]vin is not support this pixelformat
te image formats
[CAMERA_DEBUG][   63.493711] [VIN_ERR]vin is not support this pixelformat
 format index = 0, name = YUV422[   63.502495] [VIN_ERR]vin is not support this pixelformat
P
[CAMERA_DEBUG] format index =[   63.511287] [VIN_ERR]vin is not support this pixelformat
 1, name = NV16
[CAMERA_DEBUG] [   63.519961] [VIN_ERR]vin is not support this pixelformat
format index = 2, name = NV61
[[   63.528991] [VIN_ERR]vin is not support this pixelformat
CAMERA_DEBUG] format index = 3, [   63.537520] [VIN_ERR]vin is not support this pixelformat
name = YUV420
[CAMERA_DEBUG] fo[   63.546341] [VIN_ERR]vin is not support this pixelformat
rmat index = 4, name = YVU420
[[   63.555148] [VIN_ERR]vin is not support this pixelformat
CAMERA_DEBUG] format index = 5, [   63.564044] [VIN_ERR]vin is not support this pixelformat
name = NV12
[CAMERA_DEBUG] format index = 6, name = NV21
[CAMERA_DEBUG] format index = 7, name = BGGR8
[CAMERA_DEBUG] format index = 8, name = GBRG8
[CAMERA_DEBUG] format index = 9, name = GRBG8
[CAMERA_DEBUG] format index = 10, name = RGGB8
[CAMERA_DEBUG] format index = 11, name = BGGR10
[CAMERA_DEBUG] format index = 12, name = GBRG10
[CAMERA_DEBUG] format index = 13, name = GRBG10
[CAMERA_DEBUG] format index = 14, name = RGGB10
[CAMERA_DEBUG] format index = 15, name = BGGR12
[CAMERA_DEBUG] format index = 16, name = GBRG12
[CAMERA_DEBUG] format index = 17, name = GRBG12
[CAMERA_DEBUG] format index = 18, name = RGGB12
[CAMERA_DEBUG] format index = 19, name = YUYV
[CAMERA_DEBUG] format index = 20, name = UYVY
[CAMERA_DEBUG] format index = 21, name = VYUY
[CAMERA_DEBUG] format index = 22, name = YVYU
[CAMERA_DEBUG] format index = 23, name = YUYV
[CAMERA_DEBUG] format index = 24, name = UYVY
[CAMERA_DEBUG] format index = 25, name = VYUY
[CAMERA_DEBUG] format index = 26, name = YVYU
[CAMERA_DEBUG] format index = 27, name = UYVY
[CAMERA_DEBUG] format index = 28, name = VYUY
[CAMERA_DEBUG] format index = 29, name = YVYU
[CAMERA_DEBUG] format index = 30, name = YUYV
[CAMERA_DEBUG]*********************************************************
[CAMERA_DEBUG] The sensor supports the following formats :
[CAMERA_DEBUG] Index 0 : YUV422P.
[CAMERA_DEBUG] Index 1 : NV16.
[CAMERA_DEBUG] Index 2 : NV61.
[CAMERA_DEBUG] Index 3 : YUV420.
[CAMERA_DEBUG] Index 4 : YVU420.
[CAMERA_DEBUG] Index 5 : NV12.
[CAMERA_DEBUG] Index 6 : NV21.
[CAMERA_DEBUG] Index 7 : BGGR8.
[CAMERA_DEBUG] Index 8 : GBRG8.
[CAMERA_DEBUG] Index 9 : GRBG8.
[CAMERA_DEBUG] Index 10 : RGGB8.
[CAMERA_DEBUG] Index 11 : BGGR10.
[CAMERA_DEBUG] Index 12 : GBRG10.
[CAMERA_DEBUG] Index 13 : GRBG10.
[CAMERA_DEBUG] Index 14 : RGGB10.
[CAMERA_DEBUG] Index 15 : BGGR12.
[CAMERA_DEBUG] Index 16 : GBRG12.
[CAMERA_DEBUG] Index 17 : GRBG12.
[CAMERA_DEBUG] Index 18 : RGGB12.
[CAMERA_DEBUG] Index 19 : YUYV.
[CAMERA_DEBUG] Index 20 : UYVY.
[CAMERA_DEBUG] Index 21 : VYUY.
[CAMERA_DEBUG] Index 22 : YVYU.
[CAMERA_DEBUG] Index 23 : YUYV.
[CAMERA_DEBUG] Index 24 : UYVY.
[CAMERA_DEBUG] Index 25 : VYUY.
[CAMERA_DEBUG] Index 26 : YVYU.
[CAMERA_DEBUG] Index 27 : UYVY.
[CAMERA_DEBUG] Index 28 : VYUY.
[CAMERA_DEBUG] Index 29 : YVYU.
[CAMERA_DEBUG] Index 30 : YUYV.
[CAMERA_DEBUG]**********************************************************
[CAMERA_DEBUG] The YUV422P supports the following resolutions:
[CAMERA_DEBUG] Index 0 : 1920 * 1088
[CAMERA_DEBUG] Index 1 : 1920 * 1088
[CAMERA_DEBUG] Index 2 : 1920 * 1088
[CAMERA_DEBUG]**********************************************************
[CAMERA_DEBUG] The NV16 supports the following resolutions:
[CAMERA_DEBUG] Index 0 : 1920 * 1088
[CAMERA_DEBUG] Index 1 : 1920 * 1088
[CAMERA_DEBUG] Index 2 : 1920 * 1088
[CAMERA_DEBUG]**********************************************************
[CAMERA_DEBUG] The NV61 supports the following resolutions:
[CAMERA_DEBUG] Index 0 : 1920 * 1088
[CAMERA_DEBUG] Index 1 : 1920 * 1088
[CAMERA_DEBUG] Index 2 : 1920 * 1088
[CAMERA_DEBUG]**********************************************************
[CAMERA_DEBUG] The YUV420 supports the following resolutions:
[CAMERA_DEBUG] Index 0 : 1920 * 1088
[CAMERA_DEBUG] Index 1 : 1920 * 1088
[CAMERA_DEBUG] Index 2 : 1920 * 1088
[CAMERA_DEBUG]**********************************************************
[CAMERA_DEBUG] The YVU420 supports the following resolutions:
[CAMERA_DEBUG] Index 0 : 1920 * 1088
[CAMERA_DEBUG] Index 1 : 1920 * 1088
[CAMERA_DEBUG] Index 2 : 1920 * 1088
[CAMERA_DEBUG]**********************************************************
[CAMERA_DEBUG] The NV12 supports the following resolutions:
[CAMERA_DEBUG] Index 0 : 1920 * 1088
[CAMERA_DEBUG] Index 1 : 1920 * 1088
[CAMERA_DEBUG] Index 2 : 1920 * 1088
[CAMERA_DEBUG]**********************************************************
[CAMERA_DEBUG] The NV21 supports the following resolutions:
[CAMERA_DEBUG] Index 0 : 1920 * 1088
[CAMERA_DEBUG] Index 1 : 1920 * 1088
[CAMERA_DEBUG] Index 2 : 1920 * 1088
[CAMERA_DEBUG]**********************************************************
```

挂载TF卡分区

```
root@TinaLinux:/# df -h
Filesystem                Size      Used Available Use% Mounted on
/dev/root                16.8M     16.8M         0 100% /rom
devtmpfs                 26.0M         0     26.0M   0% /dev
tmpfs                    27.2M         0     27.2M   0% /tmp
/dev/by-name/rootfs_data
                         43.5M    136.0K     41.1M   0% /overlay
overlayfs:/overlay       43.5M    136.0K     41.1M   0% /
tmpfs                    27.2M         0     27.2M   0% /run
/dev/ubi0_6              29.9M     24.0K     28.3M   0% /mnt/UDISK
/dev/mmcblk0p1            3.7G    128.0K      3.7G   0% /mnt/extsd
root@TinaLinux:/# mount /dev/mmcblk0p1 /mnt/
```



使用GC2053所支持的参数拍摄五张照片，并存放到TF卡分区/mnt目录下

```
root@TinaLinux:/# camerademo NV21 1920 1088 30 bmp /mnt 5
[CAMERA]**********************************************************
[CAMERA]*                                                        *
[CAMERA]*              this is camera test.                      *
[CAMERA]*                                                        *
[CAMERA]**********************************************************
[CAMERA]**********************************************************
[CAMERA] open /dev/video0!
[CAMERA]**********************************************************
[CAMERA]**********************************************************
[CAMERA] The path to data saving is /mnt.
[CAMERA] The number of captured photos is 5.
[CAMERA] save bmp format
[   81.016864] [VIN_ERR]vin is not support this pixelformat
[   81.023161] [VIN_ERR]vin is not support this pixelformat
[   81.029406] [VIN_ERR]vin is not support this pixelformat
[   81.035712] [VIN_ERR]vin is not support this pixelformat
[   81.041841] [VIN_ERR]vin is not support this pixelformat
[   81.048112] [VIN_ERR]vin is not support this pixelformat
[   81.054244] [VIN_ERR]vin is not support this pixelformat
[   81.060641] [VIN_ERR]vin is not support this pixelformat
[   81.066969] [VIN_ERR]vin is not support this pixelformat
[   81.073229] [VIN_ERR]vin is not support this pixelformat
[   81.079441] [VIN_ERR]vin is not support this pixelformat
[   81.085738] [VIN_ERR]vin is not support this pixelformat
[CAMERA]**********************************************************
[CAMERA] Using format parameters NV21.
[CAMERA] camera pixelformat: NV21
[CAMERA] Resolution size : 1920 * 1088
[CAMERA] The photo save path is /mnt.
[CAMERA] The number of photos taken is 5.
begin ion_alloc_open
pid: 992, g_alloc_context = 0xb6f28f70
[CAMERA] Camera capture framerate is 20/1
[CAMERA] VIDIOC_S_FMT succeed
[CAMERA] fmt.type = 9
[CAMERA] fmt.fmt.pix_mp.width = 1920
[CAMERA] fmt.fmt.pix_mp.height = 1088
[CAMERA] fmt.fmt.pix_mp.pixelformat = NV21
[CAMERA] fmt.fmt.pix_mp.field = 1
[CAMERA] stream on succeed
[CAMERA] camera0 capture num is [0]
[CAMERA_PROMPT] the time interval from the start to the first frame is 178 ms
[CAMERA] camera0 capture num is [1]
[CAMERA] camera0 capture num is [2]
[CAMERA] camera0 capture num is [3]
[CAMERA] camera0 capture num is [4]
[CAMERA] Capture thread finish
[CAMERA] close /dev/video0
ion_alloc_close
pid: 992, release g_alloc_context = 0xb6f28f70
```

拍摄完成后，使用下面命令卸载分区，取下TF卡，使用读卡器即可在电脑端查看使用MIPI摄像头拍摄的照片

```
root@TinaLinux:/# umount /mnt/
```



