一、Git概述

Git是一个免费的、开源的 分布式版本控制系统 ，可以快速高效地处理从小型到大型的各种
项目 。
Git易于学习，占地面积小，性能 极快 。 它具有廉价的本地 库，方便的暂存区域和多个工作
流分支 等特性。 其性能优于 Subversion、CVS、Perforce和 ClearCase等版本控制 工具。

## 1、何为版本控制

版本控制是一种记录文件内容变化，以便将来查阅特定版本修订情况的系统

版本控制其实最重要的是可以记录文件修改历史记录，从而让用户能够查看历史版本，
方便版本切换

![image-20220710184253416](C:/Users/ning chuang tao/AppData/Roaming/Typora/typora-user-images/image-20220710184253416.png)



## 2、为什么需要版本控制

个人开发过渡到团队协作

![image-20220710184523700](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710184523700.png)



## 3、版本控制工具

### ①、集中式版本控制工具

CVS、SVN(Subversion)、VSS……

集中化的版本控制系统诸如 CVS、SVN 等，都有一个单一的集中管理的服务器，保存
所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或
者提交更新。多年以来，这已成为版本控制系统的标准做法。

这种做法带来了许多好处，每个人都可以在一定程度上看到项目中的其他人正在做些什
么。而管理员也可以轻松掌控每个开发者的权限，并且管理一个集中化的版本控制系统，要
远比在各个客户端上维护本地数据库来得轻松容易。

> 事分两面，有好有坏。这么做显而易见的缺点是中央服务器的单点故障。如果服务器宕
> 机一小时，那么在这一小时内，谁都无法提交更新，也就无法协同工作。

![image-20220710184742650](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710184742650.png)

### ②、分布式版本控制工具

Git、 Mercurial、 Bazaar、 Darcs……

像Git这种分布式版本控制工具 ，客户端提取的不是最新版本的文件快照，而是把代码仓库完整地镜像下来（本地库 ）

这样任何一处协同工作用的 文件 发生故障，事后都可以用其他客户端的本地仓库进行恢复。因为每个客户端的每一次文件提取操作，实际上都是一次对整个文件仓库的完整备份 。

> 分布式的版本控制系统出现之后，解决了集中式版本控制系统的缺陷 :
>
> 1. 服务器 断网的情况下也可以进行开发 因为版本控制是在本地进行的
> 2. 每个客户端保存的也都是整个完整的项目 包含历史记录 更加安全

![image-20220710185013110](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710185013110.png)



## 4、Git 简史

![image-20220710185106840](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710185106840.png)



## 5、Git 工作机制

![image-20220710185138781](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710185138781.png)



## 6、Git 和代码托管中心

代码托管中心是基于网络服务器的远程代码仓库，一般我们简单称为远程库。

### ① 局域网

 GitLab

### ②、互联网

+ GitHub（外网）

+ Gitee 码云（国内网站）

  

# 二、Git安装

## 1、下载地址

https://git-scm.com/

## 2、安装步骤

### ①、同意协议

![image-20220710191923456](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710191923456.png)

### ②、选择配置

推荐默认设置，然后下一步。

![image-20220710192042116](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710192042116.png)

### ③、默认编辑器

建议使用默认的 Vim编辑器，然后点击下一步。

![image-20220710192136435](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710192136435.png)

### ④、分支名设置

![image-20220710192223611](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710192223611.png)

### ⑤、修改环境变量

修改Git的环境变量，选第一个，不修改环境变量，只在 Git Bash里使用 Git。

![image-20220710192305929](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710192305929.png)

### ⑥、SSH免密连接

默认

![image-20220710192522979](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710192522979.png)

### ⑦、客户端连接协议

选择后台客户端连接协议，选默认值OpenSSL，然后下一步。

![image-20220710192553511](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710192553511.png)

### ⑧、行末换行符

![image-20220710192653148](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710192653148.png)

### ⑨、终端类型

选择Git终端类型，选择默认的 Git Bash终端，然后继续下一步。

![image-20220710192716022](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710192716022.png)

### ⑩、pull合并模式

![image-20220710192740901](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710192740901.png)

### ①①、凭据管理器

![image-20220710192758527](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710192758527.png)

### ①②、其它配置

勾选符号连接

![image-20220710192830496](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710192830496.png)

### ①③、实验室功能

![image-20220710192905446](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710192905446.png)

### ①④、完成

![image-20220710193143016](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710193143016.png)

### ①⑤、测试

右键任意位置，在右键菜单里选择Git Bash Here即可打开 Git Bash命令行终端。

![image-20220710193216116](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710193216116.png)

在Git Bash终端里输入 git --version查看 git版本，如图所示，说明 Git安装成功。

![image-20220710193325276](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710193325276.png)

# 三、Git常用命令

| 命令名称                                  | 作用           |
| ----------------------------------------- | -------------- |
| git config --global user.name 用户名 设置 | 设置用户签名   |
| git config --global user.email 邮箱       | 设置用户签名   |
| git init                                  | 初始化本地库   |
| git status                                | 查看本地库状态 |
| git add 文件名                            | 添加到暂存区   |
| git commit m " 日志信息 " 文件名          | 提交到本地库   |
| git reflog                                | 查看历史记录   |
| git reset hard 版本号                     | 版本穿梭       |

## 1、设置用户签名

### ①、基本语法

git config --global user.name 用户名
git config --global user.email 邮箱

### ②、案例实操

全局范围的签名设置：

查看全局信息：

git config --global --list

![image-20220710193953908](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710193953908.png)

> 说明：
> 签名的作用是区分不同操作者身份。
>
> 用户的签名信息在每一个版本的提交信息中能够看到，以此确认本次提交是谁做的。 Git首次安装必须设置一下用户签名，否则无法提交代码。
>
> 注意：
> 这里设置用户签名和将来登录 GitHub（或其他代码托管中心）的账号没有任何关系。

## 2、初始化本地库

### ①、基本语法

git init

### ②、案例实操

#### Ⅰ、新建工作区间

![image-20220710194937175](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710194937175.png)

#### Ⅱ、在工作区间进入git-bash

![image-20220710195024453](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710195024453.png)

#### Ⅲ、输出初始化命令

git init

![image-20220710195121672](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710195121672.png)

### ③、查看结果

查看工作区间下的隐藏项目

![image-20220710195153636](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710195153636.png)

## 3、查看本地库状态

### ①、基本语法

git status

### ②、案例实操

#### Ⅰ、首次查看

没有任何内容

![image-20220710195500019](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710195500019.png)

#### Ⅱ、新增文件

命令：

vim hello.txt

![image-20220710195728582](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710195728582.png)

#### Ⅳ、再次查看

检测到未追踪的文件

![image-20220710195802356](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710195802356.png)

## 4、添加 暂存区

### ①、将工作区的文件添加到暂存区

#### Ⅰ、基本语法

git add 文件名

#### Ⅱ、实操

![image-20220710200121960](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710200121960.png)

### ②、查看状态

![image-20220710200150607](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710200150607.png)

## 5、提交本地库

### ①、将暂存区的 文件 提交到本地库

#### Ⅰ、基本语法

git commit -m "日志信息 " 文件名

#### Ⅱ、实操

![image-20220710200339067](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710200339067.png)

### ②、查看状态

![image-20220710200413563](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710200413563.png)

## 6、修改文件

### ①、修改

![image-20220710200606794](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710200606794.png)

### ②、查看状态

![image-20220710200633573](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710200633573.png)

### ③、添加暂存区

![image-20220710200656709](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710200656709.png)

### ④、查看状态

![image-20220710200741993](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710200741993.png)

### ⑤、提交本地库

![image-20220710200854212](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710200854212.png)

### ⑥、查看状态

![image-20220710200908373](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710200908373.png)

## 7、历史版本

### ①、查看历史版本

#### Ⅰ、基本语法

git reflog 查看版本信息

git log 查看版本详细信息

#### Ⅱ、实操

![image-20220710201749296](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710201749296.png)

### ②、版本穿梭

#### Ⅰ、基本语法

git reset --hard 版本号

#### Ⅱ、操作

##### [1]、查看历史记录

git reflog

![image-20220710201156116](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710201156116.png)

##### [2]、切换版本

git reset --hard 版本号

![image-20220710201317108](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710201317108.png)

##### [3]、查看历史记录

![image-20220710201359096](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710201359096.png)

##### [4]、查看文件内容

文件内容发生改变，回到最初版本

![image-20220710201438476](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710201438476.png)



### ③、原理

Git 切换版本，底层其实是移动的HEAD 指针，具体原理如下图所示

![image-20220710201840757](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220710201840757.png)

# 四、Git分支操作

## 1、什么是分支

在版本控制过程中，同时推进多个任务，为每个任务，我们就可以创建每个任务的单独
分支。使用分支意味着程序员可以把自己的工作从开发主线上分离开来，开发自己分支的时
候，不会影响主线分支的运行。对于初学者而言，分支可以简单理解为副本，一个分支就是
一个单独的副本。（分支底层其实也是指针的引用）

![image-20220712095604693](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712095604693.png)

## 2、分支 的好处

同时并行推进多个功能开发，提高开发效率。

各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。失败
的分支删除重新开始即可。

## 3、分支 的操作

| 命令名称            | 作用                         |
| ------------------- | ---------------------------- |
| git branch 分支名   | 创建分支                     |
| git checkout 分支名 | 切换分支                     |
| git merge 分支名    | 把指定的分支合并到当前分支上 |
| git branch -v       | 查看分支                     |

### ①、创建分支

#### Ⅰ、语法

git branch 分支名

#### Ⅱ、实操

![image-20220712100307105](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712100307105.png)

### ②、查看分支

#### Ⅰ、语法

git branch -v

#### Ⅱ、实操

![image-20220712100155473](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712100155473.png)



### ③、切换分支

#### Ⅰ、语法

git checkout 分支名

#### Ⅱ、实操

![image-20220712105333268](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712105333268.png)



### ④、合并分支

#### Ⅰ、语法

git merge 分支名

#### Ⅱ、实操

##### [1]、无冲突合并

当两个分支只有一个分支进行了修改

或者分别修改的位置不一样的时候，git会自动进行合并

##### [2]、冲突合并

合并分支时，两个分支在同一个文件的同一个位置 有两套完全不同的修改。

 Git无法替我们决定使用哪一个。

必须人为修改

###### (1)、查看当前分支文件

![image-20220712105621737](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712105621737.png)

###### (2)、修改当前分支文件

![image-20220712105702922](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712105702922.png)

![image-20220712105719364](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712105719364.png)

![image-20220712105747315](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712105747315.png)

###### (3)、切换分支

![image-20220712105816956](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712105816956.png)

###### (4)、查看当前分支文件

![image-20220712105835436](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712105835436.png)

###### (5)、修改当前分支文件

![image-20220712105926633](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712105926633.png)

![image-20220712105943655](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712105943655.png)

###### (6)、合并分支，产生冲突

冲突标志：![image-20220712110101760](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712110101760.png)

![image-20220712110012346](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712110012346.png)

###### (7)、查看冲突后文件

加入了一些特殊字符，表示文件冲突的内容

![image-20220712110038968](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712110038968.png)

###### (8)、查看冲突后状态

![image-20220712110212966](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712110212966.png)

###### (8)、解决冲突

手动编辑文件，删除特殊字符，编辑想要合并的内容

![image-20220712110435825](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712110435825.png)

![image-20220712110529588](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712110529588.png)

## 4、分支示意图

master、hot-fix 其实都是指向具体版本记录的指针

当前所在的分支，其实是由HEAD决定的。所以创建分支的本质就是多创建一个指针

所以切换分支的本质就是移动HEAD 指针

> HEAD 如果指向master，那么我们现在就在master 分支上
> HEAD 如果执行hotfix，那么我们现在就在hotfix 分支上

![image-20220712110603955](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712110603955.png)

# 五、Git团队协作机制

## 1、团队内协作

![image-20220712110733449](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712110733449.png)

## 2、跨团队协作

![image-20220712110812275](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712110812275.png)

# 六、GitHub

GitHub网 址： https://github.com/

![image-20220712111145730](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712111145730.png)

## 1、创建远程仓库

### ①、登录

![image-20220712111600886](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712111600886.png)

### ②、创建仓库

![image-20220712111646603](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712111646603.png)

### ③、设置信息

#### Ⅰ、设置名称

![image-20220712111807080](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712111807080.png)

#### Ⅱ、创建

![image-20220712111821942](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712111821942.png)

## 2、远程仓库操作

### ①、添加远程仓库

#### Ⅰ、复制远程仓库https地址

![image-20220712112452704](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712112452704.png)

#### Ⅱ、查看当前连接的远程仓库

git remote -v 查看当前所有远程地址别名

![image-20220712112702646](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712112702646.png)

#### Ⅲ、添加远程仓库并起别名

git remote add 别名 远程地址

![image-20220712112720901](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712112720901.png)

#### Ⅳ、查看仓库

![image-20220712112732216](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712112732216.png)

### ②、推送本地分支到远程仓库

#### Ⅰ、语法

git push 别名 分支

#### Ⅱ、实操

多试几次

![image-20220712113200860](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712113200860.png)

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712113218332.png" alt="image-20220712113218332" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712113247474.png" alt="image-20220712113247474" style="zoom:50%;" />

![image-20220712114017971](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712114017971.png)

#### Ⅲ、在远程仓库查看

![image-20220712113956614](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712113956614.png)

### ③、克隆远程仓库到本地

#### Ⅰ、语法

git clone 远程地址

#### Ⅱ、实操

##### [1]、新建仓库

![image-20220712114804235](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712114804235.png)

##### [2]、克隆仓库

![image-20220712114750214](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712114750214.png)

#### Ⅲ、查看本地文件

![image-20220712114705112](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712114705112.png)

### ④、邀请加入团队

#### Ⅰ、选择邀请合作者

![image-20220712115517032](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712115517032.png)

#### Ⅱ、填入想要合作的人

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712115628159.png" alt="image-20220712115628159" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712115646718.png" alt="image-20220712115646718" style="zoom:50%;" />

#### Ⅲ、发送地址

![image-20220712115728090](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712115728090.png)

复制地址并通过微信钉钉等方式发送给该用户，复制内容如下：

![image-20220712115812781](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712115812781.png)

https://github.com/502502502/git-demo/invitations

#### Ⅳ、接受邀请

受邀请账号中的地址栏复制收到邀请的链接

#### Ⅵ、成功

+ 成功之后团队成员可以在自己的账号看到加入的远程仓库
+ 成员可以修改内容并push到远程仓库
+ 团队内成员可以看到修改的人以及修改的内容

### ⑤、拉取远程库内容

#### Ⅰ、基本语法

git pull 远程库地址别名 远程分支名

#### Ⅱ、案例实操

##### [1]、远程库修改文件内容

![image-20220712120857715](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712120857715.png)

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712120915714.png" alt="image-20220712120915714" style="zoom:50%;" />

#### [2]、拉取远程库内容

![image-20220712121159144](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712121159144.png)

##### [3]、查看拉取内容

![image-20220712121224931](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712121224931.png)

## 3、跨团队协作

### ①、复制地址

将远程仓库的地址复制发给邀请跨团队协作的人。

### ②、点击连接

在受邀请的 GitHub账号里的地址栏复制收到的链接，然后点击 Fork将项目叉到自
己的本地仓库 。

### ③、成功

成功后可以看到当前仓库信息

### ④、修改

修改该远程库的文件并提交

### ⑤、pullRequest

发起请求

### ⑥、邀请者查看pullRequest

查看请求

#### ⑦、讨论组内讨论

进入到聊天室，可以讨论代码相关内容。

#### ⑧、合并修改

如果代码没有问题，可以点击 Merge pull reque

## 4、SSH免密登录

我们可以看到远程仓库中还有一个SSH的地址，因此我们也可以使用 SSH进行访问

![image-20220712122334605](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712122334605.png)

### ①、生成密钥

#### Ⅰ、进入家目录

![image-20220712122654999](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712122654999.png)

#### Ⅱ、删除 .ssh 目录

![image-20220712122706122](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712122706122.png)

#### Ⅲ、运行命令生成 ssh 秘钥目录

按三次回车确认

![image-20220712122724847](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712122724847.png)

#### Ⅳ、进入 .ssh 目录查看文件列表

![image-20220712122746089](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712122746089.png)

![](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712122800266.png)

#### Ⅴ、查看 id_rsa.pub 文件内容

![image-20220712122826326](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712122826326.png)

#### Ⅵ、复制id_rsa.pub文件内容

ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDKe1eoYQrtCyRdVJiXGOq1E+PW9JOXmCaRE5EZAhfZYq8a3MXsG7nsiTCf4sSKHQc8q/FwZUwNCZWZQpMKVAMJu4GEJCM7xA8ZlV8Pmw5Ytsh/X68V6WRDoF8hbCvDFmc23y087+6m3E58VxB1m/MldstlmcrMqXjzkKIKUDb5/znShH/QkuHPkfAKBDSmy6caTbhzydXG61v6v1Oc/i8ZJ34/9AKYow17A68WSZA9+eR0tG5A2iGcUNZfobfPUPzQD8BZ9gwCp8/qcywmIpc5U5QZaO4+Va75cdsCSjMPHrOvH9SVaukZINzydZ98p82CcKb+6ypOm8xr4NzQ2gbSZ2AgVYMrVsif9FHwHb6OGvs+P/omV6QREsQEX/doSKrQH62RK4oDvEwkxSvZ+2OvHMDuQcYiz0AyNNAMYNL3UchmTssCLST+NxnZQM1H1ESZ4KzJKmBsRQE5J5R170GQe+0WoWQn1pt4/ozAQHVaRAkBZ7XF7BBAGHrtIKhXx40= 502502502

### ②、登录GitHub

### ③、进入设置

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712122151929.png" alt="image-20220712122151929" style="zoom:50%;" />

### ④、添加成功

![image-20220712122214462](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712122214462.png)

### ⑤、本地push时可免密登录





# 七、IDEA集成Git

## 1、忽略文件

与项目的实际功能无关，不参与服务器上部署运行。

把它们忽略掉能够屏蔽 IDE工具之间的差异。

### ①、安装插件

![image-20220712154158815](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712154158815.png)

##### 

### ②、新建忽略文件

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712154303972.png" alt="image-20220712154303972" style="zoom:50%;" />

![image-20220712154339818](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712154339818.png)

##### 

### ③、编辑忽略文件

```xml
### Example user template template
### Example user template

# IntelliJ project files
*.class
# Log file
*.log
# BlueJ files
*.ctxt
# Mobile Tools for Java (
.mtj.
# Package Files
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar
# virtual machine crash logs, see
http://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*
.classpath
.project
.settings
target
.idea
*.iml
out
gen
```

##### 

## 2、定位Git

![image-20220712154855767](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712154855767.png)

点击test

![image-20220712155152704](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712155152704.png)

##### 

## 3、初始化本地库

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712155402247.png" alt="image-20220712155402247" style="zoom:50%;" />

### 

+ 选择工程，添加

+ 初始化之后

  <img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712155609448.png" alt="image-20220712155609448" style="zoom:50%;" />

## 4、添加到暂存区 

右键点击项目选择Git -> Add将项目

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712155852233.png" alt="image-20220712155852233" style="zoom:50%;" />

+ 添加之后

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712155921351.png" alt="image-20220712155921351" style="zoom:50%;" />

## 5、提交到本地库

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712160012310.png" alt="image-20220712160012310" style="zoom:50%;" />

##### 

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712160107224.png" alt="image-20220712160107224" style="zoom:50%;" />

+ 提交之后

  <img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712160219013.png" alt="image-20220712160219013" style="zoom:50%;" />

## 6、创建分支

### ①、位置

![image-20220712160654501](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712160654501.png)

### ②、新建分支

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712160733497.png" alt="image-20220712160733497" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712160753397.png" alt="image-20220712160753397" style="zoom:50%;" />

## 7、切换分支

### ①、位置

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712160922356.png" alt="image-20220712160922356" style="zoom:50%;" />

##### 

### ②、操作

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712160946548.png" alt="image-20220712160946548" style="zoom:50%;" />

##### 

### 

## 8、切换版本

### ①、位置

左下角Git->Log->相应版本右键

![image-20220712161301322](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712161301322.png)

##### 

### ②、选择Revision

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712161511709.png" alt="image-20220712161511709" style="zoom:50%;" />

## 9、合并分支

如果代码没有冲突，分支直接合并成功，分支合并成功以后，代码自动提交，无需手动提交本地库 。

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712161906678.png" alt="image-20220712161906678" style="zoom:50%;" />

##### 

## 10、解决冲突

### ①、点击Merge进行手动合并

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712162304031.png" alt="image-20220712162304031" style="zoom:50%;" />

### ②、选择合并内容

![image-20220712162421621](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712162421621.png)

### ③、点击x删除

### ④、点击>> 或者<< 合并

### ⑤、合并完成

![image-20220712162605526](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712162605526.png)

# 八、IDEA集成GitHub

## 1、设置 GitHub账号

### ①、账号密码登录

![image-20220712162808822](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712162808822.png)

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712162828289.png" alt="image-20220712162828289" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712162851246.png" alt="image-20220712162851246" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712162904569.png" alt="image-20220712162904569" style="zoom:50%;" />

### ②、口令登录

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712163000955.png" alt="image-20220712163000955" style="zoom:50%;" />

#### Ⅰ、登录GitHub，进入Setting

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712163116510.png" alt="image-20220712163116510" style="zoom:50%;" />

#### Ⅱ、点击Generate new token

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712163226479.png" alt="image-20220712163226479" style="zoom:50%;" />

#### Ⅲ、输入密码

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712163315193.png" alt="image-20220712163315193" style="zoom:50%;" />

#### Ⅳ、起名字

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712163356235.png" alt="image-20220712163356235" style="zoom:50%;" />

#### Ⅴ、全选权限，点击生成

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712163432556.png" alt="image-20220712163432556" style="zoom:50%;" />

#### Ⅵ、复制口令

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712163649757.png" alt="image-20220712163649757" style="zoom:50%;" />

#### Ⅶ、输入idea，登录

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712163731478.png" alt="image-20220712163731478" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712163747059.png" alt="image-20220712163747059" style="zoom:50%;" />

## 2、分享工程到GitHub

### ①、位置

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712163926620.png" alt="image-20220712163926620" style="zoom:50%;" />

### ②、设置仓库名称

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712180535009.png" alt="image-20220712180535009" style="zoom:50%;" />

### ③、查看远程仓库

![image-20220712164911901](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712164911901.png)



## 3、push推送本地库到远程库

### ①、位置

![image-20220712165012982](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712165012982.png)

### ②、操作

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712165039534.png" alt="image-20220712165039534" style="zoom:50%;" />

#### Ⅰ、为远程仓库地址起别名

若push不上，可尝试ssh免密

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712165132567.png" alt="image-20220712165132567" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712165306969.png" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712165220840.png" alt="image-20220712165220840" style="zoom:50%;" />

#### Ⅱ、点击Push

![image-20220712165529243](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712165529243.png)

#### Ⅲ、到远程库查看代码

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712181313699.png" alt="image-20220712181313699" style="zoom:50%;" />

> 注意：
> push是将本地库代码推送到远程库，如果本地库代码跟远程库代码版本不一致，
> push的操作是会被拒绝的。也就是说， 要想 push成功，一定要保证 本地 库的版本要比远程
> 库的版本高！ 因此一个成熟的程序员在动手改本地代码之前，一定会先检查下远程库跟本地
> 代码的区别！如果本地的代码版本已经落后，切记要先 pull拉取一下远程库的代码，将本地
> 代码更新到最新以后，然后再修改，提交，推送！

## 4、pull拉取远程库到本地库

### ①、位置

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712165824960.png" alt="image-20220712165824960" style="zoom:50%;" />

### ②、操作

#### Ⅰ、选择远程库库和分支

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712181515261.png" alt="image-20220712181515261" style="zoom:50%;" />

#### Ⅱ、查看代码

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712181555181.png" alt="image-20220712181555181" style="zoom:50%;" />

> 注意：
> pull是拉取远端仓库代码到本地，如果远程库代码和本地库代码不一致，会自动
> 合并，如果自动合并失败，还会涉及到手动解决冲突的问题。

## 5、clone克隆远程库到本地

### ①、位置

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712181635158.png" alt="image-20220712181635158" style="zoom:50%;" />

### ②、操作

#### Ⅰ、输入远程仓库位置或者直接选择Github远程仓库

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712181749783.png" alt="image-20220712181749783" style="zoom:50%;" />

#### Ⅱ、点击clone

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712181819235.png" alt="image-20220712181819235" style="zoom:50%;" />

#### Ⅲ、自动创建工程

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712182036347.png" alt="image-20220712182036347" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712182058721.png" alt="image-20220712182058721" style="zoom:50%;" />

# 九、Gitee

## 1、简介

众所周知，GitHub服务器在国外， 使用 GitHub作为项目托管网站，如果网速不好的话，严重影响使用体验，甚至会出现登录不上的情况。针对这个情况，大家也可以使用国内的项目托管网站-码云。

## 2、注册登录

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712182206056.png" alt="image-20220712182206056" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712182225132.png" alt="image-20220712182225132" style="zoom:50%;" />

## 3、码云创建远程库

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712182253173.png" alt="image-20220712182253173" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712182420563.png" alt="image-20220712182420563" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712182442418.png" alt="image-20220712182442418" style="zoom:50%;" />

## 4、IDEA集成 码云

### ①、安装插件

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712182555763.png" alt="image-20220712182555763" style="zoom:50%;" />

### ②、添加账号

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712182640079.png" alt="image-20220712182640079" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712182703742.png" alt="image-20220712182703742" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712182720337.png" alt="image-20220712182720337" style="zoom:50%;" />

### ③、share

#### Ⅰ、位置

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712182837473.png" alt="image-20220712182837473" style="zoom:50%;" />

#### Ⅱ、远程库名称

![image-20220712182955795](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712182955795.png)

#### Ⅳ、点击share，查看远程库

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712183145915.png" alt="image-20220712183145915" style="zoom:50%;" />



### ④、push

#### Ⅰ、复制远程库地址

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712183421154.png" alt="image-20220712183421154" style="zoom:50%;" />

#### Ⅱ、push

操作与GitHub相同

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712183554132.png" alt="image-20220712183554132" style="zoom:50%;" />

![image-20220712183526177](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712183526177.png)

### ⑤、pull

操作与GitHub相同

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712184139989.png" alt="image-20220712184139989" style="zoom:50%;" />

### ⑥、clone

操作与GitHub相同

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712184335203.png" alt="image-20220712184335203" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712184359861.png" alt="image-20220712184359861" style="zoom:50%;" />



## 5、复制GitHub项目

### ①、新建仓库

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712184645875.png" alt="image-20220712184645875" style="zoom:50%;" />

### ②、导入已有仓库

![image-20220712184659250](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712184659250.png)



### ③、复制github仓库地址

![image-20220712184827289](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712184827289.png)

### ④、导入成功

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712185021760.png" alt="image-20220712185021760" style="zoom:50%;" />

### ⑤、手动刷新

如果GitHub项目更新了以后，在码云项目端可以手动重新同步，进行更新

![image-20220712185042402](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220712185042402.png)



# 十、GitLab

## 1、简介

GitLab是由 GitLabInc.开发，使用 MIT许可证 的基于 网络 的 Git仓库 管理工具，且具有
wiki和 issue跟踪功能。使用 Git作为代码管理工具，并在此基础上搭建起来的 web服务。
GitLab由乌克兰程序员 DmitriyZaporozhets和 ValerySizov开发，它使用 Ruby语言 写
成。后来，一些部分用 Go语言 重写。截止 2018年 5月，该公司约有 290名团队成员，以
及 2000多名开源贡献者。 GitLab被 IBM Sony JülichResearchCenter NASA Alibaba
Invincea O’ReillyMedia Leibniz-Rechenzentrum( CERN SpaceX等组织使用 。

## 2、官网

官网地址
https://about.gitlab.com/
安装说明：
https://about.gitlab.com/installation/

## 3、安装

### ①、环境配置

#### Ⅰ、下载

+ 进入清华镜像官网

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220713092249430.png" alt="image-20220713092249430" style="zoom:50%;" />

+ 输入gitLab

![image-20220713092411070](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220713092411070.png)

+ 选择gitlab-ce

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220713092334307.png" alt="image-20220713092334307" style="zoom:50%;" />

+ 选择yum/

  ![image-20220713092533668](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220713092533668.png)

  

  

+ 选择e17

  ![image-20220713092552559](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220713092552559.png)

+ 选一个.rpm下载

  ![image-20220713092731377](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220713092731377.png)

#### Ⅱ、服务器配置

##### [1]、新建虚拟机

修改主机名，ip，关闭防火墙

本机主机名：gitLab，ip：192.168.188.100

##### [2]、将下载的.rpm上传到服务器/opt/model目录

使用xftp

![image-20220713092951013](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220713092951013.png)

#### Ⅲ、安装

进入rpm文件目录

执行rpm -i gitlab-ce-8.16.8-ce.0.el7.x86_64.rpm

![image-20220713112028207](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220713112028207.png)

#### Ⅳ、修改配置文件

sudo vim /etc/gitlab/gitlab.rb

![image-20220713112533202](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220713112533202.png)

![image-20220713112506662](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220713112506662.png)

重新配置

gitlab-ctl reconfigure

![image-20220713112749308](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220713112749308.png)

重新启动

![image-20220713112720734](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220713112720734.png)

##### 

#### Ⅴ、访问git Lab

输入本机ip

![image-20220713112824832](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220713112824832.png)

#### Ⅵ、登录

修改密码

登录

![image-20220713113013081](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220713113013081.png)



#### Ⅶ、创建远程库

##### [1]、新建project

![image-20220713113058965](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220713113058965.png)

![image-20220713113130686](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220713113130686.png)

### ②、idea集成

#### Ⅰ、下载插件

![image-20220713113839983](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220713113839983.png)

#### Ⅱ、配置连接

![image-20220713114221145](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220713114221145.png)

![image-20220713114203824](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220713114203824.png)

## 4、操作

与GitHub操作相同

