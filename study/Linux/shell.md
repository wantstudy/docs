# Shell 简介

Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种程序设计语言。

Shell 是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。

Ken Thompson 的 sh 是第一种 Unix Shell，Windows Explorer 是一个典型的图形界面 Shell。

## 编写

	#!/bin/bash
	echo "Hello World !"

**说明**:

	#! 是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行，即使用哪一种 Shell。
	echo 命令用于向窗口输出文本。

## 运行

1. 作为可执行程序

> 将上面的代码保存为 test.sh，并 cd 到相应目录：

	chmod +x ./test.sh  #使脚本具有执行权限
	./test.sh  #执行脚本

注意，一定要写成 `./test.sh`，而不是 `test.sh`，运行其它二进制的程序也一样，直接写 test.sh，linux 系统会去 PATH 里寻找有没有叫 test.sh 的，而只有 /bin, /sbin, /usr/bin，/usr/sbin 等在 PATH 里，你的当前目录通常不在 PATH 里，所以写成 test.sh 是会找不到命令的，要用 ./test.sh 告诉系统说，就在当前目录找。

2. 作为解释器参数

> 这种运行方式是，直接运行解释器，其参数就是 shell 脚本的文件名，如：

	/bin/sh test.sh

这种方式运行的脚本，不需要在第一行指定解释器信息，写了也没用。


## 变量

> 声明变量时,不用加美元符号 $ ,例如

	user_name="admin"

**注意**，`变量名`和`等号`之间不能有空格，这可能和你熟悉的所有编程语言都不一样。同时，变量名的命名须遵循如下规则：

+ 命名只能使用英文字母，数字和下划线，首个字符不能以数字开头。
+ 中间不能有空格，可以使用下划线（_）。
+ 不能使用标点符号。
+ 不能使用bash里的关键字（可用help命令查看保留关键字）

> 除了直接给变量赋值,也可以使用语句给变量赋值,例如

	for file in `ls /etc`
	或者
	for file in $(ls /etc)

> 使用变量,花括号加不加都可以,是为了识别变量的边界

	user_name="admin"
	echo $user_name
	echo ${user_name}

> 只读变量 readonle

	#!/bin/bash
	myUrl="http://www.google.com"
	readonly myUrl
	myUrl="http://www.runoob.com"

重新给变量 myUrl 赋值报错

> 删除变量,使用 unset 命令可以删除变量。语法：

	unset variable_name

变量被删除后不能再次使用。unset 命令不能删除只读变量

### 字符串
_字符串是shell编程中最常用最有用的数据类型（除了数字和字符串，也没啥其它类型好用了），字符串可以用单引号，也可以用双引号，也可以不用引号_

	str='this is a \"string\"'
	str="this is a string, ${user_name}"
	

**注意**
+ 双引号里可以有变量
+ 双引号里可以出现转义字符
+ 如果不用引号,需保证变量没有空格

> 获取字符串长度

	string="abcd"
	echo ${#string} #输出 4

> 提取子字符串
	从字符串第 2 个字符开始截取 4 个字符：

	string="runoob is a great site"
	echo ${string:1:4} # 输出 unoo

> 查找子字符串
	查找字符 i 或 o 的位置(哪个字母先出现就计算哪个)：

	string="runoob is a great site"
	echo `expr index "$string" io`  # 输出 4

### 数组	
_bash支持一维数组（不支持多维数组），并且没有限定数组的大小。_

	数组名=(值1 值2 ... 值n)

> 定义数组

	array_name=(value0 value1 value2 value3)
	或者
	array_name=(
		value0
		value1
		value2
		value3
	)
	或者
	array_name[0]=value0
	array_name[1]=value1
	array_name[n]=valuen

> 读取数组

 	# n 为下标,从0开始
	valuen=${array_name[n]} 

	#取数组中的所有元素
	echo ${array_name[@]} 

	# 取得数组元素的个数
	length=${#array_name[@]}
	# 或者
	length=${#array_name[*]}

	# 取得数组单个元素的长度
	lengthn=${#array_name[n]}

## 传递参数
_我们可以在执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为：$n。n 代表一个数字，0 为文件名称, 1 为执行脚本的第一个参数，2 为执行脚本的第二个参数，以此类推…_

|参数处理|说明|
|------|------|
|$#	|传递到脚本的参数个数|
|$*	|以一个单字符串显示所有向脚本传递的参数。如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。|
|$$	|脚本运行的当前进程ID号|
|$!	|后台运行的最后一个进程的ID号|
|$@	|与$*相同，但是使用时加引号，并在引号中返回每个参数。如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。|
|$-	|显示Shell使用的当前选项，与set命令功能相同。|
|$?	|显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。|

test.sh 内容如下:

	#!/bin/bash
	echo "Shell 传递参数实例！";
	echo "执行的文件名：$0";
	echo "第一个参数为：$1";
	echo "第二个参数为：$2";
	echo "第三个参数为：$3";

执行后:

	$ chmod +x test.sh 
	$ ./test.sh 1 2 3
	Shell 传递参数实例！
	执行的文件名：./test.sh
	第一个参数为：1
	第二个参数为：2
	第三个参数为：3


## 运算符

原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用。

expr 是一款表达式计算工具，使用它能完成表达式的求值操作。

例如，两个数相加(注意使用的是反引号 \` 而不是单引号 ')：

	#!/bin/bash

	val=`expr 2 + 2`
	echo "两数之和为 : $val"

### 算数运算符

|运算符|说明|举例|
|------|------|------|
|+	|加法|	\`expr $a + $b` 结果为 30。|
|-	|减法|	\`expr $a - $b` 结果为 -10。|
|*	|乘法|	\`expr $a \* $b` 结果为  200。|
|/	|除法|	\`expr $b / $a` 结果为 2。|
|%	|取余|	\`expr $b % $a` 结果为 0。|
|=	|赋值|	a=$b 将把变量 b 的值赋给 a。|
|==	|相等| 用于比较两个数字，相同则返回 true。	[ $a == $b ] 返回 false。|
|!=|不相等|用于比较两个数字，不相同则返回 true。	[ $a != $b ] 返回 true。|


### 关系运算符
关系运算符只支持数字，不支持字符串，除非字符串的值是数字

下表列出了常用的关系运算符，假定变量 a 为 10，变量 b 为 20：
|运算符|说明|举例|
|------|------|------|
|-eq|	检测两个数是否相等，相等返回 true。	|[ $a -eq $b ] 返回 false。|
|-ne|	检测两个数是否不相等，不相等返回 true。|	[ $a -ne $b ] 返回 true。|
|-gt|	检测左边的数是否大于右边的，如果是，则返回 true。|	[ $a -gt $b ] 返回 false。|
|-lt|	检测左边的数是否小于右边的，如果是，则返回 true。|	[ $a -lt $b ] 返回 true。|
|-ge|	检测左边的数是否大于等于右边的，如果是，则返回 true。|	[ $a -ge $b ] 返回 false。|
|-le|	检测左边的数是否小于等于右边的，如果是，则返回 true。|	[ $a -le $b ] 返回 true。|

### 布尔运算符

下表列出了常用的布尔运算符，假定变量 a 为 10，变量 b 为 20：
|运算符|说明|举例|
|------|------|------|
|!|	非运算，表达式为 true 则返回 false，否则返回 true。|	[ ! false ] 返回 true。|
|-o|	或运算，有一个表达式为 true 则返回 true。|	[ $a -lt 20 -o $b -gt 100 ] 返回 true。|
|-a|	与运算，两个表达式都为 true 才返回 true。|	[ $a -lt 20 -a $b -gt 100 ] 返回 false。|


### 逻辑运算符

以下介绍 Shell 的逻辑运算符，假定变量 a 为 10，变量 b 为 20:

|运算符|说明|举例|
|------|------|------|
|&&	|逻辑的 AND	[[ $a -lt 100 && $b -gt 100 ]] 返回 false|
|\|\||	逻辑的 OR	[[ $a -lt 100 || $b -gt 100 ]] 返回 true|

### 字符串运算符
下表列出了常用的字符串运算符，假定变量 a 为 "abc"，变量 b 为 "efg"：
|运算符|说明|举例|
|------|------|------|
|=	|检测两个字符串是否相等，相等返回 true。|	[ $a = $b ] 返回 false。|
|!=	|检测两个字符串是否相等，不相等返回 true。|	[ $a != $b ] 返回 true。|
|-z	|检测字符串长度是否为0，为0返回 true。|	[ -z $a ] 返回 false。|
|-n	|检测字符串长度是否为0，不为0返回 true。|	[ -n "$a" ] 返回 true。|
|str|	检测字符串是否为空，不为空返回 true。|	[ $a ] 返回 true。|

### 文件测试运算符
|操作符|说明|举例|
|------|------|------|
|-b file|	检测文件是否是块设备文件，如果是，则返回 true。	|[ -b $file ] 返回 false。
|-c file|	检测文件是否是字符设备文件，如果是，则返回 true。	|[ -c $file ] 返回 false。
|-d file|	检测文件是否是目录，如果是，则返回 true。	|[ -d $file ] 返回 false。
|-f file|	检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。	|[ -f $file ] 返回 true。
|-g file|	检测文件是否设置了 SGID 位，如果是，则返回 true。	|[ -g $file ] 返回 false。
|-k file|	检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。	|[ -k $file ] 返回 false。
|-p file|	检测文件是否是有名管道，如果是，则返回 true。	|[ -p $file ] 返回 false。
|-u file|	检测文件是否设置了 SUID 位，如果是，则返回 true。	|[ -u $file ] 返回 false。
|-r file|	检测文件是否可读，如果是，则返回 true。|	[ -r $file ] 返回 true。
|-w file|	检测文件是否可写，如果是，则返回 true。	|[ -w $file ] 返回 true。
|-x file|	检测文件是否可执行，如果是，则返回 true。	|[ -x $file ] 返回 true。
|-s file|	检测文件是否为空（文件大小是否大于0），不为空返回 true。|	[ -s $file ] 返回 true。
|-e file|	检测文件（包括目录）是否存在，如果是，则返回 true。|	[ -e $file ] 返回 true


## printf 命令
printf 使用引用文本或空格分隔的参数，外面可以在 printf 中使用格式化字符串，还可以制定字符串的宽度、左右对齐方式等

	printf  format-string  [arguments...]

参数说明：

	format-string: 为格式控制字符串
	arguments: 为参数列表。

> 格式化

	#!/bin/bash

	printf "%-10s %-8s %-4s\n" 姓名 性别 体重kg  
	printf "%-10s %-8s %-4.2f\n" 郭靖 男 66.1234 
	printf "%-10s %-8s %-4.2f\n" 杨过 男 48.6543 
	printf "%-10s %-8s %-4.2f\n" 郭芙 女 47.9876 

输出

	姓名     性别   体重kg
	郭靖     男      66.12
	杨过     男      48.65
	郭芙     女      47.99

%s %c %d %f都是格式替代符

%-10s 指一个宽度为10个字符（-表示左对齐，没有则表示右对齐），任何字符都会被显示在10个字符宽的字符内，如果不足则自动以空格填充，超过也会将内容全部显示出来。

%-4.2f 指格式化为小数，其中.2指保留2位小数。

## 流程控制

### if else

> if

	if condition
	then
	    command1 
	    command2
	    ...
	    commandN 
	fi

> if else 

	if condition
	then
	    command1 
	    command2
	    ...
	    commandN
	else
	    command
	fi

> if else-if else 
	if condition1
	then
	    command1
	elif condition2 
	then 
	    command2
	else
	    commandN
	fi

### for 循环

	for var in item1 item2 ... itemN
	do
	    command1
	    command2
	    ...
	    commandN
	done

	for var in item1 item2 ... itemN; do command1; command2… done;

### while 语句

	while condition
	do
	    command
	done

	echo '按下 <CTRL-D> 退出'
	echo -n '输入你最喜欢的网站名: '
	while read FILM
	do
	    echo "是的！$FILM 是一个好网站"
	done

### case 语句

	echo '输入 1 到 4 之间的数字:'
	echo '你输入的数字为:'
	read aNum
	case $aNum in
	    1)  echo '你选择了 1'
	    ;;
	    2)  echo '你选择了 2'
	    ;;
	    3)  echo '你选择了 3'
	    ;;
	    4)  echo '你选择了 4'
	    ;;
	    *)  echo '你没有输入 1 到 4 之间的数字'
	    ;;
	esac

### break(跳出循环) / continue(跳出本次循环)

	#!/bin/bash
	while :
	do
	    echo -n "输入 1 到 5 之间的数字:"
	    read aNum
	    case $aNum in
	        1|2|3|4|5) echo "你输入的数字为 $aNum!"
	        ;;
	        *) echo "你输入的数字不是 1 到 5 之间的! 游戏结束"
	            break
	        ;;
	    esac
	done

## 函数

	#!/bin/bash
	funWithParam(){
	    echo "第一个参数为 $1 !"
	    echo "第二个参数为 $2 !"
	    echo "第十个参数为 $10 !"
	    echo "第十个参数为 ${10} !"
	    echo "第十一个参数为 ${11} !"
	    echo "参数总数有 $# 个!"
	    echo "作为一个字符串输出所有参数 $* !"
	}
	funWithParam 1 2 3 4 5 6 7 8 9 34 73


## 文件包含

Shell 也可以包含外部脚本。这样可以很方便的封装一些公用的代码作为一个独立的文件

	. filename   # 注意点号(.)和文件名中间有一空格

	或

	source filename

> 创建两个 shell 脚本文件。

`test1.sh` 代码如下：

	#!/bin/bash
	url="http://www.baidu.com"

`test2.sh` 代码如下：

	#!/bin/bash
	#使用 . 号来引用test1.sh 文件
	. ./test1.sh

	# 或者使用以下包含文件代码
	# source ./test1.sh

	echo "地址：$url"

接下来，我们为 test2.sh 添加可执行权限并执行：

	$ chmod +x test2.sh 
	$ ./test2.sh 
	地址：http://www.baidu.com

被包含的文件 test1.sh 不需要可执行权限。