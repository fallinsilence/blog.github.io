---
title: Linux一些配置文件
date: 2018-06-14 09:05:02
categories: Linux
---
### bash的进站与欢迎讯息
1. /etc/issue
本机登录时显示的欢迎信息

2. /etc/issue.net
远程登录时显示的欢迎信息
<!-- more -->
```
issue 内的各代码意义
\d 本地端时间的日期；
\l 显示第几个终端机接口；
\m 显示硬件的等级 (i386/i486/i586/i686...)；
\n 显示主机的网络名称；
\o 显示 domain name；
\r 操作系统的版本 (相当于 uname -r)
\t 显示本地端时间的时间；
\s 操作系统的名称；
\v 操作系统的版本。
```

3. /etc/motd
每个用户登录后都能看到的讯息

### bash的环境配置文件
#### login shell 与 non-login shell
重点在于有没有输入用户名密码来登入bash
+ login shell：取得bash时需要完整的登入流程的。从tty1-tty6登录
+ non-login shell:取得bash接口的方法不需要重复登入的举动，举例来说
	+ 以X window登入linux后，再以X的图形化接口启动终端机，此时那个终端接口并没有需要再次的输入账号与密码；
	+ 你在原本的bash环境下再次下达bash指令，同样的也没有输入账号密码，那第二个bash（子程序）也是non-login shell。

#### login shell 会读取两个文件
+ /etc/profile：这是系统整体的设定，你最好不要更改这个文件
+  ~/.bash_profile或~/.bash_login或~/.profile：术语使用者个人设定文件。
**可能引入~/.bashrc文件**

login shell读取配置文件流程
![login_shell](https://github.com/fallinsilence/usedPictures/blob/master/Linux/login_shell.png?raw=true)

#### /etc/profile
　　这个配置文件可以利用使用者的标识符(UID) 来决定很多重要的变量数据，这也是每个使用者登入取得bash时一定会读取的配置文件!所以如果你想要帮所有使用者设定整体环境，那就是改这里啰!不过，没事还是不要随便改这个文件喔这个文件设定的变量主要有:
+ PATH:会依据UID决定PATH变量要不要含有sbin 的系统指令目录;
+ MAlL:依据账号设定好使用者的mailbox到/var/spool/mail/账号名
+ USER:根据用户的账号设定此变量内容;
+ HOSTNAME:依据主机的hostname指令决定此变量内容;
+ HISTSIZE:历史命令记录笔数。
+ umask:包括root 默认为022,而一般用户为002等!
**/etc/profile可不止会做这些事而已，他还会去呼叫外部的设定数据喔!**

#### ~/.bash_profile
bash在读取了整体环境设定的/etc/profile后，接下来则是读取使用者的个人配置文件。依次为
1. ~./.bash_profile
2. ~/.bash_login
3. ~/.profile

`注意:`其实bash的login shell设定只会读取上面三个文件的其中一个，而读取的顺序是按照上面的顺序。

#### ~/.bashrc（最后读取）
当你取得non-login shell时，该bash配置文件仅会读取~/.bashrc

#### source(或者.):读入配置文件，让配置文件立即生效
让配置文件不用注销，立即生效
```
	source ~/.bashrc
    . ~/.bashrc
```

###  其他配置
+ /etc/man_db.conf:规范了使用man时，到哪里去寻找man page的路径
+ ~/.bash_history：存储了历史命令
+ ~/.bash_logout：注销bash时，系统执行的操作