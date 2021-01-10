输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

 

示例：
```
输入：nums = [1,2,3,4]
输出：[1,3,2,4] 
注：[3,1,2,4] 也是正确的答案之一。
```

提示：

`1 <= nums.length <= 50000`
`1 <= nums[i] <= 10000`

<!--more-->

## 双指针

方法很简单, 双指针就ok

```java
class Solution {
    public int[] exchange(int[] nums) {
        if(nums == null){
            throw new IllegalArgumentException();
        }
        int i = 0;
        int j = nums.length - 1;
        while(i < j){
            while(j > 0 && nums[j] % 2 == 0){
                --j;
            }
            while(i < nums.length - 1 && nums[i] % 2 == 1){
                ++i;
            }
            if(i >= j){
                break;
            }
            int tmp = nums[i];
            nums[i] = nums[j];
            nums[j] = tmp;
        }
        return nums;
    }
}
```

