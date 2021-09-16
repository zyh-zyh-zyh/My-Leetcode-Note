# 📝My LeetCode Note

## 🗺️路线

* [🔢数组](#数-组) --> [⛓️链表](#%EF%B8%8F链-表) --> [🧾哈希表](#哈-希-表) --> 🔡字符串=>🎢栈与队列=>🌳树=>🔙回溯=>💯贪心=>📡动态规划=>🧩图论=>🎯高级数据结构

按题型刷完后，再从简单刷起，做了几个类型题目之后，再慢慢做中等题目、困难题目。

* 题解语言：`Java`

---

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

---

## ⛓️链 表

### 203. 移除链表元素

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
    public ListNode removeElements(ListNode head, int val) {
        ListNode start = new ListNode(0, head);
        ListNode result = new ListNode(0, start);
        while(start.next != null){
            if(start.next.val == val){
                start.next = start.next.next;
            }
            else if(start.next != null){
                start = start.next;
            }
            else{
                break;
            }
        }

        return result.next.next;
    }
}
```

### 707. 设计链表

1. new 一个 dummyHead 的方法：

   遇到的特殊情况：

    1) 使用`addAtTail`添加到空表的表尾。

    ```java
    ["MyLinkedList","addAtTail","get"]
    [[],[1],[0]]
    ```

    2) 使用`addAtIndex`添加到表头。

   ```java
   ["MyLinkedList","addAtIndex","addAtIndex","addAtIndex","get"]
   [[],[0,10],[0,20],[1,30],[0]]
   ```

   3) 使用`deleteAtIndex`删除链表首位。

   ```java
   ["MyLinkedList","addAtHead","deleteAtIndex"]
   [[],[1],[0]]
   ```

```java
class MyLinkedList {
    Node head;

    /** Initialize your data structure here.
     Single linked list */
    public MyLinkedList() {
        //dummy head
        head = new Node(-1);
    }

    /** Get the value of the index-th node in the linked list. If the index is invalid, return -1. */
    public int get(int index) {
        Node tmpNode = new Node(-1, head.next);
        for(int i = -1; i < index; i++){
            if(tmpNode.next != null){
                tmpNode = tmpNode.next;
            }
            else{
                return -1;
            }
        }
        return tmpNode.val;

    }

    /** Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. */
    public void addAtHead(int val) {
        Node newNode = new Node(val, head.next);
        head.next = newNode;
    }

    /** Append a node of value val to the last element of the linked list. */
    public void addAtTail(int val) {
        Node tmp = new Node(-1, head.next);

        if(head.next == null){
            this.addAtHead(val);
            return;
        }
        while(tmp.next != null){
            tmp = tmp.next;
        }
        tmp.next = new Node(val);
    }

    /** Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. */
    public void addAtIndex(int index, int val) {
        int len = this.getLength();
        //index < 0
        if(index < 0){
            this.addAtHead(val);
        }
        else if(index == 0){
            head.next = new Node(val, head.next);
            }

        //index < length
        else if(index < len){
            Node tmp = new Node(-1, head.next);

            for(int i = 0; i < index; i++){
                tmp = tmp.next;
            }
            tmp.next = new Node(val,tmp.next);

        }

        //index == length
        else if(index == len){
            this.addAtTail(val);
        }

        //index > length



    }

    /** Delete the index-th node in the linked list, if the index is valid. */
    public void deleteAtIndex(int index) {
        if(index < 0){return;}
        int len = this.getLength();
        if(index >= len){return;}

        if(index == 0){
            head.next = head.next.next;
            return;
            }

        Node tmp = new Node(-1, head.next);
        for(int i = 0; i < index; i++){
            tmp = tmp.next;
        }
        tmp.next = tmp.next.next;
    }

    public int getLength(){
        Node tmp = new Node(-1, head.next);
        int count = 0;
        while(tmp.next != null){
            count++;
            tmp = tmp.next;
        }
        return count;
    }
}

class Node{
    int val;
    Node next;

    Node(){}

    Node(int val){this.val = val;}

    Node(int val, Node next){
        this.val = val;
        this.next = next;
    }
}

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList obj = new MyLinkedList();
 * int param_1 = obj.get(index);
 * obj.addAtHead(val);
 * obj.addAtTail(val);
 * obj.addAtIndex(index,val);
 * obj.deleteAtIndex(index);
 */
```

2. Solution 2:

    [代码原文出处](https://programmercarl.com/0707.%E8%AE%BE%E8%AE%A1%E9%93%BE%E8%A1%A8.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

```java
//单链表
class ListNode {
int val;
ListNode next;
ListNode(){}
ListNode(int val) {
this.val=val;
}
}
class MyLinkedList {
    //size存储链表元素的个数
    int size;
    //虚拟头结点
    ListNode head;

    //初始化链表
    public MyLinkedList() {
        size = 0;
        head = new ListNode(0);
    }

    //获取第index个节点的数值
    public int get(int index) {
        //如果index非法，返回-1
        if (index < 0 || index >= size) {
            return -1;
        }
        ListNode currentNode = head;
        //包含一个虚拟头节点，所以查找第 index+1 个节点
        for (int i = 0; i <= index; i++) {
            currentNode = currentNode.next;
        }
        return currentNode.val;
    }

    //在链表最前面插入一个节点
    public void addAtHead(int val) {
        addAtIndex(0, val);
    }

    //在链表的最后插入一个节点
    public void addAtTail(int val) {
        addAtIndex(size, val);
    }

    // 在第 index 个节点之前插入一个新节点，例如index为0，那么新插入的节点为链表的新头节点。
    // 如果 index 等于链表的长度，则说明是新插入的节点为链表的尾结点
    // 如果 index 大于链表的长度，则返回空
    public void addAtIndex(int index, int val) {
        if (index > size) {
            return;
        }
        if (index < 0) {
            index = 0;
        }
        size++;
        //找到要插入节点的前驱
        ListNode pred = head;
        for (int i = 0; i < index; i++) {
            pred = pred.next;
        }
        ListNode toAdd = new ListNode(val);
        toAdd.next = pred.next;
        pred.next = toAdd;
    }

    //删除第index个节点
    public void deleteAtIndex(int index) {
        if (index < 0 || index >= size) {
            return;
        }
        size--;
        ListNode pred = head;
        for (int i = 0; i < index; i++) {
            pred = pred.next;
        }
        pred.next = pred.next.next;
    }
}

//双链表
class MyLinkedList {
    class ListNode {
        int val;
        ListNode next,prev;
        ListNode(int x) {val = x;}
    }

    int size;
    ListNode head,tail;//Sentinel node

    /** Initialize your data structure here. */
    public MyLinkedList() {
        size = 0;
        head = new ListNode(0);
        tail = new ListNode(0);
        head.next = tail;
        tail.prev = head;
    }
    
    /** Get the value of the index-th node in the linked list. If the index is invalid, return -1. */
    public int get(int index) {
        if(index < 0 || index >= size){return -1;}
        ListNode cur = head;

        // 通过判断 index < (size - 1) / 2 来决定是从头结点还是尾节点遍历，提高效率
        if(index < (size - 1) / 2){
            for(int i = 0; i <= index; i++){
                cur = cur.next;
            }            
        }else{
            cur = tail;
            for(int i = 0; i <= size - index - 1; i++){
                cur = cur.prev;
            }
        }
        return cur.val;
    }
    
    /** Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. */
    public void addAtHead(int val) {
        ListNode cur = head;
        ListNode newNode = new ListNode(val);
        newNode.next = cur.next;
        cur.next.prev = newNode;
        cur.next = newNode;
        newNode.prev = cur;
        size++;
    }
    
    /** Append a node of value val to the last element of the linked list. */
    public void addAtTail(int val) {
        ListNode cur = tail;
        ListNode newNode = new ListNode(val);
        newNode.next = tail;
        newNode.prev = cur.prev;
        cur.prev.next = newNode;
        cur.prev = newNode;
        size++;
    }
    
    /** Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. */
    public void addAtIndex(int index, int val) {
        if(index > size){return;}
        if(index < 0){index = 0;}
        ListNode cur = head;
        for(int i = 0; i < index; i++){
            cur = cur.next;
        }
        ListNode newNode = new ListNode(val);
        newNode.next = cur.next;
        cur.next.prev = newNode;
        newNode.prev = cur;
        cur.next = newNode;
        size++;
    }
    
    /** Delete the index-th node in the linked list, if the index is valid. */
    public void deleteAtIndex(int index) {
        if(index >= size || index < 0){return;}
        ListNode cur = head;
        for(int i = 0; i < index; i++){
            cur = cur.next;
        }
        cur.next.next.prev = cur;
        cur.next = cur.next.next;
        size--;
    }
}

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList obj = new MyLinkedList();
 * int param_1 = obj.get(index);
 * obj.addAtHead(val);
 * obj.addAtTail(val);
 * obj.addAtIndex(index,val);
 * obj.deleteAtIndex(index);
 */
```

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

### 19. 删除链表的倒数第 N 个结点

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

**进阶：** 你能尝试使用一趟扫描实现吗？

1. Solution 1:

   满足**进阶**单次遍历要求。

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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode[] nodeArray = new ListNode[30];
        //dummyHead 节点
        ListNode start = new ListNode(-1, head);

        //计数 链表长度count
        int count = 0;
        //指针节点 tmp
        ListNode tmp = new ListNode(-1, head);

        //写入数组
        while(tmp.next != null){
            nodeArray[count++] = tmp.next;
            tmp = tmp.next;
        }

        int index = count - n;
        if(index == 0){
            start.next = nodeArray[0].next;
        }
        else if(index < 29){
            nodeArray[index - 1].next = nodeArray[index + 1];
        }

        //index == 29
        else{
            nodeArray[index - 1].next = null;
        }
        return start.next;
    }
}
```

2. 双指针 **【TODO】**

```java

```

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

### 142. 环形链表II

* **【TODO】** 递归:

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {

        if(head == null){return null;}
        if(head.next == head){return head;}
        else if(head.next == null){return null;}

        ListNode target = new ListNode();
        target.next = head;

        while(target != null){
            ListNode tmp = target;
            while(true){
                //下一个节点
                ListNode result = detectCycle(tmp.next);
                if(result != null){
                    return result;
                }
                //链表结束
                if(tmp.next == null){
                    return null;
                }
                //链表循环
                else if(tmp.next == target){
                    return tmp.next;
                }
                else{
                    tmp = tmp.next;
                }
            }
        }
    }
}
```

* **【TODO】** 二分法

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {

        if(head == null){return null;}
        else if(head.next == head){return head;}
        else if(head.next == null){return null;}
        else if(head.next.next == null){return null;}

        // ListNode left = head;
        ListNode tmp = head;
        ListNode target = head;
        int maxOfLens = 10;
        int lensOfCycle = 0;
        
        //初始化mid
        for(int i = 0; i < maxOfLens; i++){
            if(tmp.next == null){
                return null;
            }
            else{
                tmp = tmp.next;
            }
        }
        target = tmp;
        //计算是否成环，
        for(int i = 0; i < maxOfLens; i++){
            if(tmp.next == null){
                return null;
            }
            else if(tmp.next = target){
                lensOfCycle = i;
                break;
            }
        }

        

        while(){
            for(int i = 0; i < 10000; i++){
                if(tmp.next == null){
                    return null;
                }
                //
                else if(tmp.next == mid){
                }

            }

        }

    
    }

    public boolean

}
```

---

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

### 454. 四数相加II

* **【TODO】**

### 15. 三数之和

* **【TODO】**

### 18. 四数之和

* **【TODO】**

---

## 🔡字 符 串

* **【TODO】**

---

## 🎢栈 与 队 列

* **【TODO】**

---

## 🌳树

* **【TODO】**

---

## 🔙回 溯

* **【TODO】**

---

## 💯贪 心

* **【TODO】**

---

## 📡动 态 规 划

* **【TODO】**

---

## 🧩图 论

* **【TODO】**

---

## 🎯高 级 数 据 结 构

* **【TODO】**
