# 单元测试(Test)

为了实现一个项目中的很多方法的独立测试，我们可以使用单元测试来完成这个功能。

首先，写@Test,然后导入org.junit.Test包。

代码：

```java
package 基础;

import org.junit.Test;

public class JunitTest {
    int a=10;
    int b=20;
    @Test//声明注解，导入包：import org.junit.Test;
    public void testEquals(){
        String s1="MM";
        String s2="MM";
        System.out.println(s1.equals(s2));//true
    }
    @Test
    public void testAdd(){
        System.out.println(a+b);//30
    }
}
```

![image-20220222132515249](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220222132515249.png)

从图上看，每个Test单元测试都有一个独立的运行按钮，有了单元测试，可以不用在main方法调用函数进行测试，方便了好多。