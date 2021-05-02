+++
author = "causehhc"
title = "Keil(STM32)开发环境-(3)构建工程模板"
date = "2020-07-01"
description = ""
tags = ["STM32", "Keil"]
categories = ["嵌入式"]
+++

## 3、构建工程模板
首先查看【工程模板文件结构】
- **左侧**为Keil自动生成的文件，其中绿色部分为**文件夹**，橙色部分为**文件**。
- **右侧**为我们自己添加的文件，红色部分为**主要**编写的部分，以后的课程可能会对其他文件进行编辑。
 
![14.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1e9be98c1c4a4cb1b5c0f5f0e3d4444a~tplv-k3u1fbpfcp-watermark.image)  
`图 3-1 工程模板文件结构`
### 3.1、右侧文件构建
可以根据网上的工程模板对右侧文件夹的理解与构建。
完成对右侧文件夹的构建后，接下来对左侧文件夹进行构建。
### 3.2、左侧文件构建
#### 1. 打开Keil软件，点击【Project】-【New uVision Project…】
 
![15.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5f40df9682b24b74b79fe15a464b7de7~tplv-k3u1fbpfcp-watermark.image)  
`图 3-2 新建工程  `
#### 2. 找到之前构建的【右侧文件】位置进行操作
 
![16.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b9fcac2c78034b739582e212e48af5ea~tplv-k3u1fbpfcp-watermark.image)  
`图 3-3 选择路径  `
#### 3. 选择芯片型号，**STM32F103RE**
 
![17.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6d677404ec2f4dc6940d0a584c044f4e~tplv-k3u1fbpfcp-watermark.image)  
`图 3-4 选择芯片型号  `
#### 4. 这一步是**软件中自带**的配置【右侧文件】，我们不需要，点击Cancel
 
![18.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/707b37f3e53e464f8c7472fb156ce93e~tplv-k3u1fbpfcp-watermark.image)  
`图 3-5 在线配置环境  `
#### 5. 之后点击品字形按钮，让软件本身获取【右侧文件】的结构
 
![19.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/227abc3cf7b64d81a250b65aa872d7ac~tplv-k3u1fbpfcp-watermark.image)  
`图 3-6 按钮在界面中的位置  `
#### 6. 通过操作【New】和【Delete】完成中间边框的编写，之后点击【Add Files…】
 
![20.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/408b4ae2912c4c3e9b6f97f73954aa2c~tplv-k3u1fbpfcp-watermark.image)  
`图 3-7 项目管理界面  `
#### 7. 找到CMSIS-Src，选择全部文件，点击【Add】后，点击【Close】
 
![21.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/064a48cdea1a43788eaaf061fe685eaa~tplv-k3u1fbpfcp-watermark.image)  
`图 3-8 选择添加文件  `
#### 8. 重复步骤（6），创建，之后点击【Add Files…】  
#### 9. 找到目标源文件，选择全部文件，点击【Add】后，点击【Close】  
#### 10. 循环执行**步骤（8）**与**步骤（9）**，最终添加效果如图  
 
![22.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b9831e18edd2415a9f951c92ebce71ed~tplv-k3u1fbpfcp-watermark.image)  
`图 3-9 最终添加效果1  `
 
![23.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0ce15e1425a743629f1dfa21b022161c~tplv-k3u1fbpfcp-watermark.image)  
`图 3-10最终添加效果2  `
 
![24.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5b34cdf5572c4cb7961408d9824e6571~tplv-k3u1fbpfcp-watermark.image)  
`图 3-11最终添加效果3  `
 
![25.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a8ff890656c14494a536083a5a2a785c~tplv-k3u1fbpfcp-watermark.image)  
`图 3-12最终添加效果4  `
#### 11. 点击【OK】，可以在左侧边框看到工程模板文件已经添加完毕，接下来开始**配置工程参数**。
 
![26.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bdf72ba7792e458299f8385563c19705~tplv-k3u1fbpfcp-watermark.image)  
`图 3-13最终添加效果5  `

