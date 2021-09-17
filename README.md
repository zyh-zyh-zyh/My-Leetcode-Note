# 📝My LeetCode Note

## 🗺️路线

* [🔢数组](#数-组) --> [⛓️链表](#%EF%B8%8F链-表) --> [🧾哈希表](#哈-希-表) --> 🔡字符串=>🎢栈与队列=>🌳树=>🔙回溯=>💯贪心=>📡动态规划=>🧩图论=>🎯高级数据结构

按题型刷完后，再从简单刷起，做了几个类型题目之后，再慢慢做中等题目、困难题目。

* 题解语言：`Java`

&nbsp;

---
&nbsp;

## 🔢数 组

### 27. 移除元素

* **【TODO】**

### 26. 删除排序数组中的重复项

* **【TODO】**

### 283. 移动零

* **【TODO】**

### [844. 比较含退格的字符串](./Solutions/844.比较含退格的字符串.md)

### [977. 有序数组的平方](./Solutions/977.有序数组的平方.md)

### [209. 长度最小的子数组](./Solutions/209.长度最小的子数组.md)

### [904. 水果成篮](./Solutions/904.水果成篮.md)

### [76. 最小覆盖子串](./Solutions/76.最小覆盖子串.md)

### [59. 螺旋矩阵 II](./Solutions/59.螺旋矩阵II.md)

&nbsp;

---

&nbsp;

## ⛓️链 表

### [203. 移除链表元素](./Solutions/203.移除链表元素.md)

### [707. 设计链表](./Solutions/707.设计链表.md)

### 206. 反转链表

1. Solution 1:

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
        //len = 0
        if(head == null){
            return null;
        }

        //len = 1
        if(head.next == null){
            return head;
        }

        //len = 2
        if(head.next.next == null){
            ListNode start = head.next;
            ListNode end = head;
            start.next = end;
            end.next = null;
            return start;
        }
        
        //len > 2
        //point:
        ListNode left = head;
        ListNode right = head.next;
        left.next = null;

        ListNode tmp = right.next;
        while(tmp != null){
            if(right.next == null){
                break;
            }
            tmp = right.next;
            right.next = left;

            left = right;
            right = tmp;
            tmp = tmp.next;
        }

        right.next = left;
        return right;
    }
}
```

2. Solution 2: Leetcode官方解法

```java
   class Solution {
       public ListNode reverseList(ListNode head) {
           ListNode prev = null;
           ListNode curr = head;
           while (curr != null) {
               ListNode nextTemp = curr.next;
               curr.next = prev;
               prev = curr;
               curr = nextTemp;
           }
           return prev;
       }
   }
   
   作者：LeetCode
   链接：https://leetcode-cn.com/problems/reverse-linked-list/solution/fan-zhuan-lian-biao-by-leetcode/
   来源：力扣（LeetCode）
   著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

3. Solution 3:

   [Code from 代码随想录](https://programmercarl.com/0206.%E7%BF%BB%E8%BD%AC%E9%93%BE%E8%A1%A8.html#%E9%80%92%E5%BD%92%E6%B3%95)

```java
// 双指针
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode cur = head;
        ListNode temp = null;
        while (cur != null) {
            temp = cur.next;// 保存下一个节点
            cur.next = prev;
            prev = cur;
            cur = temp;
        }
        return prev;
    }
}
```

4. Solution 4: 递归 **【TODO】**

```java

```

### 24. 两两交换链表中的节点

1. Solution 1:

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
    public ListNode swapPairs(ListNode head) {
        //len < 2
        if(head == null){return head;}
        if(head.next == null){return head;}

        //
        //定义tmp为每次两两交换中 left node 的前一个node
        ListNode tmp = new ListNode(0, head);
        ListNode start = new ListNode(0, head.next);
        ListNode left = head;
        ListNode right = head.next;

        while(true){

            left.next = right.next;
            right.next = left;
            tmp.next = right;

            tmp = left;
            left = tmp.next;
            if(left == null){break;}
            right = left.next;

            if(right == null){break;}
        }
        return start.next;

    }
}
```

2. Solution 2:

   递归 **【TODO】**

```java

```

### [19. 删除链表的倒数第 N 个结点](./Solutions/19.删除链表的倒数第N个结点.md)

### 面试题 02.07. 链表相交

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        int lenA = getLength(headA);
        int lenB = getLength(headB);

        ListNode tmpA = new ListNode();
        tmpA.next = headA;
        ListNode tmpB = new ListNode();
        tmpB.next = headB;

        //对齐两个List
        if(lenA - lenB > 0){
            for(int i = 0; i < lenA - lenB; i++){
                tmpA = tmpA.next;
            }
        }
        else if(lenA - lenB < 0){
            for(int i = 0; i < lenB - lenA; i++){
                tmpB = tmpB.next;
            }
        }

        tmpA = tmpA.next;
        tmpB = tmpB.next;

        while(tmpA != null){
            if(tmpA == tmpB){
                return tmpA;
            }
            tmpA = tmpA.next;
            tmpB = tmpB.next;
        }
        return null;
    }

    public int getLength(ListNode head){
        if(head == null)    {return 0;}
        int count = 1;
        while(head.next != null){
            head = head.next;
            count++;
        }
        return count;
    }
}
```

### [142. 环形链表II](./Solutions/142.环形链表II.md)

&nbsp;

---

&nbsp;

## 🧾哈 希 表

### 基础

* **【TODO】**

### 242. 有效的字母异位词

* **【TODO】**

### 383. 赎金信

* **【TODO】**

### 49. 字母异位词分组

* **【TODO】**

### 438. 找到字符串中所有字母异位词

* **【TODO】**

### [349. 两个数组的交集](./Solutions/350.两个数组的交集II.md)

### 202. 快乐数

* **【TODO】**

### 1. 两数之和

* **【TODO】**

### [454. 四数相加II](./Solutions/454.四数相加II.md)

### 15. 三数之和

* **【TODO】**

### 18. 四数之和

* **【TODO】**

&nbsp;

---

&nbsp;

## 🔡字 符 串

* **【TODO】**

&nbsp;

---

&nbsp;

## 🎢栈 与 队 列

* **【TODO】**

&nbsp;

---

&nbsp;

## 🌳树

* **【TODO】**

&nbsp;

---

&nbsp;

## 🔙回 溯

* **【TODO】**

&nbsp;

---

&nbsp;

## 💯贪 心

* **【TODO】**

&nbsp;

---

&nbsp;

## 📡动 态 规 划

* **【TODO】**

&nbsp;

---

&nbsp;

## 🧩图 论

* **【TODO】**

&nbsp;

---

&nbsp;

## 🎯高 级 数 据 结 构

* **【TODO】**
