#### [剑指 Offer 09. 用两个栈实现队列](https://leetcode.cn/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

难度简单587收藏分享切换为英文接收动态反馈

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 `appendTail` 和 `deleteHead` ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，`deleteHead` 操作返回 -1 )

 

**示例 1：**

```
输入：
["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]
```

**示例 2：**

```
输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]
```

**提示：**

- `1 <= values <= 10000`
- `最多会对 appendTail、deleteHead 进行 10000 次调用`

## 思路：双栈实现

```java
class CQueue {
    Stack<Integer> s1;
    Stack<Integer> s2;
    public CQueue() {
        //ru队列用到的栈
        s1 = new Stack<>();、
        //入队列用的栈
        s2 = new Stack<>();
    }
    //入队列
    public void appendTail(int value) {
        s1.push(value);
    }
    //出队列，出栈用完再往入栈里拿
    public int deleteHead() {
        if(s2.isEmpty()){
            if(s1.isEmpty()){
                //出栈和入栈都为空
                return -1;
            }
            //出栈为空了，再将入栈的元素给出栈
            while(!s1.isEmpty()){
                s2.push(s1.pop());
            }
        }
        return s2.pop();
    }
}

```

