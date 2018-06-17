---
title: bash一些命令
date: 2018-06-12 21:58:37
tags: Linux
categories: Linux
---
### env：列出目前的shell环境下的所有环境变量与其内容
```
[root@linux ~]# env
HOSTNAME=linux.dmtsai.tw <== 这部主机的主机名称

SHELL=/bin/bash <== 目前这个环境下，使用的 Shell 是哪一个程序？

TERM=xterm <== 这个终端机使用的环境是什么类型

HISTSIZE=1000 <== 这个就是『记录指令的笔数』在 FC4 预设可记录 1000 笔

USER=root <== 使用者的名称啊！

ENV=/root/.bashrc <== 使用的个人环境设定档

MAIL=/var/spool/mail/root <== 这个使用者所取用的 mailbox 位置

PATH=/sbin:/usr/sbin:/bin:/usr/bin:/usr/X11R6/bin:/usr/local/bin:/usr/local/sbin:/root/bin <== 不再多讲啊！是执行文件指令搜寻路径

INPUTRC=/etc/inputrc <== 与键盘按键功能有关。可以设定特殊按键！

PWD=/root <== 目前使用者所在的工作目录 (利用 pwd 取出！)

LANG=en_US.UTF-8 <== 这个与语系有关，底下会再介绍！

HOME=/root <== 这个使用者的家目录啊！

_=/bin/env <== 上一次使用的指令的最后一个参数(或指令本身)
```
<!-- more -->

### set：观察所有变量
>bash不只有环境变量，还有一些与bash操作接口有关的变量，以及用户自定义的变量存在，用set可以全部显示出来。

```
BASH=/bin/bash <== bash 的主程序放置路径

COLORS=/etc/DIR_COLORS.xterm <== 使用的颜色纪录档案

COLUMNS=115 <== 在目前的终端机环境下，使用的字段有几个字符长度

HISTFILE=/root/.bash_history <== 历史命令记录的放置档案，隐藏档

HISTFILESIZE=1000 <== 存起来(与上个变量有关)的档案之指令的最大纪录笔数。

HISTSIZE=1000 <== 目前环境下，可记录的历史命令最大笔数。

HOSTTYPE=i386 <== 主机安装的软件主要类型。我们用的是 i386 兼容机器软件

IFS=$' \t\n' <== 预设的分隔符

LINES=35 <== 目前的终端机下的最大行数

MACHTYPE=i386-redhat-linux-gnu <== 安装的机器类型

MAILCHECK=60 <== 与邮件有关。每 60 秒去扫瞄一次信箱有无新信！

OLDPWD=/home <== 上个工作目录。我们可以用 cd - 来取用这个变量。

OSTYPE=linux-gnu <== 操作系统的类型！

PS1='[\u@\h \W]\$ ' <== PS1 就厉害了。这个是命令提示字符，也就是我们常见的[root@linux ~]# 或 [dmtsai ~]$ 的设定值啦！可以更动的！

$ <== 目前这个 shell 所使用的 PID

? <== 刚刚执行完指令的回传值
```
1. PS1：提示字符(`[root@linux ~]#`)的设定
	+ \d ：代表日期，格式为 Weekday Month Date，例如 "Mon Aug 1"

	+ \H ：完整的主机名称。举例来说，鸟哥的练习机 linux.dmtsai.tw ，那么这个主机名称就是 linux.dmtsai.tw

	+ \h ：仅取主机名称的第一个名字。以上述来讲，就是 linux 而已， .dmtsai.tw 被省略。

	+ \t ：显示时间，为 24 小时格式，如： HH:MM:SS

	+ \T ：显示时间， 12 小时的时间格式！

	+ \A ：显示时间， 24 小时格式， HH:MM

	+ \u ：目前使用者的账号名称；

	+ \v ： BASH 的版本信息；

	+ \w ：完整的工作目录名称。家目录会以 ~ 取代；

	+ \W ：利用 basename 取得工作目录名称，所以仅会列出最后一个目录名。

	+ \# ：下达的第几个指令。

	+ \$ ：提示字符，如果是 root 时，提示字符为 # ，否则就是 $ 啰～

2. $:关于本shell的PID
代表的是目前shell的线程代号，即pid。

3. ?：关于上个执行指令的回传值
当我们执行某些指令时，这些指令都会回传一个执行后的代码。一般来说，**如果成功的执行该指令，则会回传一个0值**，如果执行过程发生错误，就会回传错误代码，一般为非0的数值。（可以根据这个代码搜寻错误的原因）

### export：将自定义的变量转换为环境变量
+ 环境变量和自定义变量的区别：该变量是否会被子程序所继续引用。

+ 子程序仅仅会继承父程序的环境变量，不会继承父程序的自定义变量。

+ 用法： `export 变量名称`

+ 为什么环境变量的数据可以被子程序所应用呢？这是因为内存配置的关系！
> 
• 当启动一个 shell ，操作系统分配一记忆区块给 shell 使用，此区域之变量可以让子程序存取；
> 
• 利用 export 功能，可以让变量的内容写到上述的记忆区块当中(环境变量)；
>
• 当加载另一个 shell 时 (亦即启动子程序，而离开原本的父程序了)，子 shell 可以将父 shell的环境变量所在的记忆区块导入自己的环境变量区块当中

### locale：影响显示结果的语系变量
+ 设置了LANG或者是LC_ALL时，其他的语系变量就会被这两个变量所取代。

+ 整体系统默认的语系设置文件：/etc/locale.conf

### read：接收键盘的输入，把输入的值赋给变量
1. 用法
```
[root@linux ~]# read [-pt] variable
参数：
-p ：后面可以接提示字符！
-t ：后面可以接等待的『秒数！』这个比较有趣～不会一直等待使用者啦！
```

2. 例子
```
范例一：让使用者由键盘输入一内容，将该内容变成 atest 变量
[root@linux ~]# read atest
This is a test
[root@linux ~]# echo $atest
This is a test
```
```
范例二：提示使用者 30 秒内输入自己的大名，将该输入字符串做成 named 变量
[root@linux ~]# read -p "Please keyin your name: " -t 30 named
Please keyin your name: VBird Tsai
[root@linux ~]# echo $named
VBird Tsai
```

### declare/typeset：一样的功能，就是宣告变量的类型
　如果declare后面没有接任何参数，就是和使用set效果一样。

1. 用法
```
declare [-aixr] variable
参数：
-a ：将后面的 variable 定义成为数组 (array)
-i ：将后面接的 variable 定义成为整数数字 (integer)-x ：用法与 export 一样，就是将后面的 variable 变成环境变量；
-r ：将一个 variable 的变量设定成为 readonly ，该变量不可被更改内容，也不能 unset
```

2. 例子
>由于在默认的情况下，bash对变量有几个基本的定义
>> + 变量类型默认为字符串
>> + Bash环境中的数值运算，预设最多只能到达整数形态，所以1/3结果为0

    ```
    范例一：让变量 sum 进行 100+300+50 的加总结果
    [root@linux ~]# sum=100+300+50
    [root@linux ~]# echo $sum
    100+300+50 <==咦！怎么没有帮我计算加总？因为这是文字型态的变量属性啊！
    [root@linux ~]# declare -i sum=100+300+50
    [root@linux ~]# echo $sum
    450
    ```
    ```
    范例二：将 sum 变成环境变量
    [root@linux ~]# declare -x sum
    ```
3. 取消某一个属性
    ```
    declare +x sum 将-变成+可以取消某一个属性
    ```
4. 数组
    ```
    范例：设定var[1] ～ var[3] 变量。
	[root@linux ~]# var[1]="small min"
	[root@linux ~]# var[2]="big min"
	[root@linux ~]# var[3]="nice min"
	[root@linux ~]# echo "${var[1]}, ${var[2]}, ${var[3]}
    ${数组} 的方式来读取数组变量的值
    ```

###　ulimit：限制用户的某些系统资源
1. 用法
```
[root@linux ~]# ulimit [-SHacdflmnpstuv] [配额]
参数：
-H ：hard limit ，严格的设定，必定不能超过设定的值；
-S ：soft limit ，警告的设定，可以超过这个设定值，但是会有警告讯息，并且，还是无法超过 hard limit 的喔！也就是说，假设我的 soft limit为 80 ， hard limit 为 100 ，那么我的某个资源可以用到 90 ，可以超过 80 ，还是无法超过 100 ，而且在 80~90 之间，会有警告讯息的意思。
-a ：列出所有的限制额度；
-c ：可建立的最大核心档案容量 (core files)
-d ：程序数据可使用的最大容量
-f ：此 shell 可以建立的最大档案容量 (一般可能设定为 2GB)单位为 Kbytes
-l ：可用于锁定 (lock) 的内存量
-p ：可用以管线处理 (pipe) 的数量
-t ：可使用的最大 CPU 时间 (单位为秒)
-u ：单一使用者可以使用的最大程序(process)数量。
```

2. 例子
```
范例一：列出所有的限制数据
[root@linux ~]# ulimit -a
```
```
范例二：限制使用者仅能建立 1MBytes 以下的容量的档案
[root@linux ~]# ulimit -f 1024
```

3. tips
+ 想要复原ulimit设定最简单的方法就是注销再登入
+ 一般身份使用者如果以ulimit设定了-f的文件大小，那么他只能`继续减小文件容量，不能增加文件容量`

### alias:别名系统
+ 创建别名
```
	alias lm='ls -a | more'
```
+ 取消别名
```
	unalias lm
```

### history：历史命令
1. 参数
```
-n ：数字，意思是『要列出最近的 n 笔命令列表』的意思！
-c ：将目前的 shell 中的所有 history 内容全部消除
-a ：将目前新增的 history 指令新增入 histfiles 中，若没有加 histfiles ，则预设写入 ~/.bash_history
-r ：将 histfiles 的内容读到目前这个 shell 的 history 记忆中；
-w ：将目前的 history 记忆内容写入 histfiles 中！
```
>在正常的情况下，当我们以 bash 登入 Linux 主机之后，系统会主动的由家目录的 ~/.bash_history 读取以前曾经下过的指令，那么 ~/.bash_history 会记录几笔数据呢？这就与你 bash 的 HISTSIZE 这个变量设定值有关了！预设是会记录 1000 笔数据的！ 那么假设我这次登入主机后，共下达过 100 次指令，『等我注销时， 系统就会将 101~1100 这总共 1000 笔历史命令更新到 ~/.bash_history当中。』 也就是说，历史命令在我注销时，会将最近的 HISTSIZE 笔记录到我的纪录文件当中啦！ 当然，也可以用 history -w 强制立刻写入的！

2. 运用hittory执行命令
```
[root@linux ~]# !number
[root@linux ~]# !command
[root@linux ~]# !!
参数：
number ：执行第几笔指令的意思；
command ：由最近的指令向前搜寻『指令串开头为 command』的那个指令，并执行；
!! ：就是执行上一个指令(相当于按↑按键后，按 Enter)
```

3. 同一账号多次登入的history写入问题
　　有些朋友在练习linux 的时候喜欢同时开好几个bash 接口，这些bash的身份都是root 。这样会有~/.bash history 的写入问题吗?想一想，因为这些bash在同时以root 的身份登入，因此所有的bash都有自己的1000笔记录在内存中。因为等到注销时才会更新记录文件，所以啰，最后注销的那个bash才会是最后写入的数据。唔!如此一来其他bash的指令操作就不会被记录下来了(其实有被记录，只是被后来的最后一个bash所覆盖更新了)。
  由于多重登入有这样的问题，所以很多朋友都习惯单一bash登入，再用工作控制(job control)来切换不同工作!这样才能够将所有曾经下达过的指令记录下来，也才方便未来系统管理员进行指令的debug 啊!

### 指令的搜寻顺序
1. 以相对/绝对路径执行命令
2. 由alias找到该指令来执行
3. 由bash内建的（builtin）指令来执行
4. 透过$PATH这个变量的顺序搜寻到的第一个指令来执行

### stty:查阅按键内容
```
[root@linux ~]# stty [-a]
参数：
-a ：将目前所有的 stty 参数列出来；
范例：列出所有的按键与按键内容
[root@linux ~]# stty -a
speed 38400 baud; rows 40; columns 80; line = 0;
intr = ^C; quit = ^\; erase = ^?; kill = ^U; eof = ^D; eol = ;
eol2 = ; start = ^Q; stop = ^S; susp = ^Z; rprnt = ^R; werase
= ^W; lnext = ^V; flush = ^O; min = 1; time = 0;
```
+ ^是Ctrl的意思。
+ intr : 送出一个interrupt（中断）信号给目前正在运行的程序，就是终止
+ eof : End of file 的意思，代表『结束输入』
+ erase : 向后删除字符
+ kill : 删除在目前指令列上的所有文字
+ quit : 送出一个 quit 的讯号给目前正在 run 的程序
+ start : 在某个程序停止后，重新启动他的 output
+ stop : 停止目前屏幕的输出
+ susp : 送出一个 terminal stop 的讯号给正在 run 的程序。

### 通配符与特殊符号
|通配符和特殊符号|说明|
|-------------|----|
|* |代表0个或多个字符（或数字）|
|? |代表『一定有』一个字母|
|\# |批注，这个最常被使用在 script 当中，视为说明！|
|\ |跳脱符号，将『特殊字符或万用字符』还原成一般字符|
|&#124; |分隔两个管线命令的界定|
|; |连续性命令的界定（注意！与管线命令并不相同）|
|~ |使用者的家目录|
|$ |亦即是变量之前需要加的变量取代值|
|& |将指令变成后台工作|
|! |逻辑运算意义上的『非』 not 的意思！|
|/ |路径分隔的符号|
|>, >> |输出导向，分别是『取代』与『累加』|
|' |单引号，不具有变量置换的功能|
|" |具有变量置换的功能！|
|\` ` |两个『 ` 』中间为可以先执行的指令|
|( ) |在中间为子 shell 的起始与结束|
|[ ] |在中间为字符的组合|
|{ } |在中间为命令区块的组合|

### 字符转换命令
#### tr
tr可以用来删除一段信息当中的文字，或是进行文字讯息的替换。
```
tr [-ds] SET1 ...

-d ：删除讯息当中的 SET1 这个字符串；
-s ：取代掉重复的字符！
```
范例
```
范例一：将 last 输出的讯息中，所有的小写变成大写字符：
[root@linux ~]# last | tr '[a-z]' '[A-Z]'

范例二：将 /etc/passwd 输出的讯息中，将冒号 (:) 删除
[root@linux ~]# cat /etc/passwd | tr -d ':'
```

#### col
将tab键转化为对等的空格键
```
col [-x]

-x ：将 tab 键转换成对等的空格键
范例：
[root@linux ~]# cat -A /etc/man.config <==此时会看到很多 ^I 的符号，那就是 tab
[root@linux ~]# cat /etc/man.config | col -x | cat -A | more
```

### join
处理两个文件之间的数据，而且是处理**两个文件当中，有相同数据的那一行，才将他加在一起**的意思
```
join [-ti12] file1 file2

-t ：join 预设以空格符分隔数据，并且比对『第一个字段』的数据，
如果两个档案相同，则将两笔数据联成一行，且第一个字段放在第一个！
-i ：忽略大小写的差异；
-1 ：这个是数字的 1 ，代表『第一个档案要用那个字段来分析』的意思；
-2 ：代表『第二个档案要用那个字段来分析』的意思
```

范例
```
用 root 的身份，将 /etc/passwd 与 /etc/shadow 相关数据整合成一栏
[root@linux ~]# join -t ':' /etc/passwd /etc/shadow
```

### paste
将两行贴在一起，中间以tab隔开。
```
paste [-d] file1 file2

-d ：后面可以接分隔字符。预设是以 [tab] 来分隔的！
- ：如果 file 部分写成 - ，表示来自 standard input 的资料的意思。
```

范例
```
将 /etc/passwd 与 /etc/shadow 同一行贴在一起
paste /etc/passwd /etc/shadow

bin:x:1:1:bin:/bin:/sbin/nologin bin:*:12959:0:99999:7:::
daemon:x:2:2:daemon:/sbin:/sbin/nologin daemon:*:12959:0:99999:7:::
adm:x:3:4:adm:/var/adm:/sbin/nologin adm:*:12959:0:99999:7:::
# 注意喔！同一行中间是以 [tab] 按键隔开的！
```

#### expand
将tab键转化为空格键
```
expand [-t] file

-t ：后面可以接数字。一般来说，一个 tab 按键可以用 8 个空格键取代。
我们也可以自行定义一个 [tab] 按键代表多少个字符呢！
```

### split:分区命令
将一个大文件，依据大小或行数来分区，将大文件分区为小文件。
```
split [-bl] file PREFIX

-b ：后面可接欲分割成的档案大小，可加单位，例如 b, k, m 等；
-l ：以行数来进行分割
```

范例
```
范例一：我的 /etc/termcap 有七百多 K，若想要分成 300K 一个档案时？
[root@linux ~]# cd /tmp; split -b 300k /etc/termcap termcap
[root@linux tmp]# ls -l termcap*
-rw-rw-r-- 1 root root 307200 8 月 17 00:25 termcapaa
-rw-rw-r-- 1 root root 307200 8 月 17 00:25 termcapab
-rw-rw-r-- 1 root root 184848 8 月 17 00:25 termcapac
# 那个档名可以随意取的啦！我们只要写上前导文字，小档案就会以
# xxxaa, xxxab, xxxac 等方式来建立小档案的！

范例二：如何将上面的三个小档案合成一个档案，档名为 termcapback
[root@linux tmp]# cat termcap* >> termcapback


范例三：使用 ls -al / 输出的信息中，每十行记录成一个档案
[root@linux tmp]# ls -al / | split -l 10 - lsroot
# 重点在那个 - 啦！一般来说，如果需要 stdout/stdin 时，但偏偏又没有档案，有的只是 - 时，那么那个 - 就会被当成 stdin 或 stdout ～ 此处代表stdin
```

### xargs: 参数代换
产生某个指令的参数，作为别的指令的参数。
```
xargs [-0epn] command

-0 ：如果输入的 stdin 含有特殊字符，例如 `, \, 空格键等等字符时，这个 -0 参数可以将他还原成一般字符。这个参数可以用于特殊状态喔！
-e ：这个是 EOF (end of file) 的意思。后面可以接一个字符串，当 xargs 分析到这个字符串时，就会停止继续工作！
-p ：在执行每个指令的 argument 时，都会询问使用者的意思；
-n ：后面接次数，每次 command 指令执行时，要使用几个参数的意思

当 xargs 后面没有接任何的指令时，预设是以 echo 来进行输出
```

范例
```
范例一：将 /etc/passwd 内的第一栏取出，仅取三行，使用 id 这个指令将每个
账号内容秀出来
cut -d ':' -f 1 /etc/passwd |head -n 3| xargs id
# 会出现错误，因为id一次只能接受一个参数
cut -d ':' -f 1 /etc/passwd |head -n 3| xargs -n 1 id
通过-n来处理，一次给予一个参数，因此可以正常的显示了
```
**因为很多命令不支持管道，我们可以用xargs变相让它实现管道功能**

#### -的用途
当需要写数据流的流向时，可以只写一个-，根据情况表示为stdin或stdout。