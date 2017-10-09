## GROUP_CONCAT()是MySQL数据库提供的一个函数，通常跟GROUP BY一起用,具体可参考[MySQL官方文档](http://dev.mysql.com/doc/refman/5.0/en/group-by-functions.html#function_group-concat)

语法:

```
GROUP_CONCAT([DISTINCT] expr [,expr ...] [ORDER BY {unsigned_integer | col_name | expr} [ASC | DESC] [,col_name ...]] [SEPARATOR str_val])
```
### 1、例如
```
mysql> select eid,name,cname,business_id from meta_event;
+-----+--------+-----------------------+-------------+
| eid | name   | cname                 | business_id |
+-----+--------+-----------------------+-------------+
|   1 | upload | 电箱数据上传            |           6 |
|   2 | play   | 播放                   |          10 |
|   5 | view   | 页面访问               |          10 |
|   6 | exit   | 页面退出               |          10 |
|   7 | click  | 首页推荐位点击          |          10 |
|   8 | play   | 播放                  |          10 |
|   9 | order  | 订购                  |          10 |
+-----+--------+-----------------------+-------------+
7 rows in set (0.02 sec)

mysql> SELECT business_id,GROUP_CONCAT(name) FROM meta_event GROUP BY business_id;
+-------------+---------------------------------+
| business_id | GROUP_CONCAT(name)              |
+-------------+---------------------------------+
|           6 | upload                          |
|          10 | play,view,exit,click,play,order |
+-------------+---------------------------------+
2 rows in set (0.02 sec)
```
### 2.当然分隔符还可以自定义，默认是以“,”作为分隔符，若要改为“-”，则使用SEPARATOR来指定，例如：

```
mysql> SELECT business_id,GROUP_CONCAT(name SEPARATOR '-') FROM meta_event GROUP BY business_id;
+-------------+----------------------------------+
| business_id | GROUP_CONCAT(name SEPARATOR '-') |
+-------------+----------------------------------+
|           6 | upload                           |
|          10 | play-view-exit-click-play-order  |
+-------------+----------------------------------+
2 rows in set (0.03 sec)
```
### 3.除此之外，还可以对这个组的值来进行排序再连接成字符串，例如按eid降序来排：
```
mysql> SELECT business_id,GROUP_CONCAT(name ORDER BY eid DESC) FROM meta_event GROUP BY business_id;
+-------------+--------------------------------------+
| business_id | GROUP_CONCAT(name ORDER BY eid DESC) |
+-------------+--------------------------------------+
|           6 | upload                               |
|          10 | order,play,click,exit,view,play      |
+-------------+--------------------------------------+
2 rows in set (0.01 sec)
```
###4.需要注意的：
####a.int字段的连接陷阱

当你用group_concat的时候请注意，连接起来的字段如果是int型，一定要转换成char再拼起来，
否则在你执行后(ExecuteScalar或者其它任何执行SQL返回结果的方法)返回的将不是一个逗号隔开的串，
而是byte[]。

该问题当你在SQLyog等一些工具中是体现不出来的，所以很难发现。

> select group\_concat(ipaddress) from t_ip 返回逗号隔开的串  
> select group\_concat(id) from t_ip 返回byte[]  
> select group\_concat(CAST(id as char)) from t_dep 返回逗号隔开的串  
> select group\_concat(Convert(id , char)) from t_dep 返回逗号隔开的串  

附Cast,convert的用法：
CAST(expr AS type), CONVERT(expr,type) , CONVERT(expr USING transcoding_name)
CAST() 和CONVERT() 函数可用来获取一个类型的值，并产生另一个类型的值。

这个类型 可以是以下值其中的 一个：

* BINARY[(N)]
* CHAR[(N)]
* DATE
* DATETIME
* DECIMAL
* SIGNED [INTEGER]
* TIME
* UNSIGNED [INTEGER]

####b.长度陷阱
用了group_concat后，select里如果使用了limit是不起作用的.
用group_concat连接字段的时候是有长度限制的，并不是有多少连多少。但你可以设置一下。

使用group_concat_max_len系统变量，你可以设置允许的最大长度。
程序中进行这项操作的语法如下，其中 val 是一个无符号整数：
SET [SESSION | GLOBAL] group_concat_max_len = val;
若已经设置了最大长度， 则结果被截至这个最大长度。

在SQLyog中执行 SET GLOBAL group_concat_max_len = 10 后，重新打开SQLyog，设置就会生效。

