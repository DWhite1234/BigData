# 1.shell脚本

```shell
shell脚本:shell命令的集合
```

# 2.脚本编写

```she
1.创建一个脚本文件
 可以先touch创建空文件,在vim编辑
 
 也可以直接vim创建并打开编辑
 vim HelloWorld.sh(后缀名没有固定的要求,一般写sh)
2.指定脚本头部(指定解析器)-->默认解析器是bash,一般不指定也没关系
#!/bin/.bash

3.执行脚本的几种方式
	1.bash 文件名
	2.sh 文件名
	3. ./文件名(需要文件的执行权限,没有执行权限显示灰色,有显示绿色)
```

# 3.自定义变量和取值

```shell
1.声明变量(规则和java相似,不能用数字开头)
	a=1//等号两边不能有空格,否则会被认为是三个命令
	a="Hello World"//如果有空格,用单引号或双引号引起来,双引号不能转义特殊字符
	a='Hello $World'//单引号可以转义特殊字符$,让a可以原样输出
	//变量声明默认是字符串类型的,不能直接运算
	
2.使用变量
	echo $a

3.撤销变量
	unset a
	
4.变量修饰符
	readonly :只读
	export :全局
5.只读变量无法撤销
```

# 4.系统变量

```she
1. 常用系统变量
$HOME、$PWD、$SHELL、$USER等

$HOME:当前用户的主目录
$PWD:查看当前目录
$SHELL:查看当前解析器
$USER:查看当前登录用户
```

# 5.特殊变量$n

```shell
$n:
$1:标识当前脚本名称
$2-9:标识第一个到第九个输入参数
${10}:表示第10个及以上变量时,需要使用{}括起来



echo "$0 $1 $1"
  
a=10
b=20

bash HelloWorkd.sh
结果:HelloWorld

```

# 6.特殊变量$#,$*,$@,$？

```shell
    $#:（功能描述：获取所有输入参数个数，常用于循环）获取输入参数的个数
$*:（功能描述：这个变量代表命令行中所有的参数，$*把所有的参数看成一个整体）
$@:（功能描述：这个变量也代表命令行中所有的参数，不过$@把每个参数区分对待）
$？:（功能描述：最后一次执行的命令的返回状态。如果这个变量的值为0，证明上一个命令正确执行；如果这个变量的值为非0（具体是哪个数，由命令自己来决定），则证明上一个命令执行不正确了。）
```

# 7.运算符

```shell
1.$((运算式))或$[运算式]
例如:
	a=$((2+3))
	echo $a
	结果:5
例如:a=$[2+45]
	echo $a
	结果:45
	
2.expr:  + , - , \*(乘号需要转义,不能直接使用*),  /,  %  取余

例如:
	expr 2+3//运算符中间不用空格隔开,结果会原样输出
	结果:2+3
	expr 2 + 3//用空格隔开,则会计算
	结果:5
3.多元运算
例如:(1+2)*3/5
	expr \( 1 + 2 \) \* 3 /5
	结果为:1
例如:
	echo $[(1+2)*3/5]
	结果为:1

注意点:
	1.expr 多元混算需要加\转义(极其麻烦)

```

# 8.条件判断

```shell
1．基本语法
[ condition ]（注意condition前后要有空格）
注意：条件非空即为true，[ atguigu ]返回true，[] 返回false。

2. 常用判断条件

（1）两个整数之间比较
= 字符串比较
-lt 小于（less than）			-le 小于等于（less equal）
-eq 等于（equal）				-gt 大于（greater than）
-ge 大于等于（greater equal）	-ne 不等于（Not equal）

（2）按照文件权限进行判断
-r 有读的权限（read）			-w 有写的权限（write）
-x 有执行的权限（execute）

（3）按照文件类型进行判断
-f 文件存在并且是一个常规的文件（只能是文件）
-e 文件存在（只关注是否有名字相同的,不管是文件还是目录）		
-d 文件存在并是一个目录（只能是目录）

（4）多条件判断
&& 表示前一条命令执行成功时，才执行后一条命令
|| 表示上一条命令执行失败后，才执行下一条命令

注意点:运算符前后要有空格,距离左右的中括号也要有空格
```

# 9.条件判断 if

```shell
1．基本语法
if  [ 条件判断式 ]
then 
  程序 
fi 
或者 
if  [ 条件判断式 ] 
  then 
    程序 
elif  [ 条件判断式 ]
	then
		程序
else
	程序
fi
	注意事项：
（1）[ 条件判断式 ]，中括号和条件判断式之间必须有空格
（2）if后要有空格

案例:
# 判断输入的参数,如果是1,打印你好,如果是2,打印我好,如果是3,打印他好
  
if [ $1 -eq 1 ]
then
        echo "你好"
elif [ $1 -eq 2 ]
then
        echo "我好"
else
        echo "他好
```

# 10.条件判断 case

```shell
1．基本语法
case $变量名 in 
  "值1"） 
    如果变量的值等于值1，则执行程序1 
    ;; 
  "值2"） 
    如果变量的值等于值2，则执行程序2 
    ;; 
  …省略其他分支… 
  *） 
    如果变量的值都不是以上的值，则执行此程序 
    ;; 
esac
注意事项：
1)	case行尾必须为单词“in”，每一个模式匹配必须以右括号“）”结束。
2)	双分号“;;”表示命令序列结束，相当于java中的break。
3)	最后的“*）”表示默认模式，相当于java中的default，*不可以加双引号。


案例:
	case $1 in 1)
        echo "你好";;
        2)
        echo "我好";;
        3)
        echo "他好";;
        *)
        echo "都好";;
	esac

```

# 11.for循环

```shell
1．基本语法1
	for ((初始值;循环控制条件;变量变化)) 
  do 
    程序 
  done


for(( i=0;i<=100;i++))
do
        echo $i
done

```

```shell
2．基本语法2
for 变量 in 值1 值2 值3… 
  do 
    程序 
done

案例:
echo '$*' $*
echo '$#' $#
echo '$@' $@


for i in $@
do
        echo "I love $i"
done




结果:($*和$@执行结果一样,$#输出结果3)
$* xxx zzz ddd
$# 3
$@ xxx zzz ddd
I love xxx
I love zzz
I love ddd





$@和$*的区别
1.不被""引起来时,都表示所有的输入参数,可以作为循环
2.被""引起来时,$*表示整体全部的输入参数,只能一次循环,全部输出
	$@仍然表示每个全部的输入参数,仍然可以作为循环条件
	
例如1:
for i in "$*"
do
        echo "I love $i"
done
结果为:
I love xxx zzz ddd

例如2:
for i in "$@"
do
        echo "I love $i"
done

结果为:
I love xxx
I love zzz
I love ddd



```

# 12.while循环

```shell
1．基本语法
while  [ 条件判断式 ] 
  do 
    程序
  done

例如:
while [ 1 -eq 1 ]
do
        echo "Hello"
done
结果:(无限循环Hello)
Hello
Hello
Hello
Hello
Hello
Hello
Hello
Hello
Hello
Hello
Hello
Hello
Hello
Hello
Hello
Hello
Hello
Hello
Hello
Hello

```

