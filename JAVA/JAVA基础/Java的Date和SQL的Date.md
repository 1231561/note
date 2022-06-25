# Java的Date和SQL的Date

java.sql.Date是针对SQL语句使用的，它只包含日期而没有时间部分，一般在读写数据库的时候Preparadstatement的setDate()的参数和Resultset的getDate()方法的参数都是用java.sql.Date

相互转化：1、java.sql.Date转为java.util.Date

java.sql.Date date1=new java.sql.Date();

java.util.Date date2=new java.util.Date(date1.get(Time));

2、java.util.Date转为java.sql.Date

java.util.Date date1=new java.util.Date();

java.sql.Date date2=new java.sql.Date(date1.get(Time));

#### 格式化日期

使用SimpleDateFormat方法将日期格式化：

```java
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
```

用调用parse方法将格式化的日期转化为java.util.Date类型的

```java
java.util.Date date = sdf.parse("2000-05-31");
```

将java.util.Date类型转化为java.sql.Date型

```java
pre.setDate(3,  new Date(date.getTime()));
```

代码实例：

Date有两个包，一个是java.util.Date，一个是java.sql.Date,调哪个包，Date是哪个类

java.util.Date是java.sql.Date的父类。

```java
import org.junit.Test;

import java.util.Date;

public class 日期 {
    @Test
    public void DateTest(){
        //构造器1：Date()创建一个对应当前的时间的对象
        Date date1 = new Date();
        //toString（）方法：显示当前的年、月、日、时、分、秒
        System.out.println(date1);
        //getTime（）方法：获取当前Date对象的对应的毫秒数(时间戳),距离1970年1月1日0时0分0秒的毫秒数
        System.out.println(date1.getTime());
        //构造器2：Date()创建一个制定毫秒数的Date对象
        Date date2=new Date(1645629152672L);
        System.out.println(date2);
        //创建 java.sql.Date对象
        java.sql.Date date3=new java.sql.Date(1645631343229L);
        System.out.println(date3);
        //将 java.sql.Date转换为java.util.Date
        //因为java.sql.Date是java.util.Date的子类，所以向上转型就可以转变
        Date date4=new java.sql.Date(1645631343229L);
        System.out.println(date4);
        //将java.util.Date转换为java.sql.Date
        //因为java.util.Date是父类，可以先向上转型再向下转型转化为java.sql.Date，但是用getTime（）方法变为毫秒，再转化，更简单。
        Date date5=new Date(1645631343229L);
        java.sql.Date date6=new java.sql.Date(date5.getTime());

    }
}
```