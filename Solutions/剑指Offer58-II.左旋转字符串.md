# 剑指Offer58-II.左旋转字符串

```java
class Solution {
    public String reverseLeftWords(String s, int n) {
        StringBuilder sb = new StringBuilder(s);
        String cut = sb.substring(0, n);
        sb.delete(0, n).append(cut);
        return sb.toString();
    }
}
```
