输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。

要求时间复杂度为O(n)。

 

示例1:
```
输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

提示：
```
1 <= arr.length <= 10^5
-100 <= arr[i] <= 100
```



<!--more-->



### 动态规划

很经典的一道dp题. 估计大家接触的第一道dp题就是这个吧

这题如果数组全是负数的话, 最大子数组和不能返回0, 而必须是max(nums). 

很奇怪, 为什么长度为0的子数组不能算子数组呢?🙄🙄

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int maxSubSum = 0;
        int dp_i_1 = 0;
        for(int i = 0; i < nums.length; ++i){
            dp_i_1 = Math.max(dp_i_1 + nums[i], 0);
            maxSubSum = Math.max(dp_i_1, maxSubSum);
        }
        if(maxSubSum == 0){
            int max = Integer.MIN_VALUE;
            for(int i : nums){
                max = Math.max(max, i);
            }
            maxSubSum = max;
        }
        return maxSubSum;
    }
}
```

