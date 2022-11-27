# 1、UML结构

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820090241812.png" alt="image-20220820090241812" style="zoom:67%;" />

## 物件

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820091123692.png" alt="image-20220820091123692" style="zoom:67%;" />



## 关系

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820092222412.png" alt="image-20220820092222412" style="zoom:67%;" />



## 关联

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820092443835.png" alt="image-20220820092443835" style="zoom:67%;" />

## 图

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820092755050.png" alt="image-20220820092755050" style="zoom: 67%;" />



## 公共机制

包括规格说明、修饰、通用划分、扩展机制(构造型、标记值、约束)
（1）**构造型**：预定义的<< include >>、<< extend >>；用户自定义以大写字母开头
（2）**标记值**：形式为”名称=值”
（3）**约束**：形式为{约束的内容}



## 架构

逻辑视图(Logical View)：展示对象和类如何组成系统，实现系统行为

实现视图(Implementation View)：说明代码的结构

进程视图(Process View)：说明系统中并发执行和同步情况

部署视图(Logical View)：定义硬件结点的物理结构

用例视图(Use Case View)：描述干系人需求




# 2、软件模型

![image-20220820091004396](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820091004396.png)

![image-20220820091024815](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820091024815.png)



# 3、用例图(重点)



## 组成元素

![image-20220820095620147](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820095620147.png)



## 识别方法(查找事件)

主语(参与者)+动词(使用系统)+宾语(达到目标)

## 要点分析

用例是系统产生的结果值(实现的目标)
用例必须由目标系统实现
用例的提出和定义都从参与者角度考虑

## 关系

参与者与参与者：泛化
用例和参与者：关联(一对一通信，一条实线，箭头可有可无)
用例和用例：包含、泛化、扩展



## 拓展与包含

![image-20220820095936330](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820095936330.png)









# 4、类图(重点)

## 抽象层次

![image-20220820100154562](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820100154562.png)



## 组成元素

一个矩形，内含类名(Name)、属性(Attribute)、操作(Operation)。
（1）类名：首字母大写；由多个字母组成时需要合并，第二个单词首字母大写
（2）属性：语法为[可见性] 属性名称 [:属性类型] [=初始值] [{特征}]，其中可见性包括公有类型(+)(public)、受保护类型(#)(protected)、私有类型(-)(private)，单字属性名小写；多个单词，除第一个单词外其余单词首字母要大写
（3）操作：语法为[可见性] 操作名称 [(参数表)] [:返回类型] [{特征}]



## 类的种类

（1）实体类(Enity)：用于对必须存储的信息和相关行为建模的类。最终可能映射成数据库中的表以及字段，图例为

![image-20220820100528689](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820100528689.png)

（2）控制类(Control)：负责协调其他类的工作，本身并不完成任何功能，图例为

![image-20220820100541044](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820100541044.png)

（3）边界类(Boundary)：位于系统最上层，包含所有窗体、报表等硬件接口以及其他系统的接口，图例为

![image-20220820100550364](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820100550364.png)



## 关系

![image-20220820100620758](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820100620758.png)



## 构建步骤

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820100742310.png" alt="image-20220820100742310" style="zoom:67%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820100754920.png" alt="image-20220820100754920" style="zoom:67%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820100817639.png" alt="image-20220820100817639" style="zoom:67%;" />![image-20220820100838113](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820100838113.png)

![image-20220820100838113](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820100838113.png)





# 5、活动图(重点)

## 组成元素

**活动和动作**

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820124135509.png" alt="image-20220820124135509" style="zoom:67%;" />

**活动边**

+ 控制流

+ 对象流

  <img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820124224709.png" alt="image-20220820124224709" style="zoom:67%;" />

**活动结点**

+ 参数结点

+ 对象结点

+ 控制结点

  ![image-20220820124414836](C:/Users/ning chuang tao/AppData/Roaming/Typora/typora-user-images/image-20220820124414836.png)

**泳道**





## 案例

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820124737439.png" alt="image-20220820124737439" style="zoom:67%;" />





# 6、顺序图(重点)

## 组成元素

+ 参与者

+ 生命线

+ 活动条

+ 消息

  <img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820130320518.png" alt="image-20220820130320518" style="zoom:67%;" />

+ 交互框

  + alt：选择
  + opt：可选
  + loop：循环
  + par：并行



## 绘制顺序图

+ 用例分析
+ 识别对象
+ 绘制类图
+ 绘制顺序图

![image-20220820130848545](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820130848545.png)



# 7、组件图

## 组成元素

+ 供接口
+ 需接口
+ 关系
+ 实现类



## 样例

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820133447635.png" alt="image-20220820133447635" style="zoom:50%;" />



# 8、对象图

![image-20220820132039086](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820132039086.png)



# 9、部署图

## 组成元素

+ 制品：文件
+ 结点：执行制品的实体
  + 执行环境结点
  + 设备结点
+ 通信路径：表示结点间通信



## 样例

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820135839648.png" alt="image-20220820135839648" style="zoom:67%;" />![image-20220820135853291](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820135853291.png)

![image-20220820135853291](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820135853291.png)









# 10、状态图

## 组成元素

+ 状态

  + 状态种类

    <img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820140754989.png" alt="image-20220820140754989" style="zoom:67%;" />

  + 状态内部活动

  <img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820140912591.png" alt="image-20220820140912591" style="zoom:67%;" />

+ 迁移

  + 引发迁移的事件

  <img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820141049197.png" alt="image-20220820141049197" style="zoom:67%;" />

  + 文字标签

  <img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820141249225.png" alt="image-20220820141249225" style="zoom:67%;" />

  <img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820141234516.png" alt="image-20220820141234516" style="zoom:67%;" />

## 样例

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820141356609.png" alt="image-20220820141356609" style="zoom:67%;" />





# 11、通信图

## 通信图和顺序图

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820141535320.png" alt="image-20220820141535320" style="zoom:50%;" />



## 组成元素

+ 参与者
+ 链接
+ 信息

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820141640774.png" alt="image-20220820141640774" style="zoom:67%;" />



## 样例

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820144414284.png" alt="image-20220820144414284" style="zoom:67%;" />



# 13、需求分析过程

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820144755723.png" alt="image-20220820144755723" style="zoom:67%;" />





# 14、定义需求



#### 步骤

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820145224650.png" alt="image-20220820145224650" style="zoom: 67%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820145245969.png" alt="image-20220820145245969" style="zoom: 67%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820145311058.png" alt="image-20220820145311058" style="zoom:67%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820145334589.png" alt="image-20220820145334589" style="zoom:67%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820145351731.png" alt="image-20220820145351731" style="zoom:67%;" />







#### 用例描述

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820145557912.png" alt="image-20220820145557912" style="zoom:67%;" />



# 15、用例分析

#### BCE三层架构

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820151457841.png" alt="image-20220820151457841" style="zoom:67%;" />



#### 分析步骤

+ 分析交互：顺序图
+ 类图：VOPC

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820151634696.png" alt="image-20220820151634696" style="zoom:67%;" />



#### VOPC

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820151835678.png" alt="image-20220820151835678" style="zoom:67%;" />![image-20220820151902521](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820151902521.png)

![image-20220820151902521](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220820151902521.png)



# 16、试题4份

[点击跳转](https://blog.csdn.net/weixin_43973415/article/details/110397797?spm=1001.2101.3001.6650.14&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-14-110397797-blog-112176460.t0_eslanding_v1&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-14-110397797-blog-112176460.t0_eslanding_v1&utm_relevant_index=20)



