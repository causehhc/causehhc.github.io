+++
author = "causehhc"
title = "Keil(STM32)开发环境-(5)烧写测试程序"
date = "2020-07-01"
description = ""
tags = ["STM32", "Keil"]
categories = ["嵌入式"]
+++

## 5、烧写测试程序
### 5.1、编译目标文件
在烧写程序之前，需要对目标工程进行编译链接，这里keil有三种编译模式。
分别是【Translate】、【Build】、【Rebuild】。
 
![38.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ada1ede579ba4096bb9c56af733dc40e~tplv-k3u1fbpfcp-watermark.image)  
`图 5-1 按钮在界面中的位置`
#### 1. 【Translate】
编译当前源文件，这个过程中会进行语法错误的检查，但是不生成可执行文件，一般在修改.c文件后，点击这个按钮，用来查看修改后的程序是否有语法错误。
因为只是编译当前的单个文件，所以编译速度快。
#### 2. 【Build】（最常用）
编译工程中的目标文件，目标文件通常指上次修改的文件以及其他依赖于这些修改过的文件的模块，同时重新链接生成可执行文件。如果工程之前没有编译链接过，它会直接调用【Rebuild】进行全部工程所以文件的编译链接。
#### 3. 【Rebuild】
重新编译工程中所有源文件，于上次编译的结果无关，不管工程的文件之前有没有编译过，都会对所有文件重新进行编译并生成可执行文件。
因此花费时间较长，平时使用较少。
### 5.2、程序下载/烧写程序
#### 1. ST-LINK V2下载器介绍
ST-Link是用于STM8和STM32微控制器的在线调试器和编程器，也就是下载器。ST-Link具有SWIM、JATG/SWD等通信接口。
- SWIM：Single Wire Interface Module，单线接口模块。
- JATG：Joint Test Action Group，联合测试工作组，是一种国际标准测试协议。
- SWD：Serial Wire Debugging，串行调试接口。
其中我们使用SWD方式进行程序下载与调试。
 
![39.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/78f1fad49e494a00a4f531cda7f8c5ba~tplv-k3u1fbpfcp-watermark.image)  
`图 5-2 ST-Link下载器正视图`
值得注意的是，在正面的接口标识层有一个白色实心框，这对应着后面的引脚，可以通过这种方法确定引脚位置和名称（如下图）。
 
![40.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1464734cb78e4933ae2ba26e9718ef57~tplv-k3u1fbpfcp-watermark.image)  
`图 5-3 ST-Link下载器侧视图`
#### 2. 开发板下载接口介绍
在下图中框选的位置就是利用ST-Link下载器下载程序的4个引脚。
 
![41.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/654de62a5677478db37980941bb0b20e~tplv-k3u1fbpfcp-watermark.image)  
`图 5-4 开发板正视图`
其中各自的名称在旁边的电路板丝印已经被写明，为了更加直观，在下图中为大家标注出来。
 
![42.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e3695694de6e40eaa2ed1c5a0c237152~tplv-k3u1fbpfcp-watermark.image)  
`图 5-5 开发板上的SWD下载引脚`
#### 3. 开发板与下载器连接方法
连接方法就是使用杜邦线，与双方引脚对应连接。
- 下载器的SWDIO引脚 --连接-- 开发板的SWDIO引脚
- 下载器的GND引脚   --连接-- 开发板的GND引脚
- 下载器的SWCLK引脚 --连接-- 开发板的SWCLK引脚
- 下载器的3.3V引脚   --连接-- 开发板的VCC引脚
值得注意的是，他们的引脚并不是一一对应的，所以需要打乱杜邦线的顺序进行连接，为了避免引起歧义，就不放上图片进行展示。
**一定要正确连接，错误连接可能导致开发板或下载器烧毁！**
#### 4. 安装st-link驱动
（1）. 右键计算机，查看本系统是64还是32
 
![43.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e620fe5789ce498b8f15543288ba1271~tplv-k3u1fbpfcp-watermark.image)  
`图 5-6 查看本机系统`
（2）. 下载的st-link驱动安装包，双击安装
 
![44.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/eacea0e8100a46fd99e206727fc57868~tplv-k3u1fbpfcp-watermark.image)  
`图 5-7 ST-Link驱动安装程序`
（3）. 如果安装成功，完成电脑-下载器-开发板连接后，下载器上的红色指示灯会常亮。如果安装失败，下载器上的红色指示灯会闪烁，请尝试重新安装。
#### 5. 在Keil软件中点击下载
连接完毕后，即可点击下载按钮进行下载。
 
![45.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/85d5283e171243a7972f3f53d30f9899~tplv-k3u1fbpfcp-watermark.image)  
`图 5-8 按钮在界面中的位置`
出现此提示说明下载成功。
 
![46.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cf2b95cf91544d8da4f3f76205c5d58d~tplv-k3u1fbpfcp-watermark.image)  
`图 5-9 程序下载成功`

