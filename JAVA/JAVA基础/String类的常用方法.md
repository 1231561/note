# String类的常用方法

## 1.length()

获取字符串的长度。

```java
String str = "adsfaxsdfas沙发上案发地方";
System.out.println(str.length());
```

运行结果：
18

## 2.charAt(int index)

获取指定索引处的字符

```java
String str = "adsfaxsdfas沙发上案发地方";
        char[] c = {'a','d','s','f','a'};
        System.out.println(str.charAt(12));
```

运行结果：

发

## 3.indexOf(String str)

获取str在字符串对象中第一次出现的索引

```java
String str = "adsfaxsdfas沙发上案发地方";
System.out.println(str.indexOf("a",5));//5代表从索引5开始，找到第一个a出现的位置
```

结果：9

## 4.substring(int start)

从索引start开始，截取后面所有的字符串，包括start。

```java
String str = "adsfaxsdfas沙发上案发地方";
        System.out.println(str.substring(1));
```

运行结果：
dsfaxsdfas沙发上发地方

## 5.String substring(int start,int end)

从start开始到end结束，截取之间的字符串。包括start，不包括end。

```
String str = "adsfaxsdfas沙发上案发地方";
        System.out.println(str.substring(1, 12));
```

运行结果：
dsfaxsdfas沙

## 6.equals(Object obj)

比较字符串的内容是否相同。

```
String str = "adsfaxsdfas沙发上案发地方";
        System.out.println(str.equals("adsfaxsdfas沙发上发地方"));
        System.out.println(str.equals("adsfaxsdfas"));
```

运行结果：
true
false

## 7.equalsIgnoreCase(String anotherString)

比较字符串的内容是否相同，忽略大小写。

```
String str = "adsfaxsdfas沙发上案发地方";
        System.out.println(str.equalsIgnoreCase("ADsfaxsdfAs沙发上发地方"));
```

运行结果：
true

## 8.判断字符串是否为空



## 9.toCharArray()

把字符串转化为字符数组

```java
public static void main(String[] args) {

        String str = "adsfaxsdfas沙发上发地方";
        char arr[] = str.toCharArray();
        printArray(arr);
        }
public static void printArray(char a[]) {

        for(int i=0;i<a.length;i++) {
        System.out.print(a[i]+"--");
        }
        }
```

运行结果：
a–d--s–f--a–x--s–d--f–a--s–沙--发–上--发–地--方–

## 10.toLowerCase()

把字符串全部转换为小写的

```
String str1 = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
        System.out.println(str1.toLowerCase());
```

运行结果：
abcdefghijklmnopqrstuvwxyz

## 11.toUpperCase()

把字符串全部转化成大写的

```java
SString str1 = "abcdefghijklmnopqrstuvwxyz";
        System.out.println(str2.toUpperCase());
```

运行结果：
ABCDEFGHIJKLMNOPQRSTUVWXYZ

## 12.split()

取出字符串指定的字符，然后返回一段一段的字符串数组。

split(separator,howmany)的第一个参数的指定的分隔符，第二的参数是指定字符串要分成几段。

```java
String a = "let1 art can";
a.split(" ", 2);
```

结果：![image-20220503161103354](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220503161103354.png)

```java
public static void main(String[] args) {

    String str = "adsfaxsdfas沙发上发地方";
    String array[] = str.split("a");
    printString(array);
}
public static void printString(String a[]) {
    for (String S:a){
        System.out.println(S);
    }
}
```

结果：

dsf
xsdf
s沙发上发地方