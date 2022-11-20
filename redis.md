# redis

# redis学习

启动前先把redis配置文件复制一份: redis.conf

redis默认不是后台启动的，修改配置文件



redis默认安装路径：usr/local/bin

通过redis配置文件启动redis:

```
 redis-server kconfig/redis.conf
```

使用 redis-cli 进行连接测试！

```
redis-cli -p 6379
```



查看 redis 的进程是否开启;



关闭 redis 服务；



redis默认有16个数据库，默认使用第0个数据库



可以使用 select 进行切换数据库



DBSIZE 显示数据里面有多少内容

```
keys *  查看数据库所有的Key
```

清除当前数据库 flusdb



清空全部数据库 FLUSHALL



# redis是单线程的！

redis是基于内存操作的，CPU不是redis的性能瓶颈，redis的瓶颈是根据机器的内存和网络带宽，可以会用单线程来实现，所以使用了单线程。

Redis是C语言写的，官方数据第秒为 100000+ 的QPS，不比Memecache差！

## Rediso为什么单线程还这么快？

1. 
误区：高性能的服务器一定是多线程的？

2. 
误区：多线程（CPU上下文切换）一定比单线程效率高！


核心：REDIS是将所有的数据全部放在内存中的，使用单线程去操作效率是最高的，多线程（CPU上下文切换：耗时操作）对于内存系统来说，如果没有上下文切换效率就是最高的！

# 关于 Rediskey 的基本命令

==这些命令要全部记住==

```bash
127.0.0.1:6379[1]> ping
PONG
127.0.0.1:6379[1]> keys *  # 查看所有的key
(empty array)
127.0.0.1:6379[1]> set name shenyang
OK
127.0.0.1:6379[1]> keys *
1) "name"
127.0.0.1:6379[1]> set age 21
OK
127.0.0.1:6379[1]> EXISTS name  #判断当前的key是否存在
(integer) 1
127.0.0.1:6379[1]> exists name1
(integer) 0
127.0.0.1:6379[1]> move name 1
(error) ERR source and destination objects are the same
127.0.0.1:6379[1]> move name 0   # 移除当前的key
(integer) 1
127.0.0.1:6379[1]> keys *
1) "age"
127.0.0.1:6379[1]> get name
(nil)
127.0.0.1:6379[1]> get age
"21"
127.0.0.1:6379[1]>  ttl name
(integer) -2
127.0.0.1:6379[1]> expire age 10  # 设置Key的过期时间，单位是秒
(integer) 1
127.0.0.1:6379[1]> ttl name  # 查看当前Key的剩余时间
(integer) -2
127.0.0.1:6379[1]> 

========================================
127.0.0.1:6379[1]> type name   # 查看当前key的一个类型
string
127.0.0.1:6379[1]> type age
string
127.0.0.1:6379[1]>
```

# String 字符串类型讲解

```bash
127.0.0.1:6379[1]> set key1 v1  # 设置值
OK
127.0.0.1:6379[1]> get key1  #获得值
"v1"
127.0.0.1:6379[1]> keys *    # 获得所有的key
1) "key1"
2) "age"
3) "name"
127.0.0.1:6379[1]> exists key1  # 判断某一个key是否存在
(integer) 1
127.0.0.1:6379[1]> append key "hello" #追加字符串，如果key不存在，就相当于set key
(integer) 5
127.0.0.1:6379[1]> get key1
"v1"
127.0.0.1:6379[1]> append key1 "hello"
(integer) 7
127.0.0.1:6379[1]> get key1
"v1hello"
127.0.0.1:6379[1]> strlen key1  # 获取字符串的长度
(integer) 7
127.0.0.1:6379[1]> append key1 shenyang
(integer) 15
127.0.0.1:6379[1]> get key1
"v1helloshenyang"
127.0.0.1:6379[1]> 
=========================================================
# i++ 
# 步长
127.0.0.1:6379[1]> get views   # 初始为0
"0"
127.0.0.1:6379[1]> incr views  # 自增1
(integer) 1
127.0.0.1:6379[1]> incr views
(integer) 2
127.0.0.1:6379[1]> decr views
(integer) 1
127.0.0.1:6379[1]> decr views  # 自减1
(integer) 0
127.0.0.1:6379[1]> decr views
(integer) -1
127.0.0.1:6379[1]> get views
"-1"
127.0.0.1:6379[1]> incrby views 10 # 可以设置步长，指定增量
(integer) 9
127.0.0.1:6379[1]> decrby views 5  # 可以设置步长，指定减量
(integer) 4
127.0.0.1:6379[1]> 

=======================================================
# 字符串范围 range
# getrange
127.0.0.1:6379[1]> set key1 "hello , shenyang"
OK
127.0.0.1:6379[1]> get key1
"hello , shenyang"
127.0.0.1:6379[1]> getrange key1 0 3   # 截取字符串，从0到3闭区间
"hell"
127.0.0.1:6379[1]> getrange key1 0 -1  # 获取全部字符串和get key 是一样的妈
"hello , shenyang"
127.0.0.1:6379[1]> 

# setrange

127.0.0.1:6379[1]> set key2 abcdefg
OK
127.0.0.1:6379[1]> get key2
"abcdefg"
127.0.0.1:6379[1]> setrange key2 1 xxx  # 替换指定位置开始的字符串
(integer) 7
127.0.0.1:6379[1]> get key2
"axxxefg"
127.0.0.1:6379[1]> 

===========================
setex  (set with expire) # 设置过期时间
setnx  (set if not exist) #不存在设置

127.0.0.1:6379[1]> setex key3 30 "sy" # 设置key3的值为 sy,30秒后过期
OK
127.0.0.1:6379[1]> ttl key3
(integer) 25
127.0.0.1:6379[1]> keys *
1) "key2"
2) "key1"
3) "key3"
127.0.0.1:6379[1]> keys *
1) "key2"
2) "key1"
127.0.0.1:6379[1]> setnx mykey "redis" #如果mykey不存在，创建Mykey
(integer) 1
127.0.0.1:6379[1]> setnx mykey "java"  #如果mykey存在，创建失败
(integer) 0
127.0.0.1:6379[1]> keys *
1) "key2"
2) "key1"
3) "mykey"
127.0.0.1:6379[1]> get mykey
"redis"

========================================================
# mset
# mget
# msetnx
127.0.0.1:6379[1]> keys *
1) "key2"
2) "key1"
3) "mykey"
127.0.0.1:6379[1]> mset k1 sy k2 sy k3 sy  # 同时设置多个值
OK
127.0.0.1:6379[1]> keys *
1) "key2"
2) "k3"
3) "key1"
4) "mykey"
5) "k2"
6) "k1"
127.0.0.1:6379[1]> mget k1 k2 k3  # 同时获取多个值
1) "sy"
2) "sy"
3) "sy"
127.0.0.1:6379[1]> msetnx k1 sy k4 ss  # msetnx是一个原子性的操作，要么一起成功，要么一起失败
(integer) 0
127.0.0.1:6379[1]> get k4
(nil)
127.0.0.1:6379[1]> 


#对象 
set user:1 {name:sy,age:3} # 设置一个user:1 对象，值为json字符来保存一个对象

127.0.0.1:6379[1]> mset user:1:name shenyang user:1:age 21
# 这里的key是一个巧妙的设计 user:{id}:{filed},这样设计是可以的
OK
127.0.0.1:6379[1]> mget user:1:name user:1:age
1) "shenyang"
2) "21"
127.0.0.1:6379[1]> 
====================================
# getset #先get后set
127.0.0.1:6379[1]> getset db redis #如果不存在值，则返回 nil
(nil)
127.0.0.1:6379[1]> get db
"redis"
127.0.0.1:6379[1]> getset db go #如果存在，获取原来的值，并设置新的值
"redis"
127.0.0.1:6379[1]> get db
"go"
```

使用场景： value除了是我们 的字符串可以是数字！

# List 列表类型详解

基本的数据类型，在redis里面可以把它当作栈，队列，阻塞队列



```bash
========================================================
127.0.0.1:6379[1]> lpush list one  
# 将一个值或者 多个值，插入到列表头部（左）
(integer) 1
127.0.0.1:6379[1]> lpush list two
(integer) 2
127.0.0.1:6379[1]> lpush list three
(integer) 3
127.0.0.1:6379[1]> lrange list 0 -1 # 获取list中的值
1) "three"
2) "two"
3) "one"
127.0.0.1:6379[1]> lrange list 0  1 # 通过区间获取具体的值
1) "three"
2) "two"
127.0.0.1:6379[1]> rpush list four
# 将一个值或者 多个值，插入到列表头部（右）
(integer) 4
127.0.0.1:6379[1]> lrange list 0 -1
1) "three"
2) "two"
3) "one"
4) "four"

=========================================
lpop
rpop
# 移出值
127.0.0.1:6379[1]> lrange list 0 -1 
1) "three"
2) "two"
3) "one"
4) "four"
127.0.0.1:6379[1]> lpop list  #移出list的第一个元素
"three"
127.0.0.1:6379[1]> rpop list  #移出list的最后一个元素
"four"
127.0.0.1:6379[1]> lrange list 0 -1
1) "two"
2) "one"
=======================================================
#   Lindex
# 通过下标获取List中的某一个值
127.0.0.1:6379[1]> lrange list 0 -1
1) "two"
2) "one"
127.0.0.1:6379[1]> LIndex list 0
"two"
127.0.0.1:6379[1]> LIndex list 1
"one"
127.0.0.1:6379[1]> 

=======================================================
Llen
# 返回列表的长度
127.0.0.1:6379[1]> Llen list
(integer) 3

=======================================================
# 移除指定的值
#  Lrem
127.0.0.1:6379[1]> lrange list 0 -1
1) "there"
2) "there"
3) "two"
4) "one"
127.0.0.1:6379[1]> lrem list 1 one #移除list中指定个数的值，精确匹配
(integer) 1
127.0.0.1:6379[1]> lrange list 0 -1
1) "there"
2) "there"
3) "two"
127.0.0.1:6379[1]> lrem list 2 there #可以同时移除多个
(integer) 2
127.0.0.1:6379[1]> lrange list 0 -1
1) "two"
127.0.0.1:6379[1]> 

=======================================================
Ltrim 修剪 

127.0.0.1:6379[1]> lrange list 0 -1
1) "two"
2) "one"
3) "there"
4) "sysy"
127.0.0.1:6379[1]> ltrim list 1 2 # 通过下标截取指定的长度，这个list已经被改变了，只剩下截取的元素
OK
127.0.0.1:6379[1]> lrange list 0 -1
1) "one"
2) "there"
127.0.0.1:6379[1]> 

=======================================================
rpoplpush  #移除列表的最后一个元素，将他移动到新的列表中！

127.0.0.1:6379[1]> lpush mylist "hello"
(integer) 1
127.0.0.1:6379[1]> lpush mylist "hello1"
(integer) 2
127.0.0.1:6379[1]> lpush mylist "hello2"
(integer) 3
127.0.0.1:6379[1]> rpoplpush mylist myotherlist
#移除列表的最后一个元素，将他移动到新的列表中！
"hello"
127.0.0.1:6379[1]> lrange mylist 0 -1 # 原来的列表
1) "hello2"
2) "hello1"
127.0.0.1:6379[1]> lrange myotherlist 0 -1 # 目标列表确实有值
1) "hello"
127.0.0.1:6379[1]> 

=======================================================
 lset 将列表中指定下标的值替换为另外一个值，更新操作
127.0.0.1:6379[1]> lset list 0 item
(error) ERR no such key
127.0.0.1:6379[1]> lpush list sysy1
(integer) 1
127.0.0.1:6379[1]> lrange list 0 0
1) "sysy1"
127.0.0.1:6379[1]> lset  list 0 sy001  
#如果不存在就会报错，如果存在就会更新下标的值
OK
127.0.0.1:6379[1]> lrange list 0 0
1) "sy001"

=======================================================
linsert 
# 将某个具体的值插入到列表中某个值的前面或者后面
127.0.0.1:6379[1]> rpush list "hello"
(integer) 1
127.0.0.1:6379[1]> rpush list "world"
(integer) 2
127.0.0.1:6379[1]> linsert list before "world" sysy
# 插入到列表中 world 的前面插入 sysy
(integer) 3
127.0.0.1:6379[1]> lrange list 0 -1
1) "hello"
2) "sysy"
3) "world"
127.0.0.1:6379[1]> linsert list after world sy001
# 插入到列表中 world 的后面插入 sy001
(integer) 4
127.0.0.1:6379[1]> lrange list 0 -1
1) "hello"
2) "sysy"
3) "world"
4) "sy001"
```

> 小结：


1. 
它实际上是一个链表

2. 
如果key不存在，创建新的链表

3. 
如果key存在，新增内容

4. 
如果移除了所有值，空链表，也代表不存在

5. 
在两边插入或者改动值，效率最高，中间元素，相对来说效率低一点


# set集合类型详解

set中的值是不能重复的！

```bash
127.0.0.1:6379[1]> sadd myset "hello"
# set集合中增加元素
(integer) 1
127.0.0.1:6379[1]> sadd myset "sy"
(integer) 1
127.0.0.1:6379[1]> sadd myset "love"
(integer) 1
127.0.0.1:6379[1]> smembers myset  # 查看指定set的所有值
1) "love"
2) "hello"
3) "sy"
127.0.0.1:6379[1]> sismember myset hello # 判断一个值是否存指定的set中，存在
(integer) 1
127.0.0.1:6379[1]> sismember myset world
(integer) 0 # 不存在
=========================================================

127.0.0.1:6379[1]> sadd myset love  # 不能添加重复的值
(integer) 0 
127.0.0.1:6379[1]> scard myset     #获取set集合中的元素的个数
(integer) 3

=========================================================
rem

srem myset hello # 移除set集合中的指定元素


=========================================================
set是无序不重复集合

127.0.0.1:6379[1]> smembers myset
1) "love"
2) "hello"
127.0.0.1:6379[1]> srandmember myset # 随机抽选出一个元素
"hello"
127.0.0.1:6379[1]> srandmember myset
"hello"
127.0.0.1:6379[1]> srandmember myset
"love"
127.0.0.1:6379[1]> srandmember myset 2 # 随机抽选中一个元素 
1) "love"
2) "hello"

=========================================================
删除指定的key,随机删除一个 key 
127.0.0.1:6379[1]> spop myset # 随机删除set集合中的元素
"love"
127.0.0.1:6379[1]> smembers myset
1) "hello"

=========================================================

将一个指定的值，移动到另外一个set

127.0.0.1:6379> sadd myset "syy"
(integer) 1
127.0.0.1:6379> sadd myset "sss"
(integer) 1
127.0.0.1:6379> sadd myset2 "yyy"
(integer) 1
127.0.0.1:6379> smove myset myset2 syy 
# 将一个指定的值，移动到另外一个set集合
(integer) 1
127.0.0.1:6379> smembers myset2
1) "syy"
2) "yyy"

=======================================================
交集、差集、并集

127.0.0.1:6379> sadd key1 a
(integer) 1
127.0.0.1:6379> sadd key1 b
(integer) 1
127.0.0.1:6379> sadd key1 c
(integer) 1
127.0.0.1:6379> sadd key2 c
(integer) 1
127.0.0.1:6379> sadd key2 d
(integer) 1
127.0.0.1:6379> sadd key2 e
(integer) 1
127.0.0.1:6379> sdiff key1 key2  #差集（key1相对于key2说）
1) "b"
2) "a"
127.0.0.1:6379> sinter key1 key2 # 交集
1) "c"
127.0.0.1:6379> sunion key1 key2 # 并集
1) "c"
2) "e"
3) "b"
4) "a"
5) "d"
```

# HASH哈希类型详解

Map集合，key-map!  本质和string类型没有太大区别，还是一个简单的key-vlaue!

```bash
127.0.0.1:6379> hset myhash field1 shenyang  
# set一个具体 key-vlaue
(integer) 1
127.0.0.1:6379> hget myhash field1
#获取一个字段值
"shenyang"
127.0.0.1:6379> hmset myhash field1 hello field2 world
# set多个具体 key-vlaue
OK
127.0.0.1:6379> hmget myhash field1 field2
# 获取多个字段值
1) "hello"
2) "world"
127.0.0.1:6379> hgetall myhash # 获取全部的数据
1) "field1"
2) "hello"
3) "field2"
4) "world"

=========================================================

127.0.0.1:6379> hdel myhash field1  
# 删除指定的key字段！对应的vlaue也就删除了
(integer) 1
127.0.0.1:6379> hgetall myhash
1) "field2"
2) "world"

=========================================================
hlen

127.0.0.1:6379> hlen myhash
(integer) 1
127.0.0.1:6379> hmset myhash field1 hello field3 world
OK
127.0.0.1:6379> hlen myhash # 获取hash的字段数
(integer) 3
127.0.0.1:6379> hgetall myhash  
1) "field2"
2) "world"
3) "field1"
4) "hello"
5) "field3"
6) "world"

=========================================================

#判断hash中指定的字段是否存在
127.0.0.1:6379> hexists myhash field1
(integer) 1
127.0.0.1:6379> hexists myhash field4
(integer) 0

127.0.0.1:6379> hkeys myhash #只获取 field
1) "field2"
2) "field1"
3) "field3"
127.0.0.1:6379> hvals myhash #只获取 value
1) "world"
2) "hello"
3) "world"

=========================================================
incr decr


127.0.0.1:6379> hset myhash field5 5  
(integer) 1
127.0.0.1:6379> hincrby myhash field5 1  # 指定增量
(integer) 6
127.0.0.1:6379> hincrby myhash field5 -1
(integer) 5
127.0.0.1:6379> hsetnx myhash field10 world 
#如果存在不能设置，不存在就可以设置
(integer) 1
```

hash可以存变更的数据 ，hash更适合对象的存储，string更适合字符串的存储.

# Zset(有序集合详解)

在set的基础上，增加了一个值，zset k1 sy  v1

```bash
27.0.0.1:6379> zadd myset 1 one  #增加一个值
(integer) 1
127.0.0.1:6379> zadd myset 2 two 3 there   #增加多个值
(integer) 2
127.0.0.1:6379> zrange myset 0 -1  
1) "one"
2) "two"
3) "there"

========================================================

127.0.0.1:6379> zrevrange salary 0 -1 # 从大到小排序
1) "zhangsan"
2) "shenyang"
```





```bash
=========================================================
移除rem中 的元素

127.0.0.1:6379> zrange salary 0 -1
1) "shenyang"
2) "xiaohong"
3) "zhangsan"
127.0.0.1:6379> zrem salary xiaohong  # 移除有序集合中的指定元素
(integer) 1
127.0.0.1:6379> zrange salary 0 -1
1) "shenyang"
2) "zhangsan"

127.0.0.1:6379> zcard salary # 获取有序集合中的个数
(integer) 2
=========================================================


127.0.0.1:6379> zadd myset 1 hllo
(integer) 1
127.0.0.1:6379> zadd myset 2 world 3 shenyang
(integer) 2
127.0.0.1:6379> zcount myset 1 3 #获取指定区间的成员数量
(integer) 3
127.0.0.1:6379> zcount myset 1 2
(integer) 
=========================================================
```

# Geospatial地理位置详解

## 三种特殊数据类型

### geospatial地理位置

redis的Geo在redis3.2版本就推出了！这个功能可以推算地理位置两地之间的距离，方圆几里的人！

只有6个命令！



> geoadd


```bash
geoadd # 添加地理位置
#规则：两极是无法直接添加，南极和北极；我们一般会下载城市数据，直接通过java程序一次性导入
# 参数  key  值（纬度 经度 名称）
# 有效的经度从-180到180度
# 有效的纬度从-85.05112878度到85.05112878度
27.0.0.1:6379> geoadd china:city 116.40 39.90 beijing
(integer) 1
127.0.0.1:6379> geoadd china:city 121.47 31.23 shanghai
(integer) 1
127.0.0.1:6379> geoadd china:city 106.50 29.53 chongqing 114.05 22.52 shenzheng
(integer) 2
127.0.0.1:6379> geoadd china:city 120.16 30.24 hangzhou 108.96 34.26 xian
(integer) 2
```

> geopos

获得当前定位：一定是一个坐标值


```bash
127.0.0.1:6379> geopos china:city beijing 
# 获取指定的城市的经度和纬度！
1) 1) "116.39999896287918091"
   2) "39.90000009167092543"
127.0.0.1:6379> geopos china:city beijing xian
1) 1) "116.39999896287918091"
   2) "39.90000009167092543"
2) 1) "108.96000176668167114"
   2) "34.25999964418929977"
```

> geodist




```bash

127.0.0.1:6379> geodist china:city beijing shanghai km
"1067.3788"
# 查看上海到北京之间的直线距离，后面单位为km
```

> georadius  以给定的经纬度为中心，找出某一半径内的元素


```bash
#所有数据应该录入使结果更加精确
127.0.0.1:6379> georadius china:city 110 30 1000 km
# 以110 ， 30 这个经纬度为中心，寻找方圆1000km内的城市
1) "chongqing"
2) "xian"
3) "shenzheng"
4) "hangzhou"
127.0.0.1:6379> georadius china:city 110 30 500 km
1) "chongqing"
2) "xian"
127.0.0.1:6379> georadius china:city 110 30 500 km withdist
#显示到中间的距离
1) 1) "chongqing"
   2) "341.9374"
2) 1) "xian"
   2) "483.8340"
127.0.0.1:6379> georadius china:city 110 30 500 km withcoord  #显示他人的定位信息
1) 1) "chongqing"
   2) 1) "106.49999767541885376"
      2) "29.52999957900659211"
2) 1) "xian"
   2) 1) "108.96000176668167114"
      2) "34.25999964418929977"
127.0.0.1:6379> georadius china:city 110 30 500 km  withdist withcoord count 3
#筛选出指定的结果！
1) 1) "chongqing"
   2) "341.9374"
   3) 1) "106.49999767541885376"
      2) "29.52999957900659211"
2) 1) "xian"
   2) "483.8340"
   3) 1) "108.96000176668167114"
      2) "34.25999964418929977"
```

> georadiusbymember


```bash
127.0.0.1:6379> georadiusbymember china:city beijing 1000 km
#找出位于指定元素周围的其他元素
1) "beijing"
2) "xian"
```

> geohash  返回一个或者多个位置元素的Geohash表示




```bash
127.0.0.1:6379> zrange china:city 0 -1
# 查看地图中的所有元素
1) "chongqing"
2) "xian"
3) "shenzheng"
4) "hangzhou"
5) "shanghai"
6) "beijing"
127.0.0.1:6379> zrem china:city xian
#删除地图中指定的元素
(integer) 1
127.0.0.1:6379> zrange china:city 0 -1
1) "chongqing"
2) "shenzheng"
3) "hangzhou"
4) "shanghai"
5) "beijing"
```

# Hyperloglog

> 什么是基数？


A {1,,3,5,7,8,7}

B{1,3,5,7,8}

基数（不重复的元素） = 5

> 简介：


redis2.8.9版本就更新了hyperloglog数据结构，基数统计的算法！

> 测试使用


```bash
127.0.0.1:6379> PFadd mykey a b c d e f g h i j 
# 创建第一组元素 mykey
(integer) 1
127.0.0.1:6379> pfcount mykey # 统计 mykey元素的基数数量
(integer) 10
127.0.0.1:6379> pfadd mykey2 i j z x c v b n m
# 创建第二组元素 mykey2
(integer) 1
127.0.0.1:6379> pfcount mykey2
(integer) 9
127.0.0.1:6379> pfmerge mykey3 mykey mykey2
# 合并mykey mykey2 到mykey3 并集
OK
127.0.0.1:6379> pfcount mykey3  # 查看并集的数量
(integer) 15
```

如果允许容错，可以使用这个!

如果不允许，就使用set或者其他数据类型!

# Bitmaps

> 位存储


位图，数据结构！操作二进制位

eg:使用bitmap来记录周一到周日的打卡！

```bash
127.0.0.1:6379> setbit sign 0 1
(integer) 0
127.0.0.1:6379> setbit sign 1 0
(integer) 0
127.0.0.1:6379> setbit sign 2 0
(integer) 0
127.0.0.1:6379> setbit sign 3 1
(integer) 0
127.0.0.1:6379> setbit sign 4 1
(integer) 0
127.0.0.1:6379> setbit sign 5 0
(integer) 0
127.0.0.1:6379> setbit sign 6 0
(integer) 0
```

查看某一天是否有打卡！

```bash
127.0.0.1:6379> getbit sign 3
(integer) 1
127.0.0.1:6379> getbit sign 6
(integer) 0
```

set和get肯定是配对的！

统计操作

```bash
127.0.0.1:6379> bitcount sign # 统计这周的打卡记录,后面还有 参数，可以指定范围
(integer) 3
```

# redis事务

==Redis单条命令保证原子性 ，要么同时成功，要么同时失败，但事务不保证原子性==

Redis事务本质：一组命令的集合！一个事务中的所有命令都 被序列化，在 事务执行过程中，会按照顺序执行！

一次性、顺序性、排他性

redis事务没有隔离级别的概念！

所有的命令在事务中，并没有直接被执行，只有发起执行命令的时候才会执行

redis的事务：

1. 
开启事务（Multi）

2. 
命令入队（）

3. 
执行事务（exrc）


> 正常执行事务！


```bash
127.0.0.1:6379> multi  #  开启事务
OK
# 命令入队
127.0.0.1:6379(TX)> set k1 v1
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> get k2
QUEUED
127.0.0.1:6379(TX)> set k3 v3
QUEUED
127.0.0.1:6379(TX)> exec  # 执行事务
1) OK
2) OK
3) "v2"
4) OK
```

> 放弃事务！


```bash
127.0.0.1:6379> multi # 开启 事务
OK
127.0.0.1:6379(TX)> set k1 v1
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> set k4 v4
QUEUED
127.0.0.1:6379(TX)> discard  # 取消事务
OK 
127.0.0.1:6379> get k4  # 事务队列中的命令都不会被执行
(nil)
```

1. 
编译型异常（代码有问题，命令有错！）事务中所有的命令都不会被执行！
```bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> set k1 v1
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> set k3 v3
QUEUED
127.0.0.1:6379(TX)> getset k3  # 错误的命令
(error) ERR wrong number of arguments for 'getset' command
127.0.0.1:6379(TX)> set k4 v4
QUEUED
127.0.0.1:6379(TX)> set k5 v5
QUEUED
127.0.0.1:6379(TX)> exec #执行事务报错
(error) EXECABORT Transaction discarded because of previous errors.
127.0.0.1:6379> get k5  # 所有的命令都不会被执行
(nil)
```


2. 
运行时异常，如果事务队列中存在语法性，那么 执行命令的时候，其他命令可以正常执行的，错误命令抛出异常
```bash
127.0.0.1:6379> set k1 "v1"  # 字符串
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> incr k1  #会在执行的时候失败
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> set k3 v3
QUEUED
127.0.0.1:6379(TX)> get k3
QUEUED
127.0.0.1:6379(TX)> exec
1) (error) ERR value is not an integer or out of range
#虽然第一条命令报错了，但依然正常执行成功了！
2) OK
3) OK
4) "v3"
127.0.0.1:6379> get k2
"v2"
```



> 监控！ watch


==悲观锁==：很悲观，什么时候都会出问题，无论做什么都会加锁！

==乐观锁==：很乐观，认为什么时候都不会出问题，所以不会上锁！更新数据的时候去判断一下，在此期间是否有人在更改过数据；获取version；更新的时候比version

mysql是基于verson版本实现的；

> redis监视测试


正常执行成功！

```bash
127.0.0.1:6379> set money 100
OK
127.0.0.1:6379> set out 0
OK
127.0.0.1:6379> watch money  # 监视 money 对象
OK
127.0.0.1:6379> multi #事务正常结束，数据期间没有发生变动，这时候就正常执行成功了！
OK
127.0.0.1:6379(TX)> decrby money 20
QUEUED
127.0.0.1:6379(TX)> incrby out 20
QUEUED
127.0.0.1:6379(TX)> exec
1) (integer) 80
2) (integer) 20
```

==测试多线程修改值，使用watch可以当做redis的乐观锁操作!==

```bash
127.0.0.1:6379> set money 100
OK
127.0.0.1:6379> set out 0
OK
127.0.0.1:6379> watch money # 监视money
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> decrby money 10
QUEUED
127.0.0.1:6379(TX)> incrby out 10
QUEUED
127.0.0.1:6379(TX)> exec #执行之前，另外一个线程，修改了我们的值，这个时候，事务就会执行失败!
(nil)
```

另一个线程的操作:

```bash
127.0.0.1:6379> get money
"100"
127.0.0.1:6379> set money 1000  # 修改了值
OK
```



# Jedis

我们要使用java来操作redis

> 什么是jedis 是redis官方推荐的java连接开发工具！使用java操作redis，那么一定要对Jedis十分的熟悉






# redis 持久化

### RDB





退出redis，也会产生rdb文件；

备份自动生成一个 dump.rdb

> 如果要恢复rdb文件！




优点：

1. 
适合大规模的数据恢复；

2. 
对数据的完整性要不高！


缺点：



### AOF



重启redis就可以生效了！

如果配置文件aof文件有错误，这时候redis是启动不起来的，我们需要修复这个文件，redis提供了一个工具， redis-check-aof  –fix 进行修复



> 重写规则：如果ａｏｆ 文件大于了64ｍ，太大了！fork了一个新进程来将我们的文件进行重写


aof默认就是文件的无限追加，文件会越来越大！就会有了重写机制！





# redis发布订阅

Redis 发布订阅（pub/sub)是一种消息通信模式：发送者（pub)接收消息。

redis客户端可以订阅任意数量的频道。



这些命令被广泛应用于构建即时通信应用，比如网络聊天室、实时广播、实时提醒等。



> 测试


订阅端：

```bash
127.0.0.1:6379> subscribe shenyang  
#订阅一个频道 shenyang
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "shenyang"
3) (integer) 1
#等待读取推送的信息
1) "message" # 消息
2) "shenyang"  # 哪个频道的消息
3) "hello world sy" # 消息的具体内容

1) "message"
2) "shenyang"
3) "hello world redis"
```

发送端：

```bash
127.0.0.1:6379> publish shenyang "hello world sy"
(integer) 1
# 发布者发布消息到频道
127.0.0.1:6379> publish shenyang "hello world redis"
(integer) 1
# 发布者发布消息到频道
```





# redis 主从复制



==默认情况下，每台redis服务器都是主节点； 数据的复制是单向的，只能由主节点到从节点。一个主节点可以有多个从节点（或没有从节点），但一个从节点只能有一个主节点；==

主从复制，读写分离，大部分情况下都在读操作，减缓服务器压力！



只要在公司中，主从复制就是必须要使用的，在真实的项目中不可能单机使用！

## 环境配置

只配置从库，不配置主库

```bash
127.0.0.1:6379> info replication #查看当前库的信息
# Replication 
role:master  # 角色
connected_slaves:0  # 没有从机
master_failover_state:no-failover
master_replid:f234f72681a506eccd84b5f7e3b5d8939fdefe0c
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
```

复制3个配置文件(redis.conf) ，然后修改对应的信息

1. 
端口

2. 
pid 名字

3. 
Log文件名字

4. 
dump.rdb名字




### 一主二从

==默认情况下，第个redis服务器都是主节==，一般情况下只用配置从机就好了！

认老大！一主（79）二从（80、81）





真实的主从配置应该在配置文件中配置，是永久的，用命令配置是临时的！

==主机才可以写，从机不写内容，只能读内容！==

![](https://tse1-mm.cn.bing.net/th/id/R-C.827d25ce5dfd37b503b09afba6b15f9d?rik=2i8sdaj71vtxJg&riu=http%3a%2f%2fimg.wandouip.com%2fcrawler%2farticle%2f2019312%2fc7a1c6f5caf59c623813bb267555d82f&ehk=tur%2fQpLoFmWMYMXyjK1EhwVpHbmOEGM64VSrhLp4EHM%3d&risl=&pid=ImgRaw&r=0#alt=)

测试：主机断开连接，从机依旧连接到主机的，但是没有写操作，这个时候，主机如果回来了，从机依旧可以获取从主机写的信息；

如果 是命令行来配置的主从，这个时候如果重启了，变会变回主机！只要变为从机，立马就会从主机中获取值！

> 复制原理




如果主机断开了连接我们可以使用 SLAVEOF no one命令让自己变成主机！其他的节点可以手动连接到最新的这个节点！如果老大修复了，就重新连接！

## 哨兵模式详解

==自动选举老大的模式！==



单个哨兵死机了就没有所以可以配置多个哨兵！





1. 配置哨兵配置文件 sentinel.conr

```bash
sentinel monitor myredis 127.0.0.1 6379 1
#sentinel monitor 被监控的名称  主机地址   端口  1
#后面的数字1，代表主机挂了，投票看让谁替换成为主机，票数最多的就会成为主机
```

1. 启动哨兵

```bash
redis-sentinel kconfig/sentinel.conf
```

如果主节点断开了，就会从从机中随机选一个重新成为老大！

如果主机回来了，只能归并到新的主机下，当做从机；这就是哨兵模式规则；

优点：

- 
哨兵集群，基于主从复制模式，所有主从配置优点，它全有

- 
主从可以切换，故障可以转移，系统的可用性就会更好

- 
哨兵模式就是主从模式的升级，手动到自动，更加方便！


缺点：

- 
redis 不好在线扩容，集群容量一旦到达上限，在线扩容就十分麻烦

- 
实现哨兵模式的配置很麻烦


# Redis缓存穿透和雪崩

## 缓存穿透（查不到）



> 解决方案 两种方案：






第种方案有点问题



## 缓存击穿（量太大，缓存过期！）



> 解决方案




## 缓存雪崩



> 解决方案



