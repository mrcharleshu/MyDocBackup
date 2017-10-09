### 1、语法：

Aho、Weinberger、Kernighan三位发明者名字首字母，三人开发的一个行文本处理工具

##### awk 'pattern + {action}' 说明：

1. 单引号''是为了和shell命令区分开；
2. 大括号{}表示一个命令分组；
3. pattern是一个过滤器，表示命中pattern的行才进行action处理；
4. action是处理动作；
5. 使用#作为注释；
例子：显示hello.txt中的第3行至第5行

```
cat hello.txt | awk 'NR==3, NR==5{print;}'
```
### 2、pattern说明

pattern参数可以是egrep正则中的一个，正则使用/pattern/ 例子：显示hello.txt中，正则匹配hello的行

```
cat hello.txt | awk '/hello/'
```
说明：

* pattern和action可以只有其一，但不能两者都没有；
* 默认的action是print；
例子：显示hello.txt中，长度大于100的行号

```
cat hello.txt | awk 'length($0)>80{print NR}'
```

### 3、内置变量

* FS 分隔符，默认是空格
* NR 当前行数，从1开始
* NF 当前记录字段个数
* $0 当前记录
* $1~$n 当前记录第n个字段 例子：显示hello.txt中的第3行至第5行的第一列与最后一列

```
cat hello.txt | awk 'NR==3, NR==5{print $1,$NF}'
```

### 4、内置函数

* gsub(r,s)：在$0中用s代替r
* index(s,t)：返回s中t的第一个位置
* length(s)：s的长度
* match(s,r)：s是否匹配r
* split(s,a,fs)：在fs上将s分成序列a
* substr(s,p)：返回s从p开始的子串

### 5、操作符
* 运算符 类似于c，支持+、-、*、/、%、++、–、+=、-=等诸多操作；
* 判断符 类似于c，支持==、!=、>、=>、~（匹配于）等诸多判断操作；

### 6、常用命令

* 打印文件的第一列(域) ： awk '{print $1}' filename
* 打印文件的前两列(域) ： awk '{print $1,$2}' filename
* 打印文本文件的总行数 ： awk 'END{print NR}' filename
* 打印文本第一行 ：awk 'NR==1{print}' filename
* 打印文本第二行第一列 ：sed -n "2, 1p" filename | awk 'print $1'
* 打印完第一列，然后打印第二列 ： awk '{print $1 $2}' filename
* 给文件开头添加行号：awk '$0=NR":"$0' filename
* 最短行：awk '(NR==1||length(min)>length()){min=$0}END{print min}' filename
* 最长行：awk '{if (length(max)<length()) max=$0}END{print max}' filename

