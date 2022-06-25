# static关键字

静态代码块随着类的加载执行。

静态代码块的执行优先级高于非静态的初始化块。

当new一个类对象时，static修饰的成员变量首先被初始化，随后时普通成员（非静态），最后调用该类的构造函数，如果有它的super类，则先调用super类的构造函数。

代码块的执行顺序：静态代码块------>非静态代码块------->构造函数

```java
public class 静态代码块 {
}
class Father{
    static {
        System.out.println("父类中的静态代码块");
    }
    Father(){
        System.out.println("父类中的构造函数");
    }
    {
        System.out.println("父类中的非静态代码块");
    }

    public static void main(String[] args) {
        System.out.println("父类中的main方法");
    }
}
class Son extends Father{
    static {
        System.out.println("子类中的静态代码块");
    }
    Son(){
        System.out.println("子类中的构造函数");
    }
    {
        System.out.println("子类中的非静态代码块");
    }

    public static void main(String[] args) {
        System.out.println("子类中的main方法");
        new Son();
    }
}
```

运行结果：

父类中的静态代码块
子类中的静态代码块
子类中的main方法
父类中的非静态代码块
父类中的构造函数
子类中的非静态代码块
子类中的构造函数



静态代码块的特点：随着类的加载而执行，而且只执行一次。

```java
class Father{
    static {
        System.out.println("父类中的静态代码块");
    }
    Father(){
        System.out.println("父类中的构造函数");
    }
    {
        System.out.println("父类中的非静态代码块");
    }

    public static void main(String[] args) {
        System.out.println("父类中的main方法");
    }
}
class Son extends Father{
    static {
        System.out.println("子类中的静态代码块");
    }
    Son(){
        System.out.println("子类中的构造函数");
    }
    {
        System.out.println("子类中的非静态代码块");
    }

    public static void main(String[] args) {
        System.out.println("子类中的main方法");
        new Son();
        new Son();
    }
}
```

运行结果：

父类中的静态代码块
子类中的静态代码块
子类中的main方法
父类中的非静态代码块
父类中的构造函数
子类中的非静态代码块
子类中的构造函数
父类中的非静态代码块
父类中的构造函数
子类中的非静态代码块
子类中的构造函数