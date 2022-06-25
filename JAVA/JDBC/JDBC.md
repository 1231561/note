# JDBC核心技术

### JavaWeb技术体系

数据持久化：把数据保持到可掉电存储设备中以供之后使用。

具体使用：硬盘、xml



![image-20220221182653689](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220221182653689.png)

## JDBC概述

JDBC是一个独立于特定数据库管理系统、通用的SQL数据库存取和操作的公共接口（一组API），定义了用来访问数据库的标准java类库，这些类库可以以一种标准的方法，方便地访问数据库资源。

JDBC为访问不同的数据库提供了一种统一的途径，

java程序员可以通过JDBC连接任何提供了JDBC驱动程序的数据库系统。

好处：

面向应用的API:Java API,抽象接口，供应用程序开发人员使用(连接数据库，执行SQL语句，获得结果)

面向数据库的API：Java Driver API，共开发商数据库驱动程序使用

程序员：不需要关注数据库的细节

厂商：只需提供标准的具体实现

![image-20220221210218384](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220221210218384.png)

## JDBC程序的编写步骤

![image-20220221211425131](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220221211425131.png)

## 获取数据库连接

步骤1、先将连接数据库的mysql-connector-java-8.0.26(jar包)拷贝的idea项目目录下，右键jar包，选导入库，模块。

步骤2、在src下创建一个软件包，创建一个java类，该类实现数据库的连接。

![image-20220221233351833](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220221233351833.png)

```java
package com.test_connection;

import org.testng.annotations.Test;

import java.io.InputStream;
import java.sql.Connection;
import java.sql.Driver;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.Properties;

public class ConnectionTest {
    //方式1
    @Test//注解，不用main方法也能运行
    public void testConnection1() throws SQLException {
        //实例化Driver对象，多态，第三方api。
        Driver driver = new com.mysql.cj.jdbc.Driver();//车
        String url = "jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf-8";//去哪
        //将用户名和密码封装到Properties中
        Properties info = new Properties();
        info.setProperty("user", "root");//人
        info.setProperty("password", "123456Hh");//钥匙
        Connection conn = driver.connect(url, info);//启动
        System.out.println(conn);
    }

    //方式2：方式1的迭代
    @Test
    public void testConnection2() throws Exception {
        //1、获取Drive实现类对象，使用反射，该方式不会出现第三方的api，使得程序具有更好的可以移植性
        Class clazz = Class.forName("com.mysql.cj.jdbc.Driver");
        Driver driver = (Driver) clazz.getDeclaredConstructor().newInstance();
        //2、提供要连接的数据库
        String url = "jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf-8";
        //3、提供需要连接的的用户名密码
        Properties info = new Properties();
        info.setProperty("user", "root");
        info.setProperty("password", "123456Hh");
        //4、获取连接
        Connection conn = driver.connect(url, info);
        System.out.println(conn);

    }

    //方式3、使用DriverManager替换Drive
    @Test
    public void testConnection3() throws Exception {
        //1、获取Drive实现类对象
        Class clazz = Class.forName("com.mysql.cj.jdbc.Driver");
        Driver driver = (Driver) clazz.getDeclaredConstructor().newInstance();
        //2、提供三个连接的基本信息
        String url = "jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf-8";
        String user = "root";
        String password = "123456Hh";
        //3、获取连接
        Connection conn = DriverManager.getConnection(url, user, password);
        System.out.println(conn);
    }

    //方式4:只加载驱动，方式3的简化
    @Test
    public void testConnection4() throws Exception {
        //1、提供三个连接的基本信息
        String url = "jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf-8";
        String user = "root";
        String password = "123456Hh";
        //加载Driver对象（加载驱动），源代码有个static方法，加载对象的时候会通过DriverManager new一个Driver对象
        Class.forName("com.mysql.cj.jdbc.Driver");
        //源代码：
//        static {
//            try {
//                java.sql.DriverManager.registerDriver(new com.mysql.cj.jdbc.Driver());
//            } catch (SQLException E) {
//                throw new RuntimeException("Can't register driver!");
//            }
//        }
        //3、获取连接
        Connection conn = DriverManager.getConnection(url, user, password);
        System.out.println(conn);
    }
    //方式5:最终版
/*    好处：1、实现了数据库和代码的分类，实现了解耦
        2、修改配置文件信息，避免程序重新打包
*/
    @Test
    public void testConnection5() throws Exception {
        //1、读取配置文件的四个基本信息
        //类加载器，getClassLoader()是系统加载器
        InputStream is=ConnectionTest.class.getClassLoader().getResourceAsStream("jdbc.properties");
        Properties pros=new Properties();
        pros.load(is);
        //2、读取配置文件4个信息
        String user = pros.getProperty("user");
        String password = pros.getProperty("password");
        String url = pros.getProperty("url");
        String driverClass = pros.getProperty("driverClass");
        //3、加载驱动
        Class.forName(driverClass);
        //4、获取连接
        Connection conn = DriverManager.getConnection(url, user, password);
        System.out.println(conn);
    }

}

```

## 操作和访问数据库

在java.sql包中有3个接口分别定义了对数据库的调用的不同方式

**Statement：**

用于执行静态SQL语句并返回它所生成结果的对象。

**PrepatedStatement:**

SQL语句被预编译并存储在此对象中，可以使用此对象多次高效地执行该语句

**CallableStatement:**

用于执行SQL存储过程

![image-20220222124115081](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220222124115081.png)

#### Statement的弊端

弊端：

1、需要拼写SQL语句

2、存在SQL注入问题。

代码实例：

```java
@Test
public void testLogin() {
   Scanner scanner = new Scanner(System.in);
   System.out.print("请输入用户名");
   String user=scanner.nextLine();
   System.out.print("请输入密码");
   String password=scanner.nextLine();
    //需要拼写SQL语句
   String sql="SELECT user,password FROM user_table WHERE user='"+user+"' AND password='"+password+"'";
    //SQL注入，导致用户数据很不安全，输入下列语句也能成功登录。
   //SELECT user,password FROM user_table WHERE user= '1' or ' AND password= '=1 or '1' = '1'
   User returnUser=get(sql,User.class);
   if(returnUser!=null)
   {
      System.out.println("登陆成功");
   }else {
      System.out.println("密码错误或者用户名不存在");
   }
}
```

**SELECT user,password FROM user_table WHERE user= '1' or ' AND password= '=1 or '1' = '1'**

之所以这句SQL语句能成登录，是因为MySQL会将上面的where语句解析为or子句1=1恒等于true。

## PreparadStatement

**PreparadStatement实现了预编译sql语句的功能，所以避免了SQL注入问题。**

如：

String sql="update customers set name =? where id=? ";

PreparadStatement通过预编译，先确定sql要执行什么操作，后面再通过传入的占位符实例确定修改的对象。

用PreparadStatements实现表的添加操作：

```java
import com.test_connection.ConnectionTest;
import org.testng.annotations.Test;

import java.io.IOException;
import java.io.InputStream;
import java.sql.*;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Properties;

public class PreparedStatementTest {
    @Test
    public void testInsert() {
        InputStream is= null;
        Connection conn = null;
        PreparedStatement pre = null;
        try {
            is = ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc.properties");//通过系统加载器以流的形式读取文档的数据(用户名，密码，url)
            Properties pros=new Properties();
            pros.load(is);
            //2、读取配置文件4个信息
            String user = pros.getProperty("user");
            String password = pros.getProperty("password");
            String url = pros.getProperty("url");
            String driverClass = pros.getProperty("driverClass");
            //3、加载驱动
            Class.forName(driverClass);
            //4、获取连接
            conn = DriverManager.getConnection(url, user, password);
//        System.out.println(conn);
            String sql="insert into customers(name,email,birth) value (?,?,?)";//?：是SQL的占位符
            pre = conn.prepareStatement(sql);//获取prepareStatement实例对象
            pre.setString(1,"Tom");//setString方法是prepareStatement类的方法，第一个参数是value 的索引，第二个参数是值
            pre.setString(2,"Tom@gmail.com");
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");//SimpleDateFormat将日期格式化。
            java.util.Date date = sdf.parse("2000-05-31");//调用parse将日期解析转化为 java.util.Date型。
            pre.setDate(3,  new Date(date.getTime()));// java.util.Date转化为 java.sql.Date型。

            //执行操作
            pre.execute();
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            //资源关闭
            try {
                if(pre!=null)//避免空指针异常，因为finally一定会执行，即使没取到对象
                pre.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
            try {
                if(conn!=null)
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
            try {
                if(is!=null)
                is.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }
}
```

#### 将连接操作和关闭资源操作封装

​	将上面实现添加时的连接操作和关闭资源操作封装在JDBCutils类里面，并通过两个静态方法实现，当用到这两个方法时，可以直接通过类名来调用方法。

```java
package com.test03_util;

import java.io.IOException;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.Properties;

public class JDBCUtils {
    //获取数据库连接
    public static Connection getConnection() throws Exception {
        //1、读取配置文件的四个基本信息
        //类加载器，getClassLoader()是系统加载器
        InputStream is= ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc.properties");
        Properties pros=new Properties();
        pros.load(is);
        //2、读取配置文件4个信息
        String user = pros.getProperty("user");
        String password = pros.getProperty("password");
        String url = pros.getProperty("url");
        String driverClass = pros.getProperty("driverClass");
        //3、加载驱动
        Class.forName(driverClass);
        //4、获取连接
        Connection conn = DriverManager.getConnection(url, user, password);
        return conn;
    }
    public static void closeResource(Connection conn, PreparedStatement pre){
        //资源关闭
        try {
            if(pre!=null)
                pre.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        try {
            if(conn!=null)
                conn.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }

    }
}
```

#### 实现修改操作

```java
@Test
public void testUpdate() {
    Connection conn = null;
    PreparedStatement pre = null;
    try {
        //1、获取连接
        conn = JDBCUtils.getConnection();
        //2、预编译sql语句，返回 PreparedStatement的实例。
        String sql="update customers set name =? where id=? ";
        pre = conn.prepareStatement(sql);
        //3、填充占位符
        pre.setObject(1,"肖邦");
        pre.setObject(2,18);
        //4、执行
        pre.execute();
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        //5、资源关闭
        JDBCUtils.closeResource(conn,pre);
    }
}
```

### 实现通用的增删改操作

​	我们发现，增删改操作只在于SQL语句和使用setObject方法的次数不同（参数数量不同），使用我们可以设计一个方法，来给增删改方法调用，实现不同的方法，所以要设计一个带可变参数来实现这个通用方法。

代码实例：

增删改通过方法：

```java
//设计一个通用方法，增删改都可以调用操作
public void update(String sql,Object...arr) {//Object...arr是Object类型的可变参数
    Connection conn = null;
    PreparedStatement pre= null;
    try {
        //1、获取连接
        conn = JDBCUtils.getConnection();
        //2、预编译sql语句，返回 PreparedStatement的实例。
        pre = conn.prepareStatement(sql);
        //3、填充占位符
        for(int i=0;i<arr.length;i++){
            pre.setObject(i+1,arr[i]);//通过可变参数数组保存的值来给setObject传递值,填充占位符
        }
        //4、执行
        pre.execute();
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
    //5、资源关闭
    JDBCUtils.closeResource(conn,pre);
    }
}
```

增删测试：

```java
@Test
public void testDelete(){
    String sql="delete from customers where id=?";
    update(sql,21);//传入sql语句和要删除行的id
}
@Test
public void testInsert1() throws ParseException {
    String sql="insert into customers(name,email,birth) values(?,?,?)";
    SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
    java.util.Date date = sdf.parse("2000-1-3");
    update(sql,"张三","zhangsan@qq.com",new Date(date.getTime()));
}
```

#### 实现查询操作

查询操作比增删改操作多了返回结果集的操作，并将返回的结果集显示出来。

代码实例：

```java
ackage com.test03_PreparedStatement;

import com.test03_bean.Customers;
import com.test03_util.JDBCUtils;
import org.testng.annotations.Test;

import java.sql.Connection;
import java.sql.Date;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

//实现Customers表的查询操作
public class CustomersForQuery {
    @Test
    public void testQuery1(){
        Connection conn = null;
        PreparedStatement pre = null;
        ResultSet res = null;
        try {
            //1、获取连接，并创建PreparedStatement实例,并填充占位符
            conn = JDBCUtils.getConnection();
            String sql="select id,name,email,birth from customers where id=?";
            pre = conn.prepareStatement(sql);
            pre.setObject(1,1);
            //2、执行，并返回结果集（查询出来的结果）
            res = pre.executeQuery();
            //3、处理结果集（next）
            if(res.next()){
                //4、获取当前这条数据的各个字段
               int id=res.getInt(1);
               String name=res.getString(2);
                String email=res.getString(3);
                Date birth=res.getDate(4);
                //5、显示查询结果
                //方式一：直接输出结果(不推荐)
    //            System.out.println("id:"+id+" name:"+name+" email:"+email+" birth:"+date);
                //方式二：保存到数组输出(不推荐)
    //            Object []o = new Object[]{id,name,email,birth};
    //            for (int i=0;i<o.length;i++){
    //                System.out.println(o[i]);
    //            }
               // 方式三：将数据封装成一个对象(推荐)
                Customers customers = new Customers(id, name, email, birth);
                System.out.println(customers.toString());
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            JDBCUtils.closeResource(conn,pre,res);
        }

    }
}
```

**显示结果集方式三介绍：**

将select查询结果封装成一个类。体现ORM编程思想（对象关系映射）。

```java
package com.test03_bean;

import java.sql.Date;
/*
ORM编程思想（对象关系映射）
一个数据表对应一个java类
表中的一条记录对应java类的一个对象
表中的一个字段对应java类的一个属性
 */
public class Customers {
    private int id;
    private String name;
    private  String email;
    private Date birth;

    public Customers() {
        
    }
    public Customers(int id, String name, String email, Date birth) {
        this.id = id;
        this.name = name;
        this.email = email;
        this.birth = birth;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public Date getBirth() {
        return birth;
    }

    public void setBirth(Date birth) {
        this.birth = birth;
    }

    @Override
    public String toString() {
        return "Customers{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", email='" + email + '\'' +
                ", birth=" + birth +
                '}';
    }
}
```

**Java和SQL数据类型对应关系**

![image-20220224155033757](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220224155033757.png)

#### 针对Customers表的通用操作

当select查询的列数不同，返回的结果集也不同，这时要设计一个方法，实现不同的查询通用。

代码实现：

```java
public Customers queryForCustomers(String sql,Object...arr) {
    Connection conn= null;
    PreparedStatement pre= null;
    ResultSet res = null;
    try {
        //1、获取连接，并创建PreparedStatement实例,并填充占位符,执行
        conn = JDBCUtils.getConnection();
        pre = conn.prepareStatement(sql);
        for(int i=0;i<arr.length;i++){
            pre.setObject(i+1,arr[i]);
        }
        //2、获取结果集的元数据（取得结果集的每一列）
        res = pre.executeQuery();
        ResultSetMetaData rmsd = res.getMetaData();
        //3、通过ResultSetMetaDate获得结果集列数
        int columnCount=rmsd.getColumnCount();
        if(res.next()){
            Customers cust = new Customers();
            //处理每个结果集的每一列
            for (int i=0;i<columnCount;i++){
                //获得列值
                //因不确定返回的列的值是什么类型的，所以用Object
                Object columnValue=res.getObject(i+1);
                //获得每个列的列名
                String columnName=rmsd.getColumnName(i+1);
               //因为不确定结果集的哪一列对应哪一个封装类的属性，所以得用反射给指定对象的属性赋值
                Class clazz=Customers.class;
                Field columnName1 = clazz.getDeclaredField(columnName);
                columnName1.setAccessible(true);
                columnName1.set(cust,columnValue);
            }
            return cust;
        }
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
    JDBCUtils.closeResource(conn,pre,res);
    }
    return null;
}
```

测试代码：

```java
@Test
public void testQueryForCustomers() throws Exception {
    String sql="select id,name,email from customers where id=?";
    Customers customers=queryForCustomers(sql,2);
    System.out.println(customers);
}
```

结果：

![image-20220224180123127](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220224180123127.png)

#### 处理封装类属性和SQL也名不相同问题

我们创建一个Order类定义了和order表的列名相关但是不相等的属性名。

Order类：

```java
package com.test03_bean;

import java.sql.Date;

public class Order {
    private  int orderid;
    private String ordername;
    private Date orderdate;

    public Order() {
    }

    public Order(int orderid, String ordername, Date orderdate) {
        this.orderid = orderid;
        this.ordername = ordername;
        this.orderdate = orderdate;
    }

    public int getOrderid() {
        return orderid;
    }

    public void setOrderid(int orderid) {
        this.orderid = orderid;
    }

    public String getOrdername() {
        return ordername;
    }

    public void setOrdername(String ordername) {
        this.ordername = ordername;
    }

    public Date getOrderdate() {
        return orderdate;
    }

    public void setOrderdate(Date orderdate) {
        this.orderdate = orderdate;
    }

    @Override
    public String toString() {
        return "Order{" +
                "orderid=" + orderid +
                ", ordername='" + ordername + '\'' +
                ", orderdate=" + orderdate +
                '}';
    }
}
```

**创建一个查询order表的类，以及查询order表的方法。**

和上面查询customer的方法不同的是，取列名的方法不同了，getColumnLabel可以取表的别名。

我们再写SQL语句查询的时候，取列的别名时，要和封装类的对应属性名相同，因为封装类的属性一般不会改。

```java
package com.test03_PreparedStatement;

import com.test03_bean.Customers;
import com.test03_bean.Order;
import com.test03_util.JDBCUtils;
import org.testng.annotations.Test;

import java.lang.reflect.Field;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;

public class OrderForQuery {
    public Order orderQuery(String sql,Object...arr)  {
        Connection conn = null;
        PreparedStatement pre = null;
        ResultSet res = null;
        try {
            //获取连接，取得PreparedStatement实例，填充占位符，执行获取元数据
            conn = JDBCUtils.getConnection();
            pre = conn.prepareStatement(sql);
            for (int i=0;i<arr.length;i++){
                pre.setObject(i+1,arr[i]);
            }
            res = pre.executeQuery();
            //结果集元数据
            ResultSetMetaData rsmd = res.getMetaData();
            int columnCount = rsmd.getColumnCount();
            if(res.next()){
                Order order = new Order();
                for (int i=0;i<columnCount;i++){
                    //获取列名，getColumnLabel有别名取别名，没有别名取原列名
                    String columnLabel = rsmd.getColumnLabel(i+1);
                    //获取每列的值
                    Object columnValue = res.getObject(i+1);
                    //通过反射给Order类的属性进行赋值
                    Class clazz=Order.class;
                    Field field = clazz.getDeclaredField(columnLabel);
                    field.setAccessible(true);
                    field.set(order,columnValue);
                }
                return order;
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
        JDBCUtils.closeResource(conn,pre,res);
        }
        return null;
    }
```

测试代码：

```java
@Test
public void testOrderQuery(){
    String sql="select order_id orderid,order_name ordername,order_date orderdate from `order` where order_id=?";
    Order order = new Order();
    order=orderQuery(sql,1);
    System.out.println(order);
}
```

结果：![image-20220224201126938](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220224201126938.png)

#### 实现不同表的查询操作

```java
package com.test03_PreparedStatement;

import com.test03_bean.Customers;
import com.test03_bean.Order;
import com.test03_util.JDBCUtils;
import org.testng.annotations.Test;

import java.lang.reflect.Field;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;

//实现不同表的通用查询操作
//使用泛型加反射来实现通用
public class PreparedStatementQuery {
    //泛型规定了传什么什么类型的实例就返回什么类型的对象
    public <T>T getInstance(Class<T> clazz,String sql,Object...arr){
        Connection conn= null;
        PreparedStatement pre= null;
        ResultSet res = null;
        try {
            //1、获取连接，并创建PreparedStatement实例,并填充占位符,执行
            conn = JDBCUtils.getConnection();
            pre = conn.prepareStatement(sql);
            for(int i=0;i<arr.length;i++){
                pre.setObject(i+1,arr[i]);
            }
            //2、获取结果集的元数据（取得结果集的每一列）
            res = pre.executeQuery();
            ResultSetMetaData rsmd = res.getMetaData();
            //3、通过ResultSetMetaDate获得结果集列数
            int columnCount=rsmd.getColumnCount();
            if(res.next()){
                //用反射实例化对象
                T t = clazz.newInstance();
                //处理每个结果集的每一列
                for (int i=0;i<columnCount;i++){
                    //获得列值
                    //因不确定返回的列的值是什么类型的，所以用Object
                    Object columnValue=res.getObject(i+1);
                    //获得每个列的列名
                    String columnName=rsmd.getColumnLabel(i+1);
                    //通过反射，给封装的对象私有属性进行赋值
                    Field field = clazz.getDeclaredField(columnName);
                    field.setAccessible(true);
                    field.set(t,columnValue);
                }
                return t;
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            JDBCUtils.closeResource(conn,pre,res);
        }
        return null;
    }
    //测试
    @Test
    public void testgetInstance(){
        String sql1="select order_id orderid,order_name ordername,order_date orderdate from `order` where order_id=?";
        Order order = getInstance(Order.class, sql1, 1);
        System.out.println(order);
        String sql2="select id,name,email,birth from customers where id=?";
        Customers customers = getInstance(Customers.class, sql2, 1);
        System.out.println(customers);
    }
}

```

结果：

![image-20220224235114659](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220224235114659.png)

#### 实现多行查询结果显示方法

当查询结果有多个时，可以用List集合来保存每一个对象(每一行结果集)

```java
//实现返回的查询结果是多个值的方法
public <T>List<T> getMoreInstance(Class<T> clazz, String sql, Object...arr){
    Connection conn= null;
    PreparedStatement pre= null;
    ResultSet res = null;
    try {
        //1、获取连接，并创建PreparedStatement实例,并填充占位符,执行
        conn = JDBCUtils.getConnection();
        pre = conn.prepareStatement(sql);
        for(int i=0;i<arr.length;i++){
            pre.setObject(i+1,arr[i]);
        }
        //2、获取结果集的元数据（取得结果集的每一列）
        res = pre.executeQuery();
        ResultSetMetaData rsmd = res.getMetaData();
        //3、通过ResultSetMetaDate获得结果集列数
        int columnCount=rsmd.getColumnCount();
        ArrayList<T> list = new ArrayList<>();
        while (res.next()){
            //用反射实例化对象
            T t = clazz.newInstance();
            //处理每个结果集的每一列
            for (int i=0;i<columnCount;i++){
                //获得列值
                //因不确定返回的列的值是什么类型的，所以用Object
                Object columnValue=res.getObject(i+1);
                //获得每个列的列名
                String columnName=rsmd.getColumnLabel(i+1);
                //通过反射，给封装的对象私有属性进行赋值
                Field field = clazz.getDeclaredField(columnName);
                field.setAccessible(true);
                field.set(t,columnValue);
            }
            list.add(t);//list存每一个对象，对象的属性都赋值了。
        }
        return list;
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        JDBCUtils.closeResource(conn,pre,res);
    }
    return null;
}
//测试
@Test
public class PreparedStatementQuery {
    public void testGetMoreInstance(){
        String sql="select id,name,email,birth from customers where id>?";
        List<Customers> list = getMoreInstance(Customers.class, sql, 12);
        list.forEach(System.out::println);直接输出list的结果。

    }
```

结果：

![image-20220225013209307](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220225013209307.png)

#### PreparedStatement对于Statement的优点

1、解决了拼串问题。

2、解决了SQL注入问题：PreparedStatement对sql语法进行预编译，将逻辑先确定好了，然后再填充占位符，而Statement则将整个sql语句直接放到sql去执行，容易造成注入问题。

3、PreparedStatement可以操作Blob(大文件)的数据，而Statement不能。

4、PreparedStatement的实现了更高效的批量操作。

### 插入Blob数据

在MySQL中，Blob是一个二进制大型对象，是一个可以存储大量数据的容器，它能容纳不同大小的数据。

插入Blob类型的数据必须使用PreparedStatement。

![image-20220225163718494](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220225163718494.png)

代码实例：

插入Blob型数据：

```java
public class BlobTest {
    @Test
    public void testInsert() throws Exception {
        Connection conn = JDBCUtils.getConnection();
        String sql="insert into customers(name,email,birth,photo) value (?,?,?,?)";
        PreparedStatement pre = conn.prepareStatement(sql);
        pre.setString(1,"马化腾");
        pre.setString(2,"ma@qq.com");
        pre.setObject(3,"1970-06-3");
        //以流的方式输入图片
        FileInputStream file = new FileInputStream(new File("src/马化腾.jpeg"));
        pre.setBlob(4,file);
        pre.execute();
        conn.close();
        pre.close();

    }
}
```

取出Blob数据：

```java
   @Test
    public void testGetBlob()  {
        Connection conn= null;
        PreparedStatement pre = null;
        ResultSet res = null;
        try {
            conn = JDBCUtils.getConnection();
            String sql="select id,name,email,birth,photo from customers where id=?";
            pre = conn.prepareStatement(sql);
            //填充占位符
            pre.setObject(1,29);
            //启动查询，并将结果存放在 ResultSet的res对象里
            res = pre.executeQuery();
            if (res.next()) {
                //获取每列信息
                int id = res.getInt(1);
                String name = res.getString(2);
                String email = res.getString(3);
                Date birth = res.getDate(4);
                Customers cust = new Customers(id, name, email, birth);
                System.out.println(cust);
                //取出Blob型数据保存
                Blob blob = res.getBlob(5);
                InputStream is = blob.getBinaryStream();
                //设置输出流数据，并给输出文件取名
                FileOutputStream fos = new FileOutputStream("mahuateng.jpg");
                byte [] buffer = new byte[1024];
                int len;
                //字节流形式读取
                while ((len=is.read(buffer))!=-1){
                    fos.write(buffer,0,len);
                }
                is.close();
                fos.close();
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
        JDBCUtils.closeResource(conn,pre,res);
        }
    }
}
```

#### PreparedStatement实现批量处理

```java
ackage com.test05_Blob;


import com.test03_util.JDBCUtils;
import org.testng.annotations.Test;

import java.sql.Connection;
import java.sql.PreparedStatement;


/*
使用PreparedStatement实现批量数据操作
 */
//方式一：
public class InsertTest {
    @Test
    public void testInsert1() {
        Connection conn = null;
        PreparedStatement pre = null;
        try {
            conn = JDBCUtils.getConnection();
            String sql="insert into goods(name ) values(?)";
            pre = conn.prepareStatement(sql);
            long start = System.currentTimeMillis();
            for (int i=0;i<20000;i++){
                pre.setObject(1,"name_"+i);
                pre.execute();
            }
           long end=System.currentTimeMillis();
            System.out.println("耗时："+(end-start));
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
        JDBCUtils.closeResource(conn,pre);
        }
    }
    @Test
    //方式二：
    //在URL后面添加?rewriteBatchedStatements=true，使SQL支持批量处理。
    public void testInsert2() {
        Connection conn = null;
        PreparedStatement pre = null;
        try {
            conn = JDBCUtils.getConnection();
            String sql="insert into goods(name ) values(?)";
            pre = conn.prepareStatement(sql);
            long start = System.currentTimeMillis();
            for (int i=0;i<=1000000;i++){
                pre.setObject(1,"name_"+i);
                //攒SQL语句，没五百条提交一次。
                pre.addBatch();
                if(i%500==0){
                    pre.executeBatch();
                    pre.clearBatch();
                }
            }
            long end=System.currentTimeMillis();
            System.out.println("耗时："+(end-start));
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            JDBCUtils.closeResource(conn,pre);
        }
    }
    @Test
    //方式三：不允许自动提交数据
    public void testInsert3() {
        Connection conn = null;
        PreparedStatement pre = null;
        try {
            conn = JDBCUtils.getConnection();
            String sql="insert into goods(name ) values(?)";
            pre = conn.prepareStatement(sql);
            conn.setAutoCommit(false);
            long start = System.currentTimeMillis();
            for (int i=0;i<=1000000;i++){
                pre.setObject(1,"name_"+i);
                pre.addBatch();
                if(i%500==0){
                    pre.executeBatch();
                    pre.clearBatch();
                }
            }
            conn.commit();
            long end=System.currentTimeMillis();
            System.out.println("耗时："+(end-start));
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            JDBCUtils.closeResource(conn,pre);
        }
    }
}
```

## 数据库事务

事务概念：

一组逻辑单元（一个或多个DML操作），使数据从一种状态变换到另一种状态。

**当一个事务中执行多个操作的时候，如果操作没有问题，都顺利执行，就将事务提交（commit）。**

**如果一个事务中有一个操作有问题，将放弃之前的所有修改操作，回滚（rollback）到事务最初的状态。**

**数据一旦提交就不可回滚到到提交之前的操作**

#### 数据自动提交的一些情况

1、DDL操作执行完后，自动提交，无论如何都弥补不了的。

2、DML默认情况也自动提交，但可以通过设置：set autocommit = false来让它不自动提交。

3、关闭连接：connection.close（）操作或者将数据库关闭都会自动提交。

代码实例：**实现AA用户给BB用户转账，且两人的操作同步进行。**

​		不考虑事务的情况时：每次执行完一天DML操作和调用update方法(JDBCUtils.closeResource(conn,pre))后都会执行commit操作。所有要想让操作变成一个事务，得要将两个DML操作的串起来，执行完两个操作再提交。所有得把SQL语句自动提交设置为false，还要把提交放在两个操作之后。

```java
package com.test03.transaction;
import com.test03.utils.JDBCUtils;
import org.junit.Test;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class Transaction {
    //设计一个通用方法，增删改都可以调用操作
    /*
    针对于数据表user_table来说
    AA用户给BB用户转账100（他俩同时执行）
     */
    //******************************未考虑事务的通用增删改版本**************************//
    //以下增删查改方法未考虑事务，没有将两个update串起来，导致一个update执行完，就提交了，所有但中途数据库发生故障，（因为AA已提交），也转出去的钱BB没收到，也无法回滚。
    @Test
    public void testUpdate(){
        //AA转账100
        String sql1="update user_table set balance=balance-100 where user=?";
        update(sql1,"AA");
        //模拟网络波动
        System.out.println(10/0);
        String sql2="update user_table set balance=balance+100 where user=?";
        update(sql2,"BB");
      

    }
    public void update(String sql,Object...arr) {
        Connection conn = null;
        PreparedStatement pre= null;
        try {
            //1、获取连接
            conn = JDBCUtils.getConnection();
            //2、预编译sql语句，返回 PreparedStatement的实例。
            pre = conn.prepareStatement(sql);
            //3、填充占位符
            for(int i=0;i<arr.length;i++){
                pre.setObject(i+1,arr[i]);
            }
            //4、执行
            pre.execute();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            //5、资源关闭
            JDBCUtils.closeResource(conn,pre);
        }
    }
    //**********************************考虑事务的通用增删改操作*********************//
    @Test
    public void testUpdateWithTX()  {
        Connection conn=null;
        try {
            conn = JDBCUtils.getConnection();
            //将自动提交设置为fasle。
            conn.setAutoCommit(false);
            //AA转账100
            String sql1="update user_table set balance=balance-100 where user=?";
            update(conn,sql1,"AA");
            //模拟网络波动
//            System.out.println(10/0);
            String sql2="update user_table set balance=balance+100 where user=?";
            update(conn,sql2,"BB");
            conn.commit();
        } catch (Exception e) {//出现网络波动后来到catch,执行回滚(rollback)
            e.printStackTrace();
            try {
                conn.rollback();
            } catch (SQLException ex) {
                ex.printStackTrace();
            }
        } finally {
            try {
                conn.setAutoCommit(true);
            } catch (SQLException e) {
                e.printStackTrace();
            }
            JDBCUtils.closeResource(conn,null);
        }
    }
    public void update(Connection conn,String sql,Object...arr) {
        PreparedStatement pre= null;
        try {
            //1、预编译sql语句，返回 PreparedStatement的实例。
            pre = conn.prepareStatement(sql);
            //2、填充占位符
            for(int i=0;i<arr.length;i++){
                pre.setObject(i+1,arr[i]);
            }
            //3、执行
            pre.execute();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            //4、资源关闭
            JDBCUtils.closeResource(null,pre);
        }
    }
}
```

### 事务的ACID属性

**1、原子性（Atomocity）**

原子性指的是事务是一个不可分割的工作单位，事务中的操作要么都发生，要么都不发生。

**2、一致性(Consistency)**

事务必须使数据库从一个一致性状态变化到另一个一致性状态。

**3、隔离性(Isolation)**

事务的隔离性是指一个事务的执行不能被其他事务干扰，即一个事务内部的操作及使用的数据对并发的其他事务时隔离的，并发执行的各个事务之间不能互相干扰。

**4、持久性**

事务一旦被提交，它对数据库的改变是永久性的，接下来的其他操作和数据库故障都对它没有什么影响。

#### 数据库并发问题

**脏读：**对于两个事务T1,T2，T1正在读取一段数据，T2正好也在更新这一段数据但没提交，被没有提交的T1读到了，这时，如果T2回滚，T1读取到的新数据就没有意义了。

**不可重复读：**对于T1，T2两个事务，T1T2同时操作同一份数据，T2更新了一些数据并提交，T1再去重复读取，数据就发生了变化，即在同一个事务中不同时间里读同一份数据，结果不同。

**幻读**：对于T1，T2两个事务，T1正在操作一段数据，T2在这段数据添加了一行并提交，T1操作时就会发现这一段数据又多出了一条数据。

#### 四种隔离级别

针对数据看并发问题，数据库提供了四种隔离级别。

1、READ UNCOMMITTED（读未提交数据）

三种并发问题都没解决

2、READ COMMITED(读已提交数据)

解决了脏读问题

3、REPEATEABLE READ(可重复读)

解决和脏读和不可重复读问题。

4、SERIALIZABLE(串行化)

解决了脏读，不可重复读，和幻读问题

这四种隔离级别越往下数据一致性越好，但是并发性越差

### JDBC-DAO的实现

**什么是DAO**

DAO（Data Access Object）访问数数据信息的类和接口

DAO模式分为几块：

1、与数据库表映射的类（保存和相关数据库表的属性）

2、BaseDAO：一个封装了增删改查方法的抽象类，不同表的操作类可以继承这个抽象类。

3、DAO接口：该接口定义了许多具体的增删改查方法，不同表的操作类可以继承该接口实现其所有方法。

4、DAOImpl（DAO的具体实施（implement））：继承BaseDAO和DAO接口，灵活地调用BaseDAO里的增删查改方法来实现各种DAO接口中的方法。

5、DAOTest(DAO的测试类)：测试DAOImpl中实现的各种具体方法。

代码示例：

**1、与数据库表映射的类**

```java
package com.bean;

import java.sql.Date;

/*
ORM编程思想（对象关系映射）
一个数据表对应一个java类
表中的一条记录对应java类的一个对象
表中的一个字段对应java类的一个属性
 */
public class Customers {
    private int id;
    private String name;
    private  String email;
    private Date birth;

    public Customers() {
    }

    public Customers(int id, String name, String email, Date birth) {
        this.id = id;
        this.name = name;
        this.email = email;
        this.birth = birth;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public Date getBirth() {
        return birth;
    }

    public void setBirth(Date birth) {
        this.birth = birth;
    }

    @Override
    public String toString() {
        return "Customers{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", email='" + email + '\'' +
                ", birth=" + birth +
                '}';
    }
}
```

**2、BaseDAO**

```java
package com.test03.dao;

import com.bean.Customers;
import com.test01.utils.JDBCUtils;

import java.lang.reflect.Field;
import java.lang.reflect.ParameterizedType;
import java.lang.reflect.Type;
import java.sql.*;
import java.util.ArrayList;
import java.util.List;

public abstract class BaseDao<T> {
   private Class<T> clazz=null;
    {//获取父类的泛型
        //this指向CustomerDAOImpl，因为由BaseDao的子类CustomerDAOImpl实例化对象
        Type genericSuperclass = this.getClass().getGenericSuperclass();//取CustomerDAOImpl的带泛型的父类的泛型
        ParameterizedType parameterizedType = (ParameterizedType) genericSuperclass;
        //将泛型保存到类型数组
        Type[] actualTypeArguments = parameterizedType.getActualTypeArguments();
        clazz= (Class<T>) actualTypeArguments[0];//只有一个泛型，第一个就是父类泛型

    }
    //增删改通用方法
    public void update(Connection conn, String sql, Object...arr) {
        PreparedStatement pre= null;
        try {
            //1、预编译sql语句，返回 PreparedStatement的实例。
            pre = conn.prepareStatement(sql);
            //2、填充占位符
            for(int i=0;i<arr.length;i++){
                pre.setObject(i+1,arr[i]);
            }
            //3、执行
            pre.execute();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            //4、资源关闭
            JDBCUtils.closeResource(null,pre);
        }
    }
    //通用查询操作，返回一条查询结果集
    public T getInstance(Connection conn,String sql,Object...arr){
        PreparedStatement pre= null;
        ResultSet res = null;
        try {
            //1、获取连接，并创建PreparedStatement实例,并填充占位符,执行
            pre = conn.prepareStatement(sql);
            for(int i=0;i<arr.length;i++){
                pre.setObject(i+1,arr[i]);
            }
            //2、获取结果集的元数据（取得结果集的每一列）
            res = pre.executeQuery();
            ResultSetMetaData rsmd = res.getMetaData();
            //3、通过ResultSetMetaDate获得结果集列数
            int columnCount=rsmd.getColumnCount();
            if(res.next()){
                //用反射实例化对象
                T t = clazz.newInstance();
                //处理每个结果集的每一列
                for (int i=0;i<columnCount;i++){
                    //获得列值
                    //因不确定返回的列的值是什么类型的，所以用Object
                    Object columnValue=res.getObject(i+1);
                    //获得每个列的列名
                    String columnName=rsmd.getColumnLabel(i+1);
                    //通过反射，给封装的对象私有属性进行赋值
                    Field field = clazz.getDeclaredField(columnName);
                    field.setAccessible(true);
                    field.set(t,columnValue);
                }
                return t;
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            JDBCUtils.closeResource(null,pre,res);
        }
        return null;
    }
    //实现返回的查询结果是多个值的方法
    public  List<T> getMoreInstance(Connection conn, String sql, Object...arr){
        PreparedStatement pre= null;
        ResultSet res = null;
        try {
            //1、获取连接，并创建PreparedStatement实例,并填充占位符,执行
            pre = conn.prepareStatement(sql);
            for(int i=0;i<arr.length;i++){
                pre.setObject(i+1,arr[i]);
            }
            //2、获取结果集的元数据（取得结果集的每一列）
            res = pre.executeQuery();
            ResultSetMetaData rsmd = res.getMetaData();
            //3、通过ResultSetMetaDate获得结果集列数
            int columnCount=rsmd.getColumnCount();
            ArrayList<T> list = new ArrayList<>();
            while (res.next()){
                //用反射实例化对象
                T t = clazz.newInstance();
                //处理每个结果集的每一列
                for (int i=0;i<columnCount;i++){
                    //获得列值
                    //因不确定返回的列的值是什么类型的，所以用Object
                    Object columnValue=res.getObject(i+1);
                    //获得每个列的列名
                    String columnName=rsmd.getColumnLabel(i+1);
                    //通过反射，给封装的对象私有属性进行赋值
                    Field field = clazz.getDeclaredField(columnName);
                    field.setAccessible(true);
                    field.set(t,columnValue);
                }
                list.add(t);
            }
            return list;
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            JDBCUtils.closeResource(null,pre,res);
        }
        return null;
    }
    //特殊情况查询情况的通用方法：比如count。
    public <E>E getValue(Connection conn,String sql,Object...arr) {
        PreparedStatement pre = null;
        ResultSet res = null;
        try {
            pre = conn.prepareStatement(sql);
            for (int i=0;i<arr.length;i++){
                pre.setObject(i+1,arr[i]);
            }
            res = pre.executeQuery();
            if(res.next()){
                return (E)res.getObject(1);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
        JDBCUtils.closeResource(null,pre,res);
        }
        return null;
    }

}

```

**3、DAO接口**

```java
package com.test03.dao;

import com.bean.Customers;

import java.sql.Connection;
import java.sql.Date;
import java.util.List;

public interface CustomerDao {
    //将cust对象添加到数据库中
    void insert(Connection conn, Customers cust);

    //根据指定id删除表的一条数据
    void  deleteById(Connection conn, int id);

    //针对内存中cust对象，去修改数据表中指定的记录
    void update(Connection conn,Customers cust);

    //针对指定的id查询得到对应的Customer对象(查询单行记录)
    Customers getCustomerById(Connection conn,int id);

    //查询出多个结果构成的集合
    List<Customers> getAll(Connection connection);

    //返回数据表中数据项目数
    Long getCount(Connection conn);

    //返回表中最大生日（年龄最小）
    Date getDate(Connection conn);
}

```

**4、DAOImpl**

```java
package com.test03.dao;
import com.bean.Customers;

import java.sql.Connection;
import java.sql.Date;
import java.util.List;

public class CustomerDAOImpl extends BaseDao<Customers> implements CustomerDao {
    //将cust对象添加到数据库中
    public void insert(Connection conn, Customers cust) {
        String sql = "insert into customers(name,email,birth) values(?,?,?)";
        update(conn, sql, cust.getName(), cust.getEmail(), cust.getBirth());
    }

    //根据指定id删除表的一条数据
    public void deleteById(Connection conn, int id) {
        String sql = "delete from customers where id=?";
        update(conn, sql, id);
    }

    //针对内存中cust对象，去修改数据表中指定的记录
    public void update(Connection conn, Customers cust) {
        String sql = "update customers set name=?,email=?,birth=? where id=?";
        update(conn, sql, cust.getName(),cust.getEmail(),cust.getBirth(),cust.getId());
    }

    //根据对应id查询得到对应的Customer对象(查询单行记录)
    public Customers getCustomerById(Connection conn, int id) {
        String sql = "select id,name,email,birth from customers where id=?";
        return getInstance(conn, sql, id);


    }

    //查询出多个结果构成的集合
    public List<Customers> getAll(Connection conn) {
        String sql = "select id,name,email,birth from customers";
        List<Customers> moreInstance = getMoreInstance(conn, sql);
        return moreInstance;
    }

    //返回数据表中数据项目数
    public Long getCount(Connection conn) {
        String sql = "select count(*) from customers";
        return getValue(conn, sql);

    }

    //返回表中最大生日（年龄最小）
    public Date getDate(Connection conn) {
        String sql = "select max(birth) from customers";
        return getValue(conn, sql);
    }
}
```

**5、DAOTest(DAO的测试类)**

```java
package com.test03.dao.junit;

import com.bean.Customers;
import com.test01.utils.JDBCUtils;
import com.test03.dao.CustomerDAOImpl;
import org.junit.Test;

import java.sql.Connection;
import java.sql.Date;
import java.util.List;

public class CustomerDAOImplTest {
    private CustomerDAOImpl dao=new CustomerDAOImpl();
    @Test
    public void insert()  {
        Connection conn = null;
        try {
            conn = JDBCUtils.getConnection();
            Customers cust = new Customers(1,"张三丰","zhangsanfen@qq.com",new Date(161543166L));
            dao.insert(conn,cust);
            System.out.println("添加成功");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                JDBCUtils.closeResource(conn,null);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }

    @Test
    public void deleteById() {
        Connection conn = null;
        try {
            conn = JDBCUtils.getConnection();
            dao.deleteById(conn,32);
            System.out.println("删除成功");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                JDBCUtils.closeResource(conn,null);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }

    @Test
    public void update() {
        Connection conn = null;
        try {
            conn = JDBCUtils.getConnection();
            Customers cust = new Customers(23,"李四","lisi@qq.com",new Date(32165156L));
            dao.update(conn,cust);
            System.out.println("更新成功");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                JDBCUtils.closeResource(conn,null);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }

    @Test
    public void getCustomerById() {
        Connection conn = null;
        try {
            conn = JDBCUtils.getConnection();
            Customers cust = dao.getCustomerById(conn, 16);
            System.out.println(cust);
            System.out.println("查询成功");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                JDBCUtils.closeResource(conn,null);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }

    @Test
    public void getAll() {
        Connection conn = null;
        try {
            conn = JDBCUtils.getConnection();
            List<Customers> all = dao.getAll(conn);
            all.forEach(System.out::println);
            System.out.println("查询成功");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                JDBCUtils.closeResource(conn,null);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }

    @Test
    public void getCount() {
        Connection conn = null;
        try {
            conn = JDBCUtils.getConnection();
            System.out.println("一共有"+dao.getCount(conn)+"行");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                JDBCUtils.closeResource(conn,null);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }

    @Test
    public void getDate() {
        Connection conn = null;
        try {
            conn = JDBCUtils.getConnection();

            System.out.println("最大生日日期："+dao.getDate(conn));
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                JDBCUtils.closeResource(conn,null);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}
```

### 数据库连接池

有了数据连接池，就不用自己创建链接了，只要获取数据库连接池，就可以直接用连接池创建链接。

#### C3P0连接池：

获取C3P0连接池：

```java
package com.test04.connection;

import com.mchange.v2.c3p0.ComboPooledDataSource;
import com.mchange.v2.c3p0.DataSources;
import org.junit.Test;

import java.beans.PropertyVetoException;
import java.sql.Connection;
import java.sql.SQLException;

public class C3P0Test {
    @Test
    public void testGetConnection1() throws PropertyVetoException, SQLException {
        //获取c3p0数据库连接池
        //方式一：
        ComboPooledDataSource cpds = new ComboPooledDataSource();
        cpds.setDriverClass( "com.mysql.jdbc.Driver" ); //loads the jdbc driver
        //mysql8.0版本的：jdbc:mysql://localhost:3306/数据库名?useUnicode=ture&characterEncoding=UTF-8&serverTimezone=GMT%2B8
        cpds.setJdbcUrl( "jdbc:mysql://localhost:3306/test?useUnicode=ture&characterEncoding=UTF-8&serverTimezone=GMT%2B8" );
        cpds.setUser("root");
        cpds.setPassword("123456Hh");
        //通过设置相关参数，对数据库连接池进行管理
        //设置初始连接数
        cpds.setInitialPoolSize(10);
        Connection conn = cpds.getConnection();
        System.out.println(conn);
        //销毁连接池，一般不用
//        DataSources.destroy(cpds);

    }
    @Test
    public void testGetConnection2() throws SQLException {
        //方式二：读取xml配置文件
        ComboPooledDataSource cpds = new ComboPooledDataSource("helloc3p0");
        Connection conn = cpds.getConnection();
        System.out.println(conn);
    }
}
```

获取C3P0连接池方式二的xml配置文件：

注意：配置信息的基本信息的name值是不可以随便定义的，要和连接池规定的名字一样，才能获取到连接数据。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<c3p0-config>
<!--        连接池的名字-->
    <named-config name="helloc3p0">
<!--        提供获取连接的四个基本信息-->
        <property name="driverClass">com.mysql.jdbc.Driver</property>
<!--      localhost:3306可省略  -->
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/test?useUnicode=ture&amp;characterEncoding=UTF-8&amp;serverTimezone=GMT%2B8</property>
        <property name="user">root</property>
        <property name="password">123456Hh</property>
<!--数据库连接池管理的基本信息-->
<!--        当数据库连接池的连接数不够时，c3p0一次性向数据库服务器申请的连接数-->
        <property name="acquireIncrement">5</property>
<!--      c3p0数据库连接池的初始化连接数  -->
        <property name="initialPoolSize">10</property>
<!--    c3p0数据库连接池维护的最小连接数    -->
        <property name="minPoolSize">10</property>
        <!--    c3p0数据库连接池维护的最大连接数    -->
        <property name="maxPoolSize">100</property>
        <!--    c3p0数据库连接池维护的最大StaStatement个数，包括PreparedStatement    -->
        <property name="maxStatements">50</property>
<!--        每个连接可以拥有最大StaStatement个数-->
        <property name="maxStatementsPerConnection">2</property>

    </named-config>
</c3p0-config>
```

### 将连接封装成为一个方法

一般的我们不会每次连接的时候都要写连接的步骤，我们可以将新建连接池，然后获取连接封装成一个方法，当我们要连接的时候调用方法就行了

代码实例：

```java
//C3P0的相关连接操作
public class JDBCUtils {
    //新建连接池要写再获取连接方法的外面，因为我们只需要使用一个连接池，获取多个连接。
        static ComboPooledDataSource cpds = new ComboPooledDataSource("helloc3p0");
    public static Connection getC3P0Connection() throws SQLException {
        Connection conn = cpds.getConnection();
        return conn;

    }

    public static void closeResource(Connection conn, PreparedStatement pre){
        //资源关闭
        try {
            if(pre!=null)
                pre.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        try {
            if(conn!=null)
                conn.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }

    }
    public static void closeResource(Connection conn, PreparedStatement pre, ResultSet res){
        //资源关闭
        try {
            if(pre!=null)
                pre.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        try {
            if(conn!=null)
                conn.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        try {
            if(res!=null)
                res.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

### DBCP数据库连接池

获取连接池连接：（推荐将方法封装，和上面的方法封装差不多）

```java
//测试DBCP的数据库连接池
public class DBCPTest {
    @Test
    public void testGetConnection1() {
        BasicDataSource source = new BasicDataSource();
        source.setDriverClassName("com.mysql.jdbc.Driver");
        source.setUrl("jdbc:mysql://localhost:3306/test?useUnicode=ture&characterEncoding=UTF-8&serverTimezone=GMT%2B8");
        source.setUsername("root");
        source.setPassword("123456Hh");
        //设置相关连接池属性
        source.setInitialSize(10);
        //最大活跃数
        source.setMaxActive(10);
        System.out.println(source);
    }
    //方式二：获取配置文件
    @Test
    public void testGetConnection2() throws Exception {
        Properties pro = new Properties();//保存配置文件的数据，建立key和value关系
        InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("DBPC.properties");//系统加载器
        pro.load(is);
        DataSource source = BasicDataSourceFactory.createDataSource(pro);//创建 DataSource对象
        Connection conn = source.getConnection();
        System.out.println(conn);


    }
}
```

配置文件：

```properties
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/test?useUnicode=ture&characterEncoding=UTF-8&serverTimezone=GMT%2B8
username=root
password=123456Hh
```

### Druid数据库连接池（重要）

现在大公司使用的连接池一般是阿里的Druid数据库连接池，该连接池继承了C3P0和DBPC连接池的优点。

建立Druid连接池（封装成方法）

```java
//获取Druid连接池的连接到的封装方法
//获取Druid连接池资源
    private static DataSource source=null;
    static {
        try {
            Properties pro = new Properties();//保存配置文件的数据，建立key和value关系
            InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("DBPC.properties");//系统加载器，将配置文件以IO流的形式读取。
            pro.load(is);//解析IO流对象。
            source = BasicDataSourceFactory.createDataSource(pro);//获取用户相关信息，建立连接
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
//获取连接的方法
    public static Connection getDruidConnection(){
        Connection conn = null;
        try {
            conn = source.getConnection();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return conn;
    }
    public static void closeResource(Connection conn, PreparedStatement pre){
        //资源关闭
        try {
            if(pre!=null)
                pre.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        try {
            if(conn!=null)
                conn.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }

    }
    public static void closeResource(Connection conn, PreparedStatement pre, ResultSet res){
        //资源关闭
        try {
            if(pre!=null)
                pre.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        try {
            if(conn!=null)
                conn.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        try {
            if(res!=null)
                res.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

相关配置文件：（Druid.properties）

```properties
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/test?useUnicode=ture&characterEncoding=UTF-8&serverTimezone=GMT%2B8
username=root
password=1234
```

### Apache-DBUtils（实现CRUD（增删改查）操作）

commons-dbutils是Apache组织提供的一个开源的JDBC工具类库,它是对JDBC的简单封装，封装了针对数据库的增删查改操作，和之前写的DAO是差不多的.

增删改查操作都是继承ResultSetHandler<T>接口实现，

```java
public interface ResultSetHandler<T> {

    /**
     * Turn the <code>ResultSet</code> into an Object.
     * 
     * @param rs The <code>ResultSet</code> to handle.  It has not been touched
     * before being passed to this method.
     * 
     * @return An Object initialized with <code>ResultSet</code> data. It is
     * legal for implementations to return <code>null</code> if the 
     * <code>ResultSet</code> contained 0 rows.
     * 
     * @throws SQLException if a database access error occurs
     */
    public T handle(ResultSet rs) throws SQLException;

}
```

### 相关测试

```java
package com.test05.dbutils;

import com.bean.Customers;
import com.test04.utils.JDBCUtils;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.ResultSetHandler;
import org.apache.commons.dbutils.handlers.BeanHandler;
import org.apache.commons.dbutils.handlers.BeanListHandler;
import org.apache.commons.dbutils.handlers.ScalarHandler;
import org.junit.Test;

import java.sql.Connection;
import java.sql.Date;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.List;

public class QueryRunnerTest {
    //测试增加操作
    @Test
    public void testInsert(){
        Connection conn = null;
        try {
            String sql="insert into customers(name,email,birth) values(?,?,?)";
            QueryRunner runner = new QueryRunner();
            conn = JDBCUtils.getDruidConnection();
            int insetCount = runner.update(conn, sql, "蔡徐坤", "caixukun@qq.com", "1999-5-9");
            System.out.println("增加了"+insetCount+"条数据");
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
        JDBCUtils.closeResource(conn,null);
        }
    }
    /*runner.query(conn, sql, ResultSetHandler<T> rsh,Object...params)方法：ResultSetHandler<T>是一个泛型接口，它的实现类有
     BeanHandler<T>保存单个结果集， BeanListHandler<T>:用List保存多个结果集； ScalarHandler()：保存特殊值的结果(Object类型)需要强转，这个方法返回的是该接口的实现类的handle()方法的返回值。*
    //测试查询操作(查询结果为一条)
    @Test
    public void testQuery1(){
        Connection conn = null;
        try {
            String sql="select name ,birth from customers where id=?";
            QueryRunner runner = new QueryRunner();
            conn = JDBCUtils.getDruidConnection();
            BeanHandler<Customers> beanHandler = new BeanHandler<>(Customers.class);
            Object query = runner.query(conn, sql, beanHandler, 23);
            System.out.println(query);
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JDBCUtils.closeResource(conn,null);
        }
    }
    //测试查询操作(查询结果为多条)
    @Test
    public void testQuery2(){
        Connection conn = null;
        try {
            String sql="select name ,birth from customers where id<?";
            QueryRunner runner = new QueryRunner();
            conn = JDBCUtils.getDruidConnection();
            BeanListHandler<Customers> beanListHandler = new BeanListHandler<>(Customers.class);
            List<Customers> query = runner.query(conn, sql, beanListHandler, 23);
            query.forEach(System.out::println);
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JDBCUtils.closeResource(conn,null);
        }
    }
    //测试查询特殊值（count）
    @Test
    public void testQuery3(){
        Connection conn = null;
        try {
            String sql="select count(*) from customers";
            QueryRunner runner = new QueryRunner();
            conn = JDBCUtils.getDruidConnection();
            ScalarHandler scalarHandler = new ScalarHandler();
            Long count = (Long) runner.query(conn, sql, scalarHandler);
            System.out.println(count);

        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JDBCUtils.closeResource(conn,null);
        }
    }
    //测试查询特殊值（max）最大生日
    @Test
    public void testQuery4(){
        Connection conn = null;
        try {
            String sql="select max(birth) from customers";
            QueryRunner runner = new QueryRunner();
            conn = JDBCUtils.getDruidConnection();
            ScalarHandler scalarHandler = new ScalarHandler();
            Date birth= (Date) runner.query(conn, sql, scalarHandler);
            System.out.println(birth);

        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JDBCUtils.closeResource(conn,null);
        }
    }
    //自定义方法
    @Test
    public void testQuery5(){
        Connection conn = null;
        try {
            String sql="select id,name,email,birth from customers where id=?";
            QueryRunner runner = new QueryRunner();
            conn = JDBCUtils.getDruidConnection();
            ResultSetHandler<Customers> handler=new ResultSetHandler<>() {
                @Override
                //ResultSet是搜索的结果集
                //handle()是ResultSetHandler<T>接口的方法，自定义方法要继承于它，并写好方法实现。
                public Customers handle(ResultSet rs) throws SQLException {
                    return null;//如果仅像让它返回空，就 return null
//                    if (rs.next()){//下面的模仿搜索一行结果集的方法实现
//                        int id =  rs.getInt(1);
//                        String name=rs.getString(2);
//                        String email=rs.getString(3);
//                        Date birth=rs.getDate(4);
//                        Customers cust = new Customers(id,name,email,birth);
//                        return cust;
//                    }
//                    return null;
                }
            };
            System.out.println(runner.query(conn,sql,handler,1));

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            JDBCUtils.closeResource(conn,null);
        }
    }
}
```
