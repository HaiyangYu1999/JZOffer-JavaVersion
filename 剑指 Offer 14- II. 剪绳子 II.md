给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m - 1] 。请问 k[0]*k[1]*...*k[m - 1] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

 

示例 1：
```
输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1
```
示例 2:
```
输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36
```

提示：

`2 <= n <= 1000`

## 1. 快速幂求余(递归)

和上一题`剑指 Offer 14- I. 剪绳子`一样, 只不过这次要求出的数很大, 需要取模.

整道题的难点都在pow(3,m) % c上面了

先用java内置函数求出pow(3,m)再取模肯定是不行的, 那样pow(3,m)会太大了超过long的范围. 所以我们只能自己定义一个函数`long pow(int a, int b, int c)` 来计算a ^ b % c.

这里就用到了快速幂.  递归的快速幂很简单, 但是循环的快速幂想要理解并不简单. 

```java
class Solution {
    public int cuttingRope(int n) {
        if(n <= 3)
            return n - 1;
        else
        {
            long ans = 0L;
            int m = n / 3;
            int remain = n % 3;
            int c = 1000000007;
            if(remain == 0)
            {
                ans = pow(3, m, c);
            }
            else if(remain == 1)
            {
                ans = pow(3, m-1, c) * 4 % c;
            }
            else
            {
                ans = pow(3, m, c) * 2 % c;
            }
            return (int) ans;
        }
    }

    private long pow(int a, int b, int c)
    {
        if(b == 0)
            return 1;
        if(b == 1)
            return a % c;
        if((b & 1) == 0)
            return pow(a, b/2, c) * pow(a, b/2, c) % c;
        else
            return (pow(a, b/2, c) * a % c) * pow(a, b/2, c) % c;
    }
}
```

## 2. 快速幂(循环)

参考于https://zhuanlan.zhihu.com/p/95902286

通过2进制的运算, 转化指数.

例如要计算a的10次幂, 10写为2进制之后为1010

可以转化为 0个a的1次幂, 1个a的2次幂, 0个a的4次幂, 1个a的8次幂.

所以依次计算即可. 

记a的起始值为a0

从a的1次幂开始, 如果n末尾为0, 就说明没有a的1次幂, ans不变,  a变为a * a. n = n / 2.

现在n = 5, 末尾为1, a = a0^2, 说明有1个a的2次幂, **ans *= a,** 再次更新a变为a * a. n = n / 2.

现在n = 2, 末尾为0, a = a0^4, 说明没有a的4次幂, ans不变, 再次更新a变为a * a. n = n / 2.

现在n = 1, 末尾为1, a = a0^8, 说明有1个a的8次幂, **ans *= a,** 再次更新a变为a * a. n = n / 2.

现在 n = 0, 结束循环.

从上面看出, ans 与a0^2相乘过一次, 与a0^8相乘一次, 刚好为a0^10.

这种方法应该是快速幂最优解法了, 对数复杂度, 常数空间

```java
private long pow(int a0, int b, int c)
    {
        long a = a0;
        if(b < 0)
            return 0;
        long ans = 1;
        int n = b;
        while(n > 0)
        {
            if((n & 1) == 1)
            {
                ans = ans * a % c;
            }
            a = a * a % c;
            n = n >> 1;
        }
        return ans;
    }
```



