# Mybatis

Mybatis是一个优秀的持久层框架。

Mybatis避免了JDBC的很多繁琐的操作，比如手动设置参数和获取结果集等。

Mybatis原名为ibatis（apache公司），现已迁入GitHub

#### mybatis中文官方文档

https://mybatis.org/mybatis-3/zh/getting-started.html

#### Mybatis的优点

1、

### 第一个Mybatis程序

**步骤：**

1、创建maven项目

2、maven导入依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
<!--父项目-->
    <groupId>com.mybatis</groupId>
    <artifactId>Mybatis</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>mybatis-01</module>
    </modules>

    <!--    依赖-->
    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.16</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.6</version>
        </dependency>

    </dependencies>
</project>
```

3、创建模块

直接在Mybatis项目创建一个子模块mybatis-01

![image-20220324171616298](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220324171616298.png)

好处：子模块mybatis-01直接继承了父项目Mybatis的依赖，就不用重新导这些依赖了。

**pom.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
<!--    父项目-->
    <parent>
        <artifactId>Mybatis</artifactId>
        <groupId>com.mybatis</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>mybatis-01</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

</project>
```

4、编写mybatis核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--配置核心配置文件-->
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
<!--               userSSL=true表示安全连接，userUnicode=true表示支持中文，serverTimezone=Asia/Shanghai表示时区，mysql8一定要配置时区，不然乱码，&amp;表示&的转义 -->
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?serverTimezone=Asia/Shanghai&amp;userSSL=true&amp;userUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="123456Hh"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <!--每个Mapper.xml配置都需要在核心配置文件注册，mybatis的资源路径不支持*等通配符-->
         <mapper resource="com/dao/UserMapper.xml"/>
    </mappers>
</configuration>
```

**5、编写mybatis工具类（MybatisUtils）**

#### SqlSession

​		每个线程都应该有它自己的 SqlSession 实例。SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是请求或方法作用域。 绝对不能将 SqlSession 实例的引用放在一个类的静态域，甚至一个类的实例变量也不行。 也绝不能将 SqlSession 实例的引用放在任何类型的托管作用域中，比如 Servlet 框架中的 HttpSession。 如果你现在正在使用一种 Web 框架，考虑将 SqlSession 放在一个和 HTTP 请求相似的作用域中。 换句话说，每次收到 HTTP 请求，就可以打开一个 SqlSession，返回一个响应后，就关闭它。 这个关闭操作很重要，为了确保每次都能执行关闭操作，你应该把这个关闭操作放到 finally 块中。

```java
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

public class MybatisUtils {
    private static SqlSessionFactory sqlSessionFactory;
    //每个基于 MyBatis 的应用都是以一个 SqlSessionFactory 的实例为核心的
    //SqlSessionFactory 的实例可以通过 SqlSessionFactoryBuilder 获得
    static {
        try {
            //获取mybatis核心配置文件资源
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            //获取sqlSessionFactory对象
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
//    有了 SqlSessionFactory，我们可以从中获得 SqlSession 的实例
    //该静态方法返回SqlSession 的实例
    public static  SqlSession getSqlSession(){
        return sqlSessionFactory.openSession();
    }
}
```

**6、创建UserDao接口**

```java
import com.pojo.User;

import java.util.List;

public interface UserDao {
    //查询所有用户
    public List<User> getUserList();
}
```

**7、编写Mapper配置文件UserMapper代替UserDao接口的实现类UserDaoImpl**（实现增删查改操作）

**以后的Dao都用Mapper来表示**

Mapper配置文件省去了实现类UserDaoImpl的建立数据库连接（preparestatement）和处理结果集（resource）的工作。

下面来实现查询所有用户的方法

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--绑定UserDao，相当于代替UserDao的实现类，实现UserDao的方法-->
<mapper namespace="com.dao.UserDao">
<!--   查询语句，id表示接口对应的返回查询方法，resultType表示返回的结果集-->
    <select id="getUserList" resultType="com.pojo.User">
<!--设置了mysql方言后，就可以直接关联数据库mybatis的表user-->
        select * from mybatis.user;
    </select>
</mapper>
```

测试：

使用完sqlSession资源后需要手动关闭，建议把它写到finally里面。

mapper底层运用到了反射

```java
import com.pojo.User;
import com.utils.MybatisUtils;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;

import java.util.List;
public class UserDaoTest {

    @Test
    public void test(){
            SqlSession sqlSession = MybatisUtils.getSqlSession();
        try {
            //方式一：通过动态代理获取UserDao的实现类对象
     	 UserDao mapper = sqlSession.getMapper(UserDao.class);
       	 List<User> userList = mapper.getUserList();
            //方式二：老版方法
//            List<User> userList = sqlSession.selectList("com.dao.UserDao.getUserList");
            //调用.getUserList方法
            for (User user:userList){
                System.out.println(user);
            }
        } finally {
            sqlSession.close();
        }
    }
}
```

#### 常出现的问题

1、编写mybatis核心配置文件后忘记注册Mapper配置文件，导致测试时找不到Mapper配置文件的路径。

**2、由于maven的约定大于配置原则，maven只会将工程resource目录下的文件导出到targert的classes目录下，而不会将工程的其他目录的文件导出。**

**所以如果Mapper配置文件放在java目录下，则需要在maven加上build标签配置**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
<!--父项目-->
    <groupId>com.mybatis</groupId>
    <artifactId>Mybatis</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>mybatis-01</module>
    </modules>

    <!--    依赖-->
    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.16</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.6</version>
        </dependency>

    </dependencies>
    <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.xml</include>
                    <include>**/*.properties</include>
                </includes>
            </resource>

            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.xml</include>
                    <include>**/*.properties</include>
                </includes>
            </resource>
        </resources>
    </build>
</project>
```

### mybatis实现增删查改

1、在接口UserMapper增加增删查改方法

UserMapper和UserDao一样

```java
public interface UserMapper {
    //查询全部用户
    public List<User> getUserList();
    //通过id查询用户
    public User getUserById(int id);
    //添加用户
    public int addUser(User user);
    //修改用户
    public int updateUser(User user);
    //删除用户
    public int deleteUser(int id);
}
```

2、Mapper配置文件也加上相关方法

相关参数：
id：方法名

resultType：方法返回值类型

parameterType：方法参数类型（可不写）

#{}相当于占位符，里面放User实例类的属性

不在#{}的属性是数据库的，在#{}里面的属性是实例类的。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--绑定UserDao，下面相当于实现UserDao的方法-->
<mapper namespace="com.dao.UserMapper">
<!--   查询语句，id表示接口对应的返回查询方法，resultType表示返回的结果集-->
    <select id="getUserList" resultType="com.pojo.User">
#  设置了mysql方言后，就可以直接关联数据库mybatis的表user
        select * from mybatis.user;
    </select>
<!--根据id查找用户-->
    <select id="getUserById" parameterType="int" resultType="com.pojo.User">
        select * from mybatis.user where id=#{id};
    </select>
<!--添加用户-->
    <insert id="addUser" parameterType="com.pojo.User">
        insert into mybatis.user(id, name, password) VALUES (#{id},#{name},#{password});
    </insert>
<!--修改用户-->
    <update id="updateUser" parameterType="com.pojo.User">
        update mybatis.user set name=#{name},password=#{password} where id=#{id};
    </update>
<!--删除用户-->
    <delete id="deleteUser" parameterType="com.pojo.User">
        delete from mybatis.user where id=#{id};
    </delete>
</mapper>
```

测试：

注意mybatis默认将数据库操作设置为事务，所有除了查询操作，都要提交事务操作1才能生效。

```java
//通过id查询用户
@Test
public void test1(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    User user = mapper.getUserById(1);
    System.out.println(user);
    sqlSession.close();
}
//添加用户
@Test
public void test2(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    int number = mapper.addUser(new User(4, "李逵", "1616166"));
    System.out.println(number);
    //提交事务
    sqlSession.commit();
    sqlSession.close();
}
//修改用户
@Test
public void test3(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    int number = mapper.updateUser(new User(1, "赵六", "156561"));
    System.out.println(number);
    sqlSession.commit();
    sqlSession.close();
}
//删除用户
@Test
public void test4(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    int number = mapper.deleteUser(4);
    System.out.println(number);
    sqlSession.commit();
    sqlSession.close();
}
```

### 模糊查询

在数据库表中加上两个姓李的用户

![image-20220324234550428](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220324234550428.png)

接口加上模糊查询方法

```java
//模糊查询
public List<User> getUserByLike(String value);
```

Mapper.xml加上模糊查询的方法

```xml
<!--    模糊查询-->
    <select id="getUserByLike" resultType="com.pojo.User" parameterType="String">
        <!--    方式1-->
        select * from mybatis.user where name like #{value};
        <!--    方式2-->
        select * from mybatis.user where name like '%${value}%';
    </select>
```

测试：

```java
//模糊查询
@Test
public void test5(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    //对应方式1：
    List<User> users = mapper.getUserByLike("%李%");
    //对应方式2：
    List<User> users = mapper.getUserByLike("李");
    System.out.println(users);
    sqlSession.close();
}
```

结果：

![image-20220324234954919](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220324234954919.png)

### 核心配置文件的优化

#### 环境配置

​	我们可以在环境配置中配置多个环境，**尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境**，可以通过default（默认环境）来切换环境。

**相关参数：**

id：环境名

transactionManager（事务管理）：默认是JDBC，MANAGED 也是一种默认管理形式

dataSource（数据资源）：默认是POOLED（连接池）

```xml
    <environments default="development">
        <!--第一个环境配置-->
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
<!--               userSSL=true表示安全连接，userUnicode=true表示支持中文，characterEncoding=UTF-8表示时区，mysql8一定要配置时区，不然乱码，&amp;表示&的转义 -->
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?serverTimezone=Asia/Shanghai&amp;userSSL=true&amp;userUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="123456Hh"/>
            </dataSource>
        </environment>
 		<!--第二个环境配置-->
        <environment id="test">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <!--               userSSL=true表示安全连接，userUnicode=true表示支持中文，characterEncoding=UTF-8表示时区，mysql8一定要配置时区，不然乱码，&amp;表示&的转义 -->
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?serverTimezone=Asia/Shanghai&amp;userSSL=true&amp;userUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="123456Hh"/>
            </dataSource>
        </environment>
    </environments>
```

#### 属性优化

**除了上面的配置方式，我们还可以使用引入外部的文件，来替换连接数据库写死的部分。**

使用propertise标签来引入外部文件：

由于约定大于配置，propertise标签必须放在配置的最前面

![image-20220325142713904](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220325142713904.png)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--配置核心配置文件-->
<configuration>
    <!--引入外部文件-->
    <properties resource="db.properties"/>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
<!--               userSSL=true表示安全连接，userUnicode=true表示支持中文，characterEncoding=UTF-8表示时区，mysql8一定要配置时区，不然乱码，&amp;表示&的转义 -->
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="com/dao/UserMapper.xml"/>
    </mappers>
</configuration>
```

**properties标签也可以在内部设置属性的值，但是外部文件的属性设置优先级大于内部属性设置。**

#### 别名优化

在核心配置文件中加上别名配置：

**方式1：给单一类起别名**

```xml
    <typeAliases>
<typeAlias alias="User" type="com.pojo.User" ></typeAlias>
    </typeAliases>
```

**方式2：给包名起别名**

```xml
    <typeAliases>
        <package name="com.pojo"/>
    </typeAliases>
```

**情况一：没注解的情况下给包下的所有类起以小写字母开头其他字母不变的别名**

**情况二：有注解的情况下，给类起注解设置的别名**

给User类起别名person

![image-20220325151421957](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220325151421957.png)

起了别名后：Mapper.xml匹配文件的resultType属性就可以不用写全路径名了，可以用别名代替

![image-20220325151523147](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220325151523147.png)

**方式1起别名适用于实体类比较少的情况。方式2使用于实体类较多的情况。**

### 核心配置的映射器配置

有三种方式：

方式一：直接根据路径找到Mapper配置文件，就是我们最初使用的方式，也是最不容易出错的方式。

```xml
    <mappers>
<mapper resource="com/dao/UserMapper.xml"/>
    </mappers>
```

方式二：直接找到Mapper配置文件绑定的接口

```xml
    <mappers>
<mapper class="com.dao.UserMapper"/>
    </mappers>
```

方式三：根据包名找到Mapper配置文件绑定的接口

```xml
    <mappers>
        <package name="com.dao"/>
    </mappers>
```

注意：方式二和方式三要求Mapper配置文件和它绑定的接口要在同一包下，而且名字要相同。

![image-20220325154210739](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220325154210739.png)

### 生命周期和作用域

**SqlSessionFactoryBuilder**：

使用它来创建一个SqlSessionFactory，之后就没有用了。

**SqlSessionFactory：**

可以把SqlSessionFactory想象成一个数据库连接池，只要被创建，在应用的运行期间一直存在。

就像数据库连接池一样，没必要创建多个SqlSessionFactory实例，会占用很多资源。

SqlSessionFactory的最佳作用域是应用作用域（Application）。

所以最简单的状态就是**单例模式**

**SqlSession**

SqlSession相当于一个连接到连接池的请求

SqlSession的实例不是线程安全的，因此不能被共享，它的最佳作用域是请求或方法作用域

使用完SqlSession后要即使关闭，就像连接池的每一个连接，用完就要关闭，不然会浪费资源。

**相关图解：**

![image-20220325163956086](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220325163956086.png)

**一个Mapper对应一个业务,例如实现增删查改的业务等。**

### 解决属性名和字段名不一致问题

属性名和字段名不一致说白了就是实体类属性和数据库列名不一致。

例如：将User类中的name属性名改成Username，而对应数据库表的属性名仍是name。

![image-20220325172411967](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220325172411967.png)

![image-20220325172402084](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220325172402084.png)

在根据id查询用户时，因为username和name不匹配，所以类型处理器就不会给username赋值，username就为null。

#### ResultMap结果集映射

在UserMapper.xml配置文件中，加上ResultMap结果集映射，resultMap标签的id属性就是resultMap的值，type就是数据类型，colum表示数据库表的列，property表示实体类对象的属性，哪个属性不匹配，就建立哪个属性与数据库表的列的联系。

```xml
    <resultMap id="UserMap" type="User">
        <result column="name" property="username"></result>
    </resultMap>
<!--根据id查找用户-->
    <select id="getUserById" resultMap="UserMap">
        select * from mybatis.user where id=#{id};
    </select>
```

### 日志

#### 日志工厂

![image-20220325175946985](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220325175946985.png)

主要使用的日志是LOG4J和STDOUT_LOGGING

#### STDOUT_LOGGING日志

1、配置

在核心配置文件的properties和typeAliases标签中添加setting标签，设置STDOUT_LOGGING日志

![image-20220325180313318](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220325180313318.png)

2、结果

执行根据id查询用户操作时，STDOUT_LOGGING日志会输出JDBC的操作等信息。

![image-20220325180430451](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220325180430451.png)

### LOG4J日志

一个灵活的日志，可以通过配置文件来控制日志输出的位置，还可以控制输出的格式等。

使用LOG4J日志步骤：

1、导入LOG4J日志的依赖

2、添加log4j.properties配置文件

```properties
log4j.rootLogger=DEBUG,console,file

#控制台输出的相关设置
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.Target = System.out
log4j.appender.console.Threshold=DEBUG
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=[%c]-%m%n

#文件输出的相关设置
log4j.appender.file = org.apache.log4j.RollingFileAppender
#生成日志文件的位置
log4j.appender.file.File=./log/test.log
log4j.appender.file.MaxFileSize=10mb
log4j.appender.file.Threshold=DEBUG
log4j.appender.file.layout=org.apache.log4j.PatternLayout
#设置日志生成的日期信息
log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n

#日志输出级别
log4j.logger.org.mybatis=DEBUG
log4j.logger.java.sql=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.ResultSet=DEBUG
log4j.logger.java.sql.PreparedStatement=DEBUG
```

在mybatis核心配置文件配置：

```xml
<settings>
    <setting name="logImpl" value="LOG4J"/>
</settings>
```

测试：

 logger.info，logger.debug，logger.error等价于System.out.println，我们可以在不同的环境下使用不同的输出，在debug环境下使用logger.debug，在try-catch环境下使用logger.error等，然后再日志生成的文件中查看日志信息。

```java
@Test
public void testLog4j(){
    logger.info("info:进入了testLog4j");
    logger.debug("debug:进入了testLog4j");
    logger.error("error:进入了testLog4j");
}
```

### 分页

通过Map集合实现

步骤：

1、在接口里添加分页方法

```java
//分页
public  List<User> getPage(Map<String,Integer> map);
```

2、在UserMapper.xml里加上分页的配置

占位符对应Map的key。

```xml
<!--    分页-->
    <select id="getPage" parameterType="map" resultMap="userMap">
        select * from mybatis.user limit #{startId} ,#{endId};
    </select>
    <resultMap id="userMap" type="map">
        <result column="name" property="username"></result>
    </resultMap>
```

3、测试：

```java
@Test
public void testPage(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    HashMap<String, Integer> map = new HashMap<>();
    map.put("startId",0);
    map.put("endId",3);
    mapper.getPage(map);
    sqlSession.close();
}
```

结果：

![image-20220325210156467](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220325210156467.png)

### Mybatis分页插件PageHelper

打开链接了解使用方式

[MyBatis 分页插件 PageHelper](https://pagehelper.github.io/)

### 使用注解

步骤1：

在接口添加查询所有用户的方法，并在其方法上面添加select注解，括号里填sql语句

```java
//查询全部用户(注解)
@Select("select * from mybatis.user")
public List<User> getUserList();
```

步骤2：将核心配置文件配置xml文件映射改为对接口映射，其中完成了注解扫描的工作。

```xml
<mappers>
    <mapper class="com.dao.UserMapper"/>
</mappers>
```

步骤3：

测试：

```java
@Test
public void test(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    List<User> userList = mapper.getUserList();
    for (User user:userList){
        System.out.println(user);
    }
    sqlSession.close();
}
```

### 使用注解完成增删查改操作

因为增删改操作都要提交才能完成，所有我们可以将默认不自动提交改为自动提交。

我们可以在生成sqlSessionFactory的实例时在openSession()方法传入true，表示增删改操作自动提交

```java
public static  SqlSession getSqlSession(){
    return sqlSessionFactory.openSession(true);
}
```

使用注解标记增删改查方法：

注意：当方法需要传递参数时，需要通过@Param给参数命名，然后将参数值传入上面注解的sql语句，所以要求@Param的参数名字要和sql语句的参数名字相同

```java
//通过id查询用户
@Select("select * from mybatis.user where id=#{id}")
public User getUserById(@Param("id") int id);
//添加用户
@Insert("insert into mybatis.user(id,name,password) values(#{id},#{username},#{password}) ")
public int addUser(User user);
//更新用户
@Update("update mybatis.user set name=#{username},password=#{password}")
public int updateUser(User user);
//删除用户
@Delete("delete from mybatis.user where id=#{id}")
public int deleteUser(@Param("id") int id);
```

测试：

不用手动提交事务了

```java
//通过id查询用户
@Test
public void test1(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    User user = mapper.getUserById(1);
    System.out.println(user);
    sqlSession.close();
}
//添加用户
@Test
public void test2(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    int number = mapper.addUser(new User(4, "李逵", "1616166"));
    System.out.println(number);
    sqlSession.close();
}
//修改用户
@Test
public void test3(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    int number = mapper.updateUser(new User(1, "赵七", "156561"));
    System.out.println(number);
    sqlSession.close();
}
//删除用户
@Test
public void test4(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    int number = mapper.deleteUser(4);
    System.out.println(number);
    sqlSession.close();
}
```

### Lombok插件

这个插件封装了许多注解

有了这个插件，就不用再手动创建无参构造方法，有参构造方法，setter和getter方法以及toString方法等。

相关注解：

@Getter and @Setter  //创建set和get方法
@FieldNameConstants
@ToString
@EqualsAndHashCode
@AllArgsConstructor//有参, @RequiredArgsConstructor and @NoArgsConstructor//无参
@Log, @Log4j, @Log4j2, @Slf4j, @XSlf4j, @CommonsLog, @JBossLog, @Flogger, @CustomLog
@Data		//创建无参、有参、toString、hascode、equals方法
@Builder
@SuperBuilder
@Singular
@Delegate
@Value
@Accessors
@Wither
@With
@SneakyThrows
@val
@var
experimental @var
@UtilityClass
Lombok config system

当使用@Data时：

自动生成无参、有参、toString、hascode、equals方法

```java
import lombok.Data;

@Data
public class User {
    private Integer id;
    private String name;
    private String password;
}
```

![image-20220325234341751](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220325234341751.png)

不能过度依赖Lombok插件，不然会失去java原本的味道

### 复杂查询

实体类方面

当实体类有多个时，且一个实体类包含另一个实体类的对象（多对一），或者一个实体类包含另一个实体类的集合(一对多)。

数据库表方面

一张表的主键是另一张表的外键。

如老师的id是学生的外键tid，多个学生关联一个老师（多对一），一个老师包含多个学生（一对多）。

#### 多对一

步骤：

1、创建老师表和学生表

老师的id是学生的外键tid

![image-20220326172628221](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220326172628221.png)

2、创建老师实体类和学生实体类

学生实体类：

```java
@Data
public class Student {
    private int id;
    private String name;
    //每个学生都有一个老师，即多对一
    private Teacher teacher;
}
```

老师实体类：

```java
@Data
public class Teacher {
    private int id;
    private String name;
}
```

3、创建学生和老师接口

学生

```java
public interface StudentMapper {
    //查询所有学生信息
    public List<Student> getAllStudent();
}
```

老师：

```java
public interface TeacherMapper {
    //根据id查找老师
    public Teacher getTeacherById( int id);
}
```

4、在StudentMapper.xml配置文件实现多表关联查询

有两种方式实现查询：

方式一：分步查询

相关参数

association：当属性为类的实例对象时，使用该标签。

主要思想：查询学生需要通过查询老师的结果来完成，查询老师需要通过学生的老师id（tid）属性传入完成。

​		要查询所有学生的信息，其中学生的实体类包含老师的类属性，所以不能通过简单的结果集映射将老师的id赋给老师对象实例，还要根据老师查询的结果来给老师对象实例赋值。

​	association标签

property：老师类属性

column：老师id属性，用于老师查询的where语句的id。

select：查询老师的结果

javaType：指明老师类的java类型。

方式二：嵌套查询
		通过sql语句两个表连接查询，查询出来的结果集是两个表的连接结果集，通过结果集映射，将学生的实体类属性从两个表连接查询结果集中对应赋值，当遇到teacher属性时，继续将老师的实体类属性从两个表连接查询结果集中对应赋值。

结果集：

![image-20220326175453752](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220326175453752.png)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.dao.StudentMapper">
    <!-- 方式一：分步查询-->
    <select id="getAllStudent" resultMap="studentMap1">
        select * from student;
    </select>
    <resultMap id="studentMap1" type="com.pojo.Student">
        <result property="id" column="id"/>
        <result property="name" column="name"/>
        <association property="teacher" column="tid" select="getTeacherById" javaType="com.pojo.Teacher"/>
    </resultMap>
    <select id="getTeacherById" resultType="com.pojo.Teacher">
        select * from teacher where id=#{id};
    </select>
  
   <!-- 方式二：嵌套查询-->
    <select id="getAllStudent" resultMap="studentMap2">
        select s.id s_id,s.name s_name,t.id t_id,t.name t_name
            from student s,teacher t
            where s.tid=tid;
    </select>
    <resultMap id="studentMap2" type="com.pojo.Student">
        <result property="id" column="s_id"/>
        <result property="name" column="s_name"/>
        <association property="teacher" javaType="com.pojo.Teacher">
            <result property="id" column="t_id"/>
            <result property="name" column="t_name"/>
        </association>
    </resultMap>
</mapper>
```

5、测试：

```java
@Test
public void  test(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
    List<Student> StudentList = mapper.getAllStudent();
    for (Student student:StudentList){
        System.out.println(student);
    }
    sqlSession.close();
}
```

#### 一对多

步骤：

1、创建老师和学生实体类

学生：

```java
@Data
public class Student {
    private int id;
    private String name;
    private int tid;
}
```

老师：

```java
@Data
public class Teacher {
    private int id;
    private String name;
    //一个老师有多个学生，即一对多
    private List<Student> students;
}
```

2、创建老师和学生接口

学生：

```java
public interface StudentMapper {
    //根据老师id查询学生
    public Student getStudentById(int tid);
}
```

老师：

```java
public interface TeacherMapper {
    //查询老师信息
    public List<Teacher> getTeacher();
}
```

3、TeacherMapper.xml实现一个老师查询出多个学生

相关参数

collection：集合标签，结果集映射的实体类存在集合或者List属性时使用

property：表示集合对象实例

column：表示老师的id，将传给学生select，通过老师id查询学生。

select：通过老师id查询出了多个学生对象将封装到老师实体类的List<Student>中

javaType：表示java的集合类的类型

ofType:表示集合泛型参数类型

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.dao.TeacherMapper">
<!--   嵌套查询-->
    <select id="getTeacher" resultMap="teacherMap1">
        select s.id s_id,s.name s_name,t.id t_id,t.name t_name
            from teacher t,student s
            where s.tid=t.id;
    </select>
    <resultMap id="teacherMap1" type="com.pojo.Teacher">
        <result property="id" column="t_id"/>
        <result property="name" column="t_name"/>
        <collection property="students" ofType="com.pojo.Student">
            <result property="id" column="s_id"/>
            <result property="name" column="s_name"/>
            <result property="tid" column="t_id"/>
        </collection>
    </resultMap>
<!--   分布查询-->
    <select id="getTeacher" resultMap="teacherMap2">
        select * from teacher;
    </select>
    <resultMap id="teacherMap2" type="com.pojo.Teacher">
        <result property="id" column="id"/>
        <result property="name" column="name"/>
        <collection property="students" column="id" select="getStudentById" javaType="ArrayList" ofType="com.pojo.Student"/>
    </resultMap>
    <select id="getStudentById" resultType="com.pojo.Student">
        select * from student where tid=#{tid};
    </select>
</mapper>
```

4、测试：

```java
@Test
public void  test(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    TeacherMapper mapper = sqlSession.getMapper(TeacherMapper.class);
    List<Teacher> teacherList=mapper.getTeacher();
    for (Teacher teacher:teacherList){
        System.out.println(teacher);
    }
    sqlSession.close();
}
```

### 动态SQL

#### IF语句

在Mapper.xml的查询语句中加入if语句，可以完成模糊查询。

例如：如果查找Blog的方法没有传入title参数，则if将不会执行，就相当于空白查询，将查询出所有结果，如果有title参数传入，将在where后面追加and title=#{title}，这根据title的值查询Blog。

where后面一定要加一个结果为true的表达式and才能正确拼接。

```xml
<select id="queryBlogByIf" resultType="com.pojo.Blog">
    select * from blog where 1=1
    <if test="title!=null">
        and title=#{title}
    </if>
</select>
```

创建Blog（博客）实体类：

```java
@Data
public class Blog {
    private String id;
    //文章标题
    private String title;
    //作者
    private String author;
    //创建日期
    private Date createTime;
    //访问人数
    private int views;
}
```

BlogMapper接口：

创建查找Blog的方法，参数是Map集合

```java
public interface BlogMapper {
    //添加博客
    public int addBlog(Blog blog);
    //查找博客
    public List<Blog> queryBlogByIf(Map<String,Object> map);
}
```

BlogMapper.xml:

根据Map集合传进来的参数，看看有没有title或者author，如果有，则追加if里面的语句。

```xml
<select id="queryBlogByIf" resultType="com.pojo.Blog">
    select * from blog where 1=1
    <if test="title!=null">
        and title=#{title}
    </if>
    <if test="author!=null">
        and author=#{author}
    </if>
</select>
```

测试：

map保存作者，作为参数。

```java
 @Test
public void test1(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);
         HashMap<String, Object> map = new HashMap<>();
         map.put("author","狂神说");
         List<Blog> blog = mapper.queryBlogByIf(map);
         for (Blog blog1:blog){
                 System.out.println(blog1);
         }
 }
```

结果：

根据作者查询到了相关Blog。

![image-20220326215046412](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220326215046412.png)

#### where语句

由于where 1=1这条语句不安全，所以可以使用动态SQL的where语法代替：

例如：

在where标签中成立的第一条if，where会自动将前面的and去掉，比如第一条if成立，则拼成select * from blog title=#{title}

当where标签中没有一条if成立，则不会将where语句拼接上去，则为查找所有的博客。

```xml
<select id="queryBlogByIf" parameterType="map" resultType="com.pojo.Blog">
     select * from blog
<where>
    <if test="title!=null">
        and title=#{title}
    </if>
    <if test="author!=null">
        and author=#{author}
    </if>
</where>
</select>
```

#### choose、when、otherwise

​	choose、when、otherwise语句只会选择一条作为拼接，如果都没有，则拼接otherwise里的语句，所有要求otherwise不能为空。

例子：

BlogMapper接口加入查找方法，参数为Map：

```java
public List<Blog> queryBlogByChoose(Map<String,Object> map);
```

BlogMapper.xml：

```xml
<select id="queryBlogByChoose" parameterType="map" resultType="com.pojo.Blog">
    select * from blog
    <where>
        <choose>
            <when test="title!=null">
                title=#{title}
            </when>
            <when test="author!=null">
                author=#{author}
            </when>
            <otherwise>
                views=9999
            </otherwise>
        </choose>
    </where>
</select>
```

测试：

![image-20220326231357649](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220326231357649.png)

```java
 @Test
public void test1(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);
         HashMap<String, Object> map = new HashMap<>();
         map.put("title","Java");
         map.put("author","狂神说");
         List<Blog> blog = mapper.queryBlogByIf(map);
         for (Blog blog1:blog){
                 System.out.println(blog1);
         }
 }
```

结果：显然，title不为空，就只执行第一条when语句，查询select * from blog where  title="Java"，返回结果
![image-20220326231411231](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220326231411231.png)

### Set语句

Set语句适用于更新数据

例子：

BlogMapper接口加入更新方法，参数为Map：

```java
//更新博客
public int updateBlog(Map<String,Object> map);
```

BlogMapper.xml：

​	如果第一个if成立第二个if不成立，set标签会自动将末尾的逗号去掉，然后拼接：update blog title=#{title},即最后一个成立的if语句有逗号就会自动去掉

```xml
<update id="updateBlog" parameterType="map">
    update blog
    <set>
        <if test="title!=null">
            title=#{title},
        </if>
        <if test="author!=null">
            author=#{author}
        </if>
    </set>
    <where>
        <if test="id!=null">
                id=#{id}
        </if>
    </where>
</update>
```

测试：

如果保存到map的参数只有title和id，就只会将第一个if语句和第三个if语句拼接到sql语句，即

 update blog  set title=#{title} where id=#{id}

转化为：update blog  set title="Java2" where id="01d4964332494bb88fdd3249fd9bf61b"

```java
         @Test
        public void test1(){
            SqlSession sqlSession = MybatisUtils.getSqlSession();
            BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);
                 HashMap<String, Object> map = new HashMap<>();
                 map.put("title","Java2");
                 map.put("id","01d4964332494bb88fdd3249fd9bf61b");
                  mapper.updateBlog(map);

         }
```

![image-20220326234337189](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220326234337189.png)



如果保存到map的参数有title、author和id

就只会将第1，2，3个if语句都拼接到sql语句

即： update blog  set title=#{title} ,author=#{author} where id=#{id}

转化为：update blog  set title="Java2" ,author="狂神说2" where id="01d4964332494bb88fdd3249fd9bf61b"

```java
         @Test
        public void test1(){
            SqlSession sqlSession = MybatisUtils.getSqlSession();
            BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);
                 HashMap<String, Object> map = new HashMap<>();
                 map.put("title","Java2");
			     map.put("author","狂神说2");
                 map.put("id","01d4964332494bb88fdd3249fd9bf61b");
                  mapper.updateBlog(map);

         }
```

![image-20220326234354550](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220326234354550.png)

### Foreach

当sql语句中涉及多个参数时，可以使用动态SQL的Foreach标签来解决，例如：

```sql
select * from blog where id in (1,2,3);
```

可以使用Foreach遍历1，2，3然后拼接起来

步骤：

1、BlogMapper接口定义查找博客的方法，参数仍使用万能Map

```java
//根据Foreach查找博客
public List<Blog> queryBlogByForeach(Map<String,List<Integer>> map);
```

2、BlogMapper.xml：

使用Foreach进行拼接sql语句

相关参数：
open：开始拼接的语句

separator：分隔符

close：结束拼接的语句

#{id}：Foreach的内容，需要遍历的属性id。

collection：集合id，通过查找Map集合的key来获取。

```xml
<select id="queryBlogByForeach" parameterType="map" resultType="com.pojo.Blog">
    select * from blog where
    <foreach collection="ids" item="id"
             open="id in (" separator="," close= ")">
        #{id}
    </foreach>
</select>
```

测试：

Map集合中保存需要遍历的ids，ids使用ArrayList集合保存。

```java
 @Test
public void test1(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);
    HashMap<String, List<Integer>> map = new HashMap<>();
     ArrayList<Integer> idList = new ArrayList<>();
     idList.add(1);
     idList.add(2);
     idList.add(3);
     map.put("ids",idList);
     List<Blog> blogList = mapper.queryBlogByForeach(map);
     for (Blog list: blogList){
         System.out.println(list);
     }
     sqlSession.close();
 }
```

结果：成功将i1，2，3的id遍历拼接到sql语句。

![image-20220327121703570](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220327121703570.png)

也可以用直接使用List集合传入方法：

```java
//根据Foreach查找博客
public List<Blog> queryBlogByForeach(List<Integer> id);
```

```xml
<select id="queryBlogByForeach" parameterType="list" resultType="com.pojo.Blog">
    select * from blog where
    <foreach collection="list" item="id"
             open="id in (" separator="," close= ")">
        #{id}
    </foreach>
</select>
```

### 动态SQL总结:

**动态SQL实质上就是使用形式像JSTL的语句的逻辑来完成动态的拼接sql语句，从而达到不同的效果。**



### Mybatis缓存

​		当数据库中的一些数据经常被用户查询，我们可以将它们放在缓存中(内存)，每次查询可以z直接读取缓存的数据，而不用再连接数据库，从磁盘中读取这些数据，省了不少时间，提高了效率。

作用：
1，提高查询效率，解决了高并发系统的性能问题

2、初步完成读写分离的作用（Tomcat能很好实现读写分离）。

3、减少数据库的交互次数，减少系统开销

#### 一级缓存和二级缓存

Mybatis定义了一级缓存和二级缓存

一级缓存：

Mybatis默认自动开启。

一级缓存的作用域是一个SqlSession（即Mapper.xml中的一个sql标签），执行SqlSession.clearCache，SqlSession.close，则对应缓存将会被清除

二级缓存：

需要手动开启

二级缓存的作用域是一个Mapper.xml。

#### 一级缓存

当使用同一条sql语句查询相同的数据时有作用

测试：通过id查询用户，查询的id相同

```java
 @Test
public void test1(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
     User user1 = mapper.getUserById(1);
     System.out.println(user1);
     System.out.println("=======================");
     User user2 = mapper.getUserById(1);
     System.out.println(user2);
     sqlSession.close();
 }
```

运行结果：

只有第一次查询用户查询了数据库，第二次查询相同id时，使用了一级缓存。

![image-20220327150126916](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220327150126916.png)

**一级缓存在以下情况下没作用：**

1、使用同一个查询方法，查询不同记录

2、使用不同的查询方法

3、使用增删改方法

#### 二级缓存

二级缓存也叫全局缓存

二级缓存的作用是在一级缓存死亡时继承一级缓存

使用步骤

1、在mybatis核心配置文件里使用setting标签来设置全局缓存开启

```xml
    <settings>
<!--        开启全局缓存-->
        <setting name="cacheEnabled" value="true"/>
    </settings>
```

2、在SQL映射区Mapper.xml使用二级缓存。

当没有为二级缓存设置属性，则需要将实体类序列化，即实现Serializable接口

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--绑定UserDao，下面相当于实现UserDao的方法-->
<mapper namespace="com.dao.UserMapper">
<!--开启二级缓存，并设置属性-->
    <cache
  eviction="FIFO"//先进先出
  flushInterval="60000"//刷新时间为60000毫秒
  size="512"//接收引用个数
  readOnly="true"/> //是否只读
<!--   查询语句，id表示接口对应的返回查询方法，resultType表示返回的结果集-->
    <select id="getUserList" resultType="user">
#  设置了mysql方言后，就可以直接关联数据库mybatis的表user
        select * from mybatis.user;
    </select>
<!--根据id查找用户-->
    <select id="getUserById" parameterType="int" resultType="com.pojo.User">
        select * from mybatis.user where id=#{id};
    </select>
<!--添加用户-->
    <insert id="addUser" parameterType="com.pojo.User">
        insert into mybatis.user(id, name, password) VALUES (#{id},#{name},#{password});
    </insert>
<!--修改用户-->
    <update id="updateUser" parameterType="com.pojo.User">
        update mybatis.user set name=#{name},password=#{password} where id=#{id};
    </update>
<!--删除用户-->
    <delete id="deleteUser" parameterType="com.pojo.User">
        delete from mybatis.user where id=#{id};
    </delete>
</mapper>
```

测试：

建立两个会话，会话一查询完id为1的用户后，关闭会话，一级缓存将缓存交给二级缓存后死亡，会话二从二级缓存中读取会话一的缓存。

```java
 @Test
public void test1(){
    SqlSession sqlSession1 = MybatisUtils.getSqlSession();
     SqlSession sqlSession2 = MybatisUtils.getSqlSession();
    UserMapper mapper1 = sqlSession1.getMapper(UserMapper.class);
     UserMapper mapper2 = sqlSession2.getMapper(UserMapper.class);
     User user1 = mapper1.getUserById(1);
     System.out.println(user1);
     sqlSession1.close();
     System.out.println("=======================");
     User user2 = mapper2.getUserById(1);
     System.out.println(user2);
    System.out.println(user1==user2);
     sqlSession2.close();
 }
```

结果：user1和user2的地址相等，说明第二次查询是读取缓存里面的数据的。

![image-20220327155519429](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220327155519429.png)

#### 用户查询数据过程

首先到二级缓存中查找有没有对应的缓存，如果没有到一级缓存去找，如果都没有则去数据库查找。

![image-20220327160800963](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220327160800963.png)

## Mybatis总结

1、配置核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--配置核心配置文件-->
<configuration>
    <properties resource="db.properties"/>
    <settings>
        <setting name="logImpl" value="LOG4J"/>
<!--        开启全局缓存-->
        <setting name="cacheEnabled" value="true"/>
    </settings>
    <typeAliases>
        <typeAlias alias="User" type="com.pojo.User" ></typeAlias>
    </typeAliases>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
<!--               userSSL=true表示安全连接，userUnicode=true表示支持中文，characterEncoding=UTF-8表示时区，mysql8一定要配置时区，不然乱码，&amp;表示&的转义 -->
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper class="com.dao.UserMapper"/>
    </mappers>
</configuration>
```

environments：环境配置，配置数据库连接所需要的用户信息，url，driver等。

mappers：配置SQL映射区（Mapper.xml）文件路径，路径可以是具体的xml文件路径，也可以是对应接口的路径，及所在包路径。

properties：引入外部配置文件，供给环境配置使用。

settings：设置，可以设置缓存的开启和日志等。

typeAliases：给Mapper.xml的Type（sql查询语句完成返回的结果）起别名。

2、Mapper.xml（SQL映射配置文件）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.dao.TeacherMapper">
<!--    多表连接查询-->
    <select id="getTeacher" resultMap="teacherMap1">
        select s.id s_id,s.name s_name,t.id t_id,t.name t_name
            from teacher t,student s
            where s.tid=t.id;
    </select>
    <resultMap id="teacherMap1" type="com.pojo.Teacher">
        <result property="id" column="t_id"/>
        <result property="name" column="t_name"/>
        <collection property="students" ofType="com.pojo.Student">
            <result property="id" column="s_id"/>
            <result property="name" column="s_name"/>
            <result property="tid" column="t_id"/>
        </collection>
    </resultMap>
<!--    子查询-->
    <select id="getTeacher" resultMap="teacherMap2">
        select * from teacher;
    </select>
    <resultMap id="teacherMap2" type="com.pojo.Teacher">
        <result property="id" column="id"/>
        <result property="name" column="name"/>
        <collection property="students" column="id" select="getStudentById" javaType="ArrayList" ofType="com.pojo.Student"/>
    </resultMap>
    <select id="getStudentById" resultType="com.pojo.Student">
        select * from student where tid=#{tid};
    </select>
</mapper>
```

Mapper.xml的作用相当于Dao接口的实现类。

namespace：设置所实现接口的路径

**select：**{

id：实现了接口的哪个方法

resultType：设置结果集应该映射到哪个实体类，如果该实体类的所有属性名和数据库查询的结果集的列名相同，结果集将把对应的值传给实体类对象属性，select标签将返回该实体类对象。

resultMap：当结果集列名和实体类对象属性名不能一一对应时，将使用resultMap标签将结果集列名和实体类对象属性名手动映射。

}

**resultMap：**结果集映射将结果集列名和实体类对象属性名手动映射

{

result：用于给实体类普通的属性和结构集进行映射

collection：用于给实体类的集合属性和结果集进行映射

association：用于给实体类的类对象属性和结果集进行映射

}

## 尚未解决的问题

### Mybatis的底层工作原理！！！！

