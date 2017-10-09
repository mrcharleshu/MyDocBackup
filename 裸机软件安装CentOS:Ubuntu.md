## CentOS配置系统软件
#### 查看版本并更新yum和清除过期的缓存
```
cat /etc/redhat-release
yum update 更新
yum clean all && yum update -y
```
#### 安装常用软件包
```
yum install wget curl tree htop zip unzip ntp zsh vim man gcc gcc-c++ -y
# bash用autojump／zsh用autojump-zsh
```
可选包：

* git rlwrap ethtool telnet epel-release
* nc(ncat - Concatenate and redirect sockets)
* openssh-clients是ssh-copy-id和scp的安装包名（yum install openssh-clients -y）
* openssh-server是sshd的安装包名（yum install openssh-server -y）
* yum-utils是package-cleanup的安装包名

> ./configure error: SSL modules require the OpenSSL library.
> ``yum install openssl openssl-devel -y``

#### 安装zsh
```
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
# 备份zsh
cp .zshrc .zshrc.bak
yum -y install autojump-zsh
# 添加plugin autojump history
sed -i 's/(git)/(git autojump history)/g' .zshrc
```
#### Bash下的autojump
在 Debian 及其衍生系统 (Ubuntu, Mint,…) 中, 激活 autojump 应用是非常重要的。
为了暂时激活 autojump 应用，即直到你关闭当前会话或打开一个新的会话之前让 autojump 均有效，你需要以常规用户身份运行下面的命令:

```
$ source /usr/share/autojump/autojump.bash on startup
# 为了使得 autojump 在 BASH shell 中永久有效，你需要运行下面的命令。
$ echo '. /usr/share/autojump/autojump.sh' >> ~/.bashrc
```
#### 查看是否安装epel
```
yum list installed | grep epel
安装epel
yum -y install epel-release
查看安装的版本
rpm -q epel-release
```  
## Ubuntu配置系统软件
#### ubuntu中默认安装了那些shell,当前正在运行的是那个版本的shell
```
$ cat /etc/shells # /etc/shells: valid login shells/bin/sh/bin/dash/bin/bash/bin/rbash
$ echo $SHELL
```
#### 安装git、wget、curl、zsh
```
$ apt-get update
$ sudo apt-get install wget curl zsh autojump tree zip unzip make git vim -y
http://ohmyz.sh/
$ sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
$ chsh -s /usr/bin/zsh
```
#### 设置zsh的参数
```
下载完毕，你将得到一个名为_.zshrc的文件
先进入家目录，备份原来的.zshrc,然后重命名_.zshrc文件
设置plugins：autojump history
$ cp .zshrc .zshrc.bak
```
## 下载软件：
```
http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html
http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
http://tomcat.apache.org/
https://nodejs.org/en/download/
http://redis.io/download
http://download.redis.io/releases/redis-3.2.5.tar.gz
http://nginx.org/en/download.html
```
## 修改Session连接时间
打开 /etc/ssh/sshd_config 文件，找到一个参数为 ClientAliveCountMax，它是设定用户端的 SSH 连线闲置多长时间后自动终止连线的数值，单位为分钟。
> /etc/ssh/ssh_config是client要连线到ssh server的设定<br>
> /etc/ssh/sshd_config是ssh server的设定


```
ClientAliveInterval 30
ClientAliveCountMax 30
```
如果这一行最前面有#号，将那个#号删除，并修改想要的时间。修改后保存并关闭文件，重新启动sshd

```
/etc/init.d/ssh 
		Usage: {start|stop|reload|force-reload|restart|try-restart|status}
/etc/init.d/ssh —help 查看帮助
```
CentOS下找不到 /etc/init.d 可以用以下命令重启，最后重启机器生效

```
service sshd restart
```
## JDK安装
```
yum -y install java-1.8
```
或者手动安装，下载好压缩包，tar -xzvf解压，.zshrc中配置路径

```
export JAVA_HOME=/root/jdk8
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
```
## NodeJS安装
#### Ubuntu
下载gz结尾文件，解压，然后进入node-v6.2.2 目录安装

```
# Makefile:77: *** Stale config.gypi, please re-run ./configure.  Stop.
./configure
make install
node -v（检查node版本）
```
或者下载xz结尾文件，解压，然后进入文件夹，bin目录下的npm运行安装n
#### CentOS
```
Enterprise Linux and Fedora的段落安装可成功（Run as root on RHEL, CentOS or Fedora, for Node.js v6 LTS:）
https://nodejs.org/en/download/package-manager/

curl --silent --location https://rpm.nodesource.com/setup_6.x | bash -
curl --silent --location https://rpm.nodesource.com/setup_7.x | bash -
yum -y install nodejs
```
## Redis安装
```
# yum安装
yum -y install redis
# 或者手动安装
wget http://download.redis.io/releases/redis-3.2.6.tar.gz
tar -xzvf redis-3.2.6.tar.gz
cd redis-3.2.6
make
```
#### 修改配置文件redis.conf（bind 61 requirepass 480）
```
sed -n '/foobared/p' redis.conf && sed -n '/^bind/p’ redis.conf
sed -i -e 's/# requirepass foobared/requirepass pwd_redis_ud/g;s/^bind/# bind/g' redis.conf
nohup ./src/redis-server ./redis.conf &
```
#### 安装后测试、连接
```
src/redis-cli
redis-cli -h 192.168.2.143 -a pwd_redis_ud
192.168.2.143:6379> ping
PONG
192.168.2.143:6379>
```
#### 查看redis的所有连接和连接数
```
netstat -lnp | grep redis
netstat -lnp | grep redis | wc -l
```

## Tomcat安装
JDK装好解压即可以运行
修改conf/server.xml 增加URIEncoding="UTF-8”（70行）

```
<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               URIEncoding="UTF-8"
               redirectPort="8443" />
```
## Python2.7环境
```
apt-get update
sudo apt-get install python-dev（error: command 'x86_64-linux-gnu-gcc' failed with exit status 1）
pip install tornado virtualenv pycrypto
pip install MySQL-python（如果不能安装，先安装下一条）
sudo apt-get install libmysqld-dev（EnvironmentError: mysql_config not found）
```
> 因为mysql_config是属于MySQL开发用的文件，而使用apt-get安装的MySQL是没有这个文件的

完

