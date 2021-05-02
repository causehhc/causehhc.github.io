+++
author = "causehhc"
title = "Keil(STM32)开发环境-(4)配置工程参数"
date = "2020-07-01"
description = ""
tags = ["STM32", "Keil"]
categories = ["嵌入式"]
+++

## 4、配置工程参数
### 4.1、打开工程模板
首先打开工程模板，这是一个基础模板，之后可以在其基础上进行**增量编程**，由于可能需要被使用很多次，所以首先将模板做好备份。
 
![27.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4022ba9ccbd24d169883fbb6c72d7136~tplv-k3u1fbpfcp-watermark.image)

`图 4-1 点击打开工程`

### 4.2、目标选项【Options for Target】的配置

点击该按钮，几乎所有工程参数的配置都将在这里进行。
 
![28.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/59ee7093d6764a6e959fa0efe88deb6a~tplv-k3u1fbpfcp-watermark.image)

`图 4-2 按钮在界面中的位置`

#### 1. 设备【Device】

这里可选择芯片型号，我们选择**STM32F103RE**

 
![29.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e57e8e2a93dc4dbba80c75086c7edcda~tplv-k3u1fbpfcp-watermark.image)

`图 4-3 【Device】界面`

#### 2. 目标【Target】

这里是关于工程目标的调试晶振频率、编译器、RAM和ROM的分配地址等，具体配置参考下图。
 
![30.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/80704d3cada24881ab8d9bc3b8655bc6~tplv-k3u1fbpfcp-watermark.image)

`图 4-4 【Target】界面`

#### 3. 输出【Output】（不操作）

这里是调节输出内容，暂不对其操作。

#### 4. 列表【Listing】（不操作）

这里是关于编译时的汇编列表相关操作，暂不对其操作。

#### 5. 用户【User】（不操作）

这里是关于用户的使用设计，不常用，暂不对其操作。

#### 6. C/C++配置【C/C++】（最为重要）

**此部分最为重要**，是关于c/c++的相关配置，有预处理、语言代码生成、包含路径、多功能控件、编译器控制字符串。
 
![31.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f7f61ca4e1f841b49608d0e25cf820da~tplv-k3u1fbpfcp-watermark.image)

`图 4-5 【C/C++】界面`

其中在本阶段**包含路径**要理解，有时明明include的一个头文件，但是程序就是报错，可能的原因就是因为程序找不到头文件所在路径，而头文件的搜索路径就是在这里面设置。
 
![32.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3789031f18e44282acf72fe694dd1d7f~tplv-k3u1fbpfcp-watermark.image)

`图 4-6 添加包含路径`

这里不需要操作，因为工程所需的路径已经被包含，这里只讲解添加过程。值得一提的是，Inc文件夹一般放置头文件（.h文件，Inc就是include的缩写），Src文件夹一般放置源文件（.c文件）。
 
![33.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/82af9d9317814738a52839262efd10f7~tplv-k3u1fbpfcp-watermark.image)

`图 4-7 将./Inc添加到路径中`

#### 7. 汇编【Asm】（不操作）

这是关于汇编的设置，暂不对其操作。

#### 8. 链接【Linker】（不操作）

这是关于链接的设置，暂不对其操作。

#### 9. 调试【Debug】（比较重要）

这个选项**比较重要**，主要用于软件仿真、硬件在线调试、程序下载配置。

有时候**程序无法烧入开发板**可能就是因为这里配置错误。

*还有一种程序**无法烧入开发板**的原因可能是因为SWD端口被关闭，这时候需要将BOOT0和BOOT1拉高，写入空程序，然后再拉低烧入正常程序，具体操作原理这就是后话了，以后会展开讲解。

这里我们用的是【ST-Link】下载器，所以选择此方式Debug。之后选择**旁边**的【Settings】，对其进行详细配置。
 
![34.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/58537797e26b49589936b41033ed0e75~tplv-k3u1fbpfcp-watermark.image)

`图 4-8 【Debug】界面`

- （a）在【Debug】栏中

是关于Debug方式和Debug速度的配置，具体配置参照下图。
 
![35.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e4770f54e0ef44d38d3cb3d439a728b0~tplv-k3u1fbpfcp-watermark.image)

`图 4-9 选择Debug方式`

- （b）在【Flash Download】栏中

是关于下载的配置，stm32需要复位后程序才会执行，所以有时候开发板观察不到任何现象，这里可能就需要按一下复位按钮。但是勾选【Reset and Run】之后，即可实现自动复位，不需要每次下载程序后再按复位按钮。
 
![36.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/88bd605933714ae485fb8febe45ec004~tplv-k3u1fbpfcp-watermark.image)

`图 4-10 选择下载设置`

- （c）在【Pack】栏中

如果上一步完成后还是不能自动复位，那么就点开【Pack】，取消勾选【Enable】。
 
![37.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1b23d245071245cdb55f83ca1faa07e1~tplv-k3u1fbpfcp-watermark.image)

`图 4-11 取消选中Enable`

#### 10. 实用工具【Utilities】（不操作）

不常用，暂不对其操作。

至此目标选项【Options for Target】配置完毕。

