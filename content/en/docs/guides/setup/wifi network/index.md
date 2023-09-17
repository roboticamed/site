---
title: "Change Wifi-Network"
linktitle: "Change wifi-network"
date: 2023-08-28
weight: 6
collapsible: false
---

 ## Configuration to change wifi network on Raspberry by ssh

Configure the Wi-Fi network of our Raspberry Pi by shh.

To edit it, type:
```bash
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```


Add the following lines at the end of the file:
```bash
network={
    ssid="network_name"
    psk="password"
    key_mgmt=WPA-PSK
}
```


You can check that it has been saved correctly by running:
```bash
sudo cat /etc/wpa_supplicant/wpa_supplicant.conf
```


And we restart the adapter:
```bash
sudo ifconfig wlan0 down; sudo ifconfig wlan0 up
```


To verify that everything is correct, you can test with:
```bash
ifconfig wlan0
```


Something similar should come out:
```bash
pi@juan:~ $ ifconfig wlan0
wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.68.64  netmask 255.255.252.0  broadcast 192.168.71.255
        inet6 fe80::3228:880:3be2:5d4a  prefixlen 64  scopeid 0x20<link>
        ether dc:a6:32:fb:94:78  txqueuelen 1000  (Ethernet)
        RX packets 4685  bytes 3536966 (3.3 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 2391  bytes 988966 (965.7 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```


Finally reboot the system
```bash
sudo reboot
```