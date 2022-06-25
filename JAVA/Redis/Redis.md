# Redis

### NoSQL

技术的分类：

1、解决性能问题：Java、Jsp、RDBMS、Tomcat、HTML、Linux、JDBC、SVN

2、解决扩展性问题：Struts、Spring、SpringMVC、Hibernate、MyBatis

3、解决性能问题：NoSQL、Java线程、Hadoop、Nginx、MQ、ElasticSearch

#### NoSQL能解决的性能问题

**1、CPU及内存压力**

​		当用户第一次进入某网页需要登陆，登录后，反向代理nginx会将登录信息session保存到1号服务器中，第二次访问网页时，nginx将请求发给2号服务器处理，但是2号服务器没有1号服务器保存的session，怎么办，还要重新登录吗？

解决方案1：用户每次请求服务器时将用户信息保存到cookie中。

缺点：不安全。

方案2：将服务器1的session保存到服务器2中。

缺点：session数据冗余、节点浪费。

方案3：保存到数据库里
缺点：IO问题多。

**方案4：缓存数据库NoSQL**

**优点：将数据保存到内存中，访问速度快。**

**数据结构简单。**

![image-20220429213052832](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220429213052832.png)

**2、IO压力**

**缓存数据库：减少IO的读操作**

![image-20220429214203179](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220429214203179.png)

### NoSQL介绍

NoSQL：全称 Not Only SQL,意思为不仅仅是SQL，即非关系型数据库

NoSQL不依赖业务逻辑方式存储，以简单的key-value模式存储，大大增加了数据库的扩展能力。

**NoSQL特点**

1、不遵循SQL标准

2、不支持ACID

3.远超于SQL的性能

**NoSQL使用场景**

1、对数据高并发的读写

2、海量数据的读写

3、对数据高可扩展性

**NoSQL就是打破原先数据库的关系结构，将性能最大化。**

### 准备工作

1、安装redis

**2、启动redis**

启动方式：

1、前台启动

后台启动redis，关闭命令窗口后，redis服务器停止。

2、后台启动(推荐)

后台启动redis，关闭命令窗口后，redis服务器不会停止。

1、备份redis启动器

2、修改启动器配置文件设置daemonize no 改为yes

3、在备份redis启动器位置启动redis ：redis-server /etc/redis.conf

4、redis-cli命令使用客户端访问redis。

5、redis-cli shutdown：关闭redis连接

### redis介绍

redis的6379端口号的由来。

由Merz字母而来，6279对应其手机九键。

**redis是单线程+多路IO复用技术。**

**Mecache是多线程+锁**

**redis支持多数据类型，持久化。**

![image-20220430202923129](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220430202923129.png)

### redis的key键操作

基本操作：

1、设置键值对

格式：set key value

2、keys * 查看所有的key

3、type key 查看这个键是什么数据类型

4、del key 删除指定键值对

5、 unlink key 根据value选择非阻塞删除，仅将key从元数据删除，后续异步操作才真正删除

6、expire key 设置key的过期时间

7、ttl key 查看key的过期状态，正数表示还有多少秒过期，-1表示永不过期(不设置过期时间的key)，-2表示已过期。

8、select 数据库数字 切换数据库

9、dbsize 查看当前数据库key的数量

10、flushdb 清空当前库

11、flushall 通杀全部库

![image-20220430213851673](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220430213851673.png)

![image-20220430214133659](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220430214133659.png)

### 常用数据类型

#### String

redis键值（value）的类型是String。

关于String的常用操作：

1、set  <key><value>添加键值对，如果键值对存在则覆盖

2、get <key>	取指定键的值

3、append<key><value> 在指定键值后面加字符串

4、strlen<key><value> 获得指定键值的长度

5、setnx<key><value>	只有在键不存在时才能添加键值对

6、incr<key>	给只包含数字的字符串值加1

7.decr<key>	给只包含数字的字符串值减1

8、incrby / decrby  <key><步长>将 key 中储存的数字值增减。自定义步长。

**测试：**

![image-20220501215050621](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220501215050621.png)

9、mset<key1><value1><key2><value2><key3><value3>	同时设置多个键值对，如果键值对存在则覆盖

10、mget<key1><key2><key3> 同时获取多个键值

11、msetnx<key1><value1><key2><value2><key3><value3> 同时设置多个键值对，只有有一个键值对存在，全部操作不成功。**原子性，有一个失败则都失败**

12、getrange<key><起始位置><末位置>	获取键值字符串起始索引到末索引的子串。

13、setrange<key><起始位置><value>	在键值字符串指定索引位置插入指定字符串键值

14、setex  <key><过期时间><value>  设置键值的同时，设置过期时间，单位秒。

**测试：**

![image-20220501221304010](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220501221304010.png)

**redis的incr<key>	decr<key>	incrby / decrby操作是原子性的。**

**所谓原子操作是指不会被线程调度机制打断的操作；**

**和java的i++,i--操作不同，java的i++,i--操作是多线程的。**



**redis的String原理：**

redis的String底层是动态字符串，像java的StringBuffer一样。

### Redis列表(List)

常用集合：
1、lpush/rpush <key><value1><value2><value3>...从列表左边/右边插入值

![image-20220502205209813](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502205209813.png)

2、lpop/rpop <key> 从列表左边/右边弹出一个值，如果列表为空，则键消失

3、rpoplpush  <key1><key2>从<key1>列表右边吐出一个值，插到<key2>列表左边。

![image-20220502205402201](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502205402201.png)

4、lrange <key><start><stop> 如果starte是0，stop是-1，则取列表所有元素

5、lindex <key><index>按照索引下标获得元素(从列表左到右)

6、llen <key>获得列表长度 

7、linsert <key>  before <value><newvalue>在<value>的后面插入<newvalue>插入值

8、lrem <key><n><value>从左边删除n个value(从左到右)

9、lset<key><index><value>将列表key下标为index的值替换成value

#### List集合的底层结构

List的数据结构为快速链表quickList。

首先在列表元素较少的情况下会使用一块连续的内存存储，这个结构是ziplist，也即是压缩列表。

它将所有的元素紧挨着一起存储，分配的是一块连续的内存。

当数据量比较多的时候才会改成quicklist。

因为普通的链表需要的附加指针空间太大，会比较浪费空间。比如这个列表里存的只是int类型的数据，结构上还需要两个额外的指针prev和next。

![img](file:///C:\Users\鹤\AppData\Local\Temp\ksohtml4932\wps1.jpg) 

Redis将链表和ziplist结合起来组成了quicklist。也就是将多个ziplist使用双向指针串起来使用。这样既满足了快速的插入删除性能，又不会出现太大的空间冗余。

### Redis集合(Set)

Redis set对外提供的功能与list类似是一个列表的功能，特殊之处在于set是可以**自动排除同个键**。

常用操作：

sadd <key><value1><value2> ..... 

将一个或多个 member 元素加入到集合 key 中，已经存在的 member 元素将被忽略

smembers <key>查询该集合的所有值。

![image-20220502212546941](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502212546941.png)

sismember <key><value>判断集合<key>是否为含有该<value>值，有1，没有0

![image-20220502212610236](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502212610236.png)

scard<key>返回该集合的元素个数。

srem <key><value1><value2> .... 删除集合中的某个元素。

spop <key>	**随机**从该集合中弹出出一个值，弹出所有值，键消失

srandmember <key><n>**随机**从该集合中取出n个值。不会从集合中删除 。

smove <source><destination>value把集合中一个值从一个集合移动到另一个集合

sinter <key1><key2>返回两个集合的**交集**元素。

![image-20220502212802052](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502212802052.png)

sunion <key1><key2>返回两个集合的**并集**元素。

sdiff <key1><key2>返回两个集合的**差集**元素(key1中的，不包含key2中的)

#### 底层实现

Redis的Set底层是由哈希表实现的dict字典。

### Redis 哈希(Hash)

Redis的 hash是一个键值对集合。

因为Redis存储数据的方式本身就是键值对，所以Redis的 hash类型表示Redis的value值是一个键值对。

因此，Redis使用 hash类型可以表示一个类。Redis的key表示类名，Redis的value表示属性名以及属性值，Redis的value的键值对的键值value也是String类型的，

常用方法：

1、hset <key><field><value>给<key>集合中的  <field>键赋值<value>

![image-20220502215240542](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502215240542.png)

2、hget <key1><field>从<key1>集合<field>取出 value 

![image-20220502215306821](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502215306821.png)

3、hmset <key1><field1><value1><field2><value2>... 批量设置hash的值

4、hexists<key1><field>查看哈希表 key 中，给定域 field 是否存在。 

![image-20220502215334822](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502215334822.png)

5、hkeys <key>列出该hash集合的所有field

![image-20220502215425573](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502215425573.png)

6、hvals <key>列出该hash集合的所有value

![image-20220502215410823](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502215410823.png)

7、hincrby <key><field><increment>为哈希表 key 中的域 field 的值加上增量 1  -1

![image-20220502215500812](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502215500812.png)

8、hsetnx <key><field><value>将哈希表 key 中的域 field 的值设置为 value ，当且仅当域 field 不存在 .

#### Redis有序集合Zset(sorted set)

​		Redis有序集合Zset和set非常相似，但是Zset给每个每个成员都关联了一个**评分(权重)**，评分作为成员的从高到低的排序依据。集合的成员是唯一的，但是评分可以是重复的。

常用命令：

1、zadd  <key><score1><value1><score2><value2>…

将一个或多个 member 元素及其 score 值加入到有序集 key 当中。

![image-20220502221844843](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502221844843.png)

2、zrange <key><start><stop>  [WITHSCORES] 

返回有序集 key 中，下标在<start><stop>之间的元素，带WITHSCORES，可以让分数一起和值返回到结果集，0 到-1表示所有

![image-20220502222315353](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502222315353.png)

3、zrangebyscore key min max [withscores] [limit offset count]

返回有序集 key 中，所有 score 值介于 min 和 max 之间(包括等于 min 或 max )的成员。有序集成员按 score 值递增(从小到大)次序排列。 

![image-20220502222725119](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502222725119.png)

4、zrevrangebyscore key maxmin [withscores] [limit offset count]        

同上，改为从大到小排列。 

![image-20220502222803654](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502222803654.png)

5、zincrby <key><increment><value>    为元素的score加上增量

6、zrem  <key><value>删除该集合下，指定值的元素 

7、zcount <key><min><max>统计该集合，分数区间内的元素个数 

8、zrank <key><value>返回该值在集合中的排名，从0开始。

对比有序链表和跳跃表，从链表中查询出51

### 有序集合Zset的底层原理

（1） 有序链表

![img](file:///C:\Users\鹤\AppData\Local\Temp\ksohtml9696\wps1.jpg) 

要查找值为51的元素，需要从第一个元素开始依次查找、比较才能找到。共需要6次比较。

（2） 跳跃表

![img](file:///C:\Users\鹤\AppData\Local\Temp\ksohtml9696\wps2.jpg) 

从第2层开始，1节点比51节点小，向后比较。

21节点比51节点小，继续向后比较，后面就是NULL了，所以从21节点向下到第1层

在第1层，41节点比51节点小，继续向后，61节点比51节点大，所以从41向下

在第0层，51节点为要查找的节点，节点被找到，共查找4次。

 

从此可以看出跳跃表比有序链表效率要高

### Redis的发布和订阅

​		Redis的发布(publish)和订阅（SUBSCRIBE）是一种通信模式。当多个客户端订阅了同一个频道channel，则其中一个客户端发送信息，其他客户端可以接收到这条信息。

1、客户端可以订阅频道如下图

![img](file:///C:\Users\鹤\AppData\Local\Temp\ksohtml9696\wps23.jpg) 

2、当给这个频道发布消息后，消息就会发送给订阅的客户端

![img](file:///C:\Users\鹤\AppData\Local\Temp\ksohtml9696\wps24.jpg) 

 测试：
1、打开一个客户端1订阅频道channel1。

![image-20220503213915625](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220503213915625.png)

2、打开另一个客户端2给频道channel1发送消息。

![image-20220503213930174](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220503213930174.png)

3、客户端1可以收到消息

![image-20220503213940857](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220503213940857.png)

### Bitmaps

现代计算机用二进制（位） 作为信息的基础单位， 1个字节等于8位， 例如“abc”字符串是由3个字节组成， 但实际在计算机存储时将其用二进制表示， “abc”分别对应的ASCII码分别是97、 98、 99， 对应的二进制分别是01100001、 01100010和01100011，如下图

![img](file:///C:\Users\鹤\AppData\Local\Temp\ksohtml9696\wps25.jpg) 

合理地使用操作位能够有效地提高内存使用率和开发效率。

​	Redis提供了Bitmaps这个“数据类型”可以实现对位的操作：

（1） Bitmaps本身不是一种数据类型， 实际上它就是字符串（key-value） ， 但是它可以对字符串的位进行操作。

（2） Bitmaps单独提供了一套命令， 所以在Redis中使用Bitmaps和使用字符串的方法不太相同。 可以把Bitmaps想象成一个以位为单位的数组， 数组的每个单元只能存储0和1， 数组的下标在Bitmaps中叫做偏移量。

![img](file:///C:\Users\鹤\AppData\Local\Temp\ksohtml9696\wps26.jpg) 

### 基本命令

**1、setbit**

（1）格式

setbit<key><offset><value>设置Bitmaps中某个偏移量的值（0或1）

**2、getbit**

（1）格式

getbit<key><offset>获取Bitmaps中某个偏移量的值

**实例：**

设置键的第offset个位的值（从0算起） ， 假设现在有20个用户，userid=1， 6， 11， 15， 19的用户对网站进行了访问， 那么当前Bitmaps初始化结果如图

![](file:///C:\Users\鹤\AppData\Local\Temp\ksohtml9696\wps27.jpg) 

测试：

setbit

![image-20220503221857655](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220503221857655.png)

getbit

![image-20220503222018903](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220503222018903.png)

**3、bitcount**

​	统计字符串被设置为1的bit数。一般情况下，给定的整个字符串都会被进行计数，通过指定额外的 start 或 end 参数，可以让计数只在特定的位上进行。start 和 end 参数的设置，都可以使用负数值：比如 -1 表示最后一个位，而 -2 表示倒数第二个位，start、end 是指bit组的字节的下标数。

测试：

![image-20220504195949683](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220504195949683.png)

**原理：redis的setbit设置或清除的是bit位置，而bitcount计算的是byte位置。**

 **K1 【10000010 00100010  00100000 00000000】，对应【0，1，2，3】**

**start取0，end取1时（1~16位），对应的用户id有1、6、11、15。**



**4、bitop**

bitop是一个复合操作， 它可以做多个Bitmaps的and（交集） 、 or（并集） 、 not（非） 、 xor（异或） 操作并将结果保存在destkey中。

(1)格式

bitop and(or/not/xor) <destkey> [key…]

#### Bitmaps与set对比

假设网站有1亿用户， 每天独立访问的用户有5千万， 如果每天用集合类型和Bitmaps分别存储活跃用户可以得到表

| set和Bitmaps存储一天活跃用户对比 |                    |                  |                        |
| -------------------------------- | ------------------ | ---------------- | ---------------------- |
| 数据类型                         | 每个用户id占用空间 | 需要存储的用户量 | 全部内存量             |
| 集合类型                         | 64位               | 50000000         | 64位*50000000 = 400MB  |
| Bitmaps                          | 1位                | 100000000        | 1位*100000000 = 12.5MB |

很明显， 这种情况下使用Bitmaps能节省很多的内存空间， 尤其是随着时间推移节省的内存还是非常可观的

| set和Bitmaps存储独立用户空间对比 |        |        |       |
| -------------------------------- | ------ | ------ | ----- |
| 数据类型                         | 一天   | 一个月 | 一年  |
| 集合类型                         | 400MB  | 12GB   | 144GB |
| Bitmaps                          | 12.5MB | 375MB  | 4.5GB |

但Bitmaps并不是万金油， 假如该网站每天的独立访问用户很少， 例如只有10万（大量的僵尸用户） ， 那么两者的对比如下表所示， 很显然， 这时候使用Bitmaps就不太合适了， 因为基本上大部分位都是0。

| set和Bitmaps存储一天活跃用户对比（独立用户比较少） |                    |                  |                        |
| -------------------------------------------------- | ------------------ | ---------------- | ---------------------- |
| 数据类型                                           | 每个userid占用空间 | 需要存储的用户量 | 全部内存量             |
| 集合类型                                           | 64位               | 100000           | 64位*100000 = 800KB    |
| Bitmaps                                            | 1位                | 100000000        | 1位*100000000 = 12.5MB |

### HyperLogLog

​		HyperLogLog的作用是统计基数，基数是指一个不含重复元素的集合的元素个数，相当于Java的Set调用size()方法。

**应用场景：统计网页用户访问量**

​	**HyperLogLog的优点：**

​		Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog不保存元素，只统计元素个数。

HyperLogLog 的优点是:在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定的、并且是很小的。

**1、pfadd** 

（1）格式

pfadd <key>< element> [element ...]  添加指定元素到 HyperLogLog 中

name：

![image-20220504205240432](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220504205240432.png)

age：

![image-20220504205304051](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220504205304051.png)

**2、pfcount**

（1）格式

pfcount<key> [key ...] 计算HLL的近似基数，可以计算多个HLL，比如用HLL存储每天的UV，计算一周的UV可以使用7天的UV合并计算即可

![image-20220504205334537](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220504205334537.png)

**3、pfmerge**

（1）格式

pfmerge<destkey><sourcekey> [sourcekey ...]  将一个或多个HLL合并后的结果存储在另一个HLL中，比如每月活跃用户可以使用每天的活跃用户来合并计算可得

![image-20220504205514048](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220504205514048.png)

### Geospatial

​		Redis 3.2 中增加了对GEO类型的支持。GEO，Geographic，地理信息的缩写。该类型，就是元素的2维坐标，在地图上就是经纬度。redis基于该类型，提供了经纬度设置，查询，范围查询，距离查询，经纬度Hash等常见操作。

#### 相关命令

**1、geoadd**

格式

geoadd<key>< longitude><latitude><member> [longitude latitude member...]  添加地理位置（经度，纬度，名称）

添加上海、北京、重庆、深圳四个城市的经纬度

![image-20220504212009864](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220504212009864.png)

**2、geopos**  

 格式

geopos  <key><member> [member...]  获得指定地区的坐标值

测试:

![image-20220504212206083](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220504212206083.png)

3、geodist

 格式

geodist<key><member1><member2>  [m|km|ft|mi ]  获取两个位置之间的直线距离

测试：上海到重庆的距离

![image-20220504212437599](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220504212437599.png)



## Jedis操作

Jedis是一个使用Java操作Redis的一个API，就像java操作数据库需要用到JDBC这个API。

#### 使用步骤

**1、idea创建Maven项目，引入Jedis依赖**

```java
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>4.2.2</version>
</dependency>
```

**2、测试连接：**

```java
public class JedisTest {
    @Test
    public void jedisTest(){
        //连接需要传入部署给Redis的虚拟机的IP地址和端口号
        Jedis jedis = new Jedis("192.168.65.240",6379);
        String ping = jedis.ping();
        System.out.println(ping);
    }
}
```

**注意：**

1、需要禁用Linux的防火墙或者开发端口号，这里采用禁用防火墙。

**systemctl stop/disable firewalld.service**

2、在redis.conf配置文件中修改两个地方：

注释掉bind 127.0.0.1   protected-mode yes 改成 no

3、重启redis

shutdown关闭后redis-server启动



**4、测试redis常用命令**

```java
public class JedisTest {
    @Test
    public void jedisTest(){
        Jedis jedis = new Jedis("192.168.65.240",6379);
        jedis.flushDB();//清空Redis库里的所有键
        Set<String> keys = jedis.keys("*");
        System.out.println(keys.size());
        jedis.set("k1","tom");//设置String的键值对
        jedis.set("k2","jack");
        Set<String> keys1 = jedis.keys("*");
        for(String key:keys1){
            System.out.println(key);
        }
        jedis.close();//关闭Jedis资源

    }
}
```

测试结果：

![image-20220504222911119](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220504222911119.png)

## Redis应用实例1：手机验证码

随机生成手机验证码，将手机号和验证码以Redis键值对保存，一个手机号一天最多发送3条验证码。

取Redis的手机号对应的验证码和用户输入的验证码对比，一样则成功。

```Java
public class PhoneCode {
    public static void main(String[] args) {
        getPhoneCount("123456");//生成验证码
//        equalCode("123456","5877623");//对比验证码
    }//对比验证码
    public static void equalCode(String phone,String code){
        Jedis jedis = new Jedis("192.168.65.240", 6379);
        String phoneCode = "phone:"+phone+":code";
        String redisCode = jedis.get(phoneCode);
        if (redisCode.equals(code)){
            System.out.println("成功");
        }else {
            System.out.println("失败");
        }
    }//更新一个手机号一天已发送的验证码以及验证码条数，条数大于三则不将验证码写入Redis。
    public static void getPhoneCount(String phone){
        Jedis jedis = new Jedis("192.168.65.240", 6379);
        String phoneCode = "phone:"+phone+":code";
        String phoneCount = "phone:"+phone+":count";
        String count = jedis.get(phoneCount);
        if(count==null){
            jedis.setex(phoneCount,24*60*60,"1");
        }else if(Integer.parseInt(count)<3){
            jedis.incr(phoneCount);
        }else{
            System.out.println("今天已发送三次验证码");
            jedis.close();
            return;
        }
        String code = getPhoneCode();
        jedis.setex(phoneCode,120,code);
        jedis.close();

    }
    //随机生成手机验证码
    public static String getPhoneCode(){
        Random random = new Random();
        StringBuilder sb = new StringBuilder();
        while(sb.length()<=6){
            sb.append(random.nextInt(10));
        }
        return sb.toString();
    }
}
```

### Redis整合SpringBoot

1、添加redis的相关依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<!-- spring2.X集成redis所需common-pool2-->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-pool2</artifactId>
</dependency>
```

2.添加配置类：

```java
@EnableCaching
@Configuration
public class RedisConfig extends CachingConfigurerSupport {

    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        RedisSerializer<String> redisSerializer = new StringRedisSerializer();
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        template.setConnectionFactory(factory);
//key序列化方式
        template.setKeySerializer(redisSerializer);
//value序列化
        template.setValueSerializer(jackson2JsonRedisSerializer);
//value hashmap序列化
        template.setHashValueSerializer(jackson2JsonRedisSerializer);
        return template;
    }

    @Bean
    public CacheManager cacheManager(RedisConnectionFactory factory) {
        RedisSerializer<String> redisSerializer = new StringRedisSerializer();
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
//解决查询缓存转换异常的问题
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
// 配置序列化（解决乱码的问题）,过期时间600秒
        RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(Duration.ofSeconds(600))
                .serializeKeysWith(RedisSerializationContext.SerializationPair.fromSerializer(redisSerializer))
                .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(jackson2JsonRedisSerializer))
                .disableCachingNullValues();
        RedisCacheManager cacheManager = RedisCacheManager.builder(factory)
                .cacheDefaults(config)
                .build();
        return cacheManager;
    }
}
```

3、添加控制器类并测试：

```java
@RestController
@RequestMapping("/RedisTest")
public class RedisTestController {
    @Autowired
    RedisTemplate redisTemplate;

    @GetMapping("")
    public String redisTest(){
        redisTemplate.opsForValue().set("name","张三");
        Object name = redisTemplate.opsForValue().get("name");
        return (String) name;
    }
}
```

![image-20220619162416445](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220619162416445.png)

## Redis事务

Redis的事务是一个单独隔离的操作（类似mysql数据库的隔离性）,事务的所有命令都会被序列化，即在事务队列里按顺序执行命令，在执行命令途中不会被其他客户端请求打断。

Redis事务采用串联多个命令来防止其他命令插队。

#### 1、基本操作

使用**multi指令**开启事务，然后进行指令组队，组完队之后将使用**exec命令**启动事务。

![image-20220619221151992](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220619221151992.png)

**discard命令**可以停止组队



当组队时，命令不合法，将会停止组队，例如以下没设置键值

![image-20220619222535393](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220619222535393.png)



当组队时命令可以执行，执行的时候有错误，如以下英文字符串不能像数字字符串一样加一，在执行过程中，k1键值不合法的加一。所以，除了k1，其他的都正常执行。

![image-20220619223347611](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220619223347611.png)

#### 2、事务解决冲突

举一个取钱的例子，如果有三个线程都进行取钱，则最终可能会产生冲突。

![image-20220620203359178](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220620203359178.png)

下面有悲观锁和乐观锁可以解决这个冲突问题。

#### 1、悲观锁

每一个线程在执行操纵时都会占用锁，等到操作完毕才会释放锁让其他线程执行，

![image-20220620203858689](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220620203858689.png)

#### 2、乐观锁

乐观锁是在线程读数据的时候不会上锁，每个线程都有操作机会，每次操作都会有一个版本号，版本号符合要求的，且操作符合要求的可以执行。例如抢票实例：100个人强1张票，每个人都有抢的机会，但是成功只有一个人。

**乐观锁适合用在多读的应用领域上，提高数据的吞吐量。**

![image-20220620204222861](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220620204222861.png)
