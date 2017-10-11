
### 前言

Shell 是一个用C语言编写的程序，它是用户使用Linux的桥梁。Shell既是一种命令语言，又是一种程序设计语言。这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。

### HelloWorld

`#!` 告诉系统其后路径所指定的程序即是解释此脚本文件的Shell程序。一般使用 `#!/bin/sh` 或者 `#!/bin/bash`

```
#!/bin/bash
echo "Hello,World!"
```

### 变量

**赋值**：等号两边没有空格

```
name="wangyuchao"
echo $name
```

**只读变量**：只读变量不能更改

```
readonly name
```

**删除变量**：被删后不能再次使用，不能删除只读变量

```
unset name
```

### 字符串

**字符串拼接**：三种方式

```
nameNew="Hello,"$name
nameNew="Hello,$name"
nameNew="Hello,${name}"
```

**字符串方法**
```
# 输出字符串长度
echo ${#name}
# 截取字符串:下标
echo ${name:1:4}
# 查找子字符串 wang 位置:下标+1
echo `expr index "$name1" wang`
```

### 数组

**定义数组**

```
allNames=(name0 name1 name2 name3)
```

**分配变量**：下标范围没有限制

```
allNames[4]="name4"
```

**使用@符号输出数组中的所有元素**

```
echo "数组所有元素为" ${allNames[@]}
```

**读取数组中的某个元素**

```
echo "allNames[2]的值为" ${allNames[2]}
```

**获取数组元素个数**

```
echo "数组allNames元素个数为" ${#allNames[@]} 
echo "数组allNames元素个数为" ${#allNames[*]}
```

### 参数传递

**获取参数**：举例：`sh test.sh 0 1 2 3`

```
echo "获得的第一个参数是 ${0} 即 $0"
echo "获得的第二个参数是 ${1} 即 $1"
echo "获得的第三个参数是 ${2} 即 $2"
echo "获得的第四个参数是 ${3} 即 $3"
echo "获得的第五个参数是 ${4} 即 $4"
```

**参数个数**：

```
echo "传递过来的参数个数 $#"
```

**所有参数**

```
echo "传递过来的参数以字符串显示 $*"
echo "传递过来的参数以字符串显示 $@"
```

**$\* 与 $@ 区别**

假设在脚本运行时写了三个参数 1、2、3，则 "*" 等价于 "1 2 3"（传递了一个参数），而 "@" 等价于 "1" "2" "3"（传递了三个参数）

```
echo "------\$*演示--------"
for i in "$*";do
  echo $i
done
echo "------\$@演示--------"
for i in "$@";do
  echo $i
done
```
### 运算符

**数字运算符**

用法如下：注意空格

```
result=`expr 1 + 2`
echo "result:" ${result}
a=1
b=2
result2=`expr $a + $b`
echo "result:" ${result2}
if [ $a == $b ]
then
  echo "a等于b"
fi
```

列表如下

```
+	 加法
-	 减法
*	 乘法
/	 除法
%	 取余
=	 赋值 a=$b 将把变量 b 的值赋给 a。
==   相等
!=   不相等
```

**关系运算符**：关系运算符只支持数字，不支持字符串，除非字符串的值是数字

用法如下

```
if [ $a -eq $b ]
then
  echo "\$a -eq \$b : a 等于 b"
else
  echo "\$a -eq \$b : a 不等于 b"
fi
```

列表如下

```
-eq 是否相等
-ne 是否不等
-gt 是否大于
-lt 是否小于
-ge 是否大于等于
-le 是否小于等于
```

**布尔运算符**：与或非

```
-o 或运算
-a 与运算
!  非运算
```

**逻辑运算符**：And Or

```
&& 运算AND
|| 运算OR
```

**字符串运算符**

用法如下

```
str="wangyuchao"
if [ $str ]
then
  echo "字符串str不为空"
fi
```

列表如下

```
=   两个字符串是否相等
!=  两个字符串是否不等
-z  字符串长度是否为0
-n  字符串长度是否不为0
str 字符串是否为空
```

**文件测试运算符**

用法如下

```
file="/Users/wangyuchao/Desktop/test.sh"
if [ -f $file ]
then
  echo "文件存在"
else
  echo "文件不存在"
fi
```

列表如下

```
-b file	检测文件是否是块设备文件
-c file	检测文件是否是字符设备文件
-d file	检测文件是否是目录
-f file	检测文件是否是普通文件（既不是目录，也不是设备文件）
-g file	检测文件是否设置了 SGID 位
-k file	检测文件是否设置了粘着位(Sticky Bit)
-p file	检测文件是否是有名管道
-u file	检测文件是否设置了 SUID 位
-r file	检测文件是否可读
-w file	检测文件是否可写
-x file	检测文件是否可执行
-s file	检测文件是否为空（文件大小是否大于0）
-e file	检测文件（包括目录）是否存在
```

### echo

**默认**

```
echo this is test.sh
echo "this is test.sh"
```

**转义字符**

```
echo \"this is test.sh\"
```

**换行**

```
echo "this is test.sh\n" # 换行
echo -e "OK! \c"         # -e 开启转义 \c 不换行
```

**原样输出字符串**:使用单引号

```
echo '$name\"'
```

**显示命令执行结果**

```
echo `date`
echo `ls -al`
```

### printf

**用法如下**：跟C语言中的printf用法类似

```
printf "Hello, Shell\n"
printf "%-10s %-8s %-4s\n" 姓名 性别 体重kg
printf "%-10s %-8s %-4.2f\n" 郭靖 男 66.1234
printf "%-10s %-8s %-4.2f\n" 杨过 男 48.6543
printf "%-10s %-8s %-4.2f\n" 郭芙 女 47.9876
printf "%d %s\n" 1 "abc"
printf "%s %s %s\n" a b c d e f g h i j
```

**反义序列列表如下**

```
\a  警告字符，通常为ASCII的BEL字符
\b  后退
\c	抑制（不显示）输出结果中任何结尾的换行字符（只在%b格式指示符控制下的参数字符串中有效）
    而且，任何留在参数里的字符、任何接下来的参数以及任何留在格式字符串中的字符，都被忽略
\f	换页（formfeed）
\n	换行
\r	回车（Carriage return）
\t	水平制表符
\v	垂直制表符
\\	一个字面上的反斜杠字符
\ddd  表示1到3位数八进制值的字符。仅在格式字符串中有效
\0ddd 表示1到3位的八进制值字符
```

### 流程控制

**注意：shell流程控制不可为空**：如果else分支没有执行语句，就不要写这个else。

**if**

```
if condition
then
    command1
    command2
    ...
    commandN
fi
```

**if...else...**

```
if condition
then
    command1
    command2
    ...
    commandN
else
    commandN
fi
```

**if...else if... else...**

```
if condition1
then
    command1
elif condition2
then
    command2
else
    commandN
fi
```

**for**：语法如下

```
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```

示例如下

```
for loop in 1 2 3 4 5
do
  echo "The value is: $loop"
done
```

**while**：语法如下

```
while condition
do
  command
done
```

示例如下

```
int=1
while(( $int<=5 ))
do
  echo $int
  let "int++"
done
# 读取键盘信息
echo "请输入你最喜欢的电影名称，按下ctrl + d 退出"
while read FILM
do
  echo "你刚输入的是" $FILM
done
```

**无限循环**

```
# 无限循环
while :
do
  commandN
done
# 无限循环
while true
do
  commandN
done
# 无限循环
for (( ; ; ))
```

**util循环**：相当于do...while...

```
until condition
do
  commandN
done
```

**case**：语法如下

```
case
case 值 in
  模式一 )
    commandN
    ;;
  模式二 )
    commandN
    ;;
  * )
    commandN
    ;;
esac
```

示例如下

```
while true
do
  echo "输入1-5之间的数字"
  read n
  case $n in
    1|2|3|4|5 )
      echo "输入为1-5范围内"
      ;;
    6 )
      echo "输入为6 continue"
      continue
      ;;
    *)
      echo "输入不在1-5范围内，结束"
      break
      ;;
  esac
done
```

### 函数

**函数格式**

```
function
[ function ] funname [()]
{
  action;
  [return int;]
}
```

示例如下

```
demoFun(){
  echo "this is my first function"
  return 4;
}
demoFun
c=$?
echo "result=" $c
```

**函数参数**：当参数大于10个时 获取参数值 应该用 ${n}

```
$#	传递到脚本的参数个数
$*	以一个单字符串显示所有向脚本传递的参数
$$	脚本运行的当前进程ID号
$!	后台运行的最后一个进程的ID号
$@	与$*相同，但是使用时加引号，并在引号中返回每个参数。
$-	显示Shell使用的当前选项，与set命令功能相同。
$?	显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误
```

示例如下

```
fun(){
  echo "param 0 is " $0
  echo "param 1 is " $1
  echo "param 2 is " $2
  echo "param 3 is " $3
  echo "param 4 is " $4
}
fun 0 1 2 3 4 5 6
```

### 重定向

**知识讲解**

一般情况下，每个 Unix/Linux 命令运行时都会打开三个文件：
标准输入文件(stdin)：stdin的文件描述符为0，Unix程序默认从stdin读取数据。
标准输出文件(stdout)：stdout 的文件描述符为1，Unix程序默认向stdout输出数据。
标准错误文件(stderr)：stderr的文件描述符为2，Unix程序会向stderr流中写入错误信息。
0 是标准输入（STDIN），1 是标准输出（STDOUT），2 是标准错误输出（STDERR）。

**命令列表**

```
command > file	将输出重定向到 file。
command < file	将输入重定向到 file。
command >> file	将输出以追加的方式重定向到 file。
n > file	    将文件描述符为 n 的文件重定向到 file。
n >> file	    将文件描述符为 n 的文件以追加的方式重定向到 file。
n>&m	        将输出文件 m 和 n 合并。
n<&m	        将输入文件 m 和 n 合并。
<< tag	        将开始标记 tag 和结束标记 tag 之间的内容作为输入
```

示例如下

```
who > users.txt      # 输出 who 执行结果到 users.txt 文件中
wc -l users.txt      # 统计 users.txt 中行数
wc -l < users.txt    # 统计 users.txt 中行数
who > users.txt 2>&1 # 输出 who 执行结果到 users.txt 文件中（错误输出和标准输出）
```

**Here Document**：Here Document 是 Shell 中的一种特殊的重定向方式，用来将输入重定向到一个交互式 Shell 脚本或程序。

格式如下

```
command << delimiter
    document
delimiter
```

示例如下

```
echo "here document"
wc -l << EOF
    欢迎来到
    菜鸟教程
    www.runoob.com
EOF
```

### 文件包含

引入其他Shell脚本文件

**格式如下**

```
. filename
或者
source filename
```

**示例如下**

```
. ./test.sh
```