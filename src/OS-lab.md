### 一、基于信号量与P/V操作同步机制的读者/写者问题的设计与实现

#### 功能要求：

+ 设置读者、写者个数（界面）；读者、写者进程的动态创建；
+ 用信号量与P/V操作同步机制实现写者优先的读者/写者问题。
+ 运行结果（界面）
  + 动态显示多个读者或一个写者的读/写操作互斥状态：显示正在进行读或写操作的进程，以及等待读或写操作的进程；
  + 动态显示写者优先的过程：即当有读者进程在读时，有写者进程要求写操作，后续的读者进程应该等待；并且当读操作的读者进程结束读操作后，等待的写者进程应能立即进行写操作。

#### 实现方式：

+ 在原来用信号量与P/V操作实现的读者/写者问题解决方法基础上进行修改，反应写者优先的原则；
+ 进行读/写操作的进程可通过随机延时的方式来模拟读/写操作的时间过程。





### 二、报告

#### 1、系统总体功能

1）系统运行后，显示设置界面，设置本次实验的读者和写者个数；

2）确定后系统每隔一定时间随机生成一个的读者和写者线程，让其运行，尝试进入工作区；

3）每个线程拥有自己的名字和类型，在界面上有两个区域，一个是工作区，一个是阻塞区

4）线程状态改变，即从阻塞区进入工作区，或者从工作区离开，或者尝试进入工作区不符合准入标准后加入阻塞区，以		动画形式显示其位置变化；

5）阻塞区有两个队列，一个是读者线程，一个是写者线程，实时更新其数量；

6）工作区有一个队列，显示正在工作的所有线程；

7）阻塞区进入工作区之间显示一道门，实时更新准入的线程类型；

8）界面实时显示已生成的读者和写者数，并显示已经工作完成的读者和写者，以及正在工作的线程数，阻塞的线程数；

8）达到设置的读者和写者线程数并且阻塞区和工作区都为空显示结束，统计本次实验耗时。





#### 2、功能模块划分及组内分工

根据所选题目的功能要求设计系统分为几个功能模块，给出系统总体功能模块构成图

对各子模块详细论述其处理功能，同时本部分应写明各模块由哪位组内同学负责开发

#### 3、功能处理流程

按流程图绘制的规范要求，给出各模块的功能流程图，同时应包括模块间功能处理流程

####   4、系统详细设计  

给出详细的数据结构定义，并对定义的数据结构含义加以解释

按流程图绘制的规范要求，给出各模块的处理流程图，处理流程图中，要求尽可能用定义的数据结构来描述       

#### 5、系统运行结果及总结

给出系统运行的结果截图，应给出具有代表意义的结果截图，截图数量不应超过3张

最后给出系统设计及实现的总结，包括系统的优缺点及是否还有可进一步进行开发的地方



>  功能流程最细写到函数调用即可，处理流程指函数内的实现过程。
>
> 如项目功能较多，涉及到的函数处理流程会比较多，此时无需把所有处理流程都写出来，只写那些核心重要的处理流程即可。

>  报告里不要贴代码，应使用文字描述、类图、顺序图、流程图等进行说明。



### 三、分工

#### 1、读者模块

+ 读出
+ 离开
+ 功能流程图
+ 处理流程图



#### 2、写者模块

+ 写入
+ 离开
+ 功能流程图
+ 处理流程图



#### 3、主函数模块

+ 信号量定义
+ 数据结构定义
+ 线程创建
+ 功能流程图
+ 模块间功能处理流程











> 1、2、3模块：
>
> + 简要描述，无需代码实现，设置函数表达想要完成的事情即可
>
> + 每个模块开发无需理会与其它模块信号量和数据结构差异问题，之后代码实现再整合





#### 4、代码实现Debug

+ 选择一门语言根据每个模块描述进行代码实现
+ Debug，调整每个模块的差异和不足
+ 结果截图



#### 5、报告

独立编写各自负责部分



### 四、参考资料

>读者-写者问题

- 「读-读」允许：同一时刻，允许多个读者同时读
- 「读-写」互斥：没有写者时读者才能读，没有读者时写者才能写
- 「写-写」互斥：没有其他写者时，写者才能写

**方案一：读者优先**

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220901163917919.png" alt="image-20220901163917919" style="zoom:80%;" />

**方案二：写者优先**

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220901164121580.png" alt="image-20220901164121580" style="zoom:80%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220901164151295.png" alt="image-20220901164151295" style="zoom:80%;" />

这里 `rMutex` 的作用，开始有多个读者读数据，它们全部进入读者队列，此时来了一个写者，执行了 `P(rMutex)` 之后，后续的读者由于阻塞在 `rMutex` 上，都不能再进入读者队列，而写者到来，则可以全部进入写者队列，因此保证了写者优先。

同时，第一个写者执行了 `P(rMutex)` 之后，也不能马上开始写，必须等到所有进入读者队列的读者都执行完读操作，通过 `V(wDataMutex)` 唤醒写者的写操作。