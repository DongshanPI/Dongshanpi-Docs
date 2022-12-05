# 使用Tina-SDK编译打包Linux Kernel
### 编译各个部分

* 修改内核配置
``` shell
book@100ask:~/tina-v853$ croot
book@100ask:~/tina-v853$ make kernel_menuconfig
```

![YuzukiHD-Lizard-09_menuconfig](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/YuzukiHD-Lizard/YuzukiHD-Lizard-09_menuconfig.png)

可以在上图的界面内修改内核配置。

* 编译打包后会在out/v851s-perf1里面生成最终的镜像
``` shell
book@100ask:~/tina-v853$ make
book@100ask:~/tina-v853$ pack
```
