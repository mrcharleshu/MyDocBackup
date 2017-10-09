### su 和 sudo 的区别：
* 共同点：都是root用户的权限；
* 不同点：
	* su仅仅取得root权限，工作环境不变，还是在切换之前用户的工作环境。  
	（run a shell with substitute user and group IDs）
	* sudo是完全取得root的权限和root的工作环境。  
	（execute a command as another user）


### su - root 和 su root（su）有什么区别？
* su - root:表示人以root身份登录

```
-, -l, --login
	make the shell a login shell, clears all envvars except for TERM, initializes HOME, SHELL, USER, LOGNAME and PATH

just like login as root, then the shell is login shell,which mean it will expericene a login process,usually .bash_profile and .bashrc will be sourced
```

* su root:表示与root建立一个链接，通过root执行命令  
like you open an interactive shell in root name,then only .bashrc will be sourced.

* 最直接的区别就是su目录还是原先用户的目录，但是su或su - root后目录就变为root用户的主目录了。

实践：
vi /root/.bashrc

 ipL=`/sbin/ifconfig eth1|grep "inet addr:"|cut -d: -f 2|cut -d" " -f1`
 export PS1="\u@$ipL:\w# "
 
 alias cdh="cd /usr/local/tads/htdocs/"


我是先root登录后：

```
root@172.25.3*.7*:~# su jackxiang
jackxiang@Tencent:/root> 
发现上面的区别了吧？由root变为jackxiang后，控制台出现不同，再来看看：
jackxiang@Tencent:/root> cdh
bash: cdh: command not found
```
cdh这个不存在，也就是/root/.bashrc这个没有被执行，注意这点。  
我们再 su - root 一下：

```
jackxiang@Tencent:/root> su - root
Password: 
root@172.25.3*.7*:~# 
root@172.25.3*.7*:~# cdh
root@172.25.3*.7*:/usr/local/tads/htdocs#
```

看上面，控制台变了吧，主要原因是什么呢?  
是因为我们在su - root,或者su root 时，这一瞬间其root时去执行了脚本:/root/.bashrc。
它告诉我们，想要修改PATH，PHP，APACHE，Mysql等的路径，都可以到这个脚本中添加即可。