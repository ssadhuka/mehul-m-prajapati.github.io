---
title: "Configuring Soft-ether on Linux"
excerpt: "Do you want to setup VPN ?"
category: VPN
tags: [soft-ether, packaging]
header:
  image: https://i.imgur.com/tOdLKhy.png
  overlay_color: "#000"
  overlay_filter: "0.6"
  overlay_image: https://www.softether.org/@api/deki/files/4/=1.2.jpg
---
Download soft-ether server from: [Soft-ether](http://www.softether-download.com/en.aspx?product=softether)

### Start Soft ether Server
```
# cd vpnserver
# make
```
Stop the firewall and promisc mode
```
# ./vpnserver start
# service firewalld stop
# ip link set ens32 promisc off
```
**OR**

- Remotely connect to soft-ether sever from windows
- Create a New HUB
- Enable local bridging from soft-ether server GUI & create a new local bridge interface tap
- Make this HUB Online

### DNS configuration

Edit dnsmasq.conf file, follow below sample configuration
```
# vim /etc/dnsmasq.conf
interface=tap_soft
dhcp-range=tap_soft,192.168.7.50,192.168.7.60,12h
dhcp-option=tap_soft,3,192.168.7.1

interface=tap_soft_2
dhcp-range=tap_soft_2,192.168.8.50,192.168.8.60,12h
dhcp-option=tap_soft_2,3,192.168.8.1
#except-interface=tap_soft
```

Set IP address of tap_soft
```
# service dnsmasq restart
# ifconfig tap_soft 192.168.7.1
```

### Soft ether Client

Export client config .vpn file from Windows Softether GUI and copy it into current directory
```
# cd vpnclient
# cp ~/Desktop/Client-1.vpn ./

# ./vpnclient start
# ./vpncmd
```
Select 2nd option: Management of VPN Client
```
VPN Client>remoteenable
VPN Client>niccreate
NicCreate command - Create New Virtual Network Adapter
Virtual Network Adapter Name: se

VPN Client>niclist
VPN Client>accountimport
AccountImport command - Import VPN Connection Setting
Import Source File Name: ./Client-1.vpn

VPN Client>accountget Client-1
VPN Client>accountlist
```

Connect to server
```
VPN Client>accountconnect Client-1
VPN Client>accountlist
```

Before we tweak the routing table, enable ip forward in "/etc/sysctl.conf"
```
# net.ipv4.ip_forward=1
# sysctl -p
# ip addr show vpn_se
# dhclient vpn_se -v
# ip addr show vpn_se
# ip neigh
```

Status Commands
```
# netstat -anlp | grep -w LISTEN
# iptables -t nat -vL
```

Reference: [Softether blog](http://blog.lincoln.hk/blog/2013/05/17/softether-on-vps-using-local-bridge/)
