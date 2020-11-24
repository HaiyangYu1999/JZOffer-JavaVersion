一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 n 级的台阶总共有多少种跳法。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

示例 1：
```
输入：n = 2
输出：2
```
示例 2：
```
输入：n = 7
输出：21
```
示例 3：
```
输入：n = 0
输出：1
```
提示：

`0 <= n <= 100`

## 1. 迭代

等价于Fibonacci问题, 只不过初始值成了(1, 1)而不是(0,1)

```java
class Solution {
    public int numWays(int n) {
        if(n == 0 || n == 1)
            return 1;
        int fib_i_2 = 1;
        int fib_i_1 = 1;
        for(int i = 2; i <= n; ++i)
        {
            int tmp = (fib_i_1 + fib_i_2) %1000000007;
            fib_i_2 = fib_i_1;
            fib_i_1 = tmp;
        }
        return fib_i_1;
    }
}
```

