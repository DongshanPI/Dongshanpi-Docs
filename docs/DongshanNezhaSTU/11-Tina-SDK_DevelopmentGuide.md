# 使用buildroot-SDK编译构建系统

### 配置开发环境



### 获取源码



### 指定配置文件



### 编译构建



### 烧写启动



译完整系统或者各个部分



* 我们编译使用的是ubuntu 18.04 系统，在进行如下编译之前需要先配置基本编译环境，参考下述命令来安装必须的软件包。

```shell
book@virtual-machine:~/Neza-D1/buildroot-2021$ sudo apt-get install -y which sed make binutils build-essential  gcc g++ bash patch gzip bzip2 perl  tar cpio unzip rsync file  bc wget python ncurses5  bazaar cvs git mercurial rsync scp subversion android-tools-mkbootimg
```

* 使用git命令clone源码

```shell
book@virtual-machine:~$ mkdir -p  ~/Neza-D1/ &&  cd ~/Neza-D1/
book@virtual-machine:~/Neza-D1$ git clone https://gitee.com/weidongshan/neza-d1-buildroot.git buildroot-2021
```

### 构建完整系统镜像




### 单独编译各个部分

* 单独编译 opensbi阶段
``` shell
book@virtual-machine:~/Neza-D1/buildroot-2021$  make opensbi-rebuild V=1
```

* 单独编译 uboot阶段
``` shell
book@virtual-machine:~/Neza-D1/buildroot-2021$  make uboot-rebuild V=1
```

* 单独编译 kernel阶段
``` shell
book@virtual-machine:~/Neza-D1/buildroot-2021$  make linux-rebuild V=1
```

* 单独编译文件系统
  * 指定完成工具链 系统配置 需要安装的包 以及所需的格式 执行如下命令，最后生成的镜像在 output/image目录下。
``` shell
book@virtual-machine:~/Neza-D1/buildroot-2021$ make  all //完整编译系统
```

