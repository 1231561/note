# Java面试题



##  面向对象

面向对象编程是利用类和对象编程的一种思想。类是对世间万物的抽象，对象是类的一个实例(具体的事物)。

#### 面向对象的特征

**封装**：封装隐藏了类的内部机制。外部只能通过某些特定方法访问，如私有的成员属性只能get、set方法或者反射进行访问，保护了类的数据。

**继承**：继承（is-a关系）是从已有的类中派生出一个类(子类),子类继承了父类的成员属性、及其成员方法 ，子类继承父类是对父类的拓展。private修饰的成员也被继承了，但是子类不能访问。final修饰的成员属性不能被继承，以及父类的构造方法不能被继承。子类继承父类后可以重新父类的方法(同名、参数列表相同，权限修饰符权限要高于父类被重写的方法)。子类调用父类的方法可以使用super关键字。

**多态**：多态必须基于继承、重写才能实现。基于多态，可以通过父类对象来调用被子类重写的方法。

**多态的作用：**

1、提高代码的维护性和扩展性。

2、屏蔽子类之间的差距，灵活性高。



## HashMap的原理

HashMap的底层由数组+红黑树+链表组成。

JDK1.8以后当数组的长度大于64或者链表的长度大于8时，HashMap将链表改成红黑树，目的是为了方便元素的查询，将查询的时间复杂度从O(n)优化为O(logn)。

![image-20220617165802005](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220617165802005.png)

## ArrayList和LinkedList的区别

ArrayList和LinkedList都实现了List接口。

**区别：**

ArrayList的底层是数组，LinkedList的底层基于链表结构。

ArrayList做数据访问与设置(get/set)效率比LinkedList高。

LinkedList做数据的删除插入操作效率比ArrayList高。

LinkedList占用的内存空间比ArrayList要大，因为LinkedList每个节点(对象)不仅要存节点的值，还有保存上一个节点和下一个节点的地址。

## JDK1.8的新特性

### 1、接口的默认方法

JDK1.8以后，给接口添加了一个默认的方法，即用default修饰的方法。默认方法可以有方法体，实现接口时，接口的默认方法不需要实现。

### 2、Lambda表达式

当一个接口只有一个抽象方法时，使用匿名内部类的方法实现抽象方法可以使用Lambda表达式来简化。

例如：

使用匿名内部类来实现Comparator接口的唯一抽象方法compare，达到字符串的排序。

```java
List<String> names = Arrays.asList("peterF", "anna", "mike", "xenia");
Collections.sort(names, new Comparator<String>() { 
    @Override public int compare(String a, String b) { 
        return b.compareTo(a); 
    } 
});
```

使用Lambda表达式简化后：

```java
List<String> names = Arrays.asList("peterF", "anna", "mike", "xenia");
Collections.sort(names, (String a, String b)->{ 
        return b.compareTo(a); 
});
```

#### 3、Date API

Java 8 在包java.time下包含了一组全新的时间日期API

#### 4、多重注解

## 高并发下的集合的发展

**第一代线程安全集合类 **

Vector、Hashtable 

是怎么保证线程安排的： 使用synchronized修饰方法*

缺点：效率低下 

**第二代线程非安全集合类**

ArrayList、HashMap

线程不安全，但是性能好，用来替代Vector、Hashtable             

使用ArrayList、HashMap，需要线程安全怎么办呢？ 

使用 Collections.*synchronizedList*(list); Collections.*synchronizedMap*(m); 

底层使用synchronized代码块锁 虽然也是锁住了所有的代码，但是锁在方法里边，并所在方法外边性能可以理解为稍有提高吧。毕竟进方法本身就要分配资源的 

 

**第三代线程安全集合类**

在大量并发情况下如何提高集合的效率和安全呢？ 

java.util.concurrent.* 

ConcurrentHashMap：

CopyOnWriteArrayList ：

CopyOnWriteArraySet：   注意 不是CopyOnWriteHashSet*

底层大都采用Lock锁（1.8的ConcurrentHashMap不使用Lock锁），保证安全的同时，性能也很高。     

## 接口和抽象类的区别

接口：

1、接口方法的权限修饰符只能是public。

2、接口除了默认方法，其他都是抽象方法

3、接口不能定义构造器

4、接口的变量都是常量(static final修饰，防止多实现时两个接口同名变量发送歧义)

5、接口中不能有静态方法

抽象类：

1.抽象类中可以定义构造器

2.可以有抽象方法和具体方法

3.抽象类中可以定义成员变量

4、抽象类不需要有抽象方法，但是有抽象方法的类必须是抽象类

## ACID是靠什么保证的

原子性由undolog日志来保证，它记录了需要回滚的日志信息，事务回滚时撤销已经执行成功的sql

一致性是由其他三大特性保证，程序代码要保证业务上的一致性

隔离性是由MVCC来保证

持久性由redolog来保证，mysql修改数据的时候会在redolog中记录一份日志数据，就算数据没有保存成功，只要日志保存成功了，数据仍然不会丢失

## BeanFactory和ApplicationContext的比较

Spring提供了两个IOC容器：一个是BeanFactory，一个是ApplicationContext。

ApplicationContext继承于BeanFactory。

不同：

ApplicationContext在容器启动时自动创建Bean单例对象，而BeanFactory要在调用getBean方法时才创建Bean对象。

## HashMap和HashTable对比

1. HashTable线程同步(synchronized)，HashMap非线程同步。
2. HashTable不允许<键,值>有空值，HashMap允许<键,值>有空值。
3. HashTable使用Enumeration，HashMap使用Iterator。
4. HashTable中hash数组的默认大小是11，增加方式的old*2+1，HashMap中hash数组的默认大小是16，增长方式是2的指数倍。

5.HashMap jdk1.8之前list + 链表 jdk1.8之后list + 链表,当链表长度到8时，转化为红黑树

6.HashMap链表插入节点的方式 在Java1.7中，插入链表节点使用**头插法**。Java1.8中变成了**尾插法**  

7.Java1.8的hash()中，将hash值高位（前16位）参与到取模的运算中，使得计算结果的不确定性增强，降低发生哈希碰撞的概率

## MVCC解决的问题

数据库并发场景有三种，分别为：

​		1、读读：不存在任何问题，也不需要并发控制

​		2、读写：有线程安全问题，可能会造成事务隔离性问题，可能遇到脏读、幻读、不可重复读

​		3、写写：有线程安全问题，可能存在更新丢失问题

​		MVCC是一种用来解决读写冲突的无锁并发控制，也就是为事务分配单项增长的时间戳，为每个修改保存一个版本，版本与事务时间戳关联，读操作只读该事务开始前的数据库的快照，所以MVCC可以为数据库解决一下问题：

​		1、在并发读写数据库时，可以做到在读操作时不用阻塞写操作，写操作也不用阻塞读操作，提高了数据库并发读写的性能

​		2、解决脏读、幻读、不可重复读等事务隔离问题，但是不能解决更新丢失问题

## MyBatis的优缺点

优点：

1、容易上手，简单易学(相对于Hibernate)，基于SQL编程

2、消除lJDBC大量冗余的代码，不需要手动开启和关闭数据库的连接

3、可以很好的兼容各种数据库

4、提供很多第三方插件(分页/逆向工程)

5、能够和Spring很好的集成

缺点：

1、SQL语句的编写工作量比较大，考验开发人员的SQL编写功底。

2、SQL语句依赖于数据库，导致MyBatis数据库移植性差

## MyBatis和hibernate的异同

相同的：

Hibernate与MyBatis都可以是通过SessionFactoryBuider由XML配置文件生成SessionFactory,然后由SessionFactory生成Session,最后由Session来开启执行事务和SQL语句。
其中SessionFactoryBuider, SessionFactory, Session的生命周期都是差不多的。Hibernate和MyBatis都支持JDBC和UTA事务处理.

不同点：

1、hibernate是全自动的，MyBatis是半自动的。hibernate完全可以通过对象关系模型实现对数据库的操作，拥有完整的JavaBean对象与数据库的映射结构来自动生成sql。MyBatis需要手动生成SQL语句。

2、hibernate的数据库移植性要优于MyBatis，因为hibernate的强大的映射结构和降低了对象和数据库的耦合度，而MyBatis的SQL语句的通用型取决于开发人员。

3、hibernate的日志系统比MyBaitis完整

4、MyBatis比hibernate易于上手

5、MyBatis比hibernate的sql优化要方便很多，因为hibernate的sql语句是自动生成的，MyBatis的sql语句是在xml文件里面的。

6、MyBatis支持动态sql的编写。

7、MyBatis的运行速度比hibernate块。

## mybatis中#{}和${}的区别

在MyBatis中，#{}表示预编译处理，而${}表示字符串替换。

MyBatis处理#{}，会把sql中的#{}替换为占位符？,通过调用PreparedStatement 的 set 方法来将请求参数的值赋给sql语句的占位符。

而${}表示字符串替换仅仅将括号里面的String字符串替换

#{}可以有效防止SQL注入，而${}不能，所以${}一般被废弃。

## MySQL隔离级别

MySQL定义了四种隔离级别，默认的隔离级别是REPEATABLE READ 可重复读。隔离级别低越低，并发性更高。

READ UNCOMMITTED 读取未提交内容

所有事务都能看到未提交的事务正在操作的数据。

READ COMMITTED 读取提交内容，使用最多的隔离级别。

事务只能看到已提交的事务操作的数据，事务从开始到提交前，所做的任何数据改变都是不可见的。

REPEATABLE READ 可重复读

事务在执行期间不同的时间读取同一份数据的结果是相同的

SERIALIZABLE 可串行化

SERIALIZABLE是在每个读的数据行上加锁，以下三个事务问题都不会出现，安全性最高，并发性最低，基本不使用。

事务会出现的问题：

1. 脏读

脏读是指一个事务读取了未提交事务执行过程中的数据。
当一个事务的操作正在多次修改数据，而在事务还未提交的时候，另外一个并发事务来读取了数据，就会导致读取到的数据并非是最终持久化之后的数据，这个数据就是脏读的数据。

2. 不可重复读

不可重复读是指对于数据库中的某个数据，一个事务执行过程中多次查询返回不同查询结果，这就是在事务执行过程中，数据被其他事务提交修改了。
不可重复读同脏读的区别在于，脏读是一个事务读取了另一未完成的事务执行过程中的数据，而不可重复读是一个事务执行过程中，另一事务提交并修改了当前事务正在读取的数据。

3. 虚读(幻读)

幻读是事务非独立执行时发生的一种现象，例如事务T1批量对一个表中某一列列值为1的数据修改为2的变更，但是在这时，事务T2对这张表插入了一条列值为1的数据，并完成提交。此时，如果事务T1查看刚刚完成操作的数据，发现还有一条列值为1的数据没有进行修改，而这条数据其实是T2刚刚提交插入的，这就是幻读。
幻读和不可重复读都是读取了另一条已经提交的事务（这点同脏读不同），所不同的是不可重复读查询的都是同一个数据项，而幻读针对的是一批数据整体（比如数据的个数）。

## MySQL的复制

1、从库通过手工执行change  master to 语句连接主库，提供了连接的用户一切条件（user 、password、port、ip），并且让从库知道，二进制日志的起点位置（file名 position 号）；    start  slave

2、从库的IO线程和主库的dump线程建立连接。

3、从库根据change  master  to 语句提供的file名和position号，IO线程向主库发起binlog的请求。

4、主库dump线程根据从库的请求，将本地binlog以events的方式发给从库IO线程。

5、从库IO线程接收binlog  events，并存放到本地relay-log中，传送过来的信息，会记录到master.info中

6、从库SQL线程应用relay-log，并且把应用过的记录到relay-log.info中，默认情况下，已经应用过的relay 会自动被清理purge



(1）master服务器将数据的改变记录二进制binlog日志，当master上的数据发生改变时，则将其改变写入二进制日志中；		

​		（2）slave服务器会在一定时间间隔内对master二进制日志进行探测其是否发生改变，如果发生改变，则开始一个I/OThread请求master二进制事件

​		（3）同时主节点为每个I/O线程启动一个dump线程，用于向其发送二进制事件，并保存至从节点本地的中继日志中，从节点将启动SQL线程从中继日志中读取二进制日志，在本地重放，使得其数据和主节点的保持一致，最后I/OThread和SQLThread将进入睡眠状态，等待下一次被唤醒。

![image-20220618204626457](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220618204626457.png)

## 索引

#### mysql聚簇和非聚簇索引的区别

mysql的索引类型跟存储引擎是相关的，innodb存储引擎数据文件跟索引文件全部放在ibd文件中，而myisam的数据文件放在myd文件中，索引放在myi文件中，其实区分聚簇索引和非聚簇索引非常简单，只要判断数据跟索引是否存储在一起就可以了。

#### 索引的基本原理 

1、为什么需要索引？

加快查询速度

2、索引是什么

索引是存储引擎用于快速找到记录的一种数据结构。

3、索引的原理

4、索引的数据结构有哪些

Inodb存储引擎默认是 B+Tree索引

Memory存储引擎默认Hash索引，也可以使用B+Tree索引，但只有Memory存储引擎支持Hash索引；

MyISAM存储引擎默认是 B+Tree索引

## 索引的结构有哪些

索引的数据结构和具体存储引擎的实现有关，mysql中使用较多的索引有hash索引，B+树索引，innodb存储引擎的索引实现为B+树，memory存储引擎为hash索引。

#### B+树索引

B+树是一个平衡的多叉树，从根节点到每个叶子节点的高度差值不超过1，而且同层级的二节点间有指针相关连接，在B+树上的常规检索，从根节点到叶子节点的搜索效率基本相当，不会出现大幅波动，而且基于索引的顺序扫描时，也可以利用双向指针快速左右移动，效率非常高。因为，B+树索引被广泛应用于数据库、文件系统等场景。

#### Hash索引

哈希索引就是采用一定的哈希算法，把键值换算成新的哈希值，检索时不需要类似B+树那样从根节点到叶子节点逐级查找，只需一次哈希算法即可立刻定位到相应的位置，速度非常快。Hash索引在等值查询时效率非常高，前提是键值是唯一的。但是Hash索引的键值是无序的，所有进行范围搜索效率很低，以及模糊查询、大量键值重复的情况下会产生Hash冲突。

## MySQL常用的存储引擎

#### InnoDB

InnoDB存储引擎: 主要面向OLTP(Online Transaction Processing，在线事务处理)方面的应用，是第一个完整支持ACID事务的存储引擎(BDB第一个支持事务的存储引擎，已经停止开发)。

特点：

1 支持行锁
2 支持外键
3 支持自动增加列AUTO_INCREMENT属性
4 支持事务
5 支持MVCC模式的读写
6 读的效率低于MYISAM
7.写的效率高优于MYISAM
8.适合频繁修改以及设计到安全性较高的应用
9.清空整个表的时候，Innodb是一行一行的删除

#### MyISAM

MyISAM存储引擎: 是MySQL官方提供的存储引擎，主要面向OLAP(Online Analytical Processing,在线分析处理)方面的应用。

特点：

1 独立于操作系统，当建立一个MyISAM存储引擎的表时，就会在本地磁盘建立三个文件，例如我建立tb_demo表，那么会生成以下三个文件tb_demo.frm,tb_demo.MYD,tb_demo.MYI
2 不支持事务，
3 支持表锁和全文索引
4 MyISAM存储引擎表由MYD和MYI组成，MYD用来存放数据文件，MYI用来存放索引文件。MySQL数据库只缓存其索引文件，数据文件的缓存交给操作系统本身来完成；
5 MySQL5.0版本开始，MyISAM默认支持256T的单表数据；
6.选择密集型的表：MYISAM存储引擎在筛选大量数据时非常迅速，这是他最突出的优点
7.读的效率优于InnoDB
8.写的效率低于InnoDB
9.适合查询以及插入为主的应用
10.清空整个表的时候，MYISAM则会新建表
