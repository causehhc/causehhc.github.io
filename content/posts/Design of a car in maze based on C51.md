+++
author = "causehhc"
title = "【迷宫小车】基于51单片机的迷宫电脑鼠的设计"
date = "2020-01-02"
description = ""
tags = ["C51"]
categories = ["嵌入式"]
+++

# 一、简介
课设题目，这里简单总结一下思路。
# 二、功能分析
1. 任意穿行 **（寻找终点）**  
实现对步进电机的控制，包括正转反转及调速；对电机动作组进行封装，实现各种标准动作（前进一格，左转，右转，掉头，修正）。
2. 记录信息 **（遍历迷宫）**  
通过建立8*8迷宫数组，在低四位以绝对方向记录墙的信息，高四位记录来的方向。以便后期分析和回溯。
3. 最短路径 **（BFS算法）**  
利用广度优先算法计算等高表，从终点反向查找最短路径。
# 三、硬件驱动程序
## 1、数码管驱动
```
//数码管驱动程序
sbit tube1 = P4^3;
sbit tube2 = P4^2;
uchar table[]={0xc0,0xf9,0xa4,0xb0,0x99,0x92,0x82,0xf8,0x80,0x90,  0x88,0x83,0xc6,0xa1,0x86,0x8e,0x00};
void view1(uchar num){
	tube1=1;tube2=0;P0=table[num];
}
void view2(uchar num){
	tube1=0;tube2=1;P0=table[num];
}
```
## 2、红外传感器组驱动
```
//红外传感器组驱动程序
sbit A0 = P4^0;
sbit A1 = P2^0;
sbit A2 = P2^7;
sbit irR1 = P2^1;
sbit irR2 = P2^2;
sbit irR3 = P2^3;
sbit irR4 = P2^4;
sbit irR5 = P2^5;
bit irC=0, irL=0, irR=0, irLU=0, irRU=0;
void init_tim2(uint us){  // 初始化TIM2
	EA = 1;
	ET2 = 1;
	TH2 = RCAP2H = (65536-us)/256;
	TL2 = RCAP2L = (65536-us)%256;
	TR2 = 1;
}
void ir_on(uchar num){  // 开启第num-1号红外发射级
	A0 = (num)&0x01;
	A1 = (num)&0x02;
	A2 = (num)&0x04;
}
uchar get_ir(uchar num){
  if(num==1) return irL;
  else if(num==2) return irLU;
  else if(num==3) return irC;
  else if(num==4) return irRU;
  else if(num==5) return irR;
  else return 0;
}
void ir_test(){ // 测试某个方向的红外
	while(1){
		if(irL||irR||irC){
			beep = 0;
		}else{
			beep = 1;
		}
	}
}
void tim2() interrupt 5{ // TIM2中断服务函数
	static bit ir = 0;
	static unsigned char n=1;
	TF2 = 0;
	if(!ir)	{
		ir_on(n-1);
	}else{
		if(n==1){
			if(irR1)	irC=0;
			else			irC=1;
		}else if(n==2){
			if(irR2)	irLU=0;
			else			irLU=1;
		}else if(n==3){
			if(irR3)	irL=0;
			else			irL=1;
		}else if(n==4){
			if(irR4)	irR=0;
			else			irR=1;
		}else if(n==5){
			if(irR5)	irRU=0;
			else			irRU=1;
		}
	}
	if(ir)	n++;
	if(n>5)	n=1;
	ir=~ir;
}
```
## 3、步进电机驱动
```
//步进电机驱动程序
void write_pin(uchar temp){
  P1 = temp;
}
void delay(uint z){  // 延时函数
	uchar i, j;
	while(--z){
		_nop_();
		i=2;
		j=199;
		do{
			while(--j);
		}while(--i);
	}
}
uchar forward[]={0x11,0x33,0x22,0x66,0x44,0xcc,0x88,0x99};
uchar reverse[]={0x11,0x99,0x88,0xcc,0x44,0x66,0x22,0x33};
uchar for_rev[]={0x11,0x93,0x82,0xc6,0x44,0x6c,0x28,0x39};
static uint step = 0;
uchar fix_path(uchar i){  // 路线修正
    step = 0;
    while((get_ir(2)||get_ir(4)) && !get_ir(3)){
        if(get_ir(2))	write_pin(forward[i++] | 0xf0);
        else if(get_ir(4))	write_pin(reverse[i++] | 0x0f);
        if(i==8)	i=0;
        step++;
        delay(3);
    }
    return i;
}
void go_rel(uchar relD){  // 向某个相对方向前进一格
    uchar num;
    uchar i, j;
    if(relD == 0) num = 104;
    if(relD == 1||relD == 3) num = 48;
    if(relD == 2) num = 100;

    for(j=0;j<num;j++){
        for(i=0;i<8;i++){
            if(relD == 1||relD == 2)	write_pin(forward[i]);
            if(relD == 3)	write_pin(reverse[i]);
            if(relD == 0){
                write_pin(for_rev[i]);
                i = fix_path(i);
            }
            if(relD==0)	num -= step/16;
            delay(3);
        }
    }
}
```


# 四、算法分析
## 1、遍历迷宫
遍历迷宫（回溯法）的一般流程如下：
![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a8f1ae59c83b442b8c5814f970241a60~tplv-k3u1fbpfcp-zoom-1.image)

### 1.1、定义绝对方向和相对方向
```
//算法部分
/** 绝对方向（以迷宫左上为参照）：       相对方向：
  *     0           											0:直行
  * 3       1       											1:右转
  *     2           											2:掉头
  *                 											3:左转
**/
/** 工具函数集（0）：一些必要的初始化及通用函数
	* 	初始化数组
	*		高四位记录来的方
	*		低四位依据绝对方向记录记录四周信息（默认来的方向无墙）
**/
void init_map(uchar map[SIZE][SIZE]){  // 初始化数组
  uchar i, j;
  for(i=0;i<SIZE;i++){
    for(j=0;j<SIZE;j++){
      map[i][j] = 0xff;
    }
  }
}
uchar abs_to_rel(uchar absD, uchar absD_t){  // 绝对方向->相对方向
	//absD：当前绝对方向
	//absD_t：期望转换到哪个绝对方向
  uchar relD = 0;
  relD = (absD_t - absD) % 4;
  if(relD > 127)	relD += 4;
	//返回相对方向
  return relD;
}
```

### 1.2、记录当前坐标的迷宫信息
当前坐标为初始值时（说明没走过）记录信息，高四位记录来的方向，低四位依据绝对方向记录四周信息。由于在整个行进过程中（遍历阶段）迷宫数组的值只会被写入一次，这保证了在高四位中始终记录的是最开始时小车进入的方向。依此特点，小车可以在回溯或冲刺时有效安全地使用这些信息。
```
/** 关键函数集（1）：记录信息
	* 	当前坐标为初始值时，记录信息：
	*			高四位记录来的方向
	*			低四位依据绝对方向记录记录四周信息（默认来的方向无墙）
**/
uchar read_ir(uchar relD){ // 读取相对方向的红外值
    if(relD == 0) return get_ir(3);  // 前红外
    if(relD == 1) return get_ir(5);  // 右红外
    if(relD == 3) return get_ir(1);  // 左红外
    return 0;
}
void collect_info(uchar maze[SIZE][SIZE], xyTypeDef now, uchar absD){ // 记录信息
  if(maze[now.y][now.x] == 0xff){  // 如果当前坐标未被写入，则写入（整个过程中低四位只写入一次）
    uchar val_wall = 0xf0;
    uchar k = 0;
    uchar i;
    for(i=0; i<4; i++){  // 循环判断4个绝对方向
      k = read_ir(abs_to_rel(absD, i));
      val_wall |= (k<<i);
    }
    maze[now.y][now.x] &= val_wall;  // 将墙的信息写入低四位
    maze[now.y][now.x] &= ((absD<<4)|0x0f);  // 将来的方向写入高四位
  }
}
```
### 1.3、选择一个方向
更具记录的迷宫数组信息，选择一个合适的方向。扫描四轴，如果存在没走过的格子，直接返回这个方向。如果四周都走过，那么读取本坐标的高四位来的方向，选择回溯方向。
```
/** 关键函数集（2）：选择方向
	* 	根据记录的迷宫数组信息，选择合适的方向
	*			扫描四周，如果存在没走过的格子，直接走之
	*			如果四周都走过，那么读取高四位来的方向，进行回溯
**/
uchar is_path(uchar maze[SIZE][SIZE], xyTypeDef now, uchar absD){ // 判断此方向是否连通
  return !((maze[now.y][now.x]>>absD)&0x01);
}
uchar is_new(uchar maze[SIZE][SIZE], xyTypeDef now, uchar absD){  // 判断此方向是否为新格子
  if(absD==0) return (maze[now.y-1][now.x]>>4)==0x0f;
  if(absD==1) return (maze[now.y][now.x+1]>>4)==0x0f;
  if(absD==2) return (maze[now.y+1][now.x]>>4)==0x0f;
  if(absD==3) return (maze[now.y][now.x-1]>>4)==0x0f;
  return 0;
}
uchar search_dir(uchar maze[SIZE][SIZE], xyTypeDef now, uchar flag){ // 选择方向
  uchar i;
  uchar pre = maze[now.y][now.x] >> 4;
  uchar back;
  if(!flag){ // 冲刺标记位，如果不冲刺，则需要扫描四个方向是否可走
    for(i=0; i<4; i++){
      if(is_path(maze, now, i) && is_new(maze, now, i)){ // 判断该方向是否有墙和是否走过
        return i;
      }
    }
  }
	// 如果冲刺或者四个方向不可走，直接读取高四位进行冲刺引导或回溯
  if(pre<=1) back = pre+2;
  if(pre>=2) back = pre-2;
  return back;
}
```

### 1.4、向这个方向前进
这方面没啥说的，动起来就对了。
```
/** 关键函数集（3）：执行
	* 	根据上一步得到的信息，执行
**/
void go_to_next(xyTypeDef *now, uchar *absD, uchar absD_t){
  uchar relD = abs_to_rel(*absD, absD_t);
  // 刷新当前坐标和当前绝对方向
	if(absD_t == 0) (now->y)--;
  if(absD_t == 1) (now->x)++;
  if(absD_t == 2) (now->y)++;
  if(absD_t == 3) (now->x)--;
  *absD = absD_t;
	// 执行
	go_rel(relD);
	if(relD != 0)	go_rel(0);
}
```

## 2、最短路径
### 2.1、建立等高表
等高表原理类似于等高线，在选定起点后，与其相连的点可以设置为该点高度值+1，利用广度优先算法遍历迷宫格，本迷宫的等高表数值为：
![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/18c902d37a60470b8fdfdeca534bfc88~tplv-k3u1fbpfcp-zoom-1.image)

将其可视化后，它的形状类似一座山。
![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7bf8a608c8c646d6aea90e8a63012c93~tplv-k3u1fbpfcp-zoom-1.image)

### 2.2、反向查找
既然等高表概念理解了，那么寻找最短路径就是从山顶扔下一颗球，由重力选择的路径即为最短路径。（在机器学习领域会有一个局部最优概念，这里不阐述）
```
/** 关键函数集（4）：寻找最优路径
	* 	回溯到起点后，开始根据广度优先算法寻找最优路径
				（1）首先根据之前记录的迷宫数组建立等高表
				（2）根据等高表反向记录最优路径
				（3）将路径记录到迷宫数组的高四位，这样可以复用之前的结构，将小车引导至终点
**/
bit bfs(uchar maze[SIZE][SIZE],xyTypeDef beg, xyTypeDef end){ // 广度优先算法
  //参数初始化
	uchar height[SIZE][SIZE] = {0xff};  // 初始化等高表
  xyTypeDef queue_xy[15]; // 初始化队列（经过测试，长度管够）
  uchar queue_len = 1;  // 初始化队列长度标记量
  xyTypeDef queue_head;  // 初始化队头
  uchar j, i;  // 初始化迭代下标
  xyTypeDef temp;  // 初始化一个临时变量
  xyTypeDef now;  // 初始化当前位置

  init_map(height);
  height[beg.y][beg.x] = 0;
  queue_xy[0].x = beg.x;
  queue_xy[0].y = beg.y;
  while(queue_len>0){  // 当队列不为空
    queue_head = queue_xy[0];  // 队首元素出队
    queue_len--;
    for(j=0; j<queue_len; j++){  // （由于用数组模拟队列，所以需要将所有元素手动前移）
      queue_xy[j] = queue_xy[j+1];
    }
    for(i=0; i<4; i++){  // 判断四个方向
      temp = queue_head;
      if(i == 0)  temp.y = queue_head.y-1;
      if(i == 1)  temp.x = queue_head.x+1;
      if(i == 2)  temp.y = queue_head.y+1;
      if(i == 3)  temp.x = queue_head.x-1;
      if(temp.x>127||temp.y>127)  continue;
      if(is_path(maze, queue_head, i) && height[temp.y][temp.x]==255){  // 如果该坐标连通且第一次访问
        height[temp.y][temp.x] = height[queue_head.y][queue_head.x] + 1;  // 该坐标等高表+1
        queue_xy[queue_len++] = temp;  // 该坐标入队
      }
    }
  }
	
	// 等高表建立完毕，开始反向查找最优路径
  now.x = end.x;
  now.y = end.y;
  while(!(now.x==0&& now.y==0)){  // 如果还没到起点
    for(i=0; i<4; i++){  // 扫描四个方向
      temp = now;
      if(i == 0)  temp.y = now.y-1;
      if(i == 1)  temp.x = now.x+1;
      if(i == 2)  temp.y = now.y+1;
      if(i == 3)  temp.x = now.x-1;
      if(temp.x>127||temp.y>127)  continue;
      if(is_path(maze, now, i) && (height[temp.y][temp.x]==height[now.y][now.x]-1)){  // 如果该坐标连通且高度递减
        maze[temp.y][temp.x] |= 0xf0; // 初始化迷宫数组该坐标的高四位
        maze[temp.y][temp.x] &= ((i<<4)|0x0f);  // 将这个方向写入高四位
        now = temp;  // 切换焦点
        break;
      }
    }
  }
  return 1;
}
```
# 五、 整体架构
```
int main() {
	uchar maze[SIZE][SIZE] = {0xff}; // 定义迷宫数组，整个迷宫的数据均存储在这里
  uchar absD = 1;	// 定义开始时的绝对方向
  uchar absD_t;		// 定义目标方向（暂无）
  bit flag = 0;		// 定义冲刺标记量

  xyTypeDef beg;	// 定义起点坐标
  xyTypeDef now;	// 定义当前坐标
  xyTypeDef end;	// 定义终点坐标
  beg.x = 0;
  beg.y = 0;
  now = beg;
  end.x = 7;
  end.y = 7;

  init_map(maze);
  init_tim2(5000);
	//ir_test();

  while(1){
    if(now.x==end.x&&now.y==end.y&&flag){ // 如果冲刺完毕，退出程序
      beep = 0; delay(100); beep = 1;
      break;
    }
    if(now.x==beg.x&&now.y==beg.y&&absD==3){  // 如果回溯到起点，开始冲刺
      beep = 0; delay(100); beep = 1;
      flag = bfs(maze, beg, end);
    }
		
    collect_info(maze, now, absD);				// （1）记录迷宫信息
    absD_t = search_dir(maze, now, flag);	// （2）确定下一个方向
    go_to_next(&now, &absD, absD_t);			// （3）执行该方向

    delay(1000);  // 延时，便于手扶
		view1(now.x); view2(now.y);  // 刷新数码管显示当前坐标
  }
  while(1);
}
```
