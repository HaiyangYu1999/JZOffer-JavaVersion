实现函数double Power(double base, int exponent)，求base的exponent次方。不得使用库函数，同时不需要考虑大数问题。

 

示例 1:
```
输入: 2.00000, 10
输出: 1024.00000
```
示例 2:
```
输入: 2.10000, 3
输出: 9.26100
```
示例 3:
```
输入: 2.00000, -2
输出: 0.25000
解释: 2-2 = 1/22 = 1/4 = 0.25
```

说明:

+ -100.0 < x < 100.0
+ n 是 32 位有符号整数，其数值范围是 [−231, 231 − 1] 。

## 1. 快速幂

题本身不难, 就是要注意先判断边界条件, 这个题边界条件非常多!

首先要判断x是不是等于0. 而不是先判断n是不是等于0. (如果你先判断n, n为负, 返回1/x的-n次方. 如果这时候x为0就gg了.)

x如果等于0, 对于大于0的n, 小于0的n, 等于0的n都有处理方法.

这里还要注意判断x为0的时候是浮点数直接用`==`来判断还是要指定eps. 如果是面试可以问面试官怎么处理.

处理完x等于0之后, 再处理n小于等于0了

等于0, 小于0都好说, **但是不要忘了这有个坑`n = -2147483648 = INT_MIN`. 如果直接判断n<0就转化成1/x的-n次方那肯定这里会出错!!!**

```java
class Solution {
    public double myPow(double x, int n) {
        if(x == 0.0)
        {
            if(n == 0){
                throw new ArithmeticException("Undefined behavior");
            }
            else if(n < 0){
                return Double.POSITIVE_INFINITY;
            }
            else{
                return 0.0;
            }
        }
        if(n == 0)
            return 1.0;
        if(n < 0 && n != Integer.MIN_VALUE)
            return myPow(1/x, -n);
        if(n == Integer.MIN_VALUE)
        {
            return myPow(x, Integer.MIN_VALUE + 1) * myPow(x, -1);
        }
        double ans = 1.0;
        while(n > 0)
        {
            if((n&1) == 1)
                ans *= x;
            x *= x;
            n >>= 1;
        }
        return ans;
        
    }
}
```

