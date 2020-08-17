redis



## string



> * set key1 zhiyao
>   * set key value  进行存值
> * get key1
>   * get key 通过key进行取值
> * keys *
>   * 查看所有保存的key
> * exists key1
>   * 查询是否存在key
> *  append key1 wang
>   * append  key value 往key的value后面追加字符串
> *  strlen key1
>   * strlen key 查询key的对应的value的长度
> * 

~~~bash
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> set key1 zhiyao	#设置值
OK
127.0.0.1:6379> get key1	# 获取值
"zhiyao"	
127.0.0.1:6379> keys *	#查询所有的值
1) "key1"
127.0.0.1:6379> exists key1	
(integer) 1
127.0.0.1:6379> append key1 wang
(integer) 10
127.0.0.1:6379> get key1
"zhiyaowang"
127.0.0.1:6379> strlen key1
(integer) 10
127.0.0.1:6379> append key1 hahha
(integer) 15
127.0.0.1:6379> serlen key1
(error) ERR unknown command `serlen`, with args beginning with: `key1`,
127.0.0.1:6379> strlen key1
(integer) 15
~~~





~~~bash
127.0.0.1:6379> get views
"0"
127.0.0.1:6379> incr views #自增1
(integer) 1
127.0.0.1:6379> incr views
(integer) 2
127.0.0.1:6379> get views
"2"
127.0.0.1:6379> incr views
(integer) 3
127.0.0.1:6379> decr views	#自减1
(integer) 2
127.0.0.1:6379> decr views
(integer) 1
127.0.0.1:6379> decr views
(integer) 0
127.0.0.1:6379> decr views
(integer) -1
127.0.0.1:6379> incr views 2
(error) ERR wrong number of arguments for 'incr' command
127.0.0.1:6379> decr views
(integer) -2
127.0.0.1:6379> incrby views 10	#指定增量
(integer) 8
127.0.0.1:6379> incrby views 10
(integer) 18
127.0.0.1:6379> decrby views 10	#指定减少
(integer) 8



127.0.0.1:6379> getrange key1 1 3	#截取指定长度字符串
"hiy"
127.0.0.1:6379> getrange key -1
(error) ERR wrong number of arguments for 'getrange' command
127.0.0.1:6379> getrange key1 -1
(error) ERR wrong number of arguments for 'getrange' command
127.0.0.1:6379> getrange key1 0 -1	#当指定结尾为 -1 时表示取值到最后一位
"zhiyaowanghahha"
127.0.0.1:6379>



127.0.0.1:6379> set key2 abcdef
OK
127.0.0.1:6379> setrange key2 3 qwe	#替换从指定为位置开始的字符串
(integer) 6
127.0.0.1:6379> get key2
"abcqwe"
127.0.0.1:6379>

~~~



~~~bash
127.0.0.1:6379> setex key3 20 "adsafdf"		#设置key3 20秒过期,值为"adsafdf"
OK
127.0.0.1:6379> get key3
"adsafdf"
127.0.0.1:6379> get key3
(nil)
127.0.0.1:6379> ttl key3
(integer) -2
127.0.0.1:6379> setnx mykey dsfsdf
(integer) 0
127.0.0.1:6379> get mykey
"sdfsdfsd"

~~~

~~~bash
127.0.0.1:6379> mset k1 1 k2 2 k3 3 k4 4 k5 5	#批量设置值
OK
127.0.0.1:6379> keys *
1) "k5"
2) "k1"
3) "k2"
4) "k4"
5) "k3"
127.0.0.1:6379> mget k1 k2 k3 k4 k5		#批量获取值
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
~~~

~~~bash
127.0.0.1:6379> msetnx k1 1 k2 2 k3 3 k4 4 k5 5 k6 6 k7 7	
#msetnx是原子性操作,要么一起成功,要么一起失败(当key已经存在时用msetnx创建为失败,导致整个操作全部失败)
(integer) 0
127.0.0.1:6379> keys *
1) "k5"
2) "k1"
3) "k2"
4) "k4"
5) "k3"
127.0.0.1:6379> msetnx k6 6 k7 7
(integer) 1
127.0.0.1:6379> keys *
1) "k7"
2) "k5"
3) "k1"
4) "k2"
5) "k4"
6) "k6"
7) "k3"
127.0.0.1:6379> msetnx k1 1 k2 2 k3 3 k4 4 k5 5 k6 6 k7 7
(integer) 0
~~~

~~~bash
127.0.0.1:6379> getset db asdasd	#先get后set	如果不存在则返回nil,然后设置value
(nil)
127.0.0.1:6379> get db
"asdasd"
~~~



## list

在redis中list的所有操作都是l开头的

~~~bash
127.0.0.1:6379> lpush 1 asd	# 讲过一个或者多个值插入列表的头部
(integer) 1
127.0.0.1:6379> lpush 2 1231
(integer) 1
127.0.0.1:6379> lpush 1 asdasd
(integer) 2
127.0.0.1:6379> lpush 1 dasdasfdgfhcvb
(integer) 3
127.0.0.1:6379> lrange 1
(error) ERR wrong number of arguments for 'lrange' command
127.0.0.1:6379> lrange 1 0 -1		#或者列表中的值,可以指定区间
1) "dasdasfdgfhcvb"
2) "asdasd"
3) "asd"


127.0.0.1:6379> Rpush 1 1		#将一个或者多个值插入列表尾部
(integer) 4
127.0.0.1:6379> lrange 1 0 -1
1) "dasdasfdgfhcvb"
2) "asdasd"
3) "asd"
4) "1"

​~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
127.0.0.1:6379> lrange 1 0 -1
1) "dasdasfdgfhcvb"
2) "asdasd"
3) "asd"
4) "1"
127.0.0.1:6379> lpop 1	#移除list第一个元素
"dasdasfdgfhcvb"
127.0.0.1:6379> rpop 1		#移除list最后一个元素
"1"
127.0.0.1:6379> lrange 1 0 -1
1) "asdasd"
2) "asd"

#############################################################################

127.0.0.1:6379> lindex 1 1		#通过下标获取值
"asd"
127.0.0.1:6379> lindex 1 2
(nil)
127.0.0.1:6379> lindex 1 0
"asdasd"


127.0.0.1:6379> llen 1		#查询list列表的值的长度
(integer) 2

#################################
127.0.0.1:6379> rpush mylist heelo hello1 hello2 hello3
(integer) 4
127.0.0.1:6379> ltrim mylist 1 2		#截取list列表中指定的长度,list只剩下截取的部分
OK
127.0.0.1:6379> lrange mylist 0 -1
1) "hello1"
2) "hello2"

########################################################################

127.0.0.1:6379> rpush mylist 1 2 3 4 5
(integer) 5
127.0.0.1:6379> rpoplpush mylist mylist2		#移除列表中最后一个元素并加入到咪表列表中
"5"
127.0.0.1:6379> lrange mylist 0 -1
1) "1"
2) "2"
3) "3"
4) "4"
127.0.0.1:6379> lrange mylist2 0 -1
1) "5"



127.0.0.1:6379> lrange list 0 -1
1) "1"
2) "2"
3) "1"
127.0.0.1:6379> lset list 0 0		#将list中指定下标的值替换为另外一个值,相当于更新操作,如果list不存在,会报错,
OK
127.0.0.1:6379> lrange list 0 -1
1) "0"
2) "2"
3) "1"





127.0.0.1:6379> rpush list 1 2 3
(integer) 3
127.0.0.1:6379> linsert list before "2" "2.3"		
#将某个具体的值插入到list中某个元素的前面或者后面
(integer) 4
127.0.0.1:6379> lrange list 0 -1
1) "1"
2) "2.3"
3) "2"
4) "3"
127.0.0.1:6379> linsert list after "2" "2.3"
(integer) 5
127.0.0.1:6379> lrange list 0 -1
1) "1"
2) "2.3"
3) "2"
4) "2.3"
5) "3"
~~~

+ list实际上是一个链表,
+ 如果key不存在,创建新的链表
+ 如果key存在,新增内容
+ 如果移除了所有值,空链表,页代表不存在
+ 在2边插入或者改动值效率最高,中间元素相对效率降低



## set



~~~bash
127.0.0.1:6379> sadd set 1 2 3		#set添加元素
(integer) 3
127.0.0.1:6379> smembers set	 #查看指定set所有值
1) "1"
2) "2"
3) "3"


127.0.0.1:6379> sismember set 1		#判断指定值是否在指定set元素中
(integer) 1
127.0.0.1:6379> sismember set 4
(integer) 0
127.0.0.1:6379> sismember set 2
(integer) 1

127.0.0.1:6379> scard set		#获取set中元素个数
(integer) 3


127.0.0.1:6379> srem set 1		#移除set中指定元素
(integer) 1
127.0.0.1:6379> smembers set
1) "2"
2) "3"

127.0.0.1:6379> srandmember set		#随机获取set中的一个元素
"2"
127.0.0.1:6379> srandmember set
"2"
127.0.0.1:6379> srandmember set
"2"
127.0.0.1:6379> srandmember set
"3"

127.0.0.1:6379> smembers set
1) "2"
2) "3"
127.0.0.1:6379> spop set		#随机删除一个元素
"2"
127.0.0.1:6379> smembers set
1) "3"

127.0.0.1:6379> smove set set2 1	#将一个指定值移动到另外一个set中
(integer) 1
127.0.0.1:6379> smembers set2
1) "1"


127.0.0.1:6379> sdiff set set2		#指定2个set做差集
1) "4"
2) "5"

127.0.0.1:6379> sinter set set2		#指定2个set做交集
1) "2"
2) "3"

127.0.0.1:6379> sunion set set2		#指定2个set做并集
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
~~~







## hash

~~~bash
127.0.0.1:6379> hset my zhiyao 1	# set一个具体的key-vlaue
(integer) 1
127.0.0.1:6379> hget my zhiyao		#获取一个字段值
"1"
127.0.0.1:6379> hget my	

127.0.0.1:6379> hmset my zhiyao 1 a 2 b 3 c 4	#设置多个key-value
OK
127.0.0.1:6379> hmget my a b c	#获取多个key-value
1) "2"
2) "3"
3) "4"
127.0.0.1:6379> hgetall my	#获取全部的数据
1) "zhiyao"
2) "1"
3) "a"
4) "2"
5) "b"
6) "3"
7) "c"
8) "4"
127.0.0.1:6379> hdel my zhiyao		#删除指定的key的key-value
(integer) 1
127.0.0.1:6379> hgetall my
1) "a"
2) "2"
3) "b"
4) "3"
5) "c"
6) "4"

127.0.0.1:6379> hlen my		#获取hash的长度
(integer) 3


127.0.0.1:6379> hexists my a		#判断hash中的指定key是否存在
(integer) 1
127.0.0.1:6379> hexists my d
(integer) 0

127.0.0.1:6379> hkeys my	#获取所有的key
1) "a"
2) "b"
3) "c"
127.0.0.1:6379> hvals my		#获取所有的值
1) "2"
2) "3"
3) "4"

127.0.0.1:6379> hincrby my a 1		#指定key的value进行自增
(integer) 3
127.0.0.1:6379> hincrby my a 1
(integer) 4


127.0.0.1:6379> hsetnx my d 5		#如果指定key不存在,则新增
(integer) 1
127.0.0.1:6379> hsetnx my d 5
(integer) 0
~~~





## zset

~~~bash
127.0.0.1:6379> zadd my 1 a		#添加以一个值
(integer) 1
127.0.0.1:6379> zadd my 2 b 3 c 4 d 5 e		#添加以多个值
(integer) 4
127.0.0.1:6379> zrange my 0 -1		#遍历
1) "a"
2) "b"
3) "c"
4) "d"
5) "e"


127.0.0.1:6379> zadd salary 2000 a 3000 b 4000 c
(integer) 3
127.0.0.1:6379> zrangebyscore salary -inf +inf	#排序显示区间内的值(默认从小到大)
1) "a"
2) "b"
3) "c"

127.0.0.1:6379> zrevrange salary 0 -1		#排序显示区间内的值(从大到小排序)
1) "c"
2) "b"

127.0.0.1:6379> zrangebyscore salary -inf +inf withscores	#排序显示区间内的值(并显示key)
1) "a"
2) "2000"
3) "b"
4) "3000"
5) "c"
6) "4000"

	
127.0.0.1:6379> zrem salary a		#移除指定元素
(integer) 1
127.0.0.1:6379> zrange salary 0 -1
1) "b"
2) "c"

127.0.0.1:6379> zcard salary		#查看zset集合个数
(integer) 2
~~~

