## Linux知识

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

### Linux系统调用

##### 磁盘刷新、同步（sync、fsync、fdatasync）

* sync

sync函数只是将所有修改过的块缓冲区排入写队列，然后就返回，它并不等待实际写磁盘操作结束。通常称为update的系统守护进程会周期性地（一般每隔30秒）调用sync函数。这就保证了定期冲洗内核的块缓冲区。命令sync(1)也调用sync函数。

* fsync

fsync函数只对由文件描述符filedes指定的单一文件起作用，并且**阻塞等待写磁盘操作结束，然后返回**。

* fdatasync

fdatasync函数类似于fsync，但它只影响文件的数据部分。而除数据外，fsync还会同步更新文件的属性。

fdatasync函数类似于fsync，但它只影响文件的数据部分。而除数据外，fsync还会同步更新文件的属性。

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
  ```

* VIM使用

  **批量替换**

  ```shell
  :let i=0|g/\(asd\)[0-9]/s//\=submatch(1).i/|let i=i+1   #将文中出现的所有asd数字替换为asd数字（数字从0开始，每次递增1）
  ```

  参考文献： http://blog.itpub.net/25897606/viewspace-746838/  

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

  `find ./ -maxdepth 1 -type f -exec rm -r {} \;`：将前面find指令的结果传给后面的{}然后执行后面的指令

  `find ./ -maxdepth 1 -type f -ok rm -r {} \;`：将前面find指令的结果传给后面的{}然后执行后面的指令，会让你确认是否删除

  `find ./ -maxdepth 1 -type f | xargs  rm -r `：将前面find指令的结果传给后面，然后执行后面的指令,当结果集过大时会分段 ，因此比-exec优越，但xargs命令对传过来的结果集默认以空格分割，因此如果一个文件名中有空格，如：abc dd，那么xargs会把这一个文件看成两个文件：abc和dd，解决办法是两个命令都加上-print0：以null分割结果集。xargs将前面的内容作为参数传给后续命令执行，而不是文本内容。

### linux杂项知识

#### 动态库优先搜索路径：

```shell
export LD_LIBRARY_PATH=自己的动态链接库路径:$LD_LIBRARY_PATH
```

去除export过的环境变量：`unset LD_LIBRARY_PATH`

##### 系统默认编辑器修改

`update-alternatives --config editor` 修改系统默认编辑器

**命令行操作**

* ctrl+a  跳到开头
* ctrl+e 跳到结尾
* ctrl+w 删除光标前一个单词

#### date

`date +%Y-%m-%d' '%H:%M:%S.%N`  打印时间，精确到纳秒