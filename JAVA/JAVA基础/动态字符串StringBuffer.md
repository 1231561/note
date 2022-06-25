# 动态字符串StringBuffer

#### 构造方法

可以调用StringBuffer的有参构造方法来实例化对象。

例如：

输出结果就和String一样。

```java
StringBuffer stringBuffer = new StringBuffer("hello");
System.out.println(stringBuffer);//hello
```

结果：

hello

#### 追加字符方法append

该方法可以将整型的、字符型的、字符串型的数据追加到原字符串末尾。

```java
StringBuffer stringBuffer = new StringBuffer("hello");
stringBuffer.append("p");
System.out.println(stringBuffer);
```

结果：hellop

#### 反转字符串方法reverse

该字符串可将原字符串反转

```java
    StringBuffer stringBuffer = new StringBuffer("hello");
    stringBuffer.reverse();
    System.out.println(stringBuffer);
}
```

结果：olleh