### odb数据导入和导出
[下载官网](http://trafodion.incubator.apache.org/download.html)
[下载clients](http://mirrors.tuna.tsinghua.edu.cn/apache/incubator/trafodion/apache-trafodion-2.0.1-incubating/apache-trafodion_clients-2.0.1-incubating.tar.gz)
[参考链接](http://trafodion.incubator.apache.org/docs/sql_reference/index.html#using_trafodion_sql_to_access_hive_tables)


### UNLOAD导出
* 先su到hdfs用户下，创建一个hdfs的路径

```
hadoop fs -mkdir -p /bulkload 创建导出数据目录
hadoop fs -chmod -R 777 /bulkload 然后目录用户权限
```

* 然后sqlci中导出数据

```
UNLOAD WITH PURGEDATA FROM TARGET MERGE FILE 'test_table.gz' OVERWRITE COMPRESSION GZIP INTO '/bulkload' SELECT * FROM TRAFODION.TEST_SCHEMA.TEST_TABLE;
```

* 然后查看一下导出的数据

```
hadoop fs -ls /bulkload
hadoop fs -cat /bulkload/test_table.gz
```

* 然后把导出的数据放入本地文件夹

```
chmod 777 /downloads/
hadoop fs -copyToLocal /bulkload/test_table.gz /home/downloads/
或
hadoop fs get /root/ds.gz /home/downloads/
```

### LOAD借助hive导入
* 另外一个机器把test_table.gz保存在docker环境下的/home/downloads下并且在hdfs用户下创建导入目录

```
hadoop fs -mkdir -p /hive/bulkload
```

* 把数据包从本地移动到hdfs的路径下面

```
hadoop fs -copyFromLocal /home/downloads/test_table.gz /hive/bulkload
或
hadoop fs put /home/downloads/test_table.gz /hive/bulkload
```

* 查看/hive/bulkload下的数据包
```
hadoop fs -ls /hive/bulkload
```

* 现在在trafodion用户下新建test_table表

```
CREATE SCHEMA TEST_SCHEMA;
SET SCHEMA TEST_SCHEMA;
CREATE TABLE TEST_TABLE(c1 int not null, c2 int not null, primary key (c1));
```
> hive中也可以创建schema，后面查询就用hive.pksaas.xxx
create schema pksaas;

* 然后在Hive中创建Hive外部表，指向前面对应的HDFS文件
<font color=red>

``` 
//设置显示当前的数据库
set hive.cli.print.current.db=true;
//查看hive中创建的表
show tables;
//hive中也可以创建schema
create schema pksaas;use pksaas;
//后面查询就用hive.pksaas.xxx;
create external table test_tbl (c1 int,c2 int) row format delimited fields terminated by '|' location '/hive/bulkload’;
//查看表的详细信息
describe extended eboxdata;
//查看表结构
describe formatted test_tbl;
create external table test_tbl (c1 int,c2 int) row format delimited fields terminated by '|' location '/hive/bulkload';
//看下表数据
select * from test_tbl limit 1;
```
</font>

* 现在加载hive数据到Trafodion表

> hive中可以预先设置要导入表的最大字段长度，默认32K（Trafodion 环境中执行）
> // Controll Query Default
> ``CQD HIVE_MAX_STRING_LENGTH '2048’;``

```
// load into 这种方法叫 bulk load（hdfs file -> hfile）500万数据
load into TRAFODION.TEST_SCHEMA.TEST_TABLE select * from hive.hive.test_tbl;
load with continue on error into TEST_SCHEMA.TEST_TABLE select * from hive.hive.test_tbl;
```
如果数据量不大的话用upsert

```
// trickle load
// insert into table & upsert into table & upsert using load into table
upsert using load into TEST_SCHEMA.TEST_TABLE select * from hive.hive.test_tbl;
```
End
