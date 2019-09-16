unsigned    无符号的，使用的场景在有符号数据类型，例如 int数据类型，默认是有符号的。（有符号的数据类型就可以取到负数，无符号的，最小数就是0。）

# 存储引擎

存储引擎就是如何存储数据，如何为数据建立索引和如何更新，查询数据等技术的实现方法。

mysql默认支持多种的存储引擎，以适应不同领域的数据库应用需求，用户可以通过选择使用不同的存储引擎提高应用的效率，提供灵活的存储。

#### 查看当前的存储引擎

```
show variables like ‘%storage_engine’;
show engines;
```

### mysql 常用的存储引擎

myisam     这个引擎存储限制是最大的有256TB。它 支持全文索引，支持数索引。（支持全文索引是它最大的优点）

memory    这个引擎存储限制就是运行计算机的运行内存，它的优点就是速度快，而且支持哈希索引，和支持数据缓存。）

innodb   这个引擎都是目前mysql中使用最多的存储引擎，因为它功能强大，   它的存储限制是64TB，主要功能是     支持事务，支持数索引，支持数据缓存，支持外键。（mysql的默认引擎就是它。）

##### 表的引擎

innodb   和 myisam

curd   操作  ： 增删改查。

c      create insert 插入

u      update   更新，修改

r       read  select 查询

d      delete    删除

less /ect/my.cnf              默认的存储路径

datadir =/data/mysql

innodb  在写的操作上非常有优势（事务） CUD 全是写的操作。

myisam  在读的操作上非常的有优势（健全的索引）R操作。

### 引擎的存储方式

myisam 将一张表存储为三个文件

demo.frm     表的结构

demo.MYD    存储的是数据

demo.MYI   存储的是表的索引。

#### myisam的文件是可以任意移动的

innodb 将一张表存储为两个文件

demo.frm    表的结构 + 表的索引

demo.ibd     存储的是数据

ibd 存储是有限的，存储不足自动创建ibd1，ibd2.

####   innodb 的文件创建在那个数据库中，不能任意的移动。

1.innodb

事务型数据的首选引擎，支持事务安全表（ACID），支持行锁定和外键，innodb是默认的mysql引擎。

### 存储引擎的选择

一般来说，对插入和并发性要求比较高的，或者需要外键及事务支持的选择innodb。

插入较少，查询较多的场景，优先考虑myisam。 

#### 使用引擎

一般是在建表的时候选择

```
create table abc（name char（10））engine=myisam charset=utf8；
create table xyz （name char（10））engine=myisam charset=utf8；
```

# 索引

索引就是为特定的mysql字段进行一些特定的算法排序，比如二叉树的算法和哈希算法，哈希算法是通过建立特征值，然后根据特征值来快速查找。

mysql索引的建立对于mysql的高效运行是很重要的，索引可以大大提高mysql的检索速度。

用的最多的，mysql默认的索引数据结构btree。

通过btree 算法建立索引的字段，比如扫描20行就能得到未使用btree 前扫描了2^20行的结果。

哈希索引比较特殊，时间复杂度为 O（1），但是只适合等值比较方式的查询，不适合范围或大小比较进行查询。

索引的优点：

+ 一个字：快，使用索引能极大提升查询速度。

索引的缺点

1.额外的使用一些存储的空间。

2.索引会让写的操作变慢。

### 索引的创建原则

1.适合用于频繁查找的列

2.适合经常用于条件判断的列。

3.适合经常由排序的列。

4.不舍和数据不多的列。

5.不适合很少查询的列。

### 创建索引

1.键表时添加索引

```
create table 表 （
	id int not null，
	username varchar（16）not null，
	index 索引名（字段名[长度]）
	);
```

2.后期添加索引

```
create index 索引名  on 表名（字段[长度]）
```

###  删除索引

```
drop index [索引名] on 表名；
# 删除一个索引的时候，使用的是drop   ，其中索引名是属于可选参数，如果不选的的话，直接就是删除了这个表中的所有的索引。
```

### 唯一索引

它与前面的普通索引类似，不同的就是  ： 索引列的值，必须是唯一的，但是是允许与空值的。如果组合索引，则列值的组合必须是唯一的。

```
create unique index 索引名 on 表（字段名（长度））；
---或者是
create table 表 （
	id  int not null，
	username varchar（16） not null，
	unique 索引名 （字段名（长度））
	）；
#   唯一索引的创建分为两种形式，一种是后期添加，这个时候，和之前添加索引的时候，唯一的差别就是在index的前面加一个 unique  。另一种就是在创建表的时候，添加，和在表中创建普通索引是一样的（没有on的），区别在于将index换成unique。
```

###  查看索引

```
show index from　表名；
```

#  关系与外键

##### 关系

+ 一对一 
  + 在A表中，有一条记录，在B表中同样有唯一条记录相匹配。
  + 比如： 学生表和成绩表。
+ 一对多
  + 在A表中有一条记录，在B表中有多条记录一直对应。
  + 比如，论坛中的，博主和写的文章。
+ 多对多
  + A表中的一条记录有多条B表数据对应，同样B表中一条数据在A表中也有多条与之对应。
  + 比如：论坛中，博主收藏了别人的文章。

## 外键

外键是一种约束、他只是保证数据的一致性，并不能给系统性能带来任何好处。

建立外键时，都会在外键上建立对应的索引，外键 的存在会在每一次数据插入修改时进行约束检查，如果不满足外键约束，则禁止数据的插入或修改，这必然带来一个问题，就是在数据量特别大的情况下，每一次约束检查必然导致性能的下降。

处于性能的考虑，如果我们的系统对性能要求比较高，那么可以考虑在生产环境中不适用外键。

1.构造数据

```
-- 用户表   存储的数据是用户的id和用户的姓名，这里存储的就是用户的信息，注意这个一个表，可以存储很多用户的信息。
create table `user` (
`id` int unsigned primary key auto_increment,
`name` char(32) not null unique
) charset=utf8;
-- 商品表    存储的是商品的id和名字以及价格  ，这里可以存储很多的信息。
create table `product` (
`id` int unsigned primary key auto_increment,
`name` char(32) not null unique,
`price` float
) charset=utf8;
-- 用户信息表: 一对一     用户的信息表，在这个表中记录着用户的详细信息，将这个用户详细信息和用户表中的id进行关联，通过外键连接，索引的时候，直接索引就好了。
create table `userinfo` (
`id` int unsigned primary key auto_increment,
`phone` int unsigned unique,
`age` int unsigned,
`location` varchar(128)
) charset=utf8;
-- 用户组表: 一对多       用户组，这里存储的信息是用户的id和名字，和用户表不同的是，这里的id是可以重复的，存储的应该是，按照先后的顺序，购买过的用户表，在这个表中，id和别的表是没有关联的，它存储的就是一个用户多次购物的记录。一个用户可以多次的购物，所以在这个用户组表中，就可以见到用户的名字多次的出现，所以在关联的时候，与它关联的是就是用户的名字。
create table `group` (
`id` int unsigned primary key auto_increment,
`name` char(32) not null unique
) charset=utf8;
-- 订单表: 多对多     这里存储的数据有id ，uid ，pid 分别对应着订单的序号，用户的id 以及商品的id ，关联的时候，他们两个分别是和用户信息表，以及商品表进行关联的。
create table `order` (
`id` int unsigned primary key auto_increment,
`uid` int unsigned,
`pid` int unsigned
) charset=utf8;
# 使用的编码全部都是utf8 。 上面使用添加的unsigned 属性是无符号属性，就是只有与整数，mysql中默认的都是有符号的，所以在创建的时候，根据需要可以给字段填上一个无符号属性。
```

2.添加外键

```
-- 为 user 和 userinfo 建立立关联的外键     一对一
alter table userinfo add constraint fk_user_id foreign key(id) references
user(id);
-- 建立立用用户与组的外键约束   一对多
alter table `user` add `gid` int unsigned;
alter table `user` add constraint `fk_group_id` foreign key(`gid`)
references `group`(`id`);
-- 建立立用用户、商品、订单的外键约束   多对多
alter table `order` add constraint `fk_user_id` foreign key(`uid`)
references `user`(`id`);
alter table `order` add constraint `fk_product_id` foreign key(`pid`)
references `product`(`id`);

###  在进行连接的时候，是是分为主表和子表达的。        主表就是通过外键关联到子表，实现数据的跳转，这里一般都是以主表作为主导，如果是需要修改表中信息，只有先修改子表中的数据，才能在主表中进行修改，不然直接修改主表，会因为外键的约束作用，而导致操作的失败。
作为连接的字段，不一定都是主键，只要保证其不唯一性就行了。

### 添加外键的格式 
alter table `子表名`  add constraint `fk_'主表名'_id` foregin key('子表的关联字段') references `主表名`(字表的关联字段)


### 通常主表中的都是一些比较全面，但是不是很精细的消息。字表的数据都是一些比较详细的数据存储的是的单个个体，更加详细。


alter table ’主表名‘ add constraint fk_'子表名'_id foregin key('主表的关联字段') references '字表名'(字表的关联字段)
```

