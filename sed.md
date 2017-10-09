[来自博客](http://www.cnblogs.com/ggjucheng/archive/2013/01/13/2856901.html)

### sed [-nefri] [动作]
选项与参数：

+ -n ：使用安静(silent)模式。在一般 sed 的用法中，所有来自 STDIN 的数据一般都会被列出到终端上。但如果加上 -n 参数后，则只有经过sed 特殊处理的那一行(或者动作)才会被列出来。
+ -e ：直接在命令列模式上进行 sed 的动作编辑；
+ -f ：直接将 sed 的动作写在一个文件内， -f filename 则可以运行 filename 内的 sed 动作；
+ -r ：sed 的动作支持的是延伸型正规表示法的语法。(默认是基础正规表示法语法)
+ -i ：直接修改读取的文件内容，而不是输出到终端。

### 动作说明： [n1[,n2]]function
n1, n2 ：不见得会存在，一般代表『选择进行动作的行数』，举例来说，如果我的动作是需要在10到20行之间进行的，则『 10,20[动作行为] 』

### sed命令：
+ a ：新增， a 的后面可以接字串，而这些字串会在新的一行出现（目前的下一行）
+ c ：取代， c 的后面可以接字串，这些字串可以取代 n1,n2 之间的行
+ d ：删除，因为是删除啊，所以 d 后面通常不接任何东西
+ g ：获得内存缓冲区的内容，并替代当前模板块中的文本（行内全面替换）
+ G ：获得内存缓冲区的内容，并追加到当前模板块文本的后面
+ __h ：拷贝模板块的内容到内存中的缓冲区__
+ H ：追加模板块的内容到内存中的缓冲区
+ i ：插入，i 的后面可以接字串，而这些字串会在新的一行出现（目前的上一行）
+ __n ：读取下一个输入行，用下一个命令处理新的行而不是用第一个命令__
+ N ：追加下一个输入行到模板块后面并在二者间嵌入一个新行，改变当前行号码
+ p ：列印，亦即将某个选择的数据印出。通常 p 会与参数 sed -n 一起运行
+ s ：取代，可以直接进行取代的工作哩！通常这个 s 的动作可以搭配正规表示法！例如 1,20s/old/new/g 就是
+ w ：表示把行写入一个文件
+ W ：file 写并追加模板块的第一行到file末尾
+ x ：表示互换模板块中的文本和缓冲区中的文本
+ y ：表示把一个字符翻译为另外的字符（但是不用于正则表达式）
+ \1：子串匹配标记
+ <font color=red>__& ：已匹配字符串标记__</font>
+ ! 表示后面的命令对所有没有被选定的行发生作用
+ = 打印当前行号码
+ \#把注释扩展到下一个换行符以前

### sed替换标记
__g p w x y \1 &__

### 定界符
以上命令中字符 / 在sed中作为定界符使用，也可以使用任意的定界符：

```
sed 's:test:TEXT:g'
sed 's|test|TEXT|g'
```
> 定界符出现在样式内部时，需要进行转义：
> 
```
sed 's/\/bin/\/usr\/local\/bin/g'
```

#### 把test内容中单引号替换成双引号
```
sed 's/'"'"/'"''/g' test  ==> sed 's/' " ' " / ' " ' '/g' test
解析下：
's/' => 要进行替换操作，后紧跟匹配字符
"'"  => 用双引号包裹着单引号
/    => 分割符
'"'  => 用单引号包裹着双引号
'/g' => 分隔符，全局替换
```

```
sed 's/\]/\"/g'   替换]为"
sed 's/\[/\"/g'   替换[为"
```

### 对每行处理，文本替换

#### 1.替换：s命令
把文件file中出现jcdd的换出ganji.

```
sed 's/jcdd/ganji/g' file;
```
> g标志在整行范围内把jcdd都替换为ganji。如果没有g标记，则只有每行第一个匹配的jcdd被替换成ganji。g换出Ng代表第N处开始出现的替换  

```
sed -n 's/^jcdd/ganji/p' file;  
```
> -n选项和p标志一起使用表示只打印那些发生替换的行，如果某一行开头的jcdd被替换成ganji，就打印它。  

```
sed 's/^192.168.0.1/&localhost/' file;
```
> &符号表示替换换字符串中被找到的部份。所有以192.168.0.1开头的行都会被替换成它自已加localhost，变成192.168.0.1localhost。

```
sed -i 's/jcdd/ganji/g' file;
```
> -i选项可以使替换后的文件保存更新

<font color=red>__当需要从第N处匹配开始替换时，可以使用 /Ng：__

```
$ echo sksksksksksk | sed 's/sk/SK/2g'
skSKSKSKSKSK
$ echo "title.header.01.the.first.subtitle.is.here.footer" | sed 's/\./_/3g’
title.header.01_the_first_subtitle_is_here_footer
```
</font>

#### 2.删除：d命令
```
sed '/^$/d' file 移除空白行，空白行用^$匹配
sed '2d' file 删除第二行
sed '2,$d' file 删除第二行到尾行之间的所有行
sed '$d' file 删除文件最后一行
sed '/jcdd/d' file 删除包含jcdd的行
sed '/^test/'d file 删除文件中所有开头是test的行
```

#### 3.查询：p命令
```
sed -n '/jcdd/p' file; 显示包含jcdd的所有行
```

***

### 增加一行
```
sed '1a drink tea' file; 第一行后增加字符串drink tea
sed '1,3a drink tea' file; 第一行到第三行后加字符串
```

### 在一个文件中的开头可能有一个或者多个空格，我们需要将这些开头的空格删除。
```
sed 's/^[][ ]*//g' filename
```

### 替换分隔符
文件中数据是由一个或者制表位（多个空格）分隔开的，将这些空格替换为特定字符

```
sed -e 's/[ ][ ]*/,/g' filename
sed -e 's/[[:space:]][[:space:]]*/ /g' filename
```
这样将空格或者制表位替换为”逗号"了。

### 选定行的范围：,（逗号）
+ 所有在模板test和check所确定的范围内的行都被打印
 
```
sed -n '/test/,/check/p' file
```

+ 打印从第5行开始到第一个包含以test开始的行之间的所有行

```
sed -n '5,/^test/p' file
```

+ 对于模板test和west之间的行，每行的末尾用字符串aaa bbb替换

```
sed '/test/,/west/s/$/aaa bbb/' file
```

### 引用
sed表达式可以使用单引号来引用，但是如果表达式内部包含变量字符串，就需要使用双引号

```
test=hello
echo hello WORLD | sed "s/$test/HELLO"
HELLO WORLD
```

### <font color=red>已匹配字符串标记&</font>
正则表达式 \w\+ 匹配每一个单词，使用 [&] 替换它，& 对应于之前所匹配到的单词

```
echo this is a test line | sed 's/\w\+/[&]/g'
[this] [is] [a] [test] [line]
```
所有以192.168.0.1开头的行都会被替换成它自已加localhost

```
sed 's/^192.168.0.1/&localhost/' file
192.168.0.1localhost
```

### 子串匹配标记\1
匹配给定样式的其中一部分

```
echo this is digit 7 in a number | sed 's/digit \([0-9]\)/\1/'
this is 7 in a number
```
命令中 digit 7，被替换成了 7。样式匹配到的子串是 7，\(..\) 用于匹配子串，对于匹配到的第一个子串就标记为 \1，依此类推匹配到的第二个结果就是 \2，例如：

```
echo aaa BBB | sed 's/\([a-z]\+\) \([A-Z]\+\)/\2 \1/'
BBB aaa
```
love被标记为1，所有loveable会被替换成lovers，并打印出来：

```
sed -n 's/\(love\)able/\1rs/p' file
```

###多点编辑：e命令
* -e选项允许在同一行里执行多条命令

```
sed -e '1,5d' -e 's/test/check/' file
```
上面sed表达式的第一条命令删除1至5行，第二条命令用check替换test。命令的执行顺序对结果有影响。如果两个命令都是替换命令，那么第一个替换命令将影响第二个替换命令的结果。

* 和 -e 等价的命令是 --expression

```
sed --expression='s/test/check/' --expression='/love/d' file
```

### 从文件读入：r命令
file里的内容被读进来，显示在与test匹配的行后面，如果匹配多行，则file的内容将显示在所有匹配行的下面：

```
sed '/test/r file' filename
```

### 写入文件：w命令
在example中所有包含test的行都被写入file里：

```
sed -n '/test/w file' example
```

### 追加（行下）：a\命令
将 this is a test line 追加到 以test 开头的行后面：

```
sed '/^test/a\this is a test line' file
```
在 test.conf 文件第2行之后插入 this is a test line：

```
sed -i '2a\this is a test line' test.conf
```

### 插入（行上）：i\命令
将 this is a test line 追加到以test开头的行前面：

```
sed '/^test/i\this is a test line' file
```
在test.conf文件第5行之前插入this is a test line：

```
sed -i '5i\this is a test line' test.conf
```

### 下一个：n命令
如果test被匹配，则移动到匹配行的下一行，替换这一行的aa，变为bb，并打印该行，然后继续：

```
sed '/test/{ n; s/aa/bb/; }' file
```

### 变形：y命令
把1~10行内所有abcde转变为大写，注意，正则表达式元字符不能使用这个命令：

```
sed '1,10y/abcde/ABCDE/' file
```

### 退出：q命令
打印完第10行后，退出sed

```
sed '10q' file
```

### 保持和获取：h命令和G命令
在sed处理文件的时候，每一行都被保存在一个叫模式空间的临时缓冲区中，除非行被删除或者输出被取消，否则所有被处理的行都将 打印在屏幕上。接着模式空间被清空，并存入新的一行等待处理。

```
sed -e '/test/h' -e '$G' file
```
> 在这个例子里，匹配test的行被找到后，将存入模式空间，h命令将其复制并存入一个称为保持缓存区的特殊缓冲区内。第二条语句的意思是，当到达最后一行后，G命令取出保持缓冲区的行，然后把它放回模式空间中，且追加到现在已经存在于模式空间中的行的末尾。在这个例子中就是追加到最后一行。简单来说，任何包含test的行都被复制并追加到该文件的末尾。

### 保持和互换：h命令和x命令
互换模式空间和保持缓冲区的内容。也就是把包含test与check的行互换：

```
sed -e '/test/h' -e '/check/x' file
```

### 脚本scriptfile
sed脚本是一个sed的命令清单，启动Sed时以-f选项引导脚本文件名。Sed对于脚本中输入的命令非常挑剔，在命令的末尾不能有任何空白或文本，如果在一行中有多个命令，要用分号分隔。以#开头的行为注释行，且不能跨行。
> sed [options] -f scriptfile file(s)
打印奇数行或偶数行

+ 方法1：

```
sed -n 'p;n' file  奇数行
sed -n 'n;p' file  偶数行
```

- 方法2：

```
sed -n '1~2p' file  奇数行
sed -n '2~2p' file  偶数行
```

### 打印匹配字符串的下一行
```
grep -A 1 SCC file
sed -n '/SCC/{n;p}' file
awk '/SCC/{getline; print}' file
```
