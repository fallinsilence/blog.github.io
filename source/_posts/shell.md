---
title: shell简介
date: 2018-06-12 09:57:05
categories: Linux
tags: Linux
---

### 什么是shell
　　系统kernel(核心)和硬件直接交互，核心发出指令控制硬件工作。我们控制shell，shell传送指令到kelnel，kelnel再控制硬件工作，从而我们控制了硬件工作。
<!-- more -->
>kernel 是『核心』的意思，而 Shell 是『壳』的意思，呵呵！也就是说， shell是最外头的咚咚！而 kernel 乃是最内层的的咚咚啦！核心是操作系统的最底层的东西！ 这个核心里头包括了各种的支持硬件的工具！当然啰，如果你的硬件太新，而你的 kernel 并没有支持的话，那么很抱歉，你的 Shell 能力再怎么强，也没有办法使硬件工作的！ 这样可以了解了吗？呵呵！没错！使计算机主机工作的正是核心的任务，但是操作核心来替使用者工作的，却是 shell 喔！

### /etc/shells文件
　　系统中合法的shell要写入/etc/shells文件中。因为系统某些服务在运作过程中，会去检查使用者能够使用的shells，而这些shell是从/etc/shells文件中查出来的。
>一个用户如果要使用某个shell，/etc/shells文件必须存在这个shell

### shell命令操作快捷键
+ ctrl + u 从光标处**向前**删除指令串
+ ctrl + k 从光标处**向后**删除指令串
+ ctrl + a 让光标移动到整个指令串的**最前面**
+ ctrl + e 让光标移动到整个指令串的**最后面**

### shell变量
#### 变量的创建
1. 创建一个变量
```
	myName=silence
```
	`注意`
    + 等号两边不能直接接空格
    + 变量名称只能是英文字母与数字，并且开头字符不能是数字
    + 变量内容若有空格可使用双引号或单引号将变量内容包裹起来，但有所区别:
		- 双引号内的特殊字符如$等，可以保有原来的特性(可以用$取变量的值)
		```
        	var="lang is $LANG"
            echo $var
            结果为：lang is zh_CN.UTF-8
        ```
		- 单引号内的特殊字符只当纯文本解析
		```
            var="lang is $LANG"
            echo $var
            结果为：lang is $LANG
        ```
2. 可以使用转义字符 \ 将特殊符号（如Enter,$,\,空格符,'等）变成一般字符。
      ```
      	myname=hu\ gang 即可输入空格
        echo $myname
        结果为：hu gang
      ```
3. 在执行指令时，需要读取其他指令的值时，可以使用反单引号*\`指令\`*或*$(指令)*。
    ```
    	version=$(uname -r)
        echo $version
        输出结果为：3.10.0-229.e17.x86_64
    ```
4. 为变量扩增内容，可使用"$变量名" 或 ${变量名}累加内容
	`为PATH变量追加:/home/bin`
```
	PATH="$PATH":/home/bin
    PATH=${PATH}:/home/bin
```
5. 通常大写字符为系统默认变量
6. 取消变量：unset 变量名
```
	unset myname
```
7. 在当前shell设置的变量只能在当前shell有效，若想该变量能在其他子程序执行，则需要用`export 变量名`来使变量变成环境变量
>什么是『子程序』呢？就是说，在我目前这个 shell 的情况下，去启用另一个新的 shell ，新的那个 shell 就是子程序啦！在一般的状态下，父程序的自定义变量是无法在子程序内使用的。但是透过 export 将变量变成环境变量后，就能够在子程序底下应用了！

#### 变量内容的删除
1. 用法
+ \#：从字符串最开始匹配，删除匹配到的字符串中最短的那个
+ \##：删除匹配到的字符串中最长的那个
+ %：从后面向前删除内容。删除匹配到的字符串中最短的那个
+ %%：删除匹配到的字符串中最长的那个

2. 例子
```
	path="/usr/bin:/usr/local/sbin"
    echo ${path#/*:}
    结果为：/usr/local/sbin
```

#### 变量内容的取代
+ ${PATH/sbin/SBIN}    将PATH变量内容内的sbin变为大写
+ /：只会取代第一个匹配的（从前到后）
+ //：会取代所有匹配的

#### 测试变量
当变量不存在或为空字符串时，对他进行处理

|变量设定方式 |str没有设定 |str为空字符串 |str已设定非为空字符串|
|-----------|-----------|------------|------------------|
|var=${str-expr} |var=expr |var= |var=$str|
|var=${str:-expr} |var=expr |var=expr |var=$str|
|var=${str+expr} |var= |var=expr |var=expr|
|var=${str:+expr} |var= |var= |var=expr|
|var=${str=expr}  |str=expr <br /> var=expr  |str不变 <br /> var= |str不变 <br />var=$str|
|var=${str:=expr}  |str=expr <br />var=expr  |str=expr <br />var=expr|str 不变  <br />var=$str|
|var=${str?expr} |expr 输出至stderr |var= |var=$str|
|var=${str:?expr} |expr 输出至stderr |expr输出至stderr| var=str|

