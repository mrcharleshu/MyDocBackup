##### 在Linux下面使用sqlplus很不爽，上下键，退格键都不能用，严重降低生产效率。
##### 某一天终于发现了这个rlwrap这个好东西，特写此文记录。由于时间关系，可以从这里下载安装包。下载后，将.zip扩展名去掉，传到Linux服务器上面。先装上一些安装rpm

### 一. 安装readline
OS的安装光盘里提供了readline包.

```
[root@oracle11g ~]# rpm -Uvh readline*
error: Failed dependencies: libtermcap-devel is needed by readline-devel-5.1-1.1.i386.rpm
[root@oracle11g ~]# rpm -Uvh libtermcap-devel-2.0.8-46.1.i386.rpm
[root@oracle11g ~]# rpm -Uvh readline*
package readline-5.1-1.1 is already installed
[root@oracle11g ~]# rpm -Uvh readline-devel-5.1-1.1.i386.rpm
```
### 二、安装rlwrap
```
[root@oracle11g ~]# tar -zxvf rlwrap-0.30.tar.gz
[root@oracle11g ~]# cd rlwrap-0.30
[root@oracle11g rlwrap-0.30]# ./configure
[root@oracle11g rlwrap-0.30]# make
[root@oracle11g rlwrap-0.30]# make install
```

### 三、方便使用rlwrap
```
[root@oracle11g rlwrap-0.30]# vi /home/oracle/.bash_profile
添加
alias sqlplus='rlwrap sqlplus'
alias rman='rlwrap rman'
```
 
Linux下的SQL Plus 终于可以像Windows下的那样使用了。

***
> TrafodionDB中在trafodion用户中编辑.bashrc文件，加上
> 
> ```
alias trafci="rlwrap trafci"
alias sqlci="rlwrap sqlci"
```