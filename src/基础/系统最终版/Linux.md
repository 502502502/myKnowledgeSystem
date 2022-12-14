## 1、如何理解一切皆文件

在 Linux 操作系统中，所有被操作系统管理的资源，例如网络接口卡、磁盘驱动器、打印机、输入输出设备、普通文件或是目录都被看作是一个文件



## 2、inode 是什么?有什么作用呢?

虽然，我们将文件存储在了块(block)中，但是我们还需要一个空间来存储文件的 元信息 metadata ：如某个文件被分成几块、每一块在的地址、文件拥有者，创建时间，权限，大小等。这种 存储文件元信息的区域就叫 inode。

- **inode** ：记录文件的属性信息，可以使用 stat 命令查看 inode 信息。
- **block** ：实际文件的内容，如果一个文件大于一个块时候，那么将占用多个 block，但是一个块只能存放一个文件。（因为数据是由 inode 指向的，如果有两个文件的数据存放在同一个块中，就会乱套了）



## 3、文件类型有哪些

- **普通文件（-）** ： 用于存储信息和数据， Linux 用户可以根据访问权限对普通文件进行查看、更改和删除。比如：图片、声音、PDF、text、视频、源代码等等。
- **目录文件（d，directory file）** ：目录也是文件的一种，用于表示和管理系统中的文件，目录文件中包含一些文件名和子目录名。打开目录事实上就是打开目录文件。
- **符号链接文件（l，symbolic link）** ：保留了指向文件的地址而不是文件本身。
- **字符设备（c，char）** ：用来访问字符设备比如键盘。
- **设备文件（b，block）** ： 用来访问块设备比如硬盘、软盘。
- **管道文件(p,pipe)** : 一种特殊类型的文件，用于进程之间的通信。
- **套接字(s,socket)** ：用于进程间的网络通信，也可以用于本机之间的非网络通信。



## 4、Linux 的目录结构是什么样的

Linux 文件系统的结构层次鲜明，就像一棵倒立的树，最顶层是其根目录：

![image-20220909103925205](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220909103925205.png)

/bin： 存放二进制可执行文件(ls、cat、mkdir 等)，常用命令一般都在这里；
/etc： 存放系统管理和配置文件；
/home： 存放所有用户文件的根目录，是用户主目录的基点，比如用户 user 的主目录就是/home/user，可以用~user 表示；
/usr ： 用于存放系统应用程序；
/opt： 额外安装的可选应用程序包所放置的位置。一般情况下，我们可以把 tomcat 等都安装到这里；
/proc： 虚拟文件系统目录，是系统内存的映射。可直接访问这个目录来获取系统信息；
/root： 超级用户（系统管理员）的主目录（特权阶级^o^）；
/sbin: 存放二进制可执行文件，只有 root 才能访问。这里存放的是系统管理员使用的系统级别的管理命令和程序。如 ifconfig 等；
/dev： 用于存放设备文件；
/mnt： 系统管理员安装临时文件系统的安装点，系统提供这个目录是让用户临时挂载其他的文件系统；
/boot： 存放用于系统引导时使用的各种文件；
/lib ： 存放着和系统运行相关的库文件 ；
/tmp： 用于存放各种临时文件，是公用的临时文件存储点；
/var： 用于存放运行时需要改变数据的文件，也是某些大文件的溢出区，比方说各种服务的日志文件（系统启动日志等。）等；
/lost+found： 这个目录平时是空的，系统非正常关机而留下“无家可归”的文件（windows 下叫什么.chk）就在这里。



## 5、 Linux 基本命令有哪些

###  **目录切换命令**

- **`cd usr`：** 切换到该目录下 usr 目录
- **`cd ..（或cd../）`：** 切换到上一层目录
- **`cd /`：** 切换到系统根目录
- **`cd ~`：** 切换到用户主目录
- **`cd -`：** 切换到上一个操作所在目录



###  **目录的操作命令(增删改查)**

- **`mkdir 目录名称`：** 增加目录。

- **`ls/ll`**（ll 是 ls -l 的别名，ll 命令可以看到该目录下的所有目录和文件的详细信息）：查看目录信息。

- **`find 目录 参数`：** 寻找目录（查）

  >  ① 列出当前目录及子目录下所有文件和文件夹: `find .`；	
  >
  > ② 在`/home`目录下查找以.txt 结尾的文件名:`find /home -name "*.txt"` ,忽略大小写: `find /home -iname 				"*.txt"` ；
  >
  > ③ 当前目录及子目录下查找所有以.txt 和.pdf 结尾的文件:`find . \( -name "*.txt" -o -name "*.pdf" \)`或`find . -name "*.txt" -o -name "*.pdf"`。

- **`mv 目录名称 新目录名称`：** 修改目录的名称（改）。

  > 注意：mv 的语法不仅可以对目录进行重命名而且也可以对各种文件，压缩包等进行 重命名的操作。
  >
  > mv 命令用来对文件或目录重新命名，或者将文件从一个目录移到另一个目录中。

- **`mv 目录名称 目录的新位置`：** 移动目录的位置---剪切（改）。

  > 注意：mv 语法不仅可以对目录进行剪切操作，对文件和压缩包等都可执行剪切操作。
  >
  > 另外 mv 与 cp 的结果不同，mv 好像文件“搬家”，文件个数并未增加。
  >
  > 而 cp 对文件进行复制，文件个数增加了。

- **`cp -r 目录名称 目录拷贝的目标位置`：** 拷贝目录（改），-r 代表递归拷贝 。

  > 注意：cp 命令不仅可以拷贝目录还可以拷贝文件，压缩包等，拷贝文件和压缩包时不 用写-r 递归。

- **`rm [-rf] 目录` :** 删除目录（删）。

  >  注意：rm 不仅可以删除目录，也可以删除其他文件或压缩包，为了增强大家的记忆， 无论删除任何目录或文件，都直接使用`rm -rf` 目录/文件/压缩包。



###  **文件的操作命令(增删改查)**

- **`touch 文件名称`:** 文件的创建（增）。

- **`cat/more/less/tail 文件名称`** ：文件的查看（查） 。

  > 命令 `tail -f 文件` 可以对某个文件进行动态监控，例如 tomcat 的日志文件， 会随着程序的运行，日志会变化，可以使用 `tail -f catalina-2016-11-11.log` 监控 文 件的变化 。

- **`vim 文件`：** 修改文件的内容（改）。

  > vim 编辑器是 Linux 中的强大组件，是 vi 编辑器的加强版，vim 编辑器的命令和快捷方式有很多。
  >
  > 在实际开发中，使用 vim 编辑器主要作用就是修改配置文件，下面是一般步骤： `vim 文件------>进入文件----->命令模式------>按i进入编辑模式----->编辑文件 ------->按Esc进入底行模式----->输入：wq/q!` （输入 wq 代表写入内容并退出，即保存；输入 q!代表强制退出不保存）。

- **`rm -rf 文件`：** 删除文件（删）。



###  **压缩文件的操作命令**

**1）打包并压缩文件**

Linux 中的打包文件一般是以.tar 结尾的，压缩的命令一般是以.gz 结尾的。而一般情况下打包和压缩是一起进行的，打包并压缩后的文件的后缀名一般.tar.gz。 命令：`tar -zcvf 打包压缩后的文件名 要打包压缩的文件` ，其中：

- z：调用 gzip 压缩命令进行压缩
- c：打包文件
- v：显示运行过程
- f：指定文件名

> 假如 test 目录下有三个文件分别是：aaa.txt bbb.txt ccc.txt，如果我们要打包 test 目录并指定压缩后的压缩包名称为 test.tar.gz 可以使用命令：**`tar -zcvf test.tar.gz aaa.txt bbb.txt ccc.txt` 或 `tar -zcvf test.tar.gz /test/`**

**2）解压压缩包**

命令：`tar [-xvf] 压缩文件`

其中：x：代表解压

> 将 /test 下的 test.tar.gz 解压到当前目录下可以使用命令：**`tar -xvf test.tar.gz`**
>
> 将 /test 下的 test.tar.gz 解压到根目录/usr 下:**`tar -xvf test.tar.gz -C /usr`**（- C 代表指定解压的位置）



###  Linux 的权限命令

+ **`ls -l`** ：查看某个目录下的文件或目录的权限

  <img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220909114048557.png" alt="image-20220909114048557" style="zoom: 67%;" />

  > **文件的类型：**
  >
  > - d： 代表目录
  > - -： 代表文件
  > - l： 代表软链接（可以认为是 window 中的快捷方式）
  >
  > **Linux 中权限分为以下几种：**
  >
  > - r：代表权限是可读，r 也可以用数字 4 表示
  > - w：代表权限是可写，w 也可以用数字 2 表示
  > - x：代表权限是可执行，x 也可以用数字 1 表示
  >
  > 
  >
  > 对于文件：
  >
  > | 权限名称 | 可执行操作                  |
  > | -------- | --------------------------- |
  > | r        | 可以使用 cat 查看文件的内容 |
  > | w        | 可以修改文件的内容          |
  > | x        | 可以将其运行为二进制文件    |
  >
  > 对于目录：
  >
  > | 权限名称 | 可执行操作               |
  > | -------- | ------------------------ |
  > | r        | 可以查看目录下列表       |
  > | w        | 可以创建和删除目录下文件 |
  > | x        | 可以使用 cd 进入目录     |
  >
  > 
  >
  > **文件所有者、所在组、其它组**
  >
  > - **所有者(u)** ：一般为文件的创建者，谁创建了该文件，就天然的成为该文件的所有者，用 `ls ‐ahl` 命令可以看到文件的所有者 也可以使用 chown 用户名 文件名来修改文件的所有者 。
  > - **文件所在组(g)** ：当某个用户创建了一个文件后，这个文件的所在组就是该用户所在的组用 `ls ‐ahl`命令可以看到文件的所有组也可以使用 chgrp 组名 文件名来修改文件所在的组。
  > - **其它组(o)** ：除开文件的所有者和所在组的用户外，系统的其它用户都是文件的其它组。

+ `chmod`：修改文件/目录的权限的命令

  > **`chmod u=rwx,g=rw,o=r aaa.txt`** 或者 **`chmod 764 aaa.txt`**



###  Linux 用户管理命令

- `useradd 选项 用户名`:添加用户账号
- `userdel 选项 用户名`:删除用户帐号
- `usermod 选项 用户名`:修改帐号
- `passwd 用户名`:更改或创建用户的密码
- `passwd -S 用户名` :显示用户账号密码信息
- `passwd -d 用户名`: 清除用户密码
- `groupadd 选项 用户组` :增加一个新的用户组
- `groupdel 用户组`:要删除一个已有的用户组
- `groupmod 选项 用户组` : 修改用户组的属性



###  其他常用命令

- **`pwd`：** 显示当前所在位置

- `sudo + 其他命令`：以系统管理者的身份执行指令，也就是说，经由 sudo 所执行的指令就好像是 root 亲自执行。

- **`grep 要搜索的字符串 要搜索的文件 --color`：** 搜索命令，--color 代表高亮显示

- **`ps -ef`/`ps -aux`：** 这两个命令都是查看当前系统正在运行进程

  > 如果想要查看特定的进程可以使用这样的格式：**`ps aux|grep redis`** （查看包括 redis 字符串的进程），也可使用 `pgrep redis -a`。

  > 注意：如果直接用 ps（（Process Status））命令，会显示所有进程的状态，
  > 通常结合 grep 命令查看某进程的状态。

- **`kill -9 进程的pid`：** 杀死进程（-9 表示强制终止。）

  > 先用 ps 查找进程，然后用 kill 杀掉

- `ifconfig`：查看当前系统的网卡信息

- `ping`：查看与某台机器的连接情况

- `netstat -an`：查看当前系统的端口使用

- **`shutdown`：** `shutdown -h now`： 指定现在立即关机

  > `shutdown +5 "System will shutdown after 5 minutes"`：指定 5 分钟后关机，同时送出警告信息给登入用户。

- **`reboot`：** **`reboot`：** 重开机

  > **`reboot -w`：** 做个重开机的模拟（只有纪录并不会真的重开机）。





## 6、如何设置开机自启动？

以zookper为例

1. 新建一个脚本 zookeeper
2. 为新建的脚本 zookeeper 添加可执行权限，命令是:`chmod +x zookeeper`
3. 把 zookeeper 这个脚本添加到开机启动项里面，命令是：`chkconfig --add zookeeper`
4. 如果想看看是否添加成功，命令是：`chkconfig --list`



## 7、环境变量的启动顺序

- 用户级别环境变量 : `~/.bashrc`、`~/.bash_profile`。
- 系统级别环境变量 : `/etc/bashrc`、`/etc/environment`、`/etc/profile`、`/etc/profile.d`。

配置文件执行先后顺序为：`/etc/enviroment` –> `/etc/profile` –> `/etc/profile.d` –> `~/.bash_profile` –> `/etc/bashrc` –> `~/.bashrc`

> 建议用户级别环境变量在 `~/.bash_profile`中配置，系统级别环境变量在 `/etc/profile.d` 中配置



## 8、环境变量生命周期

- 永久的：需要用户修改相关的配置文件，变量永久生效。
- 临时的：用户利用 `export` 命令，在当前终端下声明环境变量，关闭 shell 终端失效。



## 9、如何读取环境变量

```shell
# 列出当前的环境变量值
export -p
```

`echo` 命令可以输出指定环境变量的值。

```shell
# 输出当前的PATH环境变量的值
echo $PATH
# 输出当前的HOME环境变量的值
echo $HOME
```



## 10、 如何修改环境变量

`export`命令可以修改指定的环境变量。对当前 shell 终端生效，关闭 shell 终端就会失效。修改完成之后，立即生效。

```shell
export CLASSPATH=./JAVA_HOME/lib;$JAVA_HOME/jre/lib
```





 `vim` 命令修改环境变量配置文件。这种方式修改环境变量永久有效。

```
vim ~/.bash_profile
```

> 如果修改的是系统级别环境变量则对所有用户生效，如果修改的是用户级别环境变量则仅对当前用户生效。

修改完成之后，需要 `source` 命令让其生效或者关闭 shell 终端重新登录。

```shell
source /etc/profile
```