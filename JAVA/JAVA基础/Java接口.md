# Java接口

​		接口是一种特殊的类，类方法全都是抽象类(没有方法体，只有方法名的类)，所有继承接口的类必须得全部实现（重写）接口的抽象方法成为实现类，不然该类也是抽象类。

**接口可以多继承，即接口可以继承多个接口**，**类只能单继承**

UserDao接口：

```java
public interface UserDao {
    public void print();
}
```

如果UserDaoImpl类要继承UserDao接口，有两种处理方式，要不实现print()方法变成实现类，要不就定义为抽象类。

![image-20220316113255471](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220316113255471.png)

**接口不能直接实例化，因为接口的所有方法都是抽象方法，没有方法体。**

**所以只能通过多态（实例化实现类），来调用接口的方法。**

UserDaoImpl是UserDao接口的一个实现类。

```java
public class UserDaoImpl implements UserDao{
    @Override
    public void print() {
        System.out.println("dao.....");
    }
}
```

**可以使用多态new一个实现类对象，或者通过注入（Spring）调用接口UserDao的print()方法。**

```java
    @Test
    public void test3(){
        UserDao userDao=new UserDaoImpl();
        userDao.print();
    }
}
```

结果：

dao.....