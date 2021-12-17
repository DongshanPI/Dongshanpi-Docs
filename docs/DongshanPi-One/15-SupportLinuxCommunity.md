# 社区版本Linux开发
``` shell
sudo ./SNANDer -p mstarddc -c /dev/i2c-4:49 -e
sudo ./SNANDer -p mstarddc -c /dev/i2c-4:49 -w GCIS.bin
sudo ./SNANDer -p mstarddc -c /dev/i2c-4:49 -a 0x140000 -l 0x54C0 -w IPL.bin
sudo ./SNANDer -p mstarddc -c /dev/i2c-4:49 -a 0x200000 -l 0xBF58 -w ~/idosom2d01-ipl
```