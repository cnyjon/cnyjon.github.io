---
layout: post
title:  "在Linux上安装Tinc VPN"
date:   2015-04-27 01:25:32

---

# 在Ubuntu上安装Tinc VPN

`tinc`是荷兰一所大学计算机学院开发的一个非常强大的安全VPN通信软件，它可以穿越两个内网进行防火墙穿洞互相链接，软件本身支持socke服务端代理，可供客户端通过代理上网，他的强大之处还在与可以使众多客户端形成点到点的端口链接，这点`OPENVPN` 可做不到，所以安装了`tinc`的客户端可以使大家在互联网上一起玩局域网游戏，


### 安装Tinc

	$ sudo apt-get update

	$ sudo apt-get install tinc

### 配置Tinc

创建文件

	$ sudo mkdir -p /etc/tinc/myvpn/hosts

编辑文件

	$ sudo vi /etc/tinc/myvpn/tinc.conf

添加以下几行

	Name = AAA              #添加名为AAA的节点
	AddressFamily = ipv4    #使用IPv4
	Interface = tun0        #虚拟网卡

接着创建`myvpn`的hosts配置文件

	$ sudo vi /etc/tinc/myvpn/hosts/AAA

添加以下两行

	Address = 1.2.3.4      #服务器的公网IP地址
	Subnet = 10.0.0.1/32   #VPN的虚拟网卡内网地址

接着生成我们的密钥

	$ sudo tincd -n myvpn -K4096

创建一个`myvpn` VPN开始运行时自动运行的脚本

	$ sudo vi /etc/tinc/myvpn/tinc-up

添加如下行

	#!/bin/sh
	ifconfig $INTERFACE 10.0.0.1 netmask 255.255.255.0

当开始运行`myvpn` VPN时，这个脚本会创建一个网络适配器，分配10.0.0.1 IP给它。
接着再创建一下关闭的脚本。

	$ sudo vi /etc/tinc/myvpn/tinc-down

添加

	#!/bin/sh
	ifconfig $INTERFACE down

改变这两个文件的权限

	$ sudo chmod 755 /etc/tinc/myvpn/tinc-*

现在服务器节点已建立完成，建立客端节点和它类似。

	$ sudo mkdir -p /etc/tinc/myvpn
	$ sudo vi /etc/tinc/myvpn/tinc.conf

	Name = BBB
	AddressFamily = ipv4
	Interface = tun0
	ConnectTo = AAA

	$ sudo vi /etc/tinc/myvpn/hosts/BBB

	Subnet = 10.0.0.2/32

	$ sudo tincd -n myvpn -K4096

	$ sudo vi /etc/tinc/myvpn/tinc-up

	ifconfig $INTERFACE 10.0.0.2 netmask 255.255.255.0

	$ sudo vi /etc/tinc/myvpn/tinc-down

	ifconfig $INTERFACE down

	$ sudo chmod 755 /etc/tinc/myvpn/tinc-*

在AAA主机上

	$ scp /etc/tinc/myvpn/hosts/AAA root@BBB:/etc/tinc/myvpn/hosts/

在BBB主机上

	$ scp /etc/tinc/myvpn/hosts/BBB root@AAA:/etc/tinc/myvpn/hosts/

最后两台机都运行

	$ sudo tincd -n myvpn

