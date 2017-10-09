### Yellowdog Updater Modified
#### yum  is  an  interactive, rpm based, package manager. It can automatically perform system updates, including dependency analysis and obsolete processing based on "repository" metadata. It can also perform installation of new packages, removal of old packages and perform queries on the installed and/or available packages among many other commands/services (see below). yum is similar to other high level package managers like apt-get and smart.

#### While there are some graphical interfaces directly to the yum code, more recent graphical interface development is happening with PackageKit and the gnome-packagekit application

#### YUM基于RPM包管理工具，能够从指定的源空间（服务器，本地目录等）自动下载目标RPM包并且安装，可以自动处理依赖性关系并进行下载、安装，无须繁琐地手动下载、安装每一个需要的依赖包。此外，YUM的另一个功能是进行系统中所有软件的升级。如上所述，YUM的RPM包来源于源空间，在RHEL中由/etc/yum.repos.d/目录中的.repo文件配置指定。YUM的系统配置文件位于/etc/yum.conf。

#### 常用命令
```
yum -y install package-name（安装RPM包）
yum update package-name（升级rpm包）
yum remove package-name（卸载rpm包）
yum list（列出可安装的软件包）
yum list installed（查看在系统上已安装的软件包）
yum info（显示关于软件包或组的详细信息）
yum search（在软件包详细信息中搜索指定字符串）
yum check-update（检查是否有软件包更新）
yum provides /etc/sysconfig/nfs（查看特定文件属于哪个软件包）
yum makecache（创建元数据缓存）
yum repolist（显示已配置的源）
yum install epel-release -y（安装epel源）
```

### 语法
yum (选项) (参数)

### 选项
+ -h：显示帮助信息
+ -y：对所有的提问都回答“yes”
+ -c：指定配置文件
+ -q：安静模式
+ -v：详细模式
+ -d：设置调试等级（0-10）
+ -e：设置错误等级（0-10）
+ -R：设置yum处理一个命令的最大等待时间
+ -C：完全从缓存中运行，而不去下载或者更新任何头文件

### 参数
+ install：安装rpm软件包
+ update：更新rpm软件包
+ check-update：检查是否有可用的更新rpm软件包
+ remove：删除指定的rpm软件包
+ list：显示软件包的信息
+ search：检查软件包的信息
+ info：显示指定的rpm软件包的描述信息和概要信息
+ clean：清理yum过期的缓存
+ shell：进入yum的shell提示符
+ resolvedep：显示rpm软件包的依赖关系
+ localinstall：安装本地的rpm软件包
+ localupdate：显示本地rpm软件包进行更新
+ deplist：显示rpm软件包的所有依赖关系

###部分常用的命令包括：
+ 自动搜索最快镜像插件：``yum install yum-fastestmirror``
+ 安装yum图形窗口插件：``yum install yumex``
+ 查看可能批量安装的列表：``yum grouplist``

#### 安装
```
yum install #全部安装
yum install package1 #安装指定的安装包package1
yum groupinsall group1 #安装程序组group1
```

#### 更新和升级
```
yum update #全部更新
yum update package1 #更新指定程序包package1
yum check-update #检查可更新的程序
yum upgrade package1 #升级指定程序包package1
yum groupupdate group1 #升级程序组group1
```

#### 查找和显示
```
yum info package1 #显示安装包信息package1
yum list #显示所有已经安装和可以安装的程序包
yum list package1 #显示指定程序包安装情况package1
yum groupinfo group1 #显示程序组group1信息
yum search string 根据关键字string查找安装包
```

#### 删除程序
```
yum remove | erase package1 #删除程序包package1
yum groupremove group1 #删除程序组group1
yum deplist package1 #查看程序package1依赖情况
```

#### 清除缓存
```
yum clean packages #清除缓存目录下的软件包
yum clean headers #清除缓存目录下的 headers
yum clean oldheaders #清除缓存目录下旧的 headers
```

#### 包有重复duplicate
```
package-cleanup --dupes #列出重复的包
package-cleanup --cleandupes #删除重复的包
```
执行完上面两个命令， 再yum update成功解决问题，如果还不信，用rpm

```
rpm -e --justdb <package-version>
```
It's important to use the "--justdb" option, and make sure that you specify the version so that you don't delete any files or important packages, just the rpm database entries.

