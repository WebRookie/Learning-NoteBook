## Redis - Redis知识体系详解



### 知识体系

知识体系

![img](https://www.pdai.tech/images/db/redis/db-redis-overview.png)



* Redis入门 - Redis概念和基础
  * Redis是一种支持key - value等多种数据结构等存储系统。可用于缓存、事件发布或订阅、高速队列等场景。支持网络，提供字符串、哈希、列表、队列、集合结构直接存取，基于内存，可持久化



![img](https://www.pdai.tech/images/db/redis/db-redis-object-2-2.png)



* Redis入门 - 数据类型： 5种基础数据类型详解
  * Redis所有key（键）都是字符串。一般在谈论基础数据结构时，讨论的都是存储值的数据类型。主要包含5种数据类型，分别是:String、List、Set、Zset、Hash
* Redis - 3种特殊数据类型
  * Redis除了5种基础数据类型还有三种特殊数据类型，分别是HyperLogLogs（基数统计），Bitmaps（位图）和geospatial（地理位置）
* Redis - 数据类型： Stream
  * Redis5.0新增了一个数据结构Stream，是一个强大的支持的可持久化的消息队列
* Redis - 底层数据结构 - 对象机制
  * Redis的5种基础数据类型分别是String、列表 List、哈希 Hash、集合Set、有序集合zset。以及5.0中Stream。其实Redis每种对象都是由**对象结构（redisObject） 与对应编码的数据结构**组合而成。
* Redis进阶 - 持久化： RDB和AOF机制详解
  * 为了防止数据丢失以及服务重启能够恢复数据，Redis支持数据的持久化。主要分为两种，分别是RDB和AOF；实际情况下还会使用两种混合情况。
* Redis - 消息传递： 发布订阅模式
  * Redis发布订阅（pub / sub）是一种消息通信模式： 发送者（pub）发送消息，订阅者（sub）接受消息。
* Redis进阶  - 事物： Redis事务机制
  * Redis采用事件驱动机制来处理大量的网络IO。通过他自己实现的事件驱动库ae_event
* Redis - 事务 - Redis事务详解
  * Redis事务本质是一组命令的集合。事务支持一次执行多个命令，一个事务中所有命令都会被序列化。在事务执行过程中，会按照顺序串行化执行队列中的命令，其他客户端提交的命令请求不会插入到事务执行命令序列中。
* Redis进阶 - 高可用  - 主从复制
  * 要避免单点故障，即保证高可用，便需要冗余（副本）方式提供集群服务。而Redis提供了主从库模式，以保证数据副本的一致，主从库之间采用的是读取分离的方式。
* Redis进阶 - 高可用 - 哨兵机制（Redis Sentinel）详解
  * 在主从复制的基础上，如鬼哦注节点发生故障怎么办？在Redis主从集群中，哨兵机制是实现主从库自动切换的关键机制。有效的解决了主从复制模式下故障转移的问题。
* Redis - 高可扩展： 分片技术（Redis Cluster）
  * TBD



> 实践常遇见的问题

* Redis进阶 - 缓存问题 ： 一致性、穿击、穿透、雪崩、污染等





### Redis概念和基础

#### 什么是Redis

Redis是一款内存高速缓存数据库。Redis全称为：Remote Dictionary Server（远程数据服务），Redis是一个key - value存储系统（键值存储系统），支持丰富的数据类型，String 、 List、set、zset



#### 为什么需要Redis？

> Redis的优点

* 读写性能优异
  * Redis能读的速度是110000此 / s，（11w）写的速度是81000次 / s （8w）
* 数据类型丰富
  * Redis支持二进制的 String， List ,Hash , Sets 和有序的Set数据类型操作
* 原子性
  * Redis所有操作都是原子性。同时Redis还支持对几个操作全并后的原子性执行。
* 丰富的特性
  * 支持key过期等特性
* 持久化
  * Redis支持RDB， AOF等持久化方式
* 发布订阅
  * 支持发布订阅模式
* 分布式
  * Redis Cluster



#### Redis的使用场景

##### 热点数据的缓存

作为缓存使用，一般有两种方式保存数据

* 读取前，先去读取Redis，如果没有数据，读取数据库，将数据存入redis
* 插入数据时，同时写入Redis



分析：

方案一： 实施简单，但是需要注意的地方：

* 避免缓存击穿（数据库没有需要命中的数据，就导致，Redis一直没有数据，而一直命中数据库）
* 数据的实时型会相对差一些

方案二： 数据实时型强，但是开发时不便于统一处理。

总结：方案一是勇于对于数据实时型要求不高的场景，方案二是适用字典表、数据量不大的数据存储。



##### 限时业务的相关场景

redis中可以使用expire命令设置一个键的生存时间，到期后redis会删除它。可以运用在限时的优惠活动、手机验证码等业务场景。



##### 计数器相关问题

redis由于incrby命令可以实现原子性的递增，所以可以运用于高并发的秒杀活动、分布式序列号的产生、具体业务还可以体现在一个人可以发送多少条验证码、一个接口一分钟限制多少请求。一个接口一天限制调用多少次等。



##### 分布式锁

TBD



##### 延时操作

订单生产后占用库存，10分钟后检验用户是否真的购买，如果没有购买将改单据设置无效，同时还原库存。 redis提供了 Keys pace Notifications功能。允许客户订阅Pub/ Sub频道。以便以某种方式接受影响Redis数据集的事件。所以上述需求就可以改成，在订单产生时，设置一个key。同时设置10分钟后过期。我们在后台实现一个监听器，监听key的实效，监听到key失效时将后续逻辑加上。

当然也可以利用 rabbitmq、activemq 等消息中间件的延迟队列服务实现需求。



##### 排行榜相关

比如点赞排行榜，做一个SortedSet，然后以用户的openid作为上面username，以用户的点赞数作为上面的score，然后针对每一个用户做一个hash，通过zrangebyscore就可以按照点赞数获取排行榜，然后根据username获取用户hash信息。



##### 点赞、好友等相互关系存储

Redis利用集合的一些命令，比如交集、并集、差集等

在微博应用中，每个用户关注的 人在一个集合中。就很容易实现求两个人的共同好友。

 

##### 简单队列

由于Redis有List push 和List pop 就可以实现队列操作





### Redis数据结构简介

|   结构类型   |               结构存储的值               | 结构的读写能力                                               |
| :----------: | :--------------------------------------: | ------------------------------------------------------------ |
| String字符串 |        可以是字符串、整数或浮点数        | 对整个字符串或者字符串的一部分进行操；对整数或浮点数进行自增或自减操作； |
|   List列表   | 一个链表，链表上每个节点都包含一个字符串 | 对链表两端进行push和pop操作，读取单个或多个元素；根据值查找或删除元素； |
|   Set集合    |           包含字符串的无序集合           | 字符串的集合，包含的基础方法有。是否存在，添加、获取、删除；还有计算交集、并集、差集等 |
|   Hash散列   |          包含键值对的无序散列表          | 包含方法有添加、获取、删除单个元素                           |
| Zset有序集合 |        和散列一样，用于存储键值对        | 字符串成员于浮点分数之间的有序映射；元素的排列顺序由分数的大小决定；包含方法有 添加、获取、删除单个元素以及根据分值返回或成员来获取元素。 |



#### String字符串

> String 是redis中最基本的数据类型，一个key对应一个value

String 类型是二进制安全的，意味着redis的String可以包含任何数据。如数字，字符串，jpg图片或者序列化对象。



|  命令  |          简述          |       使用        |
| :----: | :--------------------: | :---------------: |
|  GET   | 获取存储在给定键中的值 |     GET name      |
|  SET   | 设置存储在给定键中的值 |  SET name value   |
|  DEL   | 删除存储在给定键中的值 |     DEL name      |
|  INCR  |    将键存储的值加1     |     INCR key      |
|  DECR  |    将键存储的值减1     |     DECR key      |
| INCRBY |  将键存储的值加上整数  | INCRBY key amount |
| DECRBY |  将键存储的值减去整数  | DECRBY key amount |



* 实战场景
  * 缓存： 经典使用场景，把常用信息，字符串，图片或者视频等信息，放到redis中，redis作为缓存层，mysql做持久化层， 降低mysql读写压力。
  * 计数器： redis是单线程模式，一个命令完了才会执行下一个，同时数据可以一步落地到其他数据源
  * session： 常见的方案Spring session + redis实现session共享



#### List列表

> Redis中的List其实就是链表（Redis用双端链表实现List）

使用List结构，我们可以轻松地实现最新的消息排队功能（比如新浪微博的TimeLine）。List的另一个应用的就是消息队列，可以利用List的PUSH操作，将任务放在List中，然后工作线程再POP操作将任务去除进行执行。



|  命令  |                             简述                             |       使用       |
| :----: | :----------------------------------------------------------: | :--------------: |
| RPUSH  |                    将给定植推入到列表右端                    | RPUSH key value  |
| LPUSH  |                    将给定值推入到列表左端                    | LPUSH key value  |
|  RPOP  |           从列表的右端弹出一个值，并返回被弹出的值           |     RPOP key     |
|  LPOP  |            从列表左端弹出一个值，并返回被弹出的值            |     LPOP key     |
| LRANGE |                 获取列表在给定范围上的所有值                 | LRANGE key 0 -1  |
| LINDEX | 通过索引获取列表的元素。也可以使用负数下标，以-1表示列表的最后一个元素，-2表示列表的倒数第二个元素，以此类推 | LINDEX key index |



* 使用列表的技巧
  * lpush + lpop = Stack（栈）
  * lpush + rpop = Queue （队列）
  * lpush + itrim = Capped Collection （有限集合）
  * lpush + brpop = Message Queue （消息队列）



* 实际常用场景
  * 微博TimeLine： 有人发布微博，用lpush 加入时间轴，展示新的列表信息
  * 消息队列



##### Set集合

> Redis的Set是String类型的无序集合。集合成员是唯一的，这就意味着集合中不能出现重复数据。

Redis中集合是通过哈希表实现的，所以添加、删除、查找的复杂度都是O（1）

|   命令    |               简述                |         使用         |      |
| :-------: | :-------------------------------: | :------------------: | :--: |
|   SADD    |     向集合添加一个或多个成员      |    SADD key value    |      |
|   SCARD   |         获取集合的成员数          |      SCARD key       |      |
| SMEMBERS  |       返回集合中的所有成员        | SMEMBERS key member  |      |
| SISMEMBER | 判断member元素是否是集合key的成员 | SISMEMBER key member |      |

* 实际场景
  * 标签（tag），给用户添加标签，或者用户给消息添加标签，这样有同一类标签或者类似标签的可以给推荐关注的事或者关注的人
  * 点赞，或点踩，收藏等。可以放到set实现



#### Hash散列

> Redis hash 是一个 string 类型的field （字段）和 value（值） 的映射表，hash适合存储对象。



|  命令   |                   简述                   |             使用              |
| :-----: | :--------------------------------------: | :---------------------------: |
|  HSET   |                添加键值对                | HSET hash-key sub-key1 vlaue1 |
|  HGET   |            获取指定散列键的值            |      HGET hash-key key1       |
| HGETALL |        获取散列中包含所有的键值对        |       HGETALL hash -key       |
|  HDEL   | 如果给定键存在于散列中，那么就移除这个键 |    HDEL hash-key sub-key1     |

* 实际场景
  * 缓存： 能直观，相比String更节省空间，比如 用户信息，视频信息等。





#### Zset有序集合

> Redis有序集合和集合一样也是string类型元素的集合，且不允许重复的成员，不同的是每一个元素都会关联一个double类型的分数，redis正是通过分数来为集合中的成员进行从小到大的排序。

有序集合的成员是唯一的，但是分数（score）却可以重复。有序集合是通过两种数据结构实现；

1. **压缩列表（ziplist）**：ziplist是为了提高存储效率而设计的一种特殊编码的双向链表。他可以存储字符串或者整数。存储整数的二进制而不是字符串形式存储，。它能在O（1）的时间复杂度。完成list两端的push和pop操作，但是因为每次操作都需要重新 分配ziplist的内存。所以实际复杂度和ziplist的内存使用量相关。
2. 跳跃表（zSkiplist）： 跳远表的性能可以保证在查找，删除，添加等操作时候在对数期望时间内完成，这个性能是可以和平衡树来相比较的，而且在实现方面比平衡要有优雅，这是采用跳跃表的主要原因。跳跃表的复杂度O（log（n））



|  命令  |                            简述                            |              使用              |
| :----: | :--------------------------------------------------------: | :----------------------------: |
|  ZADD  |         将一个带有给定分值的成员添加到有序集合里面         |   ZADD zset-key 178 member1    |
| ZRANGE | 根据元素在有序集合中所处的位置，从有序集合中就获取多个元素 | ZRANGE zset-key 0-1 withccores |
|  ZREM  |      如果给定元素存在于有序集合中，那么就移除这个元素      |     ZREM zset-key member1      |
|        |                                                            |                                |



* 实战场景
  * 排行榜： 有序集合经典使用场景。例如网站上根据用户关注数，更细时间，等排行





### 数据类型 - 3种特殊类型详解



#### HyperLogLogs（基数统计）

* 什么是基数？

  举个例子 A = { 1, 2, 3, 4, 5} B = { 3, 5, 6, 7, 9};那么基数（不重复的元素 ） = 1，2，4，6，7，9

* HyperLogLogs基数统计来解决什么问题?

这个结构可以非常省内存去统计各种计数，比如注册IP数、每日访问IP数、页面实时UV、在线用户数，共同好友数等.

* 它的优势在哪？

HyperLogLogs在Redis中每个键占用的内存都是12K，理论存储接近2^64个值



#### Bitmap （位存储）

> Bitmap即位图数据结构，都是操作二进制来进行记录，只有0 和 1两个状态

* 用来解决问题？

比如统计用户信息，活跃、不活跃，登录、未登录。打开，未打卡。两个状态的，都可以使用Bitmaps

如果存储一年的打开状态需要多少内存呢？ 365天= 365bit 1字节 = 8bit 46个字节左右。

```
setbit sign 0 1 // 周一打卡

getbit sign 3 // 1 判断周4是否打卡


bitcount sign // 4 统计打卡天数 4天
```





#### geospatial（地理位置）



##### geoadd

> 添加地理位置

```
geoadd china:city 118.76 32.04 manjing 112.55 37.86 taiyuan 123.43 41.80 shenyang
```

**规则**

两级无法直接添加，我们一般会下载城市数据(这个网址可以查询 GEO： http://www.jsons.cn/lngcode)



##### geopos

> 获取指定的成员的经度和纬度

```
geopos china:city taiyuan
> "112.54"
> "37.86"
```

获得当前定位，一定是一个坐标值



##### geodist

> 如果不存在，返回空

单位如下：

* m
* km
* mi 英里
* ft 英尺



### Stream详解



#### 为什么会设计Stream

用过Redis做消息队列都了解，基于redis的消息队列有很多种

* PUB / SUB， 订阅 / 发布模式
  * 但是发布订阅模式无法持久化，如果出现网络断开、Redis宕机等，消息就会被丢弃
* 基于List LPUSH + BRPOP或者基于Sorted-Set的实现
  * 支持了持久化，但是支持多播、分组消费

核心问题：设计一个消息队列需要考虑哪些问题？

* 消息生产
* 消息消费
  * 单播和多播（多对多）
  * 阻塞和非阻塞读取
* 消息有序性
* 消息的持久化

![img](https://www.pdai.tech/images/db/redis/db-redis-stream-1.png)



#### Stream的结构

每个Stream都有唯一的名称，它就是Redis 的可以，在我们首次使用xadd指令追加消息时自动创建



#### 增删改查

消息队列相关命令

* XADD - 添加消息到末尾
* XTRIM - 对流进行修剪，限制长度
* XDEL - 删除消息
* XLEN - 获取流包含的元素数量，即消息长度
* XRANGE - 获取消息列表，会自动过滤已经删除的消息
* XREVRANGE - 反向获取消息列表， ID从小到大
* XREAD - 以阻塞或非阻塞方式获取消息列表























