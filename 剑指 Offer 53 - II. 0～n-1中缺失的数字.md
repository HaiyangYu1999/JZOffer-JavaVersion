一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。

 

示例 1:
```
输入: [0,1,3]
输出: 2
```
示例 2:
```
输入: [0,1,2,3,4,5,6,7,9]
输出: 8
```

限制：

`1 <= 数组长度 <= 10000`



### 二分查找变种

**事实上这个题就等价于查找数组中第一个使得`nums[i] != i `成立的`i`**

根据这个特性, 可以进行二分查找. 设立双指针left和right, 并且计算出mid

如果`nums[mid] == mid`, 根据数组是递增的, 可以判断对于任意的`0 <= i <= mid`, 都有`nums[i] == i`, 所以要从数组右半部分找

如果`nums[mid] != mid`, 那么肯定存在一个`i`, 满足 `0 <= i <= mid`, 都有`nums[i] != i`, 所以要从数组左半部分找.

```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int left = 0;
        int right = nums.size() - 1;
        while(left <= right){
            int mid = left + (right - left) / 2;
            if(mid == nums[mid]){
                left = mid + 1;
            }
            else{
                right = mid - 1;
            }
        }
        return left;
    }
};
```

