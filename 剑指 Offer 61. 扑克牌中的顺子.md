从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。

 

示例 1:
```
输入: [1,2,3,4,5]
输出: True
```

示例 2:
```
输入: [0,0,1,2,5]
输出: True
```

限制：
```
数组长度为 5 

数组的数取值为 [0, 13] .
```





### 逻辑判断

首席按对nums排序

能组成顺子的条件是

+ 非0的最小值和最大值的差值不能超过4, 如果超过了, 必定不能组成. 例如`[3,9,0,0,0]`这三张大小王无论充当什么数字都不会成为顺子

+ 除了0之外不能有重复的数字. 例如`[3,3,0,0,0]` 这三张大小王无论充当什么数字都不会成为顺子. 

其他的情况都能组成顺子.

```c++
class Solution {
public:
    bool isStraight(vector<int>& nums) {
        std::sort(nums.begin(), nums.end());
        int min = INT_MAX;
        int max = nums.back();
        for(int i = 0; i < nums.size(); ++i){
            if(nums[i] != 0){
                min = std::min(nums[i],min);
            }
            if(i > 0 && nums[i] != 0 && nums[i] == nums[i-1]){
                return false;
            }
        }
        if(max - min > nums.size() - 1){
            return false;
        }
        return true;
    }
};
```

