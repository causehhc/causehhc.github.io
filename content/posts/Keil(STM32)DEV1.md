+++
author = "causehhc"
title = "Keil(STM32)开发环境-(1)Keil软件安装步骤"
date = "2020-07-01"
description = ""
tags = ["STM32", "Keil"]
categories = ["嵌入式"]
+++

## 1、Keil软件安装步骤
### 1.1、安装详细步骤
#### 1. 从官网得到Keil安装包（MDK-Arm）：
https://www.keil.com/download/product/ 

![1.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/beb17828f6234f008e9a6c52fd17a25a~tplv-k3u1fbpfcp-watermark.image)  
`图 1-1 Keil官网下载界面  `
#### 2. 在非系统盘（除c盘外）新建Keil_v5文件夹，用于存放keil软件。

![2.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/13631f95cd9a44eabde0086c37fd9893~tplv-k3u1fbpfcp-watermark.image)  
`图 1-2 选择路径  `
#### 3. 双击MDK531.EXE开始安装，点击【next】。
 
![3.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c4a31dbcc26a4d25b4f5f7391bbab947~tplv-k3u1fbpfcp-watermark.image)  
`图 1-3 安装界面  `
#### 4. 勾选【agree】，点击【next】。
 
![4.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cb40a0db247148cc8c2c8cb6a364f05f~tplv-k3u1fbpfcp-watermark.image)  
`图 1-4 安装界面  `
#### 5. 选择安装路径【Browser…】
放置在我们在一步建好的Keil_v5文件夹中，标蓝的部分需要手动输入（注意大写），路径不能带中文，点击【next】。
 
![5.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7ec5695d787044859f8cc9b3b05d7c83~tplv-k3u1fbpfcp-watermark.image)  
`图 1-5 选择两个路径  `
#### 6. 填写用户信息，全部填【0】即可，点击【next】。
 
![6.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a74a3518f58240289c149d7cad3c4975~tplv-k3u1fbpfcp-watermark.image)  
`图 1-6 填写信息  `
#### 7. 点击【finish】，安装完毕。
 
![7.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/88bf013e1a5e494986f83aa8c379ddf3~tplv-k3u1fbpfcp-watermark.image)  
`图 1-7 安装界面  `
#### 8. 之后会弹出芯片包的安装程序，由于Keil公司服务器/网络问题，会一直卡在检查更新过程中（底边框可以查看进度）。所以需要取消勾选【Packs】-【Check for Updates on Lunch】，然后关闭。
 
![8.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/447a893252644f37a7af2f9406323cb5~tplv-k3u1fbpfcp-watermark.image)  
`图 1-8 包安装器  `

