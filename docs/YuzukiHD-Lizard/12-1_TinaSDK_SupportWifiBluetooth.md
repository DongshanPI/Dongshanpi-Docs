# 使用Tina-SDK支持启动WiFi

## Wifi配网

1.启动WiFi

```
root@TinaLinux:~# ifconfig wlan0 up
[ 1311.470054] ieee80211_do_open: vif_type=2, p2p=0, ch=3, addr=08:f9:56:88:5c:46
[ 1311.478327] [STA] !!!xradio_vif_setup: id=0, type=2, p2p=0, addr=08:f9:56:88:5c:46
[ 1311.491819] [AP_WRN] BSS_CHANGED_ASSOC but driver is unjoined.
[ 1311.510396] IPv6: ADDRCONF(NETDEV_UP): wlan0: link is not ready
root@TinaLinux:~# ifconfig -a
lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

p2p0      Link encap:Ethernet  HWaddr 08:F9:56:88:5C:47
          BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

sit0      Link encap:IPv6-in-IPv4
          NOARP  MTU:1480  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

wlan0     Link encap:Ethernet  HWaddr 08:F9:56:88:5C:46
          UP BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)
```

2.开发板如下图所示接入天线

![YuzukiHD-Lizard-12-1-wifi](https://photos.100ask.net/dongshanpi-docs/YuzukiHD-Lizard/YuzukiHD-Lizard-12-1-wifi.png)

3.扫描WiFi

```
root@TinaLinux:/# iw wlan0 scan | grep "SSID"
        SSID: Programmers
```

4.填写WiFi密码

```
root@TinaLinux:/# vim /etc/wpa_supplicant.conf
```

```shell
ctrl_interface=/var/log/wpa_supplicant
update_config=1

network={
    ssid="WiFi名"
    psk="密码"
}

示例：
ctrl_interface=/var/log/wpa_supplicant
update_config=1

network={
    ssid="test"
    psk="12345678"
}
```

5.连接网络

连接前需要创建socket通信的目录

```
root@TinaLinux:/# mkdir -p /var/log/wpa_supplicant
root@TinaLinux:/# wpa_supplicant -B -c /etc/wpa_supplicant.conf -i wlan0
Successfully initialized wpa_supplicant
root@TinaLinux:/# udhcpc -i wlan0
udhcpc: started, v1.27.2
udhcpc: sending discover
udhcpc: sending select for 192.168.137.174
udhcpc: lease of 192.168.137.174 obtained, lease time 604800
udhcpc: ifconfig wlan0 192.168.137.174 netmask 255.255.255.0 broadcast +
udhcpc: setting default routers: 192.168.137.1
root@TinaLinux:/# ping www.100ask.net
PING www.100ask.net (118.25.119.100): 56 data bytes
64 bytes from 118.25.119.100: seq=0 ttl=50 time=50.877 ms
64 bytes from 118.25.119.100: seq=1 ttl=50 time=90.278 ms
64 bytes from 118.25.119.100: seq=2 ttl=50 time=72.428 ms
```



