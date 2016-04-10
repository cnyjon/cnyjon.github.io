---
layout: post
title:  "如何在树霉派中通过命令行设置WiFi"
date:   2016-04-11 23:05:20
categories: RaspberryPi

---
#### 1. 先安装工具和驱动(网卡为TP-Link TL-WN722N)

	sudo apt-get update
	sudo apt-get install wireless-tools usbutils
	sudo apt-get install firmware-atheros
 	sudo wget http://linuxwireless.org/download/htc_fw/1.3/htc_9271.fw
 	sudo cp htc_9271.fw /lib/firmware

#### 2. 编辑配置文件`/etc/network/interfaces`

	auto lo

	iface lo inet loopback
	iface eth0 inet dhcp

	# 下面是新增的无线配置
	auto wlan0
	iface wlan0 inet dhcp
	wpa-conf /etc/wpa.conf
	
#### 3. 安装上无线网卡，然后检测是不可用

	sudo iwconfig
	sudo iwlist wlan0 scan
	
#### 4. 创建`wpa.conf`文件，例如: `sudo vi /etc/wpa.conf`

	network={
	ssid="NETWORK-SSID"
	psk="YOURPASSWORD"

	# Protocol type can be: RSN (for WP2) and WPA (for WPA1)
	proto=RSN

	# Key management type can be: WPA-PSK or WPA-EAP (Pre-Shared or Enterprise)
	key_mgmt=WPA-PSK

	# Pairwise can be CCMP or TKIP (for WPA2 or WPA1)
	pairwise=CCMP TKIP

	#Authorization option should be OPEN for both WPA1/WPA2 (in less commonly used are SHARED and LEAP)
	auth_alg=OPEN
	}
	
#### 5. 最后重启设备

	sudo reboot