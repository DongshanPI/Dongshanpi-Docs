# 获取配套sdk源码
> 源码介绍

## 配置开发环境
> 我们的开发环境基于ubuntu-18.04系统 为了避免爬坑浪费您的时间 请尽量和我们保持一致。

### 获取源码

``` shell
book@100ask:~$ mkdir DongshanPiOne-TAKOYAKI  && cd  DongshanPiOne-TAKOYAKI
book@100ask:~/DongshanPiOne-TAKOYAKI$ repo init -u  https://gitee.com/weidongshan/manifests.git -b linux-sdk -m  SSD202D/dongshanpi-one_takoyaki_dlc00v030.xml
book@100ask:~/DongshanPiOne-TAKOYAKI$ repo sync -j4
```

### 配置编译环境
* 设置交叉编译工具链

## 简述开发流程  