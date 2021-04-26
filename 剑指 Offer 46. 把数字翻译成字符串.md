给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

 

示例 1:
```
输入: 12258
输出: 5
解释: 12258有5种不同的翻译，分别是"bccfi", "bwfi", "bczi", "mcfi"和"mzi"
```

提示：
```
0 <= num < 231
```





### 动态规划

这个动态规划应该很显然.

令dp[i]表示num的0到i-1位组成的字串可以翻译的所有可能数.

那么对于第i+1位字符, 可以自己单独翻译, 也可以考虑和第i位的字符合起来组成一个10到25之间的整数进行翻译.

所以, `dp[i] = dp[i-1] + ((twoDigitNum >= 10 && twoDigitNum < 26) ? dp[i-2] : 0);`



````c++
class Solution {
public:
    int translateNum(int num) {
        string s = std::to_string(num);
        int dp[s.size() + 1];
        dp[0] = 1;
        dp[1] = 1;
        for(int i = 2; i < s.size() + 1; ++i){
            char twoDigit[] = {s[i-2],s[i-1],'\0'};
            int twoDigitNum = std::stoi(twoDigit);
            dp[i] = dp[i-1] + ((twoDigitNum >= 10 && twoDigitNum < 26) ? dp[i-2] : 0);
        }
        return dp[s.size()];
    }
};
````

