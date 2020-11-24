找出数组中重复的数字。


在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

示例 1：

输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 


限制：

2 <= n <= 100000

<!--more-->

## 1 原地哈希

因为0到n-1这些值刚好对应数组下标. 所以每次碰到数组中一个数nums[i], 就在下标为nums[i]的元素上加n. 如果发现一个数两次被加到n, 说明nums[i]出现了2次, 返回即可. 取负数也可以但是里面有0不好处理.

缺点是如果数组太大, `2*n-1 > INT_MAX`, 这个方法就不太好用了.

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        final int n = nums.length;
        for(int i = 0; i != n; ++i)
        {
            int tmp = nums[i] % n;
            if(nums[tmp] >= n)
            {
                return tmp;
            }
            else
            {
                nums[tmp] += n;
            }
        }
        return -1;
    }
}
```

## 2 数组重排

我们重新安排顺序. 试图将每个元素m放置到位置m上. **如果有两个元素都试图放置到同一个位置, 说明这两个元素肯定是相同的.**

具体安排如下, 我们对于第i个位置的元素nums[i], 检查i是否等于nums[i]

+ 如果`i == nums[i]`, 就说明元素nums[i]已经被置于正确的位置上了, 直接i++判断下一个即可
+ 如果`i != nums[i]`, 就说明需要调整元素位置, 把nums[i]放到正确的位置上. 这时要判断对于`j = nums[i]`, **我们检查`nums[i]`和`nums[j]`的元素是否相同**
  + 如果相同, 即`nums[i] == nums[j]`, 说明我们找到了数组中两个相同的元素, 他们对应的下标分别是i和j. 返回nums[i]即可
+ 如果不相同即`nums[i] != nums[j]`, 就swap(nums[i], nums[j]), 即把元素nums[i]放到正确的位置上. 重复此循环直到nums[i] == i 或者找到了一个重复的元素. 

由于每一次swap都会使一个元素被正确归位, 所以最多执行n次就可以正确的归位整个数组, 找到重复的元素.

故虽然有个嵌套循环, 但是时间复杂度仍为O(n)

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        final int n = nums.length;
        for(int i = 0; i != n; ++i)
        {
            while(nums[i] != i)
            {
                int j = nums[i];
                if(nums[i] == nums[j])
                    return nums[i];
                //swap nums[i] and nums[j]
                int tmp = nums[i];
                nums[i] = nums[j];
                nums[j] = tmp;
            }
        }
        return -1;
    }
}
```

