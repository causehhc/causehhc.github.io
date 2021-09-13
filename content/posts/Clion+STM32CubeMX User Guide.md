+++
author = "causehhc"
title = "Clion+STM32CubeMX使用指南"
date = "2020-06-25"
description = ""
tags = ["STM32","Clion","JetBrains"]
categories = ["嵌入式"]

+++

## 〇、软件清单
- STM32CubeMX
- Clion
- en.stsw-link009.zip —— ST-Link V2的驱动，Clion需要更新一下这个驱动
- gcc-arm-none-eabi-5_4-2016q3-20160926-win32.exe —— win下的arm架构交叉编译环境
- java1.8.0_261-jdk
- java1.8.0_261-jre
- LLVM-9.0.0-win64.exe —— c编译器，我VScode搭配的是这个+下面的MinGW
- MinGW —— c编译器，知乎那个教程好像用的是这个
- openocd-esp32-win32-0.10.0-esp32-20200526.zip —— 下载程序用的
- st_nucleo_f103rb_stlink_v2.cfg —— 自己写的一个OpenOCD的配置文件，放在XXX\openocd-- esp32\share\openocd\scripts\board下，创建工程时需要选用这个
## 一、CubeMX
### 1、简介
- cubemx是ST官方出的，一款使用HAL库，自动生成stm32初始化代码的软件。与其配套的IDE叫STM32CubeIDE，我没用过，你们可以试试。
- **HAL库**：Hardware abstract layer，硬件抽象层，ST官方出的。首先要和标准库做对比，野火那套用的库函数就是标准库，它的特点是非常细节，具体到硬件实施，接近寄存器操作。而HAL库封装程度要高一点，各种函数或者变量的命名格式也很容易看懂，总之就是用的更顺手一点。
- **自动生成**：字面意思，FULL AUTO。
- **初始化代码**：意思就是只帮你初始化了，启动还是需要自己启动的。野火那套是初始化和启动写一起了，假如有多个设备联合工作，初始化好的就自己先跑起来了（例如定时器中断），相当异端。
### 2、使用流程
#### 2.1、一般使用流程
1. 芯片选型
2. 配置RCC：选HSE-Crystal/Ceramic Resonator，配置时钟树
3. 配置SYS：选Debug-Serial Wire
4. 其他
- 注：只有当1-3步完成之后才能配置别的，否则可能会遇到奇怪的bug，例如没选debug方式的话下次程序就下不进去。
#### 2.2、新手上路流程
1. 一般软件下好打开之后就会让你导包，点这个

![1.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bd8d0c786cea40398f65c61b7b1f194b~tplv-k3u1fbpfcp-watermark.image)
2. 选STM32F1系列，安装

![2.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cfccc321bf594f46b1d8c2e01878c803~tplv-k3u1fbpfcp-watermark.image)
3. 开始上路，新建工程

![3.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7edebc33bf9a40a085aa07fee7b47451~tplv-k3u1fbpfcp-watermark.image)
4. 直接输型号

![4.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/15c7c8f489fd4e2c89f352a68f4f4da0~tplv-k3u1fbpfcp-watermark.image)
5. 打开界面后我也不做翻译了，上面这些选项卡懂得都懂，不知道可以查一下

![5.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2d96270e1bf64fb692d14e89bd99a456~tplv-k3u1fbpfcp-watermark.image)
6. 配置系统核心（System Core），字面意思，都核心了肯定是重要的东西，在这里需要配置两个东西，一个是RCC，一个是SYS，其余默认。这俩一个是时钟树，一个是Debug方式，也就是下载方式，我们用的是SWD。

![6.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/871d445ba6b848b39392aad73d227dda~tplv-k3u1fbpfcp-watermark.image)

![7.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d697032ba07a478ead41703221c6182b~tplv-k3u1fbpfcp-watermark.image)
7. 然后打开Clock Configuration，这里是真·时钟树，一般直接在这里输入72然后回车就完事了，软件会自动给你找合适的方案。
 
![8.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fde66ffa66b1414e88551cf2ff6f5dc3~tplv-k3u1fbpfcp-watermark.image)
8. 然后配置其他东西，这里就直接参考（可以暂时不弄）：
定时器中断：https://blog.csdn.net/toopoo/article/details/79748683
定时器PWM：https://blog.csdn.net/toopoo/article/details/79749851
定时器输入捕获：https://blog.csdn.net/as480133937/article/details/99407485
9. 然后保存，起个名字ctrl+S
 
![9.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9d46f212e3954f6d9e7e7d8cdf034d7c~tplv-k3u1fbpfcp-watermark.image)
10. 然后选工具链和版本
 
![10.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c994790614ed4a979c8ed2688fdc1cb3~tplv-k3u1fbpfcp-watermark.image)
11. 选这俩
 
![11.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/774c1c7e9985464f88465db845bf0cd0~tplv-k3u1fbpfcp-watermark.image)
12. 然后再保存一边直接点生成就好
 
![12.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/44d28ac5dec943ff8f9521da9796fe6b~tplv-k3u1fbpfcp-watermark.image)
13. 然后可以看一下它生成的代码，值得注意的一点就是，你自己编写的代码必须放在这种格式里面【USER CODE BEGIN/END】，写在其他地方的话下次【生成】会被直接删掉。
 
![13.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/339f4742c6ac4329853e9366b3b8afab~tplv-k3u1fbpfcp-watermark.image)
14. 剩下的自己看注释就好了。
## 二、Clion+CubeMX联动
clion是啥就不用多说了，目前为数不多支持正经c语言的现代化大厂IDE。  
pycharm、idea、goland都是jetbrains一家公司的，它们有统一的界面，非常友好。
### 2.1、预习
- 用clion编stm32程序，首先看这篇：https://zhuanlan.zhihu.com/p/63672432
- 以上工作配置好后（意思是可以编译通过），开始配置流程，如果不安这个步骤来的话可能联动不起来。
- **只有当以上工作完成后才可以继续下一步。**
### 2.2、一般使用流程
1. 首先打开clion，创建一个stm32cubemx的工程。要明白工作区和工程文件夹的概念。例如下图中，建立在桌面的工作区【workspace】，在工作区中建立工程文件夹【project】，之后工程文件内容就存放在文件夹【project】里。**如果是单级目录的话，cubemx或clion识别不到工具链。**

![14.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5c4a5a3e65b743009b8211f2e0a770bd~tplv-k3u1fbpfcp-watermark.image)
2. 打开.ioc文件。
 
![15.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7d8e6efd33674fe6836a29fa83bca197~tplv-k3u1fbpfcp-watermark.image)

3. 在【芯片选型】之后，【设置时钟树】，【设置debug方式】之后。
 
![16.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f14ede425f004d23a64142d5434db141~tplv-k3u1fbpfcp-watermark.image)
4. 按照下图仔细配置，包括**文件名和文件路径**，文件名要和原来的一样，以便成功覆盖原文件。
 
![17.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/811319438e3f466da6a40811ca7181a7~tplv-k3u1fbpfcp-watermark.image)

![18.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3556cf90e9f44a91838c2ec3c985bac9~tplv-k3u1fbpfcp-watermark.image)
5. 只有出现这个提示后，文件才会被成功覆盖
 
![19.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1739b7205c3b4a738b15ee8f491f2459~tplv-k3u1fbpfcp-watermark.image)
6. 切换回clion，会自动刷新，选择【这个】，【这个】是我自己写的，等你们做到这步之后我发你们。点开mian
 
![20.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/665b462a7ade4ae7b324d524bcfdd204~tplv-k3u1fbpfcp-watermark.image)
7. 然后就可以编代码了，小锤子是编译、箭头就编译后下载。
 
![21.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e9e9b8df145c4ad3af1c022761c35149~tplv-k3u1fbpfcp-watermark.image)
8. 如果在cubemx对引脚重新命名之后，他这里代码也会自动补全。
 
![22.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0354e41faaab46a79f041791e263536f~tplv-k3u1fbpfcp-watermark.image)
 
![23.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d2a4b29698a9499a954ba9ae10b352b6~tplv-k3u1fbpfcp-watermark.image)
9. 然后就下载完成了
 
![24.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/717283e137044d7492168789b9e5b0d2~tplv-k3u1fbpfcp-watermark.image)
10. 点亮LED小灯
 
![111.jpg](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c758ee785599413290d720ec6cddaec5~tplv-k3u1fbpfcp-watermark.image)
## 三、Issues
1. 在使用cubemx前，确保java版本为1.8及以上、并且正确的配置环境变量，即java、javac均可在cmd中运行。
2. 在使用clion时，需要正确的部署本机编译器和交叉编译器。本机编译器采用LLVM+MinGW联合使用，交叉编译器请安装gcc-arm-none-eabi。
3. 在clion中，如果遇到找不到某种编译器，请检查是否正确安装本机编译器与交叉编译器。
4. 下载程序时提示XXXLevel-Low，请更新ST-LINK V2驱动。
5. 如果使用在线调试功能，请在ToolChain中Debug下选择：XXX\GNU Tools ARM Embedded\5.4 2016q3\bin\arm-none-eabi-gdb.exe。
