地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

 

示例 1：
```
输入：m = 2, n = 3, k = 1
输出：3
```
示例 2：
```
输入：m = 3, n = 1, k = 0
输出：1
```
提示：

`1 <= n,m <= 100`
`0 <= k <= 20`

<!--more-->

## 1. 动态规划

`dp[i][j]`表示是否能达到`(i, j)`

所以得到关系, `dp[i][j] = (dp[i-1][j] == true || dp[i][j-1] == true) && (intToDigitSum(i) + intToDigitSum(j) <= k)`. **即能达到(i, j)的条件为至少能到(i-1, j)或(i, j-1), 并且i和j的数位和小于等于k**

这里有个问题, 就是**为什么只判断能到(i-1, j)或(i, j-1)就可以了?** 

事实上, 这个假设确实是对的, 也就是说, 不会出现一种情况, `(i-1, j)`和`(i, j-1)`不可达, 但`(i,j)`可达. 具体证明我还不会, 但是画个图慢慢理解还是能理解出来的.

我感觉这个题BFS或DFS更适合. 不会出现上面的状态方程正确性证明证不出来的问题, 直接模拟就完事了. 而且复杂度还一样.

```java
class Solution {
    public int movingCount(int m, int n, int k) {
        boolean[][] dp = new boolean[m][n];
        dp[0][0] = true;
        for(int i = 1; i < n; ++i)
        {
            dp[0][i] = (dp[0][i-1] == true) && (intToDigitSum(i) <= k);
        }
        for(int i = 1; i < m; ++i)
        {
            dp[i][0] = (dp[i-1][0] == true) && (intToDigitSum(i) <= k);
        }

        for(int i = 1; i < m; ++i)
        {
            for(int j = 1; j < n; ++j)
            {
                dp[i][j] = (dp[i-1][j] == true || dp[i][j-1] == true) && (intToDigitSum(i) + intToDigitSum(j) <= k);
            }
        }
        int cnt = 0;
        for(boolean[] i : dp)
        {
            for(boolean j : i)
            {
                if(j)
                    ++cnt;
            }
        }
        return cnt;
        
    }
    private int intToDigitSum(int n)
    {
        int ans = 0;
        while(n > 0)
        {
            ans += n % 10;
            n /= 10;
        }
        return ans;
    }
}
```

## 2. BFS

```java
class Solution {
    public int movingCount(int m, int n, int k) {
        boolean[][] dp = new boolean[m][n];
        Queue<Pair<Integer, Integer>> q = new ArrayDeque<>();
        int[] di = new int[]{0,0,1,-1};
        int[] dj = new int[]{1,-1,0,0};
        int cnt = 0;
        dp[0][0] = true;
        q.offer(new Pair<Integer,Integer>(0,0));
        ++cnt;
        while(!q.isEmpty())
        {
            Pair<Integer, Integer> curr = q.poll();
            int i = curr.getKey();
            int j = curr.getValue();
            for(int s = 0; s < di.length; ++s)
            {
                int newI = i + di[s];
                int newJ = j + dj[s];
                if(newI >=0 && newI < m && newJ >= 0 && newJ < n && !dp[newI][newJ] && intToDigitSum(newI) + intToDigitSum(newJ) <= k)
                {
                    dp[newI][newJ] = true;
                    q.offer(new Pair<Integer, Integer>(newI, newJ));
                    ++cnt;
                }
            }
        }
        return cnt;
        
    }
    private int intToDigitSum(int n)
    {
        int ans = 0;
        while(n > 0)
        {
            ans += n % 10;
            n /= 10;
        }
        return ans;
    }
}
```

