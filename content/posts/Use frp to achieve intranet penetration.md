+++
author = "causehhc"
title = "使用frp实现内网穿透"
date = "2021-04-11"
description = ""
tags = ["Ubuntu"]
categories = ["中间件"]
+++

# 一、简述  
首先说明我们为什么要实现内网穿透、是因为其他用户需要访问到我们在内网中部署的一些服务，而一般我们的入网设备是没有被分配到公网ip的，所以我们需要借助一台带有公网ip的服务器来对我们的消息进行转发。  
- frp项目地址：https://github.com/fatedier/frp  
- frp使用介绍：https://github.com/fatedier/frp/blob/master/README_zh.md   

以下是【其他用户】访问【内网设备】的一条路径，其中我选择的方案是：公网服务器=阿里云，内网设备=ubuntu1604  

在公网服务器中运行frps、在内网设备中运行frpc  
![frp模型.jpg](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/03fbb4dbe3b14246b7e5435dea5545cc~tplv-k3u1fbpfcp-watermark.image)
# 二、服务端配置（阿里云服务器）
1. 下载frp并解压，我使用的是0.36.2版本，可自行切换其他版本。
```
wget https://github.com/fatedier/frp/releases/download/v0.36.2/frp_0.36.2_linux_amd64.tar.gz
tar -zxvf frp_0.36.2_linux_amd64.tar.gz
cd frp_0.36.2_linux_amd64
```
2. 编辑frps.ini
.ini结尾一般为配置文件，其中frps.ini为程序运行时所需的配置文件。  
而frps_full.ini为所有配置的demo，大家可以按需配置。  
```
# 通用配置
[common]
# frps服务在本机的监听地址及端口
bind_addr = 0.0.0.0
bind_port = 7000

# frp 控制面板
dashboard_port = 7500
dashboard_user = admin
dashboard_pwd = admin

# 默认日志输出位置(这里输出到标准输出)
log_file = /dev/stdout
# 日志级别，支持: debug, info, warn, error
log_level = info
log_max_days = 3
```
3. 启动服务  
```
./frps -c frps.ini
```
4. 以上命令在退出终端后就会停止，所以我们需要使frps在后台运行  
(1)运行
```
nohup ./frps -c frps.ini >/dev/null 2>&1 &
```
（2）停止
先找到这个进程再kill
```
ps -aux|grep frp| grep -v grep
kill -9 [进程号]
```
# 三、客户端配置（内网设备）  
客户端作为发起链接的主动方，只需要正确配置服务器地址，以及要映射客户端的哪些服务端口等即可
1. 下载frp并解压
```
wget https://github.com/fatedier/frp/releases/download/v0.36.2/frp_0.36.2_linux_amd64.tar.gz
tar -zxvf frp_0.36.2_linux_amd64.tar.gz
cd frp_0.36.2_linux_amd64
```
2. 编辑frpc.ini文件
```
# 通用配置
[common]
# 服务端地址及端口
server_addr = 你的公网ip
server_port = 7000


log_file = /dev/stdout
log_level = info
log_max_days = 3


# 将本地 ssh 映射到服务器
[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 6000

# 发布本地 web 服务
[web]
type = tcp
# 本地回环ip
local_ip = 127.0.0.1
# 5000一般为flask的默认开放端口
local_port = 5000
# 云服务器的80端口
remote_port = 80
```
3. 启动服务  
```
./frpc -c frpc.ini
```
# 四、测试启动情况
**注意**：由于云服务器的防火墙设置，在测试之前开放所需的端口，例如：7500（控制台端口）、以及你开放的各种【remote_port】。
![阿里云防火墙.jpg](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/43963059b5de40c7a745bc76a8cb83c9~tplv-k3u1fbpfcp-watermark.image)

服务端和客户端都启动后，访问`http://你的公网ip:7500`
![启动情况.jpg](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3fb464689be3476d9cf49d21867076fc~tplv-k3u1fbpfcp-watermark.image)

以上实现的功能有：
- 通过访问【server_addr:remote_port】来实现间接访问【local_ip:local_port】，从而达到访问内网设备的服务。
- 访问【server_addr:7500】进入frp的控制台。


