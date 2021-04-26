统计一个数字在排序数组中出现的次数。

 

示例 1:

输入: nums = [5,7,7,8,8,10], target = 8
输出: 2
示例 2:

输入: nums = [5,7,7,8,8,10], target = 6
输出: 0


限制：

0 <= 数组长度 <= 50000



### 二分查找

查找第一个大于等于target的下标i, 和第一个大于target的下标left,

两者相减即可

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int i = 0;
        int j = nums.size() - 1;
        if(nums.empty() || target > nums[j] || target < nums[i]){
            return 0;
        }
        while(i <= j){
            int mid = i + (j - i) / 2;
            if(nums[mid] >= target){
                j = mid - 1;
            }
            else{
                i = mid + 1;
            }
        }
        int left = 0;
        int right = nums.size() - 1;
        while(left <= right){
            int mid = left + (right - left) / 2;
            if(nums[mid] > target){
                right = mid - 1;
            }
            else{
                left = mid + 1;
            }
        }
        return (i - left) * (-1);
    }
};
```

