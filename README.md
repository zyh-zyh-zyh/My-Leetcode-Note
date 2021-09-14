# 📝My LeetCode Note

## 🗺️路线

* 🔢数组=> ⛓️链表=> 🧾哈希表=>🔡字符串=>🎢栈与队列=>🌳树=>🔙回溯=>💯贪心=>📡动态规划=>🧩图论=>🎯高级数据结构

按题型刷完后，再从简单刷起，做了几个类型题目之后，再慢慢做中等题目、困难题目。

* 题解语言：`Java`

---

## 🔢数组

### 27.移除元素

### 26.删除排序数组中的重复项

### 283.移动零

### 844.比较含退格的字符串

可能思路： 倒序 **【TODO】**

```java
class Solution {
    public boolean backspaceCompare(String s, String t) {
        char[] charS = new char[s.length()];
        char[] charT = new char[t.length()];

        //下一位将要录入的索引值
        int countS = 0;
        int countT = 0;
    
        // 1.遍历String s，录入到char[] charS
        for(int i = 0; i < s.length(); i++){
            char value = s.charAt(i);
    
            // 1.1 非#符号：录入一位char
            if(value != '#'){
                charS[countS] = value; 
                countS++;
            }
    
            // 1.2 是#符号
            else{
                if(countS > 0){
                    // 1.2.1 charS索引还未到最左（0），即 删除前一位
                    countS--;
                }
                else{
                    // 1.2.2 charS索引已到最左（0），即 charS 已经为空 不改变
                }
                
            }
    
        }


        // 2.同理遍历String T，录入到char[] charT
        for(int i = 0; i < t.length(); i++){
            char value = t.charAt(i);
    
            // 2.1 非#符号：录入一位char
            if(value != '#'){
                charT[countT] = value; 
                countT++;
            }
    
            // 2.2 是#符号
            else{
                if(countT > 0){
                    // 2.2.1 charS索引还未到最左（0），即 删除前一位
                    countT--;
                }
                else{
                    // 2.2.2 charS索引已到最左（0），即 charS 已经为空 不改变
                }
                
            }
    
        }


        // 3. 比较charS 前(countS -1)位 与 charT 前(countT -1)位 是否相等
    
        // 两char[] 有效长度不等
        if(countS != countT){
            return false;
        }
    
        // 两 char[] 都为空
        if(countS == 0){
            return true;
        }


        int index = 0;
    
        while(index < countS){
            if(charS[index] != charT[index]){
                return false;
            }
            index++;
        }
        return true;
    }
}
```

### 977. 有序数组的平方

双指针 倒序 同向

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int lens = nums.length;
        if(lens < 0){
            return nums;
        }
        else if(lens == 0){
            int v = nums[0] * nums[0];
            nums[0] = v;
            return nums;
        }

 
        //获取第一个非负数的索引：
        int firstNonNeg = lens;
        for(int i = 0; i < lens; i++){
            if(nums[i] >= 0){
                firstNonNeg = i;
                break;
            }
            //若firstNonNeg == lens，则数组只含有负数
        }

        //数组中所有数大于等于0
        if(firstNonNeg == 0){
            for(int i = firstNonNeg; i < lens; i++){
               nums[i] = nums[i] * nums[i];
            }
            return nums;
        }

        int[] result = new int[lens];

        //数组中只有负数
        if(firstNonNeg == lens){
            for(int i = firstNonNeg - 1; i >= 0; i--){
               result[lens - 1 - i] = nums[i] * nums[i];
            }
            return result;
        }



        //以firstNonNeg为起点 双指针  反向遍历数组
        int left = firstNonNeg - 1;
        int right = firstNonNeg;

        boolean LRun = true;
        boolean RRun = true;
        int v1;
        int v2;
        int index = 0;


        while(LRun && RRun){
            v1 = nums[left] * nums[left];
            v2 = nums[right] * nums[right];

            if(v1 <= v2){
                result[index] = v1;
                index++;
                left--;

                if(left == -1){
                   LRun = false;
                   break;
                }
            }
            else{
                result[index] = v2;
                index++;
                right++;

                if(right == lens){
                    RRun = false;
                    break;
                }
            }
        }

        //左侧遍历结束，右侧还未结束，进入if
        if(!LRun){
            while(right < lens){
                result[index] = nums[right] * nums[right];
                index++;
                right++;
            }
        }
        //右侧遍历结束，左侧还未结束 进入else
        else{
            while(left >= 0){
                result[index] = nums[left] * nums[left];
                index++;
                left--;
            }
        }
        return result;
    }
}
```

### 209.长度最小的子数组

满足其和 `≥ target` 的长度最小的 **连续子数组**。

* my solution: 

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int N = nums.length;
        if(N <= 0)    return 0;

        int left = 0;
        int right = 0;

        //当前
        int sum = nums[left];

        //当前数组长度
        int account = 1;
        //满足条件的最小长度
        int minLen = N + 1;

        while(true){

            //1.该数组满足条件 则收缩左指针
            if(sum >= target){
                if(minLen > right - left + 1)   minLen = right - left + 1;
                sum -= nums[left];
                left++;
                account--;
                if((left == N) || (left > right))  break;
            }
            //2.数组之和不满足条件：
            else{
                //2.1.数组之和小于target，且目前数组长度account比满足条件的最小长度minLen小 则右移右指针
                if(account < minLen){
                    right++;
                    if(right == N) break;
                    sum += nums[right];
                    account++;
                }
                //2.2.数组之和小于target，但目前数组长度account大于等于minLen，则左右指针同时右移
                else{
                    int temp = nums[left];
                    left++;
                    right++;
                    if(right == N) break;
                    sum = sum - temp + nums[right];
                }
            }
        }

        if(minLen == N + 1){
            return 0;
        }
        else return minLen;


    }
}
```

* 官方解法

```java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return 0;
        }
        int ans = Integer.MAX_VALUE;
        int start = 0, end = 0;
        int sum = 0;
        while (end < n) {
            sum += nums[end];
            while (sum >= s) {
                ans = Math.min(ans, end - start + 1);
                sum -= nums[start];
                start++;
            }
            end++;
        }
        return ans == Integer.MAX_VALUE ? 0 : ans;
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/minimum-size-subarray-sum/solution/chang-du-zui-xiao-de-zi-shu-zu-by-leetcode-solutio/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 904.水果成篮

```java
class Solution {
    public int totalFruit(int[] fruits) {
        int len = fruits.length;
        if(len <= 0) return 0;
        if(len < 3) return len;

        int type1 = -1;
        int type2 = -1;
        int max = -1;

        for(int i = 0; i < len; i++){
            type1 = fruits[i];
            //当前计数 水果类型数
            int types = 1;
            boolean toEnd = true;

            for(int j = i + 1; j < len; j++){
                //types 只有两种取值：1 or 2;

                if(types == 1){
                    if(fruits[j] == type1){

                    }
                    else{
                        types++;
                        type2 = fruits[j];
                    }
                }
                else{
                    if(fruits[j] != type1 && fruits[j] != type2){
                        int sub = j - i;
                        max = Math.max(max, sub);
                        toEnd = false;
                        break;
                    }
                }
            }
            if(toEnd){
                int sub = len - i;
                max = Math.max(max, sub);
            }
        }

        return max;

    }
}
```

### 76. 最小覆盖子串

* 思路1：
  1. 创建并维护一个list 存储t中元素是否已被取出访问。 **【TODO】**

```java
class Solution {
    public String minWindow(String s, String t) {
        char[] charsS = s.toCharArray();
        char[] charsT = t.toCharArray();

        int start = 0;
        int end = 0;
        boolean[] visited = new boolean[charsT.length];

        //记录该字符是否起作用（如 s:abcad, t:abcd, 则滑窗滑到abcad时，s中第二个a还未起作用）
        boolean[] used = new boolean[cahrsS.length];

        //charsS索引，存储 在t中存在的字符的索引表
        int[] queue = new int[charsS.length];
        //queue的index
        int indexOfQueue = 0;

        int left = 0;
        int right = 0;

        //最小字串的左右索引
        int minStart = -1;
        int minEnd = -1;


        //s中 当前滑窗含有符合条件的t字符的数量
        int count = 0;

        //
        int temp = -1;


        //遍历s寻找 第一个 在t存在的字符 得到在s中的位置：start
        while(start < charsS.length){

            temp = isExistInRest(charsS[start], charsT, visited);
            if(temp == -1){
                start++;
            }
            else{
                //搜索到
                queue[indexOfQueue++] = start;
                used[start] = true;
                 break;
            }
        }
        minStart = start;
            

        end = start + 1;
        //遍历寻找 第一个 符合条件的子字串
        //即 未被全部visited，则继续循环
        while(!allTrue(visited)){

            if(end >= charsS.length){
                return "";
            }
            else{
                temp = isExistInRest(charsS[end], charsT, visited);
                if(temp == -1) {end++;}
                else{
                    queue[indexOfQueue++] = end; 
                    used[end] = true;
                    end++;
                }
            //allTrue(visited) == true，找到满足条件的第一个子字串
            //end >= charsS.length，遍历结束
            }
        }
        minEnd = end;
        int minLen = minEnd - minStart + 1;

        //以 上一步 找到的子字串为滑窗初始状态，开始滑动
        while(start < charsS.length && end < charsS.length){

            char delChar = charsS[start];
            start = queue[++left];

            int nextIndex = findNextChar(delChar, charsS, index, end);
            if(nextIndex == -1) { }

            

            

            

        }






    }

    //indexOf()
    public int findNextChar(char des, char[] charsS, int indexOfStart, int indexOfEnd){
        for(int i = indexOfStart; i <= indexOfEnd; i++){
            if(des == charsS[i]){
                return i;
            }
        }
        return -1;
    }

    //在 charsT 中搜索还未被visited 标记的字符des，则对visited数组进行标记，并且返回第一次出现的index
    public int isExistInRest(char des, char[] charsT, boolean[] visited){
        for(int i = 0; i < charsT.length; i++){
            if(!visited[i]){
                visited[i] = true;
                return i;
            }
        }
        return -1;
    }

    //判断boolean数组是否全为true
    public boolean allTrue(boolean[] list){
        for(int i = 0; i < list.length; i++){
            if(!list[i]){   return false;}
        }
        return true;
    }

    public boolean isExist(char des, Stirng t){

        for(int i = 0; i < charsT.length; i++){
            if(t.indexOf(des) != -1){
                return true;
            }
        }
        return false;
    }
}
```

solution2:

链表

### 59. 螺旋矩阵 II

用`top`, `bottom`, `left`, `right`分别代表四条边界，指针遍历边界内的数（不含边界），到达边界则将指针回退一位。

```java
class Solution {
    public int[][] generateMatrix(int n) {
        if(n == 1) return new int[][]{{1}};
        int num = 1;

        int[][] mat = new int[n][n];

        int left = -1;
        int right = n;
        int top = -1;
        int bottom = n;
        int i = 0;
        int j = 0;

        mat[0][0] = 1;

        while(right - left > 1 || bottom - top > 1){
            //go right =>
            for(j += 1; j < right; ++j){
                mat[i][j] = ++num;
            }
            j--;
            top++;

            //go down \/
            for(i += 1; i < bottom; ++i){
                mat[i][j] = ++num;
            }
            i--;
            right--;

            //go left <=
            for(j -= 1; j > left; --j){
                mat[i][j] = ++num;
            }
            j++;
            bottom--;

            //go up /\
            for(i -= 1; i > top; --i){
                mat[i][j] = ++num;
            }
            i++;
            left++;
        }
        return mat;
    }
}
```

---

## ⛓️链表

### 203.移除链表元素

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

### 707.设计链表

1. new一个dummyHead的方法：

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

