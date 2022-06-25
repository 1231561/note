#### [剑指 Offer II 024. 反转链表](https://leetcode-cn.com/problems/UHnkqh/)

难度简单56收藏分享切换为英文接收动态反馈

给定单链表的头节点 `head` ，请反转链表，并返回反转后的链表的头节点。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

```
输入：head = [1,2]
输出：[2,1]
```

**示例 3：**

```
输入：head = []
输出：[]
```

 

**提示：**

- 链表中节点的数目范围是 `[0, 5000]`
- `-5000 <= Node.val <= 5000`

 

**进阶：**链表可以选用迭代或递归方式完成反转。你能否用两种方法解决这道题？

## 思路

将每个节点的next指向该节点的前节点。

步骤：

1、设置三个节点，一个保存当前节点（curr），一个保存当前节点的前一个节点（pre），一个保存当前节点的下一个节点（temp）。

2、将当前节点引用head节点，pre节点指向null（头节点的前一个节点null），temp引用当前节点的下一个节点。

3、节点移动

1、将curr的下一个节点（curr.next）指向前一个节点pre（初始为null）。

2、将pre节点向前走一步，走到当前节点位置curr。

3、将当前节点向前走一步，当前节点引用下一个节点temp（temp的作用就是能让curr能跳跃到下一个节点，因为curr到下一个节点的连接已断开）。（不能让curr=curr.next，因为curr.next此时指向前一个节点，已不是指向下一个节点了）

**代码：**

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode curr = head;//当前节点的初始位置为head节点的位置。
        ListNode pre = null;//设置当前节点的前一个节点为null
        while(curr!=null){
            ListNode temp = curr.next;//temp保存当前节点的下一个节点的地址。
            curr.next=pre;//将当前节点的next指向前一个节点，实现反转
            pre=curr;//当前节点的前一个节点移动一步，走到当前节点
            curr=temp;//当前节点向前移动一步,走到下一个节点。
        }
        return pre;//将所有的节点反转后，返回反转链表的新头节点pre
    }
}
```