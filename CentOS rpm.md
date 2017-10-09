#### rpm is a powerful Package Manager, which can be used to build, install, query, verify, update, and erase individual software packages.  A package consists of an archive of files and meta-data used to install and erase the archive files. The meta-data includes helper scripts, file attributes, and descriptive information about the package.  Packages come  in  two  varieties:  binary packages, used to encapsulate software to be installed, and source packages, containing the source code and recipe necessary to produce binary packages.

#### One of the following basic modes must be selected: Query, Verify, Install/Upgrade/Freshen, Uninstall, Set Owners/Groups, Show Querytags, and Show Configuration.


### 选项

+ <font color=red>-a：查询所有安装过的包</font>
+ -b<完成阶段><套件档>+或-t <完成阶段><套件档>+：设置包装套件的完成阶段，并指定套件档的文件名称
+ -c：只列出组态配置文件，本参数需配合"-l"参数使用
+ -d：只列出文本文件，本参数需配合"-l"参数使用
+ -e<套件档>或--erase<套件档>：删除指定的套件
+ -f<文件>+：查询拥有指定文件的套件
+ -h或--hash：套件安装时列出标记
+ -i：显示套件的相关信息
+ -i<套件档>或--install<套件档>：安装指定的套件档
+ -l：显示套件的文件列表
+ -p<套件档>+：查询指定的RPM套件档
+ -q：使用询问模式，当遇到任何问题时，rpm指令会先询问用户
+ -R：显示套件的关联性信息
+ -s：显示文件状态，本参数需配合"-l"参数使用
+ -U<套件档>或--upgrade<套件档>：升级指定的套件档
+ -v：显示指令执行过程
+ -vv：详细显示指令执行过程，便于排错

```
rpm -ivh package.rpm （安装RPM包）
rpm -Uvh package.rpm （升级rpm包）
rpm -ev package（卸载rpm包）
rpm -qa（查看所有安装的软件包）
rpm -qa | grep package（查询已安装rpm包）
rpm -qf <文件名> （快速判定某个文件属于哪个软件包）
rpm -Va （为你列出所有损坏的文件）
```

### 如何安装rpm软件包
rpm软件包的安装可以使用程序rpm来完成。执行下面的命令：

```
rpm -ivh your-package.rpm
```
其中your-package.rpm是你要安装的rpm包的文件名，一般置于当前目录下。
安装过程中可能出现下面的警告或者提示：

```
... conflict with ...
```
可能是要安装的包里有一些文件可能会覆盖现有的文件，缺省时这样的情况下是无法正确安装的可以用``rpm --force -i``强制安装即可

```
... is needed by ...
... is not installed ...
```
此包需要的一些软件你没有安装可以用``rpm --nodeps -i``来忽略此信息，也就是说``rpm -i --force --nodeps``可以忽略所有依赖关系和文件问题，什么包都能安装上，但这种强制安装的软件包不能保证完全发挥功能。


### 如何卸载rpm软件包
使用命令``rpm -e``包名，包名可以包含版本号等信息，但是不可以有后缀.rpm，比如卸载软件包proftpd-1.2.8-1，可以使用下列格式：

```
rpm -ev proftpd-1.2.8-1
rpm -ev proftpd-1.2.8
rpm -ev proftpd-
rpm -ev proftpd
```
不可以是下列格式：

```
rpm -ev proftpd-1.2.8-1.i386.rpm
rpm -ev proftpd-1.2.8-1.i386
rpm -ev proftpd-1.2
rpm -ev proftpd-1
```
有时会出现一些错误或者警告：

```
... is needed by ...
```
这说明这个软件被其他软件需要，不能随便卸载，可以用rpm -e --nodeps强制

### 如何查看与rpm包相关的文件和其他信息
下面所有的例子都假设使用软件包mysql-3.23.54a-11

####1. 我的系统中安装了那些rpm软件包,讲列出所有安装过的包

```
rpm -qa
```
> 如果要查找所有安装过的包含某个字符串sql的软件包
>
> ```
> rpm -qa | grep sql
> ```

####2. 如何获得某个软件包的文件全名。

```
rpm -q mysql
```
可以获得系统中安装的mysql软件包全名，从中可以获得当前软件包的版本等信息。这个例子中可以得到信息mysql-3.23.54a-11

####3. 一个rpm包中的文件安装到那里去了？

```
rpm -ql 包名
```
注意这里的是不包括.rpm后缀的软件包的名称，也就是说只能用mysql或者mysql-3.23.54a-11而不是mysql-3.23.54a-11.rpm。如果只是想知道可执行程序放到那里去了，也可以用which，比如：

```
which mysql
```

####4. 一个rpm包中包含那些文件。
* 一个没有安装过的软件包，使用``rpm -qlp ****.rpm``
* 一个已经安装过的软件包，还可以使用``rpm -ql ****.rpm``

####5. 如何获取关于一个软件包的版本，用途等相关信息？
* 一个没有安装过的软件包，使用``rpm -qip ****.rpm``
*  一个已经安装过的软件包，还可以使用``rpm -qi ****.rpm``

####6. 某个程序是哪个软件包安装的，或者哪个软件包包含这个程序。

```
rpm -qf `which 程序名` #返回软件包的全名
rpm -qif `which 程序名` #返回软件包的有关信息
rpm -qlf `which 程序名` #返回软件包的文件列表
```
> 注意，这里不是引号，而是`，就是键盘左上角的那个键。也可以使用``rpm -qilf``，同时输出软件包信息和文件列表。

####7. 某个文件是哪个软件包安装的，或者哪个软件包包含这个文件。

> 注意，前一个问题中的方法，只适用与可执行的程序，而下面的方法，不仅可以用于可执行程序，也可以用于普通的任何文件。前提是知道这个文件名。首先获得这个程序的完整路径，可以用whereis或者which，然后使用``rpm -qf``
例如：

```
whereis ftptop
ftptop: /usr/bin/ftptop /usr/share/man/man1/ftptop.1.gz

rpm -qf /usr/bin/ftptop
proftpd-1.2.8-1

rpm -qf /usr/share/doc/proftpd-1.2.8/rfc/rfc0959.txt
proftpd-1.2.8-1
```

