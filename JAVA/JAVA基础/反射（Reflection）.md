# 反射（Reflection）

反射是动态语言的关键，程序在运行期间可以借助Reflection API取得任何类的内部信息。能直接操作任意对象的内部属性及方法。不用new就可以操纵对象。

![image-20220222144628313](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220222144628313.png)

Java,c++是静态语言。反射使JAVA有了准动态特性。

#### 反射相关的主要API

java.lang.Class:代表一个类

java.lang.reflect.Method:代表类的方法

java.lang.reflect.Field:代表类的成员变量

java.lang.reflect.Constructor:代表类的构造器

Class是一个类的类。

**反射的作用：**

**通过反射可以调用其他类的私有成员**

代码实例：

Person类

```java
package com.Reflection.java;

public class Person {
    private String name;
    public int age;
    public Person() {
    }
    public Person(String name,int age) {
        this.name=name;
        this.age=age;
    }
    private Person(String name) {
        this.name=name;
    }
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
    public void show(){
        System.out.println("我是一个人");
    }
    private String showNation(String nation){
        System.out.println("我是"+nation+"人！！！");
        return  nation;
    }
    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

反射测试类：

```java
package com.Reflection.java;

import org.junit.Test;

import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

public class ReflectionTest {
    @Test
    public void test01(){
        Person p1 = new Person("Tom",12);
        p1.age=10;
        System.out.println(p1.toString());
        p1.show();
        //在没有反射之前，不能直接在Person类外部，直接调用Person类的私有成员
    }
    @Test
    //反射
    public void test02() throws NoSuchMethodException, InvocationTargetException, InstantiationException, IllegalAccessException, NoSuchFieldException {
        //通过反射创建Person类对象
        Class clazz=Person.class;
        Constructor con =clazz.getConstructor(String.class,int.class);
        Object obj=con.newInstance("Tom",12);
        Person p1=(Person) obj;
        System.out.println(p1.toString());
        //通过反射调用对象指定的方法、属性。
        //调用属性
        Field age=clazz.getDeclaredField("age");//getDeclaredField获取当前运行时类的所有属性，筛选等于age的属性
        age.set(p1,10);
        System.out.println(p1.toString());
        //调用方法
        Method show=clazz.getDeclaredMethod("show");
        show.invoke(p1);
        //通过反射可以调用Person的私有成员
        //通过反射实例化一个Person对象
        Constructor con1=clazz.getDeclaredConstructor(String.class);
        con1.setAccessible(true);//值为 true 则指示反射的对象在使用时应该取消 Java 语言访问检查,就可以访问私有成员
        Person p2=(Person) con1.newInstance("Tom");
        System.out.println(p2.toString());
        //获取私有成员
        //调用私有属性
        Field name=clazz.getDeclaredField("name");
        name.setAccessible(true);
        name.set(p2,"Jack");//可以修改私有成员
        name.get(p2);//直接获取私有的name值，Jack
        System.out.println(p2.toString());
        //调用私有方法
        Method showNation=clazz.getDeclaredMethod("showNation", String.class);
        showNation.setAccessible(true);
        String nation=(String)showNation.invoke(p2,"中国");//相当于对象直接调用成员方法
        System.out.println(nation);
    }
}
```

#### 几个疑问

要调用一个类的公共成员是，应该用new还是用反射？

答：用new，因为比较简便。

什么时候用反射？

答：编译的时候，不知道new哪个类的对象，这时候就用到反射，因为反射具动态性，程序在运行的时候反射可以动态的获取数据解析，来创建相应对象。

反射和面向对象的封装性冲突吗？

不冲突，一般的，类里面共有成员也可以调用私有成员，用更好的共有方法来调用私有成员，没必要用反射来直接调用私有成员。

###	关于Class

**Class如何实例化？**

运行时类就是Class的实例化，也就是说Class类的对象就是正在运行时的类。

运行时类是加载进内存运行的类。

**类的加载：**

程序经过java.exe命令后，会生成多个字节码文件（.class结尾的文件）。当我们运行时，会使用的java.exe对多个字节码文件进行解析，看哪个字节码已经有了main方法（程序入口）,就将这个字节码文件放到内存，放到内存的过程就是类的加载。加载到内存的类就是运行时类，作为Class的实例。

#### 获取Class实例的方法

```java
@Test
public void test03() throws ClassNotFoundException {
    //1、方式1：通过调用运行时类.class
    Class<Person> clazz1=Person.class;//泛型可不写
    System.out.println(clazz1);
    //2、方式2：通过运行时的对象调用.getClass方法
    Person person = new Person();
    Class clazz2=person.getClass();
    System.out.println(clazz2);
    //3、方式3：调用Class的静态方法
    Class clazz3=Class.forName("com.Reflection.java.Person");
    System.out.println(clazz3);
    //4、方式4：使用类的加载器（了解）
    ClassLoader classLoader=ReflectionTest.class.getClassLoader();
    Class clazz4=classLoader.loadClass("com.Reflection.java.Person");
    System.out.println(clazz4);
    System.out.println(clazz1==clazz3);

}
```

这四种方法，方法3最常用，因为它体现了反射的动态性，即在编译时可以根据文件路径的变化来实例化出不同的类对象。

而且四种方法获取的同一个类对象运行类的是相同的，加载到内存的运行类都缓存一定的时间，在这世间内获取的运行类都是相同的。

#### Class可以获取那些类型对象

![image-20220222184058642](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220222184058642.png)

#### 类加载器（ClassLoader）

类加载器的作用是把类转载进内存的，JVM规范定义了如下类型的类加载器

1、引导类加载器：是JVM自带的加载器，负责装载JAVA核心库（String等）进内存，改加载器无法直接获取。

2、扩展类加载器：负责将jar包装入工作库

3、系统加载类：负责将自定义类的加载到内存中（最常用）

系统加载器的父类是扩展类加载器，可以通过getfather（）方法获取，拓展类加载器的父类是引导类加载器，但是无法通过getfather（）方法获取。

#### 使用ClassLoader加载配置文件

```java
public class ClassLoaderTest {
    @Test
    public void test() throws IOException {
        Properties pro = new Properties();、//用于读取配置文件的集合
        //方法1：
//      FileInputStream f1 = new FileInputStream("src\\jdbc.properties");
//      pro.load(f1);
//      String user=pro.getProperty("user");
//      String password=pro.getProperty("password");
//        System.out.println("user="+user+" password="+password);
		//方法2：使用ClassLoader
        ClassLoader classLoader=ClassLoaderTest.class.getClassLoader();
        InputStream is=classLoader.getResourceAsStream("jdbc.properties");
        pro.load(is);
        String user=pro.getProperty("user");
        String password=pro.getProperty("password");
        System.out.println("user="+user+" password="+password);
    }
}
```

#### 通过反射，创建运行时类的对象

```java
package com.Reflection.java;

import org.junit.Test;

public class NewInstanceTest {
    @Test
    public void test() throws InstantiationException, IllegalAccessException {
        Class<Person> clazz=Person.class;
        Person p1=clazz.newInstance();
        //调用该方法，创建了Person型的运行时类对象，该方法，默认调用无参构造函数，所以该运行时类必须有无参构造函数

        System.out.println(p1);
    }

}
```

结果：![image-20220222225906840](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220222225906840.png)

newInstance()其实内部也调用了构造器，而且是Person类的构造器。

所以对该方法运行时类类有一些要求：

1、运行时类得提供一个无参构造方法

2、无参构造方法必须不能是private的。

通常，javabean中，要求提供一个public空参构造器，原因：

1、便于通过反射，创造运行时类的对象

2、便于继承时，子类调用super()时，保证父类有无参构造方法

#### 反射的动态性实例（重要）

```java
  //创建一个方法，参数是运行时类的路径，根据传入的类的路径，返回经过反射创造的类的对象，体现动态性。
public Object getInstance(String classPath) throws ClassNotFoundException, InstantiationException, IllegalAccessException {
        Class clazz=Class.forName(classPath);//通过forName方法通过类的路径来获取类
        return clazz.newInstance();//返回对象
    }
    @Test
    public void test01(){
      String classPath="";
       for (int i=0;i<10;i++){
           int num=new Random().nextInt(3);//随机生成0，1，2三个数
           switch (num){
               case 0:
                   classPath="java.util.Date";
                   break;
               case 1:
                   classPath="java.lang.Object";
                   break;
               case 2:
                   classPath="com.Reflection.java.Person";
                   break;
           }
           try {
               Object obj = getInstance(classPath);//根据不同的路径，得到不同的对象
               System.out.println(obj);//输出对象
           } catch (Exception e){
               e.printStackTrace();
           }
       }
```

结果：

![image-20220222235403904](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220222235403904.png)

**程序运行时通过传入不同的类的路径，得到多个由反射创造出来的相应对象，体现反射的动态性**

#### 反射能获取运行时类的哪些东西？

反射能获取运行时类的属性、方法、构造器结构、父类、父类的泛型、接口、所在包、注解等

较常用的有获取带泛型的运行时类的父类的泛型和获取运行时类实现的接口。

### 调用运行时类中的指定结构

结构分为属性、方法、构造器

Person类(运行时类)

```java
package com.Reflection.java;

public class Person {
    private String name;
     int age;
     public int id;

    public Person() {
    }
    public Person(String name,int age) {
        this.name=name;
        this.age=age;
    }
    private Person(String name) {
        this.name=name;
    }
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
    public void show(){
        System.out.println("我是一个人");
    }
    private String showNation(String nation){
        System.out.println("我是"+nation+"人！！！");
        return  nation;
    }
    static private void addnumber(Integer a,Integer b){
        System.out.println("a+b= "+(a+b));
    }
    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}

```

调用运行时类中的指定结构的测试类

```java
package java1;

import com.Reflection.java.Person;
import org.junit.Test;

import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;

public class ReflectionTest {
    @Test
    public void testField() throws Exception {
        Class<Person> clazz= Person.class;
        Field id=clazz.getField("id");//getField方法只能获取public的属性，不常用
        //因为非静态成员属性只能通过实例化对象进行赋值，所以先创建运行时类对象
        Person p=clazz.newInstance();
        id.set(p,1001);
        System.out.println(id.get(p));
        Field age=clazz.getDeclaredField("age");//getDeclaredField可以获取任何权限的属性
        age.setAccessible(true);//获取到的属性只有public的属性能访问，所以setAccessible(true)将不是public属性变成都可以访问的
        age.set(p,12);
        System.out.println(age.get(p));
        Field name=clazz.getDeclaredField("name");
        name.setAccessible(true);
        name.set(p,"Tom");
        System.out.println(name.get(p));
    }
    @Test
    public void testMethod() throws Exception {
        Class<Person> clazz= Person.class;
        Person p=clazz.newInstance();
        //调用非静态方法
        Method showNation=clazz.getDeclaredMethod("showNation",String.class);//参数1:方法名  参数2：形参列表
        showNation.setAccessible(true);         //参数1：方法的调用者 //参数2：实参
        Object obj=(Object) showNation.invoke(p,"中国");//invoke方法相当于通过对象直接调用方法，且返回值就是对象调用方法的返回值
        System.out.println(obj);
        //调用静态方法
        Method addnumber=clazz.getDeclaredMethod("addnumber",Integer.class,Integer.class);
        addnumber.setAccessible(true);
        addnumber.invoke(Person.class,1,2);
        //调用构造器
        Constructor<Person> declaredConstructor = clazz.getDeclaredConstructor(String.class);
        declaredConstructor.setAccessible(true);
        Person p1=declaredConstructor.newInstance("Jack");
        System.out.println(p1);

    }
}
```



## 动态代理（重要）