请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

 

示例 1：

```
输入：s = "We are happy."
输出："We%20are%20happy."
```


限制：

`0 <= s 的长度 <= 10000`

<!--more-->

## 1. 双指针

和leetcode 26的双指针解法差不多.

假设字符串后面的空间充足, 可以从后向前的构造, i指向原来字符串的最后一个元素, k指向新字符串的最后一个元素.

每当碰到一个空格时,就在k上插入"%20", 没有碰到空格时, 就插入s[i]. 同时更新i和k.



```java
class Solution {
    public String replaceSpace(String s) {
        if(s == null || s.length() == 0)
            return s;
        int spaceCount = 0;
        for(int i = 0; i < s.length(); ++i)
        {
            if(s.charAt(i) == ' ')
                ++spaceCount;
        }

        char[] chars = new char[s.length() + 2 * spaceCount];
        int k = chars.length - 1;
        for(int i = s.length() - 1; i > -1; --i)
        {
            if(s.charAt(i) == ' ')
            {
                chars[k--] = '0';
                chars[k--] = '2';
                chars[k--] = '%';
            }
            else
            {
                chars[k--] = s.charAt(i);
            }
        }
        return new String(chars);
    }
}
```

