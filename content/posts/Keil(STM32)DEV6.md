+++
author = "causehhc"
title = "Keil(STM32)开发环境-(6)程序编写与基本调试"
date = "2020-07-01"
description = ""
tags = ["STM32", "Keil"]
categories = ["嵌入式"]
+++

## 6、程序编写与基本调试
### 6.1、新建文件
通过我们之前对构建工程模板的学习，我们应该了解在创建文件时，一般需要将.c文件与.h文件一起添加。同时由用户自己创建的.c文件一般放置在./USER/Src文件夹中，由用户自己创建的.h文件一般放置在./USER/Inc文件夹中，这样整个工程才可以正常运行。
当然，这么做只是为了降低项目维护成本，但是这个习惯尤其重要。尤其是当你参与团队项目开发时，统一的开发习惯会大大增加团队的效率。
具体操作为：
#### 1. 点击【新建文件】
 
![47.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9833cff44d534d718baaf824710886ea~tplv-k3u1fbpfcp-watermark.image)  
`图 6-1 按钮在界面中的位置`
#### 2. 新建完成后点击【保存】或按【Ctrl+S】进行保存
 
![48.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3e43acb43f0a47a58f894f54b9e72359~tplv-k3u1fbpfcp-watermark.image)  
`图 6-2 新建文件后的效果`
#### 3. 打开./USER/Src，输入文件名，【加.c后缀】，之后点击【保存】
 
![49.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/dc06474cdca345a385690fa8942dd411~tplv-k3u1fbpfcp-watermark.image)  
`图 6-3 源文件保存操作`
#### 4. 重复（1）（2）操作，之后打开./USER/Inc，输入与（3）同名（后缀不同）文件名，这次【加.h后缀】，之后点击【保存】
 
![50.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7a1e64c92fb245b08801c02e65f638f7~tplv-k3u1fbpfcp-watermark.image)  
`图 6-4 头文件保存操作`
#### 5. 双击【USER】
 
![51.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/835dd626084c4debafa71d77c024b18a~tplv-k3u1fbpfcp-watermark.image)  
`图 6-5 按钮在界面中的位置`
#### 6. 找到./Src位置，选择【test.c】，点击【add】，注意不需要将.h文件也add进来。
 
![52.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d097fea8e747443d925aba054e960185~tplv-k3u1fbpfcp-watermark.image)  
`图 6-6 将源文件添加到组`
#### 7. 现在可以观察到已经添加成功了。
 
![53.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/dff44dec24b744a1b11cef0097fdef49~tplv-k3u1fbpfcp-watermark.image)  
图 6-7 最终添加效果`
### 6.2、基本编写
在本阶段完成文件间的关联即可。
#### 1. .c文件的编写
- 导入自己的.h文件
- 函数实现（这里暂不要求）
 
![54.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fa4b01b4beea41458d8f4c5a6b773184~tplv-k3u1fbpfcp-watermark.image)  
`图 6-8 源文件基本编写`
#### 2. .h文件的编写
- 预编译语句，防止重复编译
- 导入标准库 ”stm32f10x.h”
- 宏定义（这里暂不要求）
- 函数声明（这里暂不要求）
 
![55.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/dc3fc8494dd74d8fa4b597410a62f46f~tplv-k3u1fbpfcp-watermark.image)  
`图 6-9 头文件基本编写`
#### 3. main文件的增量编写
- 导入刚刚编写的 “test.h”
- 使用“test.h”自己编写的库函数（这里暂不要求）
 
![56.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f14c3886c7c1498c81e419980dcc3102~tplv-k3u1fbpfcp-watermark.image)  
`图 6-10 mian增量编写`
#### 4. 编译
点击编译，下面会显示0 Error(s), 0 Warning(s)，编译通过
 
![57.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9626ef1ff69c440ca220a979e94cdc3f~tplv-k3u1fbpfcp-watermark.image)  
`图 6-11 编译通过`
### 6.3、在线调试
在线调试分为软件在线调试与硬件在线调试。由于现在硬件成本较低，一般我们都使用硬件在线调试，也就是软件直接下载到芯片，软件与硬件同步运行，我们可以查看运行状态。
#### 1. Keil支持硬件调试，为了说明功能，我们可以编写一段简单的程序。
点击侧边栏对这一行加断点，然后点击【Start/Stop Debug Session】
 
![58.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/04ef434397094e079ac3ed7f8313ffa8~tplv-k3u1fbpfcp-watermark.image)  
`图 6-12 按钮在界面中的位置`
#### 2. 点击之后得到如下界面，如果左下角的【Command】栏中显示报错，或者与调试时的预期现象不符合，尝试退出后重新编译，再次进入。
 
![59.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e53ec22b53924a9cbda893e90dda8459~tplv-k3u1fbpfcp-watermark.image)  
`图 6-13 界面整体介绍`
#### 3. 由于篇幅有限，所以只介绍一些基本按钮的使用。
- 复位【Reset】：使程序复位到初始状态。
- 运行【Run】：让程序全速运行，直到遇到断点，程序将暂停。
- 停止【Stop】：停止运行。
- 单步调试【Step】：每点一次按钮，程序运行一步，遇到函数会进入。
- 逐行调试【Step Over】：每点一次按钮，程序运行一行，意味着遇到函数不进入。
- 跳出调试【Step Out】：每点一次按钮，程序跳出这个函数，直到回到main函数。


