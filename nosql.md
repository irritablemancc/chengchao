nosql

编译    make     (有makefile的情况下，执行这个文件。)

redis-cli       (redis 一般只放在内网中。)

存如数据       set  cheng   cc

获取数据         get  cheng     ===》    cc

它是键值对，cheng是键，cc是值。

zset    有序集合

bitmap    

redisdoc.com      文档网址

incr 起始数值   自增

hash

hset    xxx    a  123  b    456      

hset     字典名   key  value

list

RPUSH   mm 1      从右边开始插入

  LPUSH    mm       从左边开始插入

双端链表

lpop mm    左边弹出

rpop mm    右边弹出

集合

sadd    sss 11 22   33  44 55 

sadd   ffff 44 55  66  77  88

交集，差集

zset

zadd  rank  98 zhd  78 hxp 

有序集合

ZRANGE   rank 0 -1

排行榜

r = redis.Redis()

r.keys（） 字符创

r.sinter()     集合

r.hgetall()    哈希    (字典)   所有的都是一个字符串 b""

r.get()       r.set()









mongodb        库          数据集       一条数据



