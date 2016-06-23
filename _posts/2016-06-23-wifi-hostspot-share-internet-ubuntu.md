---
layout: post
title: Setup Wifi Hotspot and Share Internet - Ubuntu
tags: [ubuntu,hotspot]
---
Here I will be telling you a step by step procedure for setting up a hotspot in ubuntu and sharing your Internet through it (with less headache).

First, you need to install two packages

```bash
sudo apt-get install hostapd bridge-utils
```
Now edit **/etc/hostapd/hostapd.conf** with (If not present create it)

```
interface=wlan0
bridge=br0
ssid=YOUR_SSID
hw_mode=g
channel=6
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=YOUR_PASSWORD
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
macaddr_acl=0
```
Dont forget to replace **YOUR_SSID** and **YOUR_PASSWORD**.

Make it live by Editing **/etc/default/hostapd**, set the line to be:

```
DAEMON_CONF="/etc/hostapd/hostapd.conf"
```

Now Bridge the network connections. Open **/etc/network/interfaces**. Assuming that **eth0** is the internet source of your system. Change the configuration something like this.(Make sure to remove iface wlan0)

```
#loopback adapter
auto lo
iface lo inet loopback
#wired adapter
iface eth0 inet dhcp
#bridge
auto br0
iface br0 inet dhcp
bridge_ports eth0 wlan0
```
Make hostapd to be run everytime on boot:

```
sudo update-rc.d hostapd enable
```

Now reboot.. You can start or stop hostapd with

```
sudo service hostapd stop
```

or

```
sudo service hostapd start
```
**Note-:** Booting may take a little time if one/both of the interface fails (eth0 wlan0). For example, if you are using an external usb wifi card and removed it while booting, Bridging will fail, hence increase in overall booting time.
