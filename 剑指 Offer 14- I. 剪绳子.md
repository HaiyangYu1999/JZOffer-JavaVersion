给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m-1] 。请问 k[0]*k[1]*...*k[m-1] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

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

`2 <= n <= 58`

## 1. 贪心

如果我们给定了绳子长度n, 和绳子个数m, 那么就可以在O(1)时间内算出来最大乘积.

假设绳子可以取值实数, 最大解肯定是每个绳子的长度都一样长, 这样才能最大. 很简单的原理, 如果一个绳子长为a, 一个绳子长为b, (a != b), 那么换一种剪法, 都剪成(a + b) / 2的长度, 那么新的乘积`((a + b) / 2) ^ 2`肯定大于原来的乘积`ab`. (均值不等式)

所以, 每个绳子的长度就为n/m的时候最大. 但是这里要求绳子长度为整数, 所以必定有的绳子长度是`floor(n/m)`, 有的长度为`floor(n/m) + 1`. 但是不会出现任意两条绳子的差值超过1的情况. 

所以, 应该有`n % m`条绳子长为`floor(n/m) + 1`, `m - n % m `条绳子长为`floor(n/m)`. 计算乘积即可.

最后, m由2遍历到n, 找出最大的返回即可.

```java
class Solution {
    public int cuttingRope(int n) {
        double maxProduct = 0.0;
        for(int m = 2; m <= n; ++m)
        {
            maxProduct = Math.max(maxProduct, cuttingRope(n,m));
        }
        return (int) maxProduct;
    }

    private double cuttingRope(int n, int m)
    {
        int len = n / m;
        int len_Plus1 = len + 1;
        int count_len_Plus1 = n % m;
        int count_len = m - count_len_Plus1;
        return Math.pow(len, count_len) * Math.pow(len_Plus1, count_len_Plus1);
    }
}
```

## 2. 数学推导

https://leetcode-cn.com/problems/jian-sheng-zi-lcof/solution/mian-shi-ti-14-i-jian-sheng-zi-tan-xin-si-xiang-by/

这个方法真的巧妙, 我一开始用的绳子的个数m推导的, 即求(n/m)^m的最大值, 得到m = n/e. 然后去用两个整数逼近m. 但是这个是不对的, 因为当n = 30的时候, m = 11.03. 如果用11和12逼近的话, 永远不会找到最大值是m = 10.

但是用绳子的长度x就可以, 推导出来x = e的时候有最大值. 整数逼近e为2或3, 同时, 2个3的乘积比3个2的乘积大, 就一段一段的截取3就可以了.

同时要注意特殊情况, 如果最后一段是1, 那么把上一段的3匀出来成为2 * 2, 会比3 * 1大.

```java
class Solution {
    public int cuttingRope(int n) {
        if(n <= 3)
            return n - 1;
        else
        {
            int m = n / 3;
            int remain = n % 3;
            if(remain == 0)
            {
                return (int) Math.pow(3, m);
            }
            else if(remain == 1)
            {
                return (int) Math.pow(3, m-1) * 4;
            }
            else
            {
                return (int) Math.pow(3, m) * 2;
            }
        }
    }
}
```

