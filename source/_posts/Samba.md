---
title: Samba
feature: true
tags:
  - 服务
categories:
  - 技术类
keywords: 'Samba,服务,网络,共享'
description: 最近刚刚换了工作的地方,工作的网络环境需要随时的分享文件,传输文件,便搭建了Samba服务,由于环境中存在Mac电脑,便顺带把Mac的时间机器(Time Machine)和Samba联动了
date: 2021-05-14 19:59:51
---

# Samba 服务搭建

## 需求分析

我们工作室人员不是很多,但是日常使用需要大量的文件拷贝复制,或者是备份任务,需要一个文件共享服务。

- 每个人需要一个单独地空间存储文件
- 需要一个单独的共用文件夹共享文件

## 网络环境分析

<img src="https://ww1.sinaimg.cn/large/005ZbjyVly1gqk6msqklkj30uq16a400.jpg" alt="005ZbjyVly1gqk6mnto1nj30uq16awfz" style="zoom: 50%;" />

## Samba 服务搭建

:::tip ENV

- OS：CentOS 7

:::

### 安装Samba

```shell
yum install -y samba
```



### 配置文件

每个用户使用自己的家目录来存储自己的文件，这个在LInux中是自动完成的，配置简单，且符合需求。

配置文件`cat /etc/samba/smb.conf`内容如下

```
# See smb.conf.example for a more detailed config file or
# read the smb.conf manpage.
# Run 'testparm' to verify the config is correct after
# you modified it.

[global]
	workgroup = SAMBA
	security = user

	passdb backend = tdbsam
	
	read size = 65536
	#socket options = TCP_NODELAY SO_KEEPALIVE SO_RCVBUF=512 SO_SNDBUF=512 IPTOS_LOWDELAY
	socket options = TCP_NODELAY IPTOS_LOWDELAY SO_RCVBUF=131072 SO_SNDBUF=131072
	getwd cache = yes

	follow symlinks = no
	wide links = no
	
	read raw = yes
	write raw = yes

	large readwrite = yes

	signing_required=no

	printing = cups
	printcap name = cups
	load printers = yes
	cups options = raw
	max connections = 0
	deadtime = 0
	max log size = 500

	use sendfile = yes
	read prediction = yes
	write raw = yes
	read raw = yes
	max xmit=65535
	aio read size = 16384
	aio write size = 16384	
	min protocol = SMB2
	ea support = yes

[homes]
	comment = Home Directories
	valid users = %S, %D%w%S
	browseable = Yes
	read only = No
	inherit acls = Yes
	# Mac
	vfs objects = catia fruit streams_xattr
	fruit:aapl = yes
	fruit:metadata = stream
	fruit:time machine = yes
	fruit:posix_rename = yes
	fruit:veto_appledouble = no
	fruit:wipe_intentionally_left_blank_rfork = yes
	fruit:delete_empty_adfiles = yes

[printers]
	comment = All Printers
	path = /var/tmp
	printable = Yes
	create mask = 0600
	browseable = No

[print$]
	comment = Printer Drivers
	path = /var/lib/samba/drivers
	write list = @printadmin root
	force group = @printadmin
	create mask = 0664
	directory mask = 0775

[public]
	comment = Public Path
	path = /home/public
	writable = yes
	guest ok = no
	browseable = yes
```



:::tip Mac

```
[homes]
	comment = Home Directories
	valid users = %S, %D%w%S
	browseable = Yes
	read only = No
	inherit acls = Yes
	# Mac
	vfs objects = catia fruit streams_xattr
	fruit:aapl = yes
	fruit:metadata = stream
	fruit:time machine = yes
	fruit:posix_rename = yes
	fruit:veto_appledouble = no
	fruit:wipe_intentionally_left_blank_rfork = yes
	fruit:delete_empty_adfiles = yes
```

这里的Mac以下的部分是为了配置Mac的TimeMachine（时间机器）备份。

:::

### 启动服务

```shell 
sudo systemctl start smb
```

###  设置开机启动

```shell
sudo systemctl enable smb
```

## 客户端连接

### Windows

同时摁下<kbd>Windows</kbd> + <kbd>R</kbd>,在弹出的窗口输入`\\192.168.31.250 [这里是samba的IP地址]`然后输入用户名、密码验证身份就可以使用了。

### Mac

打开访达窗口，同时摁下<kbd>Command</kbd> + <kbd>K</kbd>,在弹出的窗口输入`smb://192.168.31.250 [这里是Samba的IP地址]`然后输入用户名、密码验证身份使用。
