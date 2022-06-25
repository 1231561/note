# List集合

List接口继承了Colletion接口，所以List集合拥有所有Colletion方法。

![image-20220120181905335](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220120181905335.png)

常用方法：

### 1、 void add(int index,E element)

在指定位置插入元素，后面的元素都往后移动一个单位。

### 2、boolean **addAll**(int index, Collection<? extends E> c)

在指定的位置中插入c集合中的所有元素，如果集合发生变化，则返回true,否则返回false。

```java
public class List {
    public static void main(String[] args) {
        ArrayList<Object> a = new ArrayList<>();
        ArrayList<Object> b = new ArrayList<>();
        ArrayList<Object> c = new ArrayList<>();
        a.add(111);
        a.add(222);
        a.add("sdasd");
        b.add(333);
        b.add(100);
        Boolean x=a.addAll(2,b);
        System.out.println(a);
        System.out.println(x);
        Boolean y=a.addAll(2,c);
        System.out.println(a);
        System.out.println(y);
    }
}
```



### 3、E get(int index)

返回list集合中指定索引位置的元素。

```java
Object obj=a.get(2);
System.out.println(obj);
```

输出：333

### 4、int indexOf(Object o)

返回list集合中第一次出现o对象的索引位置，如果list集合中没有o对象，那么就返回-1

```
System.out.println(a.indexOf(111));
System.out.println(a.indexOf(5555));
```

}

### 5、E **remove**(int index)

删除指定索引的对象 

## 6、E **set**(int index, E element)

在索引为index位置的元素更改为element元素。

## 7、List<E> **subList**(int fromIndex, int toIndex)

输入索引fromIndex到toIndex之间的所有元素，左闭右开。

```java
System.out.println(a.subList(1,4));
```

输出：[222, 333, 100]

### 8、clear（）

清除集合里面的元素，new出来的对象还在。

