# 剑指Offer 05.替换空格

* 使用 `StringBuilder`

```java
class Solution {
    public String replaceSpace(String s) {
        StringBuilder sb = new StringBuilder(s);
        int len = sb.length();

        for (int i = len - 1; i >= 0; i--) {
            if (sb.charAt(i) == ' ') {
                sb.deleteCharAt(i);
                sb.insert(i, "%20");
            }
        }
        return sb.toString();
    }
}
```
