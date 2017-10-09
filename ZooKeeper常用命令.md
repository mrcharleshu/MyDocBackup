
### ls
使用 ls 命令来查看某个目录包含的所有文件，例如：

```
[zk: 127.0.0.1:2181(CONNECTED) 1] ls /
```
### ls2
使用 ls2 命令来查看某个目录包含的所有文件，与ls不同的是它查看到time、version等信息

```
[zk: 127.0.0.1:2181(CONNECTED) 1] ls2 /
```
### create
创建znode，并设置初始内容，例如创建一个新的 znode节点“ test ”以及与它关联的字符串

```
[zk: 127.0.0.1:2181(CONNECTED) 1] create /test "hello" 
```
### get
获取znode的数据，如下：

```
[zk: 127.0.0.1:2181(CONNECTED) 1] get /test
```

### set
修改znode内容，例如：

```
[zk: 127.0.0.1:2181(CONNECTED) 1] set /test "ricky"
```

### delete
删除znode

```
[zk: 127.0.0.1:2181(CONNECTED) 1] delete /test
```

### quit
退出客户端

### help
帮助命令