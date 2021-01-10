这个题等价于leetcode 287.

Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.

There is only **one duplicate number** in `nums`, return *this duplicate number*.

**Follow-ups:**

1. How can we prove that at least one duplicate number must exist in `nums`? 
2. Can you solve the problem **without** modifying the array `nums`?
3. Can you solve the problem using only constant, `O(1)` extra space?
4. Can you solve the problem with runtime complexity less than `O(n2)`?

 

**Example 1:**

```
Input: nums = [1,3,4,2,2]
Output: 2
```

**Example 2:**

```
Input: nums = [3,1,3,4,2]
Output: 3
```

**Example 3:**

```
Input: nums = [1,1]
Output: 1
```

**Example 4:**

```
Input: nums = [1,1,2]
Output: 1
```

 

**Constraints:**

- `2 <= n <= 3 * 104`
- `nums.length == n + 1`
- `1 <= nums[i] <= n`
- All the integers in `nums` appear only **once** except for **precisely one integer** which appears **two or more** times.

<!--more-->

## 1 原地修改再改回来(原地哈希)

面试的时候可以问问面试官可不可以这么做. 如果可以的话这种方法最简单. 

**对于遍历的i, 把nums[i]位置的元素置为负. 当遍历到另一个下标j的时候, 如果`nums[i] == nums[j]`, 那么将nums[j]位置的元素置为负的时候发现nums[j]位置的元素已经是负数了, 所以肯定出现了重复!**最后把数组所有元素变为正数.

```java
class Solution {
    public int findDuplicate(int[] nums) {
        int ans = -1;
        for(int i = 0; i < nums.length; ++i)
        {
            int value = Math.abs(nums[i]);
            if(nums[value] < 0)
            {
                ans = value;
                break;
            }
            else
                nums[value] = -Math.abs(nums[value]);
        }
        for(int i = 0; i < nums.length; ++i)
        {
            nums[i] = Math.abs(nums[i]);
        }
        return ans;
    }
}
```

## 2. 二分查找

不同于之前的二分查找, 都是按照下标二分数组. 

但是这次要按照值域来二分.

因为所有的数都在1到n中, 我们可以设置`mid = begin + (end - begin) / 2`

**在数组中统计等于mid的个数cnt0, 小于mid并且大于等于begin的个数为cnt1, 大于mid并且小于等于end的个数为cnt2.**

+ 如果`cnt0 > 1`说明mid重复, 返回mid
+ 如果`cnt1 > mid - begin` 说明小于mid的元素有重复的. (因为从`begin`到`mid-1`一共就只有`mid - begin`个元素, cnt1比这个值大, 肯定有重复的元素!) 再令`begin = begin, end = mid - 1`重新查找即可
+ 如果`cnt2 > end - mid`说明大于mid的元素有重复的. (因为从`mid + 1`到`end`一共就只有`end - mid`个元素, cnt2比这个值大, 肯定有重复的元素!) 再令`begin = mid + 1, end = end`重新查找即可

但是这种方法慢, 复杂度O(nlogn) 不是线性复杂度

**同时, 这种方法只能在数字范围==小于==数组元素数量的情况下使用.**  举个例子, 如果是剑指offer 03题第一问的描述

> 在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

**就找不出来了. 因为数字范围为n, 数组长度也为n. 反例`int[] a = {0, 1, 2, 3, 5, 5, 6, 7, 8, 9, 10, 11, 12};`**

**因为`cnt0 + cnt1 + cnt2 = 数组长度`. 完全有机会使得`cnt0 == 1, cnt1 == mid - begin, cnt2 == end - mid `. 导致在循环的时候会满足上述3个条件都不满足`cnt0 > 1`, `cnt1 > mid - begin` , `cnt2 > end - mid`. while进入死循环.** 

**而在本题中, 数字范围为n, 数组长度为n-1, 所以不会出现上述3个条件`cnt0 > 1`, `cnt1 > mid - begin` , `cnt2 > end - mid`至少会满足一个条件! while不会进入死循环.**

```java
class Solution {
    public int findDuplicate(int[] nums) {
        final int n = nums.length - 1;
        int begin = 1;
        int end = n;
        while(begin <= end)
        {
            int mid = begin + (end - begin) / 2;
            int cnt0 = 0;
            int cnt1 = 0;
            int cnt2 = 0;
            for(int i = 0; i != nums.length; ++i)
            {
                if(nums[i] == mid)
                    ++cnt0;
                else if(nums[i] < mid && nums[i] >= begin)
                    ++cnt1;
                else if(nums[i] > mid && nums[i] <= end)
                    ++cnt2;
            }
            if(cnt0 > 1)
                return mid;
            else if(cnt1 > mid - begin)
            {
                end = mid - 1;
            }
            else if(cnt2 > end - mid)
            {
                begin = mid + 1;
            }
        }
        return -1;
    }
}
```

## 3. 将数组看成有环链表

首先将数组看成链表, 第i个元素的下一个元素为第nums[i]个元素.

**因为数组中的值都是在1到n中的, 我们可以不断重复地做`i = nums[i]`的操作并且不越界.** 

**这就说明了链表肯定有环!!** 

可以这样理解 : 因为每个next指针(nums[i])都指向了一个确定的元素, 而不是指向了null (nums[i] = -1). 我们可以无限的进行i = i->next (i= nums[i])的操作.

**所以环最开始的节点肯定有两个指针指向它. 在数组中就对应了有两个值相同, 都等于成环第一个节点的下标.**



**但是要注意, 数组中一定不能含有元素0. 如果含有0, 就错了.** 

比如 数组 a = [1,0,2,2]. 我们从0开始寻找环的第一个节点. 会发现一直在1->0->1->0->1中循环而找不到重复的元素2.

没有0的时候, 是可以的. 例如, 数组 a = [1,3,2,2] 1->3->2->2->2->.....所以找到了环2->2



```java
class Solution {
    public int findDuplicate(int[] nums) {
        int fast = nums[nums[0]];
        int slow = nums[0];
        while(fast != slow)
        {
            fast = nums[nums[fast]];
            slow = nums[slow];
        }
        slow = 0;
        while(fast != slow)
        {
            fast = nums[fast];
            slow = nums[slow];
        }
        return fast;
    }
}
```

