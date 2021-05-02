+++
author = "causehhc"
title = "Keil(STM32)开发环境-(2)Keil导入设备系列包"
date = "2020-07-01"
description = ""
tags = ["STM32", "Keil"]
categories = ["嵌入式"]
+++

## 2、Keil导入设备系列包
### 2.1、导入芯片包详细步骤
#### 1. 下载芯片包 
Keil5不像Keil4那样自带了很多厂商的MCU型号，Kei5需要自己安装芯片包，例如我们用的STM32F103RET6。可以从官网下载芯片包：http://www.keil.com/dd2/pack/
 
![9.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d9a9ad31c7e04d5f9d5d114da153069f~tplv-k3u1fbpfcp-watermark.image)  
`图 2-1 Keil官网下载包  `
也可以直接使用**Keil.STM32F1xx_DFP.2.3.0.pack**，其中**xx**代表兼容不同版本，本芯片包可以支持我们当前所用的STM32F103系列，同时也支持其他F1系列，例如F100、F101等。
#### 2. 打开Keil软件，点击包安装器【Pack Installer】按钮。
 
![10.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3d469c54995c4d99ad507353d05b36ae~tplv-k3u1fbpfcp-watermark.image)  
`图 2-2 包安装器在界面中的位置  `
#### 3. 等待界面稳定后，点击【File】-【Import…】
 
![11.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/328ff3317dc047338e15b8cf2a3a1dc9~tplv-k3u1fbpfcp-watermark.image)  
`图 2-3 选择导入`
#### 4. 选择**Keil.STM32F1xx_DFP.2.3.0.pack**，点击【打开】。
 
![12.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5d1caea752b044dfb8a48467a0927354~tplv-k3u1fbpfcp-watermark.image)  
`图 2-4 选择包路径  `
#### 5. 等待安装完毕后退出界面，至此芯片包导入完毕，可以开始下一步。
 
![13.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7c3d054682644f5da82f344538b443cd~tplv-k3u1fbpfcp-watermark.image)  
`图 2-5 导入成功  `

