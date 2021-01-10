数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。

 

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

 

示例 1:
```
输入: [1, 2, 3, 2, 2, 2, 5, 4, 2]
输出: 2
```

限制：
```
1 <= 数组长度 <= 50000
```

<!--more-->



## 摩尔投票

和 leetcode 169相同, 使用摩尔投票可以解决.

```java
class Solution {
    public int majorityElement(int[] nums) {
        if(nums == null){
            throw new NullPointerException();
        }
        int major = 0;
        int majorCnt = 0;
        for(int i = 0; i < nums.length; ++i){
            if(nums[i] == major){
                ++majorCnt;
            }
            else{
                if(majorCnt == 0){
                    major = nums[i];
                    majorCnt = 1;
                }
                else{
                    --majorCnt;
                }
                
            }
        }
        return major;
    }
}
```

