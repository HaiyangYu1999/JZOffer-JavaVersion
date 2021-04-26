我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。

 

示例:
```
输入: n = 10
输出: 12
解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
```
说明:  
```
1 是丑数。
n 不超过1690。
```





### 动态规划

以前没做过这种题还真是不好想, 还是要多做多积累啊

https://leetcode-cn.com/problems/chou-shu-lcof/solution/chou-shu-by-leetcode-solution-0e5i/

假如我们存放一个数组, 里面都是排好序的丑数, 我们有前n-1个丑数, 如何来求第n个丑数呢?

因为丑数只能是2,3,5的乘积, 所以第n个丑数一定是前面n-1个丑数乘2, 乘3, 乘5得到的

所以把前面的n-1个丑数都乘2, 乘3,乘5得到的所有数中找出比第n-1个丑数大的数中的最小值来即为第n个丑数

但是这样做计算量太大

因为前n-1个数已经排好序了, 我们可以利用这个信息来降低计算量. 假设数组dp[n]中按顺序存放n个丑数

肯定存在一个下标t2, 使得dp[t2] * 2 > 第n-1个丑数, 而对于小于t2的i, dp[i] * 2 < 第n-1个丑数. 这样, 我们就只需要考虑dp[t2]*2了. 因为大于t2的下标肯定更大, 不可能是大于第n-1个丑数的第一个数, 小于t2的下标肯定小于第n-1个丑数.

同理可以找到下标t3, t5

这三个下标对应的数

```
int m2 = dp[t2] * 2;
int m3 = dp[t3] * 3;
int m5 = dp[t5] * 5;
```

m2, m3, m5的最小者即为第n个丑数

```java
class Solution {
    public int nthUglyNumber(int n) {
        if(n == 1){
            return 1;
        }
        int[] dp = new int[n];
        dp[0] = 1;
        int t2 = 0;
        int t3 = 0;
        int t5 = 0;
        for(int i = 1; i < n; ++i){
            int m2 = dp[t2] * 2;
            int m3 = dp[t3] * 3;
            int m5 = dp[t5] * 5;
            int min = Math.min(Math.min(m2,m3),m5);
            if(min == m2){
                ++t2;
                dp[i] = m2;
            }
            if(min == m3){       // 因为m2和m3可能相同, 所以这里不能用else if
                ++t3;
                dp[i] = m3;
            }
            if(min == m5){
                ++t5;
                dp[i] = m5;
            }
        }
        return dp[n-1];
    }
}
```



