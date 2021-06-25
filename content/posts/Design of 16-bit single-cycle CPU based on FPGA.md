+++
author = "causehhc"
title = "基于FPGA的16位单周期CPU设计"
date = "2020-07-11"
description = ""
tags = ["FPGA"]
categories = ["嵌入式"]
+++

# 基于FPGA的16位单周期CPU设计
## 一、实验过程
### 1、ALU程序设计；
![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/efcd8eda7b064530aec2ac89124abc17~tplv-k3u1fbpfcp-watermark.image)

数据通路调用这个模块，根据得到的控制信号对输入数据进行处理，处理功能有：addu,add等。
### 2、寄存器组设计；
![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/16ee5de30ec147edb4fe5ee890a9b195~tplv-k3u1fbpfcp-watermark.image)

由多个寄存器构成，用于存储各个数据结果。
### 3、PC程序设计；
![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ba6c89904a934624a71d0edeea991335~tplv-k3u1fbpfcp-watermark.image)

给出程序指令的地址，使得程序能够通过地址顺序执行。
### 4、下地址部件设计；
![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/eaea441fb316422bb72727f8daba6263~tplv-k3u1fbpfcp-watermark.image)

地址逻辑转移器通过各个信号和指令中的操作码，判断下一条指令的地址，并能给出程序执行的下一条指令的地址。
### 5、指令译码器设计
![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e377114bd1514e8f89b063d5be396bcc~tplv-k3u1fbpfcp-watermark.image)

根据OP与FUNC（R型指令）共同输出最终执行指令。
### 6、立即数扩展器设计
![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/06835f7cb8154af1b5de23415475bee1~tplv-k3u1fbpfcp-watermark.image)

立即数扩展部件主要分为有符号扩展和无符号扩展，用于将8位立即数扩展为16位数据。扩展方式根据指令集中立即数是否有符号来判断，有符号扩展主要针对beq,bne,bgt,addi等指令，其余指令中的立即数采用无符号扩展。
### 7、实验主电路的设计
![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5360e50d4e574236b9f9edfa5ec951f9~tplv-k3u1fbpfcp-watermark.image)
## 二、实验结果与分析
### 1、实验结果和分析（采样表格形式）
#### （1）数据存储器地址及内容
![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d72b15b2e1f942439d12c1c5db3da09d~tplv-k3u1fbpfcp-watermark.image)
#### （2）程序清单以及功能说明
![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/05d8e265c61d45268e1454f0464520fa~tplv-k3u1fbpfcp-watermark.image)
#### （3）运行结果对比和结果判断
![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/08c7c943660646a9af41ca1a87431ff4~tplv-k3u1fbpfcp-watermark.image)
### 2、错误或异常现象分析
现象：在ROM中指令无法正常跳转

分析：问题主要出现在PC与ROM的连接方式错误，以及在观察现象时软件显示没有进行正确设置。

[资料下载](https://drive.google.com/file/d/1zY14rZ6dGtSThl3_RN_iASiv4kbrcWwb/view?usp=sharing)
