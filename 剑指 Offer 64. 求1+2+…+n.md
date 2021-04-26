求 1+2+...+n ，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

 

示例 1：
```
输入: n = 3
输出: 6
```
示例 2：
```
输入: n = 9
输出: 45
```

限制：

`1 <= n <= 10000`



### 利用短路逻辑代替if

来自https://leetcode-cn.com/problems/qiu-12n-lcof/solution/mian-shi-ti-64-qiu-1-2-nluo-ji-fu-duan-lu-qing-xi-/的答案, 很棒的思路.

可以用 `a && b`来代替

```
if(a){
	b;
}
```

```java
class Solution {
    public int sumNums(int n) {
        boolean x = n > 1 && (n += sumNums(n-1)) > 1;
        return n;
    }
}
```

