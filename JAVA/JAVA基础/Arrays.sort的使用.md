# Arrays.sort的使用

Arrays.sort可以实现数组的排序。

Arrays.sort默认使用Comparable接口实现自然排序，即升序排序。

String、Integer，Float、Double等实现了Comparable接口。

String实现了Comparable接口的compareTo方法：字符串1.compareTo(字符串2)，返回字符串1首字符的ascii-字符串2首字符的ascii。

#### 使用默认的Comparable接口

实例：

​		因为String实现了Comparable接口的compareTo方法，按照首字符的Ascii码进行比较，Arrays.sort按照Comparable接口的自然排序规则对字符串数组进行升序排序。

```java
public static void main(String[] args) {
    String[] a = new String[]{"jkl", "abc", "xyz"};
    Arrays.sort(a);
    for(String s:a){
        System.out.println(s);
    }
}
```

#### 使用自定义的Comparator接口

可以实现自定义Comparator接口，重写compare方法，定义自己的排序规则。

#### 格式

```java
//a表示要排序的数组,泛型T规定了数组a的类型。
Arrays.sort(a, new Comparator<T>() {
    @Override//重写compare方法
    //数组中，参数o1在参数o2的后面位置。
    public int compare(T o1, T o2) {
        //自定义排序规则
    }
});
```

​	compare方法默认定义了升序规则，即比较参数o1和参数o2的大小，o1是o2后面的元素，如果o1小于o2，返回负数，然后进行交换。如果o1大于o2，返回正数，不进行交换。

我们可以重写compare方法，实现降序排序。

如下，在比较方法前加上-，-o1.compareTo(o2)。如果o1小于o2，返回正数，不进行交换。如果o1大于o2，返回负数数，进行交换。

**Arrays.sort是根据compare方法返回的结果是正数还是负数进行排序的，如果返回的是负数则交换两个参数，如果返回的是正数，则不交换。**

#### 实例1

```java
public static void main(String[] args) {
    String[] a = new String[]{"jkl", "abc", "xyz"};
    Arrays.sort(a, new Comparator<String>() {
        @Override
        public int compare(String o1, String o2) {
            return -o1.compareTo(o2);
        }
    });
    for(String s:a){
        System.out.println(s);
    }
}
结果:xyz jkl abc
```

#### 实例2

```java
    public static void main(String[] args) {
        Integer[] a = new Integer[]{3, 2, 5,8,1};
        Arrays.sort(a, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                //写法1：
//                if(o1<o2){//如果后面的数比前面的小，不交换
//                    return 1;
//                }
//                if (o1>o2){//如果后面的数比前面的大，交换
//                    return  -1;
//                }
//                return 0;
                //写法2：
                return -Integer.compare(o1,o2);//o1大于o2返回-1，o1小于o2返回1
            }
        });
        for(Integer s:a){
            System.out.println(s);
        }
    }
结果：
8
5
3
2
1

```

### 使用自定义的compare()方法实现排序

我们可以使用自定义的compare()，来对数字进行排序。

**格式： Arrays.sort(数组名,(a,b)->自定义compare(a,b))**



#### 以下是自定义的compare()方法的实例：

#### [937. 重新排列日志文件](https://leetcode-cn.com/problems/reorder-data-in-log-files/)

难度简单154

给你一个日志数组 `logs`。每条日志都是以空格分隔的字串，其第一个字为字母与数字混合的 **标识符** 。

有两种不同类型的日志：

- **字母日志**：除标识符之外，所有字均由小写字母组成
- **数字日志**：除标识符之外，所有字均由数字组成

请按下述规则将日志重新排序：

- 所有 **字母日志** 都排在 **数字日志** 之前。
- **字母日志** 在内容不同时，忽略标识符后，按内容字母顺序排序；在内容相同时，按标识符排序。
- **数字日志** 应该保留原来的相对顺序。

返回日志的最终顺序。

 

**示例 1：**

```
输入：logs = ["dig1 8 1 5 1","let1 art can","dig2 3 6","let2 own kit dig","let3 art zero"]
输出：["let1 art can","let3 art zero","let2 own kit dig","dig1 8 1 5 1","dig2 3 6"]
解释：
字母日志的内容都不同，所以顺序为 "art can", "art zero", "own kit dig" 。
数字日志保留原来的相对顺序 "dig1 8 1 5 1", "dig2 3 6" 。
```

**示例 2：**

```
输入：logs = ["a1 9 2 3 1","g1 act car","zo4 4 7","ab1 off key dog","a8 act zoo"]
输出：["g1 act car","a8 act zoo","ab1 off key dog","a1 9 2 3 1","zo4 4 7"]
```

 

**提示：**

- `1 <= logs.length <= 100`
- `3 <= logs[i].length <= 100`
- `logs[i]` 中，字与字之间都用 **单个** 空格分隔
- 题目数据保证 `logs[i]` 都有一个标识符，并且在标识符之后至少存在一个字

通过次数27,074

提交次数43,194

#### 思路：自定义排序Arrays.sort()

```java
class Solution {
    public String[] reorderLogFiles(String[] logs) {
        Pairs [] p = new Pairs[logs.length];
        for(int i=0;i<logs.length;i++){
            p[i] = new Pairs(logs[i],i);//初始化日志类
        }//a是b的后一个元素
        Arrays.sort(p,(a,b)->compareLog(a,b));//使用自定义排序，对日志类数组进行排序
        String [] temp = new String[logs.length];
        for(int i=0;i<logs.length;i++){
            temp[i] = p[i].log;//将排序好的日志类数组的日志保存到新的字符串数组
        }
        return temp;
    }
    //自定义排序, pairs1是paris2的后一个元素
    public int compareLog(Pairs pairs1,Pairs paris2){
        int index1 = pairs1.index;//日志索引
        int index2 = paris2.index;
        String log1 = pairs1.log;//日志字符串
        String log2 = paris2.log;
        String s1[] = log1.split(" ",2);//将日志字符串分为标识符和日志内容(除了标识符部分)
        String s2[] = log2.split(" ",2);
        boolean b1 = Character.isDigit(s1[1].charAt(0));//日志内容的第一个字符是否为数字
        boolean b2 = Character.isDigit(s2[1].charAt(0));
        if(!b1&&!b2){//如果前后两个日志都是字母日志
            if(s1[1].compareTo(s2[1])!=0){//而且这两个日志不相同
                return s1[1].compareTo(s2[1]);//按照字符串的字母大小进行升序排序
            }
            return s1[0].compareTo(s2[0]);//否则按照日志标识符字符串进行升序排序
        }
        if(b1&&b2){//如果两个前后两个日志都是数字日志,则保持原来的字母日志顺序
            return index1-index2;
        }
        return  b1 ? 1 : -1;//如果前面的日志是字母日志，后面的是数字日志，不交换；否则交换
    }
}
//日志类，保存日志字符串log以及日志字符串在logs的索引index
class Pairs{
    public String log;
    public int index;
    Pairs(String log,int index){
        this.log = log;
        this.index = index;
    }
}
```



