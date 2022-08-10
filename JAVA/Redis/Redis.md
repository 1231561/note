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

## 秒杀案例

使用idea+redis模拟秒杀实例

秒杀成功：商品数-1，添加一个成功用户的id

![image-20220625195915909](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220625195915909.png)

#### 基本实现

**JSP页面**

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
<h1>iPhone 13 Pro !!!  1元秒杀！！！
</h1>

<form id="msform" action="${pageContext.request.contextPath}/doseckill" enctype="application/x-www-form-urlencoded">
   <input type="hidden" id="prodid" name="prodid" value="0101">
   <input type="button"  id="miaosha_btn" name="seckill_btn" value="秒杀点我"/>
</form>

</body>
<script  type="text/javascript" src="${pageContext.request.contextPath}/script/jquery/jquery-3.1.0.js"></script>
<script  type="text/javascript">
$(function(){
   $("#miaosha_btn").click(function(){
      var url=$("#msform").attr("action");
        $.post(url,$("#msform").serialize(),function(data){
          if(data=="false"){
             alert("抢光了" );
             $("#miaosha_btn").attr("disabled",true);
          }
      } );    
   })
})
</script>
</html>
```

**Servlet**

```java
/**
 * 秒杀案例
 */
public class SecKillServlet extends HttpServlet {
   private static final long serialVersionUID = 1L;

    public SecKillServlet() {
        super();
    }

   protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

      String userid = new Random().nextInt(50000) +"" ;
      String prodid =request.getParameter("prodid");
      
      boolean isSuccess=SecKill_redis.doSecKill(userid,prodid);
      response.getWriter().print(isSuccess);
   }

}
```

**秒杀方法实现**

```java
//秒杀过程
public static boolean doSecKill(String uid,String prodid) throws IOException {
   //1 uid和prodid非空判断
   if(uid == null || prodid == null){
      return false;
   }
   //2 连接redis
   Jedis jedis = new Jedis("192.168.65.240", 6379);
   //3 拼接key
   // 3.1 库存key
   String kcKey = "sk:"+prodid+":qt";
   // 3.2 秒杀成功用户key
   String userKey = "sk:"+prodid+":user";
   //监视库存
   //4 获取库存，如果库存null，秒杀还没有开始
   String kc = jedis.get(kcKey);
   if(kc == null){
      System.out.println("秒杀还没开始，请等待");
      jedis.close();
      return false;
   }
   // 5 判断用户是否重复秒杀操作
   if(jedis.sismember(userKey,uid)){
      System.out.println("你已秒杀过，不能在秒杀了哦");
      jedis.close();
      return false;
   }

   //6 判断如果商品数量，库存数量小于1，秒杀结束
   if(Integer.parseInt(kc) <= 0){
      System.out.println("秒杀结束");
      return false;
   }

   //7 秒杀过程
   //库存减一
   jedis.decr(kcKey);
   //添加秒杀成功用户
   jedis.sadd(userKey,uid);
   System.out.println("秒杀成功了");
   jedis.close();
   return true;
}
```

**测试**

1、设置十个秒杀商品

![image-20220625191645483](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220625191645483.png)

2、点击秒杀：
![image-20220625191703292](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220625191703292.png)

3、点击一次秒杀后

![image-20220625192506017](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220625192506017.png)

4.点击十次后

![image-20220625192804079](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220625192804079.png)

#### 使用ab工具模拟并发

**1、安装Linux并发测试工具httpd-tools**

***yum install httpd-tools***

**2、vim postfile 模拟表单提交参数,以&符号结尾;存放当前目录**

&表示后面还可以添加新的参数

![image-20220625200324261](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220625200324261.png)

**3、初始化商品数量**

![image-20220625200539692](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220625200539692.png)

**4、执行并发操作**

-n 即requests，用于指定压力测试总共的执行次数。
-c 即concurrency，用于指定的并发数。
-t 即timelimit，等待响应的最大时间(单位：秒)。
-b 即windowsize，TCP发送/接收的缓冲大小(单位：字节)。
-p 即postfile，发送POST请求时需要上传的文件，此外还必须设置-T参数。
-u 即putfile，发送PUT请求时需要上传的文件，此外还必须设置-T参数。
-T 即content-type，用于设置Content-Type请求头信息，例如：application/x-www-form-urlencoded，默认值为text/plain。
-v 即verbosity，指定打印帮助信息的冗余级别。
-w 以HTML表格形式打印结果。
-i 使用HEAD请求代替GET请求。
-x 插入字符串作为table标签的属性。
-y 插入字符串作为tr标签的属性。
-z 插入字符串作为td标签的属性。
-C 添加cookie信息，例如：“Apache=1234”(可以重复该参数选项以添加多个)。
-H 添加任意的请求头，例如：“Accept-Encoding: gzip”，请求头将会添加在现有的多个请求头之后(可以重复该参数选项以添加多个)。
-A 添加一个基本的网络认证信息，用户名和密码之间用英文冒号隔开。
-P 添加一个基本的代理认证信息，用户名和密码之间用英文冒号隔开。
-X 指定使用的和端口号，例如:“126.10.10.3:88”。
-V 打印版本号并退出。
-k 使用HTTP的KeepAlive特性。
-d 不显示百分比。
-S 不显示预估和警告信息。
-g 输出结果信息到gnuplot格式的文件中。
-e 输出结果信息到CSV格式的文件中。
-r 指定接收到错误信息时不退出程序。
-h 显示用法信息，其实就是ab -help。

执行语句：

**ab -n 2000 -c 200 -k -p ~/postfile -T application/x-www-form-urlencoded http://192.168.83.1:8080/Seckill/doseckill**

测试结果：

![image-20220625201037328](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220625201037328.png)

![image-20220625201047799](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220625201047799.png)

**由测试结果可以看出当前并发测试存在超卖问题，还有可能存在连接超时问题**

#### 解决连接超时和超卖问题

**1、解决连接超时问题：设置连接池**

生成连接池对象的工具类

```java
public class JedisPoolUtil {
   private static volatile JedisPool jedisPool = null;

   private JedisPoolUtil() {
   }

   public static JedisPool getJedisPoolInstance() {
      if (null == jedisPool) {
         synchronized (JedisPoolUtil.class) {
            if (null == jedisPool) {
               JedisPoolConfig poolConfig = new JedisPoolConfig();
               poolConfig.setMaxTotal(200);
               poolConfig.setMaxIdle(32);
               poolConfig.setMaxWaitMillis(100*1000);
               poolConfig.setBlockWhenExhausted(true);
               poolConfig.setTestOnBorrow(true);  // ping  PONG
             
               jedisPool = new JedisPool(poolConfig, "192.168.65.240", 6379, 60000 );
            }
         }
      }
      return jedisPool;
   }

   public static void release(JedisPool jedisPool, Jedis jedis) {
      if (null != jedis) {
         jedisPool.returnResource(jedis);
      }
   }
}
```

**2、解决超卖问题：**

增加乐观锁、增加事务

```java
    //秒杀过程
   public static boolean doSecKill(String uid,String prodid) throws IOException {
      //1 uid和prodid非空判断
      if(uid == null || prodid == null){
         return false;
      }
      //2 连接redis
      //通过连接池得到jedis对象
      JedisPool jedisPool = JedisPoolUtil.getJedisPoolInstance();
      Jedis jedis = jedisPool.getResource();
      //3 拼接key
      // 3.1 库存key
      String kcKey = "sk:"+prodid+":qt";
      // 3.2 秒杀成功用户key
      String userKey = "sk:"+prodid+":user";
      //监视库存
      //增加乐观锁
      jedis.watch(kcKey);
      //4 获取库存，如果库存null，秒杀还没有开始
      String kc = jedis.get(kcKey);
      if(kc == null){
         System.out.println("秒杀还没开始，请等待");
         jedis.close();
         return false;
      }
      // 5 判断用户是否重复秒杀操作
      if(jedis.sismember(userKey,uid)){
         System.out.println("你已秒杀过，不能在秒杀了哦");
         jedis.close();
         return false;
      }

      //6 判断如果商品数量，库存数量小于1，秒杀结束
      if(Integer.parseInt(kc) <= 0){
         System.out.println("秒杀结束");
         return false;
      }

      //7 秒杀过程
      //事务组队操作
      //库存减一
      Transaction multi = jedis.multi();
      multi.decr(kcKey);
      //添加秒杀成功用户
      multi.sadd(userKey,uid);
      List<Object> result = multi.exec();
      if (result == null || result.size() == 0){
         System.out.println("秒杀失败了");
         jedis.close();
         return false;
      }
      System.out.println("秒杀成功了");
      jedis.close();
      return true;
   }
}
```

**测试：**

没有出现超卖

![image-20220625204924820](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220625204924820.png)

#### 库存遗留问题

使用乐观锁，当秒杀的商品较多时，容易产生库存遗留问题。即当一个用户秒杀成功后，就会更改商品的版本号，则后面的用户就不能进行秒杀了。

解决方法：**使用lua脚本**。

**lua固定模板：**

```java
public class SecKill_redisByScript {
   
   private static final  org.slf4j.Logger logger =LoggerFactory.getLogger(SecKill_redisByScript.class) ;

   public static void main(String[] args) {
      JedisPool jedispool =  JedisPoolUtil.getJedisPoolInstance();
 
      Jedis jedis=jedispool.getResource();
      System.out.println(jedis.ping());
      
      Set<HostAndPort> set=new HashSet<HostAndPort>();

   // doSecKill("201","sk:0101");
   }
   
   static String secKillScript ="local userid=KEYS[1];\r\n" + 
         "local prodid=KEYS[2];\r\n" + 
         "local qtkey='sk:'..prodid..\":qt\";\r\n" + 
         "local usersKey='sk:'..prodid..\":usr\";\r\n" + 
         "local userExists=redis.call(\"sismember\",usersKey,userid);\r\n" + 
         "if tonumber(userExists)==1 then \r\n" + 
         "   return 2;\r\n" + 
         "end\r\n" + 
         "local num= redis.call(\"get\" ,qtkey);\r\n" + 
         "if tonumber(num)<=0 then \r\n" + 
         "   return 0;\r\n" + 
         "else \r\n" + 
         "   redis.call(\"decr\",qtkey);\r\n" + 
         "   redis.call(\"sadd\",usersKey,userid);\r\n" + 
         "end\r\n" + 
         "return 1" ;
          
   static String secKillScript2 = 
         "local userExists=redis.call(\"sismember\",\"{sk}:0101:usr\",userid);\r\n" +
         " return 1";

   public static boolean doSecKill(String uid,String prodid) throws IOException {

      JedisPool jedispool =  JedisPoolUtil.getJedisPoolInstance();
      Jedis jedis=jedispool.getResource();

       //String sha1=  .secKillScript;
      String sha1=  jedis.scriptLoad(secKillScript);
      Object result= jedis.evalsha(sha1, 2, uid,prodid);

        String reString=String.valueOf(result);
      if ("0".equals( reString )  ) {
         System.err.println("已抢空！！");
      }else if("1".equals( reString )  )  {
         System.out.println("抢购成功！！！！");
      }else if("2".equals( reString )  )  {
         System.err.println("该用户已抢过！！");
      }else{
         System.err.println("抢购异常！！");
      }
      jedis.close();
      return true;
   }
}
```

## Redis持久化RDB

RDB是指在指定的时间间隔内将内存中的数据集快照写入磁盘， 也就是行话讲的Snapshot快照，它恢复时是将快照文件直接读到内存里。

**持久化流程：**

父进程调用fork方法生成一个一模一样的子进程，一般情况下子进程和父进程共用同一段物理内存。只有进程空间的各段的内容要发生变化时，才会将父进程的内容复制一份给子进程。



![image-20220626163345651](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220626163345651.png)

​		Redis会单独创建（fork）一个子进程来进行持久化，会先将数据写入到 一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。 整个过程中，主进程是不进行任何IO操作的，这就确保了极高的性能 如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那RDB方式要比AOF方式更加的高效。**RDB的缺点是最后一次持久化后的数据可能丢失。**

#### dump.rdb文件

dump.rdb文件的位置在redis.conf配置文件有设置，默认在Redis启动时命令行所在目录下(usr/local/bin)。

![image-20220626164611337](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220626164611337.png)

#### SNAPSHOTTING配置快照

配置20秒内，有3个及其3个以上的key发生改变，就会生成快照，保存到持久层。

每20秒都会检测释放有3个或以上的key发生变化，如果有，就将其保存的持久层。

所有最后一次持久化后的数据可能丢失。

![image-20220626164704469](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220626164704469.png)

**save和bgsave**

save ：save时只管保存，其它不管，全部阻塞。手动保存。不建议。

bgsave：Redis会在后台异步进行快照操作， 快照同时还可以响应客户端请求。

可以通过lastsave 命令获取最后一次成功执行快照的时间

**flushall命令**

执行flushall命令，也会产生dump.rdb文件，但里面是空的，无意义

**Save**

格式：save 秒钟 写操作次数

RDB是整个内存的压缩过的Snapshot，RDB的数据结构，可以配置复合的快照触发条件，

默认是1分钟内改了1万次，或5分钟内改了10次，或15分钟内改了1次。

禁用

不设置save指令，或者给save传入空字符串

**stop-writes-on-bgsave-error**

![img](file:///C:\Users\鹤\AppData\Local\Temp\ksohtml17364\wps1.jpg) 

当Redis无法写入磁盘的话，直接关掉Redis的写操作。推荐yes.

**rdbcompression压缩文件**

![img](file:///C:\Users\鹤\AppData\Local\Temp\ksohtml17364\wps2.jpg) 

对于存储到磁盘中的快照，可以设置是否进行压缩存储。如果是的话，redis会采用LZF算法进行压缩。

如果你不想消耗CPU来进行压缩的话，可以设置为关闭此功能。推荐yes.

**rdbchecksum检查完整性**

![img](file:///C:\Users\鹤\AppData\Local\Temp\ksohtml17364\wps3.jpg) 

在存储快照后，还可以让redis使用CRC64算法来进行数据校验，

但是这样做会增加大约10%的性能消耗，如果希望获取到最大的性能提升，可以关闭此功能

推荐yes.

**rdb的备份**

先通过config get dir  查询rdb文件的目录 

将*.rdb的文件拷贝到别的地方

rdb的恢复

关闭Redis

先把备份的文件拷贝到工作目录下 cp dump2.rdb dump.rdb

启动Redis, 备份数据会直接加载

**优势**

适合大规模的数据恢复

对数据完整性和一致性要求不高更适合使用

节省磁盘空间

恢复速度快

**劣势**

Fork的时候，内存中的数据被克隆了一份，大致2倍的膨胀性需要考虑

虽然Redis在fork时使用了**写时拷贝技术**,但是如果数据庞大时还是比较消耗性能。

在备份周期在一定间隔时间做一次备份，所以如果Redis意外down掉的话，就会丢失最后一次快照后的所有修改。

## 持久化AOF

#### 简介：

​		AOF以日志的形式来记录每个写操作（增量保存），将Redis执行过的所有写指令记录下来(读操作不记录)， 只许追加文件但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis 重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作

Redis默认不开启AOF，所以需要在redis.conf中开启AOF。

#### 流程

（1）客户端的请求写命令会被append追加到AOF缓冲区内；

（2）AOF缓冲区根据AOF持久化策略[always,everysec,no]将操作sync同步到磁盘的AOF文件中；

（3）AOF文件大小超过重写策略或手动重写时，会对AOF文件rewrite重写，压缩AOF文件容量；

**1、开启AOF**

![image-20220627092258859](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220627092258859.png)

**2、重启或开启redis**

Redis7之后appendonly.aof本放到appendonlydir目录中。

![image-20220627093116143](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220627093116143.png)

**3、获取保存的key**

**说明AOF和RDB同时开启，系统默认取AOF的数据。**

![image-20220627100059040](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220627100059040.png)

**4、AOF启动/修复/恢复**

AOF的备份机制和性能虽然和RDB不同, 但是备份和恢复的操作同RDB一样，都是拷贝备份文件，需要恢复时再拷贝到Redis工作目录下，启动系统即加载。

正常恢复

修改默认的appendonly no，改为yes

将有数据的aof文件复制一份保存到对应目录(查看目录：config get dir)

恢复：重启redis然后重新加载

异常恢复

修改默认的appendonly no，改为yes

如遇到AOF文件损坏，通过/usr/local/bin/redis-check-aof--fix appendonly.aof进行恢复

备份被写坏的AOF文件

恢复：重启redis，然后重新加载

**5、AOF同步频率设置**

appendfsync always

始终同步，每次Redis的写入都会立刻记入日志；性能较差但数据完整性比较好

appendfsync everysec

每秒同步，每秒记入日志一次，如果宕机，本秒的数据可能丢失。

appendfsync no

redis不主动进行同步，把同步时机交给操作系统。

**6、Rewrite压缩**

AOF采用文件追加方式，文件会越来越大为避免出现此种情况，新增了重写机制, 当AOF文件的大小超过所设定的阈值时，Redis就会启动AOF文件的内容压缩， 只保留可以恢复数据的最小指令集.可以使用命令bgrewriteaof

2重写原理，如何实现重写

AOF文件持续增长而过大时，会fork出一条新进程来将文件重写(也是先写临时文件最后再rename)，redis4.0版本后的重写，是指上就是把rdb 的快照，以二级制的形式附在新的aof头部，作为已有的历史数据，替换掉原来的流水账操作。

## 主从复制

​	主从复制是主机数据更新后根据配置和策略， 自动同步到备机的master/slaver机制，**Master以写为主，Slave以读为主**。

主从复制的优点：

1、读写分离，减轻主服务器的负担。

2、容灾快速恢复。

![image-20220628160726333](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220628160726333.png)

#### 主从复制的具体实现步骤（一主二从）

**1、根目录创建myredis目录。**

![image-20220628160900507](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220628160900507.png)

**2、将redis.conf配置文件复制到myredis目录**

![image-20220628161046635](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220628161046635.png)

**3、将redis.conf配置文件的Appendonly关闭(改为no)**

开启daemonize yes

Log文件名字（省略）

![image-20220628162717810](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220628162717810.png)

**4、新建并配置主从服务器的3个配置文件**

三个配置文件名分别为redis6379.conf(主)、redis6380.conf(从)、redis6381.conf(从)

新建并配置主配置文件redis6379.conf

![image-20220628175125412](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220628175125412.png)

新建并配置从配置文件redis6380.conf

![image-20220628175238175](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220628175238175.png)

新建并配置从配置文件redis6381.conf

![image-20220628175313771](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220628175313771.png)

**5、启动三个服务器**

redis-server redis6379.conf

redis-server redis6380.conf

redis-server redis6381.conf

查看服务器端口：

![image-20220628175508285](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220628175508285.png)

**6、配置主从**

info replication

打印主从复制的相关信息

连接主服务器，发现它当前还没有从服务器

![image-20220628175755140](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220628175755140.png)

在两个从服务器上配置主服务器

![image-20220628180303429](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220628180303429.png)

![image-20220628180317741](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220628180317741.png)

再查询主服务器的从服务器，发现有了两个从服务器

![image-20220628180424414](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220628180424414.png)

**7、测试主从同步复制**

主服务器设置一个键值，两个从服务器都有这个键值

主：

![image-20220628180548441](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220628180548441.png)

从：

![image-20220628180643121](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220628180643121.png)

### 一主二仆

当从服务器挂掉后(shutdown)，重新启动时，需要重新连接原来的主服务器，不然它自己变成了主服务器，原来的主服务器就少了一个从服务器。

![image-20220704000039875](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220704000039875.png)

当主服务器挂掉后，从服务器仍然认他做主服务器。

主服务器6379

![image-20220704000426446](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220704000426446.png)

从服务器6380

![image-20220704000503186](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220704000503186.png)

当从服务器挂掉后又重启并连接主服务器或者主服务器挂掉后重启。这两个操作都能主从同步。当主服务器进行写操作后仍然会同步到从服务器中。即满足主从复制原理。

![image-20220704000855638](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220704000855638.png)

### 薪火相传和反客为主

薪火相传指从服务器作为从服务器的主服务器。

好处，减轻主服务器的写压力。

![image-20220704002454522](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220704002454522.png)

反客为主：

当一个master宕机后，后面的slave可以立刻升为master，其后面的slave不用做任何修改。

### 哨兵模式

前提：一主二从

**1、在myredis目录下创建sentinel.conf(哨兵配置文件按)**

**2、写入配置哨兵的内容**

sentinel monitor mymaster 127.0.0.1 6379 1

其中mymaster为监控对象起的服务器名称， 1 为至少有多少个哨兵同意迁移的数量。 

**3、启动哨兵**

执行redis-sentinel  /myredis/sentinel.conf 

![image-20220705175913573](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220705175913573.png)

**关闭主服务器测试：**

![image-20220705180213107](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220705180213107.png)

哨兵监测到主服务器宕机，打印一些信息，并选择6380主服务器作为主服务器。

![image-20220705180352566](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220705180352566.png)

6380为主服务器：

![image-20220705180602776](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220705180602776.png)

即使6379重启，6379也为6380的从服务器：

![image-20220705180837382](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220705180837382.png)

**缺点：复制延时**

由于所有的写操作都是先在Master上操作，然后同步更新到Slave上，所以从Master同步到Slave机器有一定的延迟，当系统很繁忙的时候，延迟问题会更加严重，Slave机器数量的增加也会使这个问题更加严重。

**故障恢复规则：**

![image-20220705181015452](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220705181015452.png)

优先级在redis.conf中默认：slave-priority 100，值越小优先级越高

偏移量是指与主服务器的相似度

每个redis实例启动后都会随机生成一个40位的runid

##  集群

 **什么是集群**

Redis 集群实现了对Redis的水平扩容，即启动N个redis节点，将整个数据库分布存储在这N个节点中，每个节点存储总数据的1/N。

Redis 集群通过分区（partition）来提供一定程度的可用性（availability）： 即使集群中有一部分节点失效或者无法进行通讯， 集群也可以继续处理命令请求。

redis集群是无中心化集群，即可以通过任何一个节点（服务器）进入集群(无论是主机还是从机)

![image-20220705235229464](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220705235229464.png)

**集群操作：**

**首先删除所有dump文件**

**1、设置六个服务器6379(主)、6380(主)、6381(主)、6389(从)、6390(从)、6391(从)**

每个服务器的配置文件安装下面形式来编写：

cluster-enabled yes  打开集群模式

cluster-config-file nodes-6379.conf 设定节点配置文件名

cluster-node-timeout 15000  设定节点失联时间，超过该时间（毫秒），集群自动进行主从切换。



`include /home/bigdata/redis.conf`

`port 6379`

`pidfile "/var/run/redis_6379.pid"`

`dbfilename "dump6379.rdb"`

`dir "/home/bigdata/redis_cluster"`

`logfile "/home/bigdata/redis_cluster/redis_err_6379.log"`

`cluster-enabled yescluster-config-file` 

`nodes-6379.confcluster-node-timeout 15000`

![image-20220705233440383](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220705233440383.png)

**2、启动所有服务**

![image-20220705233507317](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220705233507317.png)

**3、合体**

1、进入redis的解压目录

cd  /opt/redis-6.2.1/src

2、合体

 redis-cli --cluster create --cluster-replicas 1 127.0.0.1:6379 127.0.0.1:6380 127.0.0.1:6381 127.0.0.1:6389 127.0.0.1:6390 127.0.0.1:6391

![image-20220705234842220](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220705234842220.png)

4、测试

-c 采用集群策略连接，通过redis-c1i -c -p 6379(某一个节点)进入集群。

通过 cluster nodes 命令查看集群信息

![image-20220705235639350](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220705235639350.png)

**redis cluster 如何分配这六个节点?**

一个集群至少要有三个主节点。

选项 --cluster-replicas 1 表示我们希望为集群中的每个主节点创建一个从节点。

分配原则尽量保证每个主数据库运行在不同的IP地址，每个从库和主库不在一个IP地址上。

### slot(插槽)

一个 Redis 集群包含 16384 个插槽（hash slot）， 数据库中的每个键都属于这 16384 个插槽的其中一个， 集群使用公式 CRC16(key) % 16384 来计算键 key 属于哪个槽， 其中 CRC16(key) 语句用于计算键 key 的 CRC16 校验和 。

**一个集群的三个节点分担 16384 个插槽的三个部分**

![image-20220706163414303](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220706163414303.png)

设置不同的键值，对应存放到不同的插槽

![image-20220706163241710](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220706163241710.png)

### 故障恢复

当主机6379宕机后恢复，6379变为从机，6389变为主机。

![image-20220706165658440](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220706165658440.png)

### Jedis集群操作

```java
public class Test {
    public static void main(String[] args) {
        HostAndPort hostAndPort = new HostAndPort("192.168.65.240",6380);
        JedisCluster jedisCluster = new JedisCluster(hostAndPort);
        jedisCluster.set("key1","12");
        System.out.println(jedisCluster.get("b1"));
        jedisCluster.close();
    }
}
```

## Redis可能遇到的问题

### 缓存穿透

**描述：**

当请求的查询的key不存在，在redis中不存在，在数据库也不存在，所以数据库不会同步到redis中，多次访问会给数据库带来负担，导致redis失去了作用，若黑客利用此漏洞进行攻击可能压垮数据库。

![image-20220706185457502](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220706185457502.png)

**解决方案：**

（1） **对空值缓存：**如果一个查询返回的数据为空（不管是数据是否不存在），我们仍然把这个空结果（null）进行缓存，设置空结果的过期时间会很短，最长不超过五分钟

（2） **设置可访问的名单（白名单）：**

使用bitmaps类型定义一个可以访问的名单，名单id作为bitmaps的偏移量，每次访问和bitmap里面的id进行比较，如果访问id不在bitmaps里面，进行拦截，不允许访问。

（3） **采用布隆过滤器**：(布隆过滤器（Bloom Filter）是1970年由布隆提出的。它实际上是一个很长的二进制向量(位图)和一系列随机映射函数（哈希函数）。

布隆过滤器可以用于检索一个元素是否在一个集合中。它的优点是空间效率和查询时间都远远超过一般的算法，缺点是有一定的误识别率和删除困难。)

将所有可能存在的数据哈希到一个足够大的bitmaps中，一个一定不存在的数据会被 这个bitmaps拦截掉，从而避免了对底层存储系统的查询压力。

（4） **进行实时监控：**当发现Redis的命中率开始急速降低，需要排查访问对象和访问的数据，和运维人员配合，可以设置黑名单限制服务

### 缓存击穿

key对应的数据存在，但在redis中过期，此时若有大量并发请求过来，这些请求发现缓存过期一般都会从后端DB加载数据并回设到缓存，这个时候大并发的请求可能会瞬间把后端DB压垮。

![img](file:///C:\Users\鹤\AppData\Local\Temp\ksohtml15404\wps1.jpg) 

 

**解决方案**

key可能会在某些时间点被超高并发地访问，是一种非常“热点”的数据。这个时候，需要考虑一个问题：缓存被“击穿”的问题。

解决问题：

**（1）预先设置热门数据：**在redis高峰访问之前，把一些热门数据提前存入到redis里面，加大这些热门数据key的时长

**（2）实时调整：**现场监控哪些数据热门，实时调整key的过期时长

**（3）使用锁：**

（1） 就是在缓存失效的时候（判断拿出来的值为空），不是立即去load db。

（2） 先使用缓存工具的某些带成功操作返回值的操作（比如Redis的SETNX）去set一个mutex key

（3） 当操作返回成功时，再进行load db的操作，并回设缓存,最后删除mutex key；

（4） 当操作返回失败，证明有线程在load db，当前线程睡眠一段时间再重试整个get缓存的方法。

 

![img](file:///C:\Users\鹤\AppData\Local\Temp\ksohtml15404\wps2.jpg) 

 ### 缓存雪崩

key对应的数据存在，此时若有大量并发请求过来，但在redis中同时大量过期，这些请求发现缓存过期一般都会从后端DB加载数据并回设到缓存，这个时候大并发的请求可能会瞬间把后端DB压垮。

缓存雪崩与缓存击穿的区别在于这里针对很多key缓存，前者则是某一个key

 

正常访问

![img](file:///C:\Users\鹤\AppData\Local\Temp\ksohtml15404\wps3.jpg) 

缓存失效瞬间

![img](file:///C:\Users\鹤\AppData\Local\Temp\ksohtml15404\wps4.jpg) 

 

**解决方案**

缓存失效时的雪崩效应对底层系统的冲击非常可怕！

解决方案：

（1） **构建多级缓存架构：**nginx缓存 + redis缓存 +其他缓存（ehcache等）

（2） **使用锁或队列：**

用加锁或者队列的方式保证来保证不会有大量的线程对数据库一次性进行读写，从而避免失效时大量的并发请求落到底层存储系统上。不适用高并发情况

（3） **设置过期标志更新缓存：**

记录缓存数据是否过期（设置提前量），如果过期会触发通知另外的线程在后台去更新实际key的缓存。

（4） **将缓存失效时间分散开：**

比如我们可以在原有的失效时间基础上增加一个随机值，比如1-5分钟随机，这样每一个缓存的过期时间的重复率就会降低，就很难引发集体失效的事件。

### 分布式锁

### 设置锁和过期时间

当我么使用setnx age 10设置带锁的键值，不执行del age释放锁操作，就不能修改age的值。

![image-20220706223607029](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220706223607029.png)

设置过期时间:

当我们先设置锁，再为锁设置过去时间，时间一到，就会释放锁。

**出现问题，当设置锁使，出现宕机，则锁的过期时间没设置上。**

![image-20220706223857475](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220706223857475.png)

解决方法：**再设置锁的同时设置锁的过期时间**

![image-20220706224059953](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220706224059953.png)

