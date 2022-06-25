# i++和++i的区别

#### ++i

++i是直接给i加上1

赋值：

```java
    int i=0;
    int x=0;
    x=++i;
    System.out.println(x);//1
}
```

赋值后再++

```java
    int i=0;
    int x=0;
    x=++i;
    System.out.println(++i);//2
}
```

#### i++

++i的原理是先将i保存到临时变量temp中，然后i=i+1。

例如：

x=i++;

这里的i++,先将i使用临时变量temp保存，然后将i+1，最后将temp保存的值赋给x，所以最后x的值为0，i的值为1。

```java
int i=0;
int x=0;
x=i++;
System.out.println(x);//0
System.out.println(i);//1
```