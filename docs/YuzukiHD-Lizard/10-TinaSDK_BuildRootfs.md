# 使用buildroot-SDK编译构建rootfs

Tina默认情况是编译打包完成在tina-v853/out/v851s-perf1会生成对应的rootfs.img

## 单独编译配置procd-init

tina默认是procd-init

``` shell
book@100ask:~/tina-v853$ make
book@100ask:~/tina-v853$ pack
```



### 清理无效缓存 重新打包

``` shell
make clean
```

