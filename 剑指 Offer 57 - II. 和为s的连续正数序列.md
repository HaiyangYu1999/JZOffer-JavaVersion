输入一个正整数 target ，输出所有和为 target 的连续正整数序列（至少含有两个数）。

序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。

 

示例 1：
```
输入：target = 9
输出：[[2,3,4],[4,5]]
```
示例 2：
```
输入：target = 15
输出：[[1,2,3,4,5],[4,5,6],[7,8]]
```

限制：

`1 <= target <= 10^5`


### 枚举区间长度

假设存在这样的区间, 从i开始, 区间长度为j, 即`[i,i+1,...,i+j-1]`, 和为`j*(2*i+j-1)/2`. 问题转化为指定target, 求所有的正整数对`(i,j)`满足`j*(2*i+j-1)/2 = target`. 哦对了, j还要大于等于2.

leetcode官方题解是枚举区间开始的, 即从i开始枚举, 循环地找i, 然后再计算j. 这样的话最坏的情况下i最大为`target/2`.  `target = 2*k + 1, answer = [k, k+1]`, 枚举时复杂度为O(target/2)



我们可以改进一下, 从j开始枚举, 然后再计算j, 最坏的情况下j最大为`(-1 + sqrt(1 + 8 * target)) / 2`. 因为最坏的可能性下即为区间最长的情况, 区间从1开始最长,  并且有`1 + 2 + ... + n = target`. 即`n = (-1 + sqrt(1 + 8 * target)) / 2`  枚举时复杂度为O(sqrt(target))

对于每一个枚举的j, 因为要验证的区间是从i开始, 区间长度为j, 所以和为`j*(2*i+j-1)/2 = target`, 即`i = (((2 * target) / j) + (1-j)) / 2`是不是整数即可. 如果是整数, 说明就找到了一组解.



```c++
class Solution {
public:
    vector<vector<int>> findContinuousSequence(int target) {
        vector<vector<int>> ans;
        int maxLen = (-1 + (int) std::sqrt(1 + 8 * target)) / 2;
        for(int j = 2; j <= maxLen; ++j){
            if((2 * target) % j == 0 && (((2 * target) / j + (1 - j)) % 2 == 0)){
                int i = ((2 * target) / j + (1 - j)) / 2;
                vector<int> curr;
                for(int ii = i; ii < i + j; ++ii){
                    curr.push_back(ii);
                }
                ans.push_back(curr);
            }
        }
        std::sort(ans.begin(), ans.end(), [](const vector<int>& v1, const vector<int>& v2)->bool{return v1[0] < v2[0];});
        return ans;
    }
};
```

