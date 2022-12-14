## 配套资源
### V851s芯片手册
* V851s芯片Brief: [https://github.com/YuzukiHD/Yuzukilizard/blob/master/Hardware/Datasheets/V851S_Brief_EN_V1.0.pdf](https://github.com/YuzukiHD/Yuzukilizard/blob/master/Hardware/Datasheets/V851S_Brief_EN_V1.0.pdf)

* V851s数据手册: [https://github.com/YuzukiHD/Yuzukilizard/blob/master/Hardware/Datasheets/V851S%26V851SE_Datasheet_V1.0.pdf](https://github.com/YuzukiHD/Yuzukilizard/blob/master/Hardware/Datasheets/V851S%26V851SE_Datasheet_V1.0.pdf)

### 开发板硬件资源

* 原理图资源：[https://github.com/YuzukiHD/Yuzukilizard/blob/master/Hardware/Schematic/SCH_Schematic1_2022-11-23.pdf](https://github.com/YuzukiHD/Yuzukilizard/blob/master/Hardware/Schematic/SCH_Schematic1_2022-11-23.pdf)

* 开发板尺寸图：

### 开发板配套SDK源码
柚木PI-蜥蜴开发板是基于全志V851s设计，全志原厂提供的源码是内部在openwrt基础上自定义的一套 Linux系统，我们要针对于V851s开发 就需要先获取对应的sdk 然后在Ubuntu内进行解压缩 来编译。
tina-sdk压缩包，压缩包分为国内国外两个存放位置，如下所示，大小大概11G，下载完成后，拷贝到提前配置好Host开发环境的ubuntu系统内，然后参考 下载的目录内的README.txt文档 进行解压缩。

* 国外- GoogleDriver： https://drive.google.com/drive/folders/1_HAZRddR69hRMZAVrxFrPZXFFQiV3vE0?usp=share_link
* 国内- BaiduYun:  https://pan.baidu.com/s/115gVK-8Pt-vJi8jn2AWMYw?pwd=7n4q 提取码：7n4q 



## Tina-SDK开发文档
为了让大家可以更好的使用此开发板，我们专门提供了一个 针对于 Tina-sdk的在线开发文档，可以很方便用来进行参考学习。

### SDK模块开发指南
- [x] Linux_CCU_开发指南
- [x] Linux_CE开发指南
- [x] Linux_DMAC_开发指南
- [x] Linux_Display_开发指南
- [x] Linux_EMAC_开发指南
- [x] Linux_G2D_开发指南
- [x] Linux_GPADC_开发指南
- [x] Linux_UART_开发指南
    * [x] 概述与介绍: https://tina.100ask.net/SdkModule/Linux_UART_DevelopmentGuide-01/
    * [x] 模块配置介绍: https://tina.100ask.net/SdkModule/Linux_UART_DevelopmentGuide-02/
    * [x] 接口描述: https://tina.100ask.net/SdkModule/Linux_UART_DevelopmentGuide-03/ 
    * [x] 模块使用范例: https://tina.100ask.net/SdkModule/Linux_UART_DevelopmentGuide-04/
    * [x] 常见问题: https://tina.100ask.net/SdkModule/Linux_UART_DevelopmentGuide-05/
- [x] Linux_GPIO_开发指南
    * [x] 概述与介绍: https://tina.100ask.net/SdkModule/Linux_GPIO_DevelopmentGuide-01/
    * [x] 模块配置介绍: https://tina.100ask.net/SdkModule/Linux_GPIO_DevelopmentGuide-02/
    * [x] 接口描述: https://tina.100ask.net/SdkModule/Linux_GPIO_DevelopmentGuide-03/
    * [x] 模块使用范例: https://tina.100ask.net/SdkModule/Linux_GPIO_DevelopmentGuide-04/
    * [x] 常见问题: https://tina.100ask.net/SdkModule/Linux_GPIO_DevelopmentGuide-05/
- [x] Linux_ISP_开发指南
- [x] Linux_LCD_开发指南
    * [x] 概述与介绍: https://tina.100ask.net/SdkModule/Linux_LCD_DevelopmentGuide-01/
    * [x] IC规格: https://tina.100ask.net/SdkModule/Linux_LCD_DevelopmentGuide-02/
    * [x] 模块介绍: https://tina.100ask.net/SdkModule/Linux_LCD_DevelopmentGuide-03/    
    * [x] 硬件参数说明: https://tina.100ask.net/SdkModule/Linux_LCD_DevelopmentGuide-04/
- [x] Linux_MIPI_CSI_开发指南
- [x] Linux_MMC_开发指南
- [x] Linux_Nor_开发指南
- [x] Linux_PMIC_开发指南
- [x] Linux_RTC_开发指南
- [x] Linux_SID_开发指南
- [x] Linux_SPI-NAND_开发指南
- [x] Linux_SPINAND_UBI离线烧录_开发指南
- [x] Linux_SPI_开发指南
- [x] Linux_Standby_开发指南
- [x] Linux_TWI_开发指南
- [x] Linux_U-Boot_开发指南
- [x] Linux_USB_开发指南
- [x] Linux_安全_开发指南

### 基础组件开发指南
- [x] MPPSample使用:
    * [x] 概述与配置: https://tina.100ask.net/BasicComponent/MPP_Sample_Instructionsforuse01/
    * [x] 编译: https://tina.100ask.net/BasicComponent/MPP_Sample_Instructionsforuse02/
    * [x] 运行测试: https://tina.100ask.net/BasicComponent/MPP_Sample_Instructionsforuse03/
    * [x] 类别: https://tina.100ask.net/BasicComponent/MPP_Sample_Instructionsforuse04/
    * [x] 状态: https://tina.100ask.net/BasicComponent/MPP_Sample_Instructionsforuse05/
    * [x] 正文: https://tina.100ask.net/BasicComponent/MPP_Sample_Instructionsforuse06/                        
    * [x] 常见问题: https://tina.100ask.net/BasicComponent/MPP_Sample_Instructionsforuse07/                       
- [x] LCD调试:
    * [x] 简述: https://tina.100ask.net/BasicComponent/Linux_LCD_DebuggingGuide01/
    * [x] 模块介绍: https://tina.100ask.net/BasicComponent/Linux_LCD_DebuggingGuide02/
    * [x] 硬件参数说明: https://tina.100ask.net/BasicComponent/Linux_LCD_DebuggingGuide03/       
    * [x] 调试方法: https://tina.100ask.net/BasicComponent/Linux_LCD_DebuggingGuide04/
    * [x] 常见问题: https://tina.100ask.net/BasicComponent/Linux_LCD_DebuggingGuide05/
- [x] Display开发:
    * [x] 简述: https://tina.100ask.net/BasicComponent/Linux_Display_DevelopmentGuide01/
    * [x] 模块接口概述: https://tina.100ask.net/BasicComponent/Linux_Display_DevelopmentGuide02/
    * [x] 显示接口参数说明: https://tina.100ask.net/BasicComponent/Linux_Display_DevelopmentGuide03/
    * [x] 输出设备介绍: https://tina.100ask.net/BasicComponent/Linux_Display_DevelopmentGuide04/
    * [x] IOCTL接口描述: https://tina.100ask.net/BasicComponent/Linux_Display_DevelopmentGuide05/            
    * [x] sysfs接口描述: https://tina.100ask.net/BasicComponent/Linux_Display_DevelopmentGuide06/                  
    * [x] Data Structure: https://tina.100ask.net/BasicComponent/Linux_Display_DevelopmentGuide07/                        
    * [x] 调试: https://tina.100ask.net/BasicComponent/Linux_Display_DevelopmentGuide08/ 
- [x] LinuxPWM开发:
    * [x] 概述: https://tina.100ask.net/BasicComponent/Linux_PWM_DevelopmentGuide01/
    * [x] 术语及概念: https://tina.100ask.net/BasicComponent/Linux_PWM_DevelopmentGuide02/   
    * [x] 模块描述: https://tina.100ask.net/BasicComponent/Linux_PWM_DevelopmentGuide03/                                     
- [x] Camera开发:
    * [x] 简述: https://tina.100ask.net/BasicComponent/Linux_Camera_DevelopmentGuide01/
    * [x] 模块介绍: https://tina.100ask.net/BasicComponent/Linux_Camera_DevelopmentGuide02/      
    * [x] 模块开发: https://tina.100ask.net/BasicComponent/Linux_Camera_DevelopmentGuide03/
    * [x] 模块配置: https://tina.100ask.net/BasicComponent/Linux_Camera_DevelopmentGuide04/
    * [x] 调试常见问题: https://tina.100ask.net/BasicComponent/Linux_Camera_DevelopmentGuide05/
    * [x] 功能测试: https://tina.100ask.net/BasicComponent/Linux_Camera_DevelopmentGuide06/ 
- [x] Key快速配置:
    * [x] 模块简述:  https://tina.100ask.net/BasicComponent/Linux_Key_QuickConfiguration_UserGuide01/
    * [x] GPIOKey:  https://tina.100ask.net/BasicComponent/Linux_Key_QuickConfiguration_UserGuide02/      
    * [x] ADCKey:  https://tina.100ask.net/BasicComponent/Linux_Key_QuickConfiguration_UserGuide03/      
    * [x] AXPKeY:  https://tina.100ask.net/BasicComponent/Linux_Key_QuickConfiguration_UserGuide04/    
- [x] E907开发:
    * [x] 简述: https://tina.100ask.net/BasicComponent/Linux_E907_DevelopmentGuide01/
    * [x] 环境搭建: https://tina.100ask.net/BasicComponent/Linux_E907_DevelopmentGuide02/      
    * [x] 开发使用: https://tina.100ask.net/BasicComponent/Linux_E907_DevelopmentGuide03/
    * [x] 常见问题: https://tina.100ask.net/BasicComponent/Linux_E907_DevelopmentGuide04/   
- [x] 各平台多媒体格式: https://tina.100ask.net/BasicComponent/Linux_Multimediaformatsforeachplatform01/          
- [x] 图形系统开发: https://tina.100ask.net/BasicComponent/Linux_GraphicsSystem_DevelopmentGuide01/
- [x] 多媒体MPP开发: https://tina.100ask.net/BasicComponent/Linux_MultimediaMPP_DevelopmentGuide01/      
- [x] NPU开发简介: https://tina.100ask.net/BasicComponent/IntroductiontoLinux_NPUDevelopment01/
- [x] NPU混合量化部署: https://tina.100ask.net/BasicComponent/Linux_NPUmixedquantitativedeployment01/
- [x] NPU部署工具安装指导: https://tina.100ask.net/BasicComponent/Linux_NPUdeploymenttoolinstallationguide01/    
- [x] NPU_AI模型混合量化指导: https://tina.100ask.net/BasicComponent/Linux_NPU_AIModelHybridQuantizationGuide01/
- [x] NPU_Lenet模型之从训练到端侧部署: https://tina.100ask.net/BasicComponent/Linux_NPU_Lenetmodelfromtrainingtoendsidedeployment01/
- [x] NPU_VIPLite_API说明: https://tina.100ask.net/BasicComponent/DescriptionofLinux_NPU_VIPLite_API01/
- [x] NPU_Yolact模型部署: https://tina.100ask.net/BasicComponent/Linux_NPU_Yolactmodeldeployment01/
- [x] NPU常见网络性能分析数据: https://tina.100ask.net/BasicComponent/Linux_NPUcommonnetworkperformanceanalysisdata01/                                       
- [x] USB开发: https://tina.100ask.net/BasicComponent/Linux_USB_DevelopmentGuide01/      
- [x] 网络性能参考: https://tina.100ask.net/BasicComponent/Linux_NetworkPerformance_ReferenceGuide01/
- [x] 蓝牙开发: https://tina.100ask.net/BasicComponent/Linux_Bluetooth_DevelopmentGuide01/
- [x] 蓝牙模组移植: https://tina.100ask.net/BasicComponent/Linux_Bluetooth_ModuleMigrationGuide01/
- [x] WiFi开发: https://tina.100ask.net/BasicComponent/Linux_WiFi_DevelopmentGuide01/
- [x] WiFiRF测试: https://tina.100ask.net/BasicComponent/Linux_WiFi_RFTest_UsageGuide01/    
- [x] 无线配网: https://tina.100ask.net/BasicComponent/Linux_DistributionNetwork_DevelopmentGuide01/
- [x] 单板资源配置: https://tina.100ask.net/BasicComponent/Linux_Configuration_DevelopmentGuide01/
- [x] 音频开发: https://tina.100ask.net/BasicComponent/Linux_Audio_DevelopmentGuide01/
- [x] PMU_开发指南: https://tina.100ask.net/BasicComponent/Linux_PMU_DevelopmentGuide01/  
- [x] OTA_开发指南: https://tina.100ask.net/BasicComponent/Linux_OTA_DevelopmentGuide01/
- [x] syslog_使用指南: https://tina.100ask.net/BasicComponent/Linux_syslog_UsageGuide01/
- [x] 内存优化_开发指南: https://tina.100ask.net/BasicComponent/Linux_MemoryOptimization_DevelopmentGuide01/
- [x] 功耗管理_开发指南: https://tina.100ask.net/BasicComponent/Linux_PowerManagement_DevelopmentGuide01/
- [x] Linux启动优化开发指南: https://tina.100ask.net/BasicComponent/Linux_BootOptimization_DevelopmentGuide01/
- [x] 存储_开发指南: https://tina.100ask.net/BasicComponent/Linux_Storage_DevelopmentGuide01/
- [x] 存储性能_参考指南: https://tina.100ask.net/BasicComponent/Linux_StoragePerformance_ReferenceGuide01/
- [x] 安全_开发指南: https://tina.100ask.net/BasicComponent/Linux_Security_DevelopmentGuide01/
- [x] 打包流程_说明指南: https://tina.100ask.net/BasicComponent/Linux_packagingprocess_instructionguide01/
- [x] 系统裁剪_开发指南: https://tina.100ask.net/BasicComponent/Linux_SystemTailoring_DevelopmentGuide01/
- [x] 系统调试_使用指南: https://tina.100ask.net/BasicComponent/Linux_SystemDebugging_UsageGuide01/
- [x] 系统软件_开发指南: https://tina.100ask.net/BasicComponent/Linux_SystemSoftware_DevelopmentGuide01/