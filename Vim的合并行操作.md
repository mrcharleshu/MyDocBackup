### 第一种, 多行合并成一行,即: 

AAAAA<br/>
BBBBB<br/>
CCCCC<br/>

合并为:<br/>
AAAAA BBBBB CCCCC

##### 方法1: normal状态下 3J 其中的3是范围,可以是书签或者搜索位置等方式实现,J为合并
> 注: 如果改为3gJ的话,则合并时各行没有空白AAAAABBBBBCCCCC, 下面方法类似,不再重复这两种合并方式的区别.

##### <font color=red>
方法2: 命令状态下

* 合并1到3行 :1,3 join
* 合并1到3行 :1,3 j
* 合并所有行 :% j

</font>

##### 方法3: 传统一点的,替换换行符的方式,为避免最后一行也被换掉,范围缩小了,命令状态下  :1,2s/\n/ /

### 第二种,隔行合并,即:

AAAAA<br/>
BBBBB<br/>
CCCCC<br/>
DDDDD<br/>

合并为:<br/>
AAAAA BBBBB<br/>
CCCCC DDDDD<br/>

##### 方法1: 借用一下宏录制功能, normal状态下 qaJjq 实现录制, 然后在合适的区域重复执行n遍,这里2遍即可,normal状态下2@a

##### 方法2: 命令状态下 :1,4g/^/ join  增加了g过滤后,合并变成了隔行处理
