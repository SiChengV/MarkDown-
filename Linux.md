[TOC]



## Linux知识

### VIM

三种模式：

* 正常模式

  默认模式，打开文档进入此模式，删除、上下左右移动光标等等，可以使用快捷键

  快捷键：

  ```
  h、j、k、l：右、下、左、上。H：屏幕第一个字符，L：屏幕最后一行的第一个字符
  w、b、e：跳到下个单词词首、跳到本单词或上个单词词首、跳到本单词或下个单词词尾
  gg：跳到第一行，G：跳到最后一行，5G：跳到第五行
  v：进入行可视模式，类似于鼠标选取
  ctrl + v：进入列选择模式
  批量添加注释：进入列选择并选中列之后，I 加 // 加 esc
  自动对齐代码：v选择范围 加 = 即可
  对全文对齐：gg到文件开头 加 = 加 G
  viw：选中当前单词
  vi(tag)：如vi{：选中{}中间的内容
  #,*：向前、向后查找光标所在关键字
  %：匹配对应的括号
  拷贝当前行：yy，拷贝当前行向下的5行：5yy，粘贴：p
  删除当前行：dd，删除当前行向下的5行：5dd
  x：删除光标所在字符
  r：替换光标所在字符
  R：连续替换光标所在字符
  删除类操作符d：
  dw：删除内容直到后一个单词的起始处（d为操作符，w为动作）
  d$：删除内容直到行末
  dtf：向后删除到f
  dff：向前删除到f
  更改类操作符c：
  cw：删除内容直到后一个单词的起始处并进入插入模式
  cl：删除光标字符并进入插入模式
  在文件中查找某个词：/xxx , 跳到下一个匹配目标：n,上一个：N
  :set hls ：高亮显示所有查找内容。:nohlsearch ：移除高亮显示
  :s/old/new/g 可以替换 old 为 new（g：此行中的全部匹配项）（不加/g：此行中第一个匹配项）
  :#,#s/old/new/g ：在两行内替换所有的字符串 old 为新的字符串 new
  :! ：后接外部命令可以执行外部命令
  设置文件行号：:set nu， 取消行号：:set nonu
  ctrl + g：查看当前行及文件信息
  :sp 上下分屏
  :vsp 左右分屏
  ctrl + w ctrl + w：在窗口之间切换
  :tabnew filname 多标签
  gt：切换到下一个标签
  gT：切换到上一个标签
  ctrl + d：查看可选的命令补全
  到达最后一行：G，到达首行：gg
  到达某一行：行号+G
  0：移动到行首
  撤销动作：u
  U：撤销在这一行所做的所有动作
  反撤销：ctrl + R
  移动到20行：
  	1.显示行号：:set nu
  	2.输入： 20
  	3.输入： G
  :w TEST：该命令会以 TEST 为文件名保存整个文件
  :r FILENAME ：将FILENAME内的内容插入光标所在行
  :r !ls ：将ls命令的结果插入到光标所在行
  寄存器：
  "" 存储dcy等操作的数据
  "0 存储y过来的数据
  
  :%!xxd 查看二进制文件
  
  ```

  

* **插入/编辑模式**

  输入i：会跳到光标的前面，I：跳到行首

  输入a：会跳到光标的后面，A：跳到行尾

  输入o：另起下一行，O：另起上一行

  输入r：输入一个字符取代当前字符

* **命令行模式**

  保存、退出、读取等等

  ```
  :wq  保存并退出
  :q  退出
  :q! 不保存强制退出
  ```

* 三种模式的转换：

  * 命令行/插入模式--->正常模式模式  `esc`
  * 正常模式--->插入模式  `i`
  * 正常模式--->命令行模式 `:`或者 `/`

* vimdiff

  vimdiff a.txt b.txt对比两个文件的不同 

**批量替换**

```shell
:let i=0|g/\(asd\)[0-9]/s//\=submatch(1).i/|let i=i+1   #将文中出现的所有asd数字替换为asd数字（数字从0开始，每次递增1）
```

参考文献： http://blog.itpub.net/25897606/viewspace-746838/  

### GDB

常规

* 需要提前使用gcc的-g参数得到编译后的可执行文件，此文件包含调试信息（优化等级调为0）

* `gdb 待调试文件`：进入调试

* `set args xxx xxx`：设置命令行参数

* `list/l`：列出带行号的源代码

  `list show`：列出show函数源代码

  `list main.c:10`：列出main.c的第十行源代码

* `run/r`：运行程序，停在断点位置

* `start`：直接开始调试，相当于在main的第一行设置了断点

* `n/next`：下一条指令，会越过函数

* `s/step`：下一条指令，会进入函数

* `q`：退出gdb

断点

普通断点 break、观察断点watch和捕捉断点

* `b 行号`：在行号处设置断点， 后可以添加条件使得满足条件才停下（条件断点）
* `b 行号 if xxx`：设置条件断点 
* `disable`：使断点暂时失效
* `enable`：让断点重新生效
* `d 行号`：删除断点 
* `info b`：查看断点信息表
* `continue/c`：执行到下一个断点，无断点则执行完整个程序
* `finish`：结束当前函数调用，跳到函数调用入口

查看变量

* `p/print 变量`：查看变量的值

* `display 变量`：设置跟踪变量

* `undisplay 编号`：取消跟踪变量

* `ptype`：查看变量类型

* `x`：查看内存

  `x /12tb arr`：表示以arr为首地址，查看12个字节，以二进制显示

  * t 按二进制格式显示变量。
  * b 表示单字节

* `bt/backtrace`和`frame`

  当在一个函数中想查看不在当前函数作用域的变量时，可切换到变量所在栈帧来查看

  * `bt`：显示当前程序的栈帧
  * `frame 栈帧编号`：切换当前的栈环境为指定编号栈帧

多进程/多文件

* `b 函数名/ b 文件名:行号`：对指定的函数或源文件打断点
* `set follow-fork-mode parent/child`：只调试父/子进程

杂项

* shell：跳到shell执行shell命令，再键入exit退回到gdb

### apt包管理

下载包：`apt-get install -d <软件包>`

下载包所在目录：/var/cache/apt/archives/

### 同步异步、阻塞非阻塞

阻塞和非阻塞指的是：当前接口数据还未准备就绪时，线程是否被阻塞挂起。
同步和异步是指：当前请求是否有结果

### 环境变量相关知识

首先要认识几个和环境变量有关的文件：

- /etc/profile —— 此文件为系统的每个用户设置环境信息,当用户第一次登录时,该文件被执行.并从/etc/profile.d目录的配置文件中搜集shell的设置；
- /etc/environment —— 在登录时操作系统使用的第二个文件,系统在读取你自己的profile前,设置环境文件的环境变量；
- /etc/bashrc —— 为每一个运行bash shell的用户执行此文件.当bash shell被打开时,该文件被读取；
- ~/.profile —— 每个用户都可使用该文件输入专用于自己使用的shell信息，当用户登录时，该文件仅仅执行一次！默认情况下,它设置一些环境变量,执行用户的.bashrc文件；
- ~/.bashrc —— 该文件包含专用于你的bash shell的bash信息,当登录时以及每次打开新的shell时,该文件被读取；

### Socket知识

函数原型：`int socket(int domain, int type, int protocol);`

#### domain参数

AF_UNIX：代表本地连接

### Linux系统编程

##### 磁盘刷新、同步（sync、fsync、fdatasync）

* sync

  sync函数只是将所有修改过的块缓冲区排入写队列，然后就返回，它并不等待实际写磁盘操作结束。通常称为update的系统守护进程会周期性地（一般每隔30秒）调用sync函数。这就保证了定期冲洗内核的块缓冲区。命令sync(1)也调用sync函数。

* fsync

  fsync函数只对由文件描述符filedes指定的单一文件起作用，并且**阻塞等待写磁盘操作结束，然后返回**。

* fdatasync

  fdatasync函数类似于fsync，但它只影响文件的数据部分。而除数据外，fsync还会同步更新文件的属性。

  fdatasync函数类似于fsync，但它只影响文件的数据部分。而除数据外，fsync还会同步更新文件的属性。

##### epoll

新的I/O多路复用机制，使用回调机制代替SELECT的轮询机制，epoll的数据结构在内核态，是一个文件对象（包含了一个监听集合和就绪队列，其中监听集合用红黑树实现，目的是为了更好更快的管理文件描述符）

将就绪队列拷到用户态后再遍历就绪队列

**SELECT的不足：**

1. 用户态拷贝到内核态（大量对监听集合对拷贝）
2. 文件集合大小固定（用位图来保存）
3. 就绪机制不合理（特别是有海量连接但是少量就绪时）

**epoll的不同：**

1. 把监听和就绪分离
2. 把监听集合放内核态，不用在循环中重复设置监听集合
3. 就绪的只在就绪队列

* **epoll_create函数**

  创建一个epoll文件对象参数填大于0到数即可

  注意结束要close

* **epoll_ctl函数**

  设置监听

  struct epoll_event对应一个事件

  *epoll_ctl和struct epoll_event结构体：*

  events代表读或写事件

  ![截屏2022-03-14 下午2.19.52](./Linux.assets/截屏2022-03-14 下午2.19.52.png)

* **epoll_wait函数**

  ```c
  // epfd：   events：就绪队列数组，提前定义好  maxevents：最大就绪个数。 timeout超时时间，单位是ms,-1是永久等待
  int epoll_wait(int epfd, struct epoll_event *events, int maxevents, int timeout);
  // 返回就绪事件个数
  ```

* **epoll的边缘触发**

  有颜色则代表可以触发，epoll_wait在events参数中设置EPOLLET表示监听为边缘触发

  水平触发和边缘触发

  ![截屏2022-03-14 下午7.31.55](./Linux.assets/截屏2022-03-14 下午7.31.55.png)

* **epoll的一个例子**

  ```c++
  #include <sys/epoll.h>
  // 1.先用epoll_create创建一个epoll
  		int epfd = epoll_create(1);
  // 2.用epoll_ctl添加监视，需要提前写好struct epoll_event结构体
      struct epoll_event event;
      event.data.fd = sockFd;
      event.events = EPOLLIN;
      epoll_ctl(epfd, EPOLL_CTL_ADD, sockFd, &event);  // 监视sockFd
  // 3.使用epoll_wait阻塞进程，需提前创建一个readyQueue数组，用来存放就绪队列
      // 创建就绪队列
      struct epoll_event readyQueue[MAXNUM];    // MAXNUM为就绪队列最大长度
  		// 阻塞并开始监听
  		int readyNum = epoll_wait(epfd, readyQueue, MAXNUM, 5000);
  // 4.用for循环遍历就绪队列readyQueue，并设置对应的行为
          for(int i = 0; i < readyNum; ++i){
            if(readyQueue[i].data.fd == ...){
  						.....
          	}else if(){
  						.....
           	}
          }
  ```


##### inotify

事件触发的磁盘监控

主要涉及三个函数：

* Iinotify_init函数

  ```c++
  int inotify_init(void);   // 初始化inotify文件
  int inotify_init1(int flags);    // 带参数的初始化，如IN_NONBLOCK 非阻塞
  ```

* inotify_add_watch函数

  ```c++
  int inotify_add_watch(int fd, const char *pathname, uint32_t mask);  // 添加需要监控的文件、目录  fd为inotify_init得到的fd
  ```

* inotify_rm_watch函数

  移除监控

<font size=4>示例</font>

```c++
/* 初始化inotify */
    int notifyFd = inotify_init1(IN_NONBLOCK);
    unordered_map<int, string> wdFilePathMap;   // 保存wd监控描述符和监控的文件路径的对应关系
    if (notifyFd == -1) {
        P_LOG.error("inotify_init1 failed!");
    }
    int wd = inotify_add_watch(notifyFd, "/root/workSpace/testCode/build", IN_CREATE | IN_DELETE);   // 监控创建和删除文件操作
    wdFilePathMap[wd] = "/root/workSpace/testCode/build";
    

/* 处理监控的文件操作 */
    char buf[4096];   // 保存notifyFd内保存的具体操作信息
    int len = 0;
    const struct inotify_event *event;    // 保存具体的一个事件内容
    while (len = read(notifyFd, buf, sizeof(buf))) {   // 读取notifyFd中保存的事件信息到buf
        if (len <= 0) {
            break;
        }
      	// 将buf中的内容转换为inotify_event类型并进行处理
        for (char *ptr = buf; ptr < buf + len; ptr += sizeof(struct inotify_event) + event->len) { 
            event = (const struct inotify_event *) ptr;
            if (event->mask & IN_CREATE) {
                P_LOG.info("IN_CREATE: %s/%s", wdFilePathMap[event->wd].data(), event->name);
            }
            if (event->mask & IN_DELETE) {
                P_LOG.info("IN_DELETE: %s/%s", wdFilePathMap[event->wd].data(), event->name);
            }
        }
    }
```



### Linux常用命令

* `unrar x 文件名`：解压rar压缩的文件
  x为全路径解压，在指定解压路径时会把压缩前的路径一起解压出来
  e为无路径解压，即在指定解压路径时会把所有文件解压到指定路径，不保存原来的目录结构，即全部文件都堆在指定目录里
  参考资料：https://www.lanqiao.cn/questions/985/

* `rpm2cpio xxx.rpm | cpio -div` 将rpm包解压解压到当前目录

* `rpm -ivh xxx` 安装rpm包

* `pgrep -f {name}`   根据进程名获取pid

* `pidstat -d 1` 监控进程磁盘读写占用情况

* `w` 查看终端用户，`pkill -t pts/1` 提出对应终端的用户   踢用户利器!

* 以xx用户身份执行命令

  `runuser -l xx -c "command"` 执行路径会在该用户的家目录下

  `su - xxx -c "commad"`   和runuser一致

  `su - xxx`  切换到xxx用户，如果在脚本中使用，该句后面的命令依旧是以原来的用户权限运行

* `mount -o remount,rw,exec /`  给根目录添加读写、可执行权限

* ps

  `ps -T -p $pid` 查看所有线程

* `hexdump`：以x进制方式查看文件

* `c++filt _ZN1N1C4FuncEi`  查看符号表中的函数名， 配合`typeid(xxx).name()`可以查看一个变量的类型

* 创建一个socket，并作为一个服务端等待客户连接

  `nc -U /xxxx -l`  -U 告诉nc使用UnixSocket文件，-l告诉nc作为一个服务端 默认为TCP类型

  -u 使用UDP

* 查看进程与文件的关系 `lsof`

  用lsof | grep 进程名可以看到进程使用了哪些文件


* AWK使用

  语法：`awk 'pattern{action}' 文件名`

  pattern原来设定条件

  action原来指定动作

  * print 打印动作

  变量：

  * -v ORS="xxx" 保存输出记录分隔符
  * -v FS="xxx"  自定义分隔符
  * NR 当前行号
  * NF 总列数，$NF代表最后一列

  去重： `awk '!a[$0]++' 原文件 > 目的文件`

  BEGIN{...}、END{...}在读取文件之前/后执行

* SED使用

  语法 `sed [-in] [script] [文件名]`

  -i：表示直接修改文件

  -n：表示仅显示script处理后的结果

  **动作说明：**

  - a ：新增， a 的后面可以接字串，而这些字串会在新的一行出现(目前的下一行)～
  - c ：取代， c 的后面可以接字串，这些字串可以取代 n1,n2 之间的行！
  - d ：删除，因为是删除啊，所以 d 后面通常不接任何东东；
  - i ：插入， i 的后面可以接字串，而这些字串会在新的一行出现(目前的上一行)；
  - p ：打印，亦即将某个选择的数据印出。通常 p 会与参数 sed -n 一起运行～
  - s ：取代，可以直接进行取代的工作哩！通常这个 s 的动作可以搭配正则表达式！例如 1,20s/old/new/g 就是啦！

* SHELL使用

  **shell变量**

  ```shell
  $!  # 获取shell运行的后台程序的pid
  $?  # 获取shell中上一条指令的退出状态码
      #   0	命令成功结束
      #   1	一般性未知错误
      #   2	不适合的shell指令
      
  # 定义变量 变量加减
  num=1
  let num+=1
  
  # 获取上条指令的运行结果
  pid=`pidof xxx`
  ```

* diff命令

  diff正常格式  略

  diff上下文格式  `diff -c`   ，会分别显示出各个文件不同行的上下文

  diff合并格式 `diff -u `     git diff使用的风格

* 添加用户

  `useradd 用户名 -m`：不指定组的话会默认创建一个同名的组并加入。/home目录下会对应创建一个用户文件夹

  `useradd -d 指定路径 用户名`：创建用户名且指定该用户文件名和文件所在位置

  `useradd -g 用户组 用户名`：创建用户并添加到用户组

  添加系统用户用adduser，如果用useradd将创建一个三无用户
  添加sudo权限：在/etc/sudoers 的`root ALL=(ALL:ALL) ALL`这一行下增加`用户名 ALL=(ALL:ALL) ALL `即可

* 修改系统时间

  `date -s 16:40:30`

* grep

  grep搜索当前路径下文件中的文本内容：`grep xxx -r .`    -n：输出行号。-i：忽略大小写

  输出上下文：`-C 5` 输出匹配位置的上下5行。`-B 5` 输出匹配位置前5行。`-A 5` 输出匹配位置后5行。

* 查看二进制文件链接到哪些动态库及其路径

  `ldd xxx`

* netstat

  `netstat -tunlp`  查看进程监听的端口

* 磁盘相关

  查看磁盘挂载数据：df -h

  查看指定目录所占磁盘空间：`du -sh 目录` -s表示只显示结果

  查看目录下各个文件的大小：`du -ah --max-depth=1 目录名`
  
  *参数：*
  
  * `-s`：指定目录占用大小汇总
  * `-h`：带计量单位
  * `-a`：含文件
  * `-c`：列出明细的同时，增加汇总值
  * `--max-depth=x`：指定子目录深度为x
  
  `dd if=/dev/zero of=test.txt count=10 bs=1M`  创建大小为10M的文件
  
* **-exec / -ok / xargs**

  `find ./ -maxdepth 1 -type f -exec rm -r {} \;`：将前面find指令的结果传给后面的{}然后执行后面的指令。将前面的命令分段依次传给后面的命令，传一个执行一次命令。

  `find ./ -maxdepth 1 -type f -ok rm -r {} \;`：将前面find指令的结果传给后面的{}然后执行后面的指令，会让你确认是否删除

  `find ./ -maxdepth 1 -type f | xargs  rm -r `：将前面find指令的结果传给后面，然后执行后面的指令,当结果集过大时会分段 ，因此比-exec优越，但xargs命令对传过来的结果集默认以空格分割，因此如果一个文件名中有空格，如：abc dd，那么xargs会把这一个文件看成两个文件：abc和dd，解决办法是两个命令都加上-print0：以null分割结果集。xargs将前面的内容作为参数传给后续命令执行，而不是文本内容。将前面命令一股脑的甩给后面的命令作为参数，只执行一次后面的命令。

### linux杂项知识

#### 动态库优先搜索路径：

```shell
export LD_LIBRARY_PATH=自己的动态链接库路径:$LD_LIBRARY_PATH
```

/etc/ld.so.conf文件内记录了要搜索库的路径，使用ldconfig会搜索这些路径，并将文件加载到ld.so.cache内

去除export过的环境变量：`unset LD_LIBRARY_PATH`

##### 系统默认编辑器修改

`update-alternatives --config editor` 修改系统默认编辑器

**命令行操作**

* ctrl+a  跳到开头
* ctrl+e 跳到结尾
* ctrl+w 删除光标前一个单词

#### date

`date +%Y-%m-%d' '%H:%M:%S.%N`  打印时间，精确到纳秒