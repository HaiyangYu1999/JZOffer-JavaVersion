把n个骰子扔在地上，所有骰子朝上一面的点数之和为s。输入n，打印出s的所有可能的值出现的概率。

 

你需要用一个浮点数数组返回答案，其中第 i 个元素代表这 n 个骰子所能掷出的点数集合中第 i 小的那个的概率。

 

示例 1:
```
输入: 1
输出: [0.16667,0.16667,0.16667,0.16667,0.16667,0.16667]
```
示例 2:
```
输入: 2
输出: [0.02778,0.05556,0.08333,0.11111,0.13889,0.16667,0.13889,0.11111,0.08333,0.05556,0.02778]
```

限制：
```
1 <= n <= 11
```





### 动态规划

我们只统计出现点数的可能数量, 然后再除以6^n即可

n个骰子和为s的所有可能数量有 
```
(n-1个骰子和为s-1的数量) + (n-1个骰子和为s-2的数量) +(n-1个骰子和为s-3的数量) +(n-1个骰子和为s-4的数量) +(n-1个骰子和为s-5的数量) +(n-1个骰子和为s-6的数量)
```

递归的计算就行

思路不难, 最大的难点在于边界条件的处理, 总之挺麻烦的. 

`dp[i][j]`存储i个骰子和为j的可能性. `dp[i]`的长度为6i + 1. 

```c++
class Solution {
public:
    vector<double> dicesProbability(int n) {
        vector<vector<int>> dp;
        dp.push_back(vector<int>({1,1,1,1,1,1}));
        for(int i = 1; i < n; ++i){
            dp.push_back(vector<int>(6*(i+1), 0));
            for(int j = 0; j < dp[i].size(); ++j){
                for(int s = 1; s <= 6; ++s){
                    dp[i][j] += (j-s >= 0 && j-s < dp[i-1].size()) ? dp[i-1][j-s] : 0;
                }
            }
        }
        vector<double> ans;
        for(int i = n-1; i < 6*n; ++i){
            ans.push_back(((double) dp[n-1][i]) / std::pow(6,n));
        }
        return ans;
    }
};
```

