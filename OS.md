# 操作系统
## 概述
---
- 计算机系统的组成
    1. 硬件系统——构成计算机系统的实体
       1. 中央处理器——计算机系统的核心部件
       2. 存储器——存放程序和数据
       3. 外围设备
    2. 软件系统
       1. 系统软件
       2. 应用软件
- 操作系统的**定义**：计算机硬件系统上配置的第一个大型软件。
- 计算机系统的层次结构
  - 应用软件和用户
  - 其他系统软件
  - 操作系统
  - 硬件系统
  - 基本概念：
    - 层：层次结构是由若干个层组成的，层是具有独立功能的模块或部件
    - 接口：
    - 单项依赖：
    - 隐藏性：
- 操作系统是对硬件系统的第一次扩充，同时又作为其他软件运行和用户操作的基础——操作系统的定位和作用
- 多道程序设计与操作系统
  - 计算机系统工作流程有两种
    - 顺序执行：处理器在开始执行一道程序后，只有在这道程序执行结束，才能开启下一道程序
    - 并发执行：在内存中同时存放多道程序，处理器在开始执行一道程序的一条指令后，在这道程序完成之前，处理器可以开始执行另一道程序，甚至更多其他程序
  - 多道程序的并发执行可以提高系统的利用率
- 操作系统的特点

  操作系统的四个特征： 

  **并发性**：系统中同时存在多个运行的程序，有多个进程在同一时间间隔里运行 

  **共享性**：系统资源可被多个进程共同使用而并非被某个进程独占 

  **虚拟性**：把物理上的实体变为逻辑上的对应物的技术 

  **异步性**：进程的执行不是一贯到底，而是走走停停具有随机性 

- 操作系统的功能

  - 管理计算机系统的硬件和软件 

  - 控制计算机系统工作流程 

  - 为其他用户提提供安全、方便的运行操作环境 

  - 提高计算机系统的效率 

- 操作系统类型 

  操作系统的基本类型：批处理系统、分时系统、实时系统 

**批处理系统** 

批处理系统的概念： 

作业：用户要求计算机完成的一个任务

程序员和操作员：程序员编写程序，操作员维护管理计算机 

作业控制语言JCL和作业说明书：作业由**程序**、**数据**和**作业说明书**组成 

程序卡片和读卡机 

批处理系统的分类 

脱机批处理系统：提交→后备→执行→完成 

联机批处理系统：输入井→预输入程序→输出井→缓输出程序 

**特征：批量处理方便操作、自动执行系统利用率高、但缺少人机交互不便于调试程序** 

**分时/实时系统** 

分时系统（把处理器的工作时间分成一些很短的时间片）的特征： 

- **同时性**：多个用户同时使用系统 

- **独立性**：每个用户请求能独立地得到处理 

- **及时性**：用户在终端的请求可以很快得到主计算机的响应 

- **交互性**：通过用户与机器的相互协作决定下一步请求的操作 

实时系统（高及时性、高可靠性）：实时过程控制系统（工业军事等领域）、实时信息控制系统（证券 

银行等领域）

### PC问题
#### 简单PC问题：n=m=k=1
```
semaphore empty=1,full=0;
producer(){
  while(1){
    product=produce();
    p(empty);
    buf=product;
    v(empty);
  }
}
consumer(){
  while(1){
    p(full);
    product=buf;
    v(empty);
    consume(product);
  }
}
```
#### 一般PC问题：n=m=1，k>1，FIFO算法
```
semaphore empty=k,full=0;
int in=0,out=0;
producer(){
  while(1){
    product=produce();
    p(empty);
    buf[in]=product;
    in=(in+1)%k;
    v(empty);
  }
}
consumer(){
  while(1){
    p(full);
    product=buf[out];
    out=(out+1)%k;
    v(empty);
    consume(product);
  }
}
```
#### 复杂PC问题：n,m,k>1
```
semaphore empty=k,full=0,mutex1=mutex2=1;
int in=0,out=0
producer(){
  while(1){
    product=produce();
    p(empty);
    p(mutex1);
    buf[in]=product;
    in=(in+1)%k;
    v(mutex1);
    v(empty);
  }
}
consumer(){
while(1){
  p(full);
  p(mutex1);
  product=buf[out];
  out=(out+1)%k;
  v(mutex2);
  v(empty);
  consume(product);
}
}
```
#### 特殊PC问题
### 读者写者问题
#### 方案一：一个ws信号
```
semaphore
writer(){
  p(ws);
  write();
  v(ws);
}
reader(){
  p(ws);
  read();
  v(ws);
}
```
#### 方案一：一个ws信号,一个mutex信号
```
semaphore
writer(){
  p(ws);
  write();
  v(ws);
}
reader(){
  p(mutex);
  readcount++;
  if(readcount==1)p(ws);
  v(mutex)
  read();
  p(mutex);
  readcount--;
  if(readcount==0)v(ws); 
  p(mutex); 
}
```
#### 方案一：一个ws信号，两个mutex信号
```
semaphore
writer(){
  p(mutex0);
  p(ws);
  write();
  v(ws);
  v(mutex0);
}
reader(){
  p(mutex0);
  v(mutex0);
  p(mutex);
  readcount++;
  if(readcount==1)p(ws);
  v(mutex)
  read();
  p(mutex);
  readcount--;
  if(readcount==0)v(ws); 
  p(mutex); 
}
```
