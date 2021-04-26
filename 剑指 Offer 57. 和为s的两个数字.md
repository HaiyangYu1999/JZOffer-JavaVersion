输入一个递增排序的数组和一个数字s，在数组中查找两个数，使得它们的和正好是s。如果有多对数字的和等于s，则输出任意一对即可。

 

示例 1：
```
输入：nums = [2,7,11,15], target = 9
输出：[2,7] 或者 [7,2]
```
示例 2：
```
输入：nums = [10,26,30,31,47,60], target = 40
输出：[10,30] 或者 [30,10]
```
限制：
```
1 <= nums.length <= 10^5
1 <= nums[i] <= 10^6
```





### 双指针

因为是排序的, 所以直接双指针, 去重都不用的

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        if(nums.size() < 2){
            return vector<int>();
        }
        int i = 0;
        int j = nums.size() - 1;
        while(i < j){
            int tmp = nums[i] + nums[j];
            if(tmp == target){
                return vector<int>({nums[i],nums[j]});
            }
            else if(tmp > target){
                --j;
            }
            else{
                ++i;
            }
        }
        return vector<int>();
    }
};
```

