mysql    关系型数据库   ------表

关系型数库有局限性    --------nosql主键发展崛起。

redis   和mongodb        ---  列存储，json 文档 是一个字符串文本

搜索  

redis运行的时候，直接存储在内存中的

在Linux中安装redis的时候，可以  apt  install  -s   redis

-s   预安装，不会真的 安装，而是一个预安装，看是否可以成功。主要是用来查看安装信息。

stable 稳定的      wget 安装连接

下载下来的是一个压缩包，使用ls 查看下载的是什么格式，进行解压。

下载的是源代码，所以在安装之前，需要先编译一下（因为使用c语言编写的）

makefile       自动编译          执行make就行 了。

编译之后的文件是一个二进制文件。

make install    安装命令

&&     前面的命令执行成功，后面才会执行

||      前面执行不成功，后面才会执行，前面成功，后面就不执行了。

如果提示权限不够 在安装命令前加一个sudo 

安装好之后，需要执行    cd  /usr/local/bin

mysql 的默认端口号是3306 

redis的默认端口号是6379

只能存在字符串

ttl   查看该数据还有多少的存活时间

EX  数据的存活时间

incr   自增    数字值字符串

##### 哈希

hset  xxx a 123 b 456

在redis中存了一个字典，字典名是xxx，a，b是key，分别对应着value  123 ，456

获取字典中的值        hgetall   xxx

查看指定的key       hget xxx  a    区字典中b的值。

###### 列表

双端列表，类似一个队列。    类似于c语言中的双端链表       可以选择添加的数据是从左边开始，

LPUSH mm 1 2 3      l （左边添加存储）

RPUSH                              r (右边添加存储)

```
lpush mm 1 2 3 
# 上面这个操作是在redis中创建一个列表 mm  ，然后给列表中填入值，新添加的值，是一次从列表的最左边插入  所以最后列表 mm 的数据的顺序是  3 2 1
#如果想要列表中元素的排列顺序和添加时候的顺序是一样的  使用 rpush
rpush mm a b
# 此时数列表中元素的排序顺序是  3 2 1 a b
# 在redis中查看列表    lrange 列表名 [起始位置] [终止位置]
# 这里的范围，起始和终止 位置都是可以取到的，而且计数是用0 开始的，前后范围都是包含的，一般都是 写成起始位置下标  0   终止位置下标  -1 。
就可以查看指定 列表中的数据了。
lrange mm 0 -1 
 
# 在redis中的列表，它就像是一个迭代器一样的功能，可以一次一次的从中，一次获取数据，在redis中叫做弹出数据   使用的命令是 pop （弹出）     在它前面一般加上 r  或 l，它们分别对应是是从开始左边弹出，从右边开始弹出。
```

###### set  集合

sadd  sss 11 22 33 

创建一个集合

获取两个集合的交集

sinter  集合名1   集合名2

获取两个集合的并集

sunion  集合名1  集合名2

获取两个变量之间的差集 （这个差集，指的是，前面集合有的，而后面集合没有的，所有两个集合，使用sdiff 命令，集合的先后顺序不同，获得的结果也是不同的。）

sdiff  集合名1  集合名2

获得结果是集合1 中有的 ，而集合2中没有的数据

sdiff  集合2名  集合1名

获得结果是集合2中有的，而集合1中没有的的数据元素。

######　有序集合　　（一般的数据排行榜使用的就是它）

```
sadd rank 98 zhd 78 hxp 79 yt 85
```

zset  有序集合

range  范围

zrange  集合名  0  -1  WITHSCORES

按照（成绩） 的大小进行排序，排列的顺序是由小到大的。

也可以让它进行倒序排列   

zrevrange 集合名 0 -1  WITHSCORES

对集合中的数据，实现自增，执行一次命令，就会在原来的基础上增加指定的数值。













在爬虫阶段会用到mongodb

规范化数据。 它里面存储的都json对象。

wget   下载安装命令

