# this

### this的主要用法有两种

1、区分对象属性和传入的参数

2、调用本类其他构造方法

```java
public class A {
    private int a;
    private int b;

    public A(int a) {
        this.a = a;//哪个对象被实例化，this就指向哪个对象。
    }

    public A(int a, int b) {
        this(a);//使用调用上面的构造方法给属性a赋值，而且必须放在方法第一句。
        this.b=b;
    }
}
```