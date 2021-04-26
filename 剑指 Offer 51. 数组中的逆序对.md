在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

 

示例 1:
```
输入: [7,5,6,4]
输出: 5
```


限制：
```
0 <= 数组长度 <= 50000
```



### 归并排序

这个题一碰到没什么思路, 总以为O(n)能解决. 动态规划, 单调栈都想了. 但没想到利用排序能做

第一次做感觉还是很不容易想到的. 

思路就不写了, 贴个链接(发现最近自己越来越懒了😂😂😂)

https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/solution/shu-zu-zhong-de-ni-xu-dui-by-leetcode-solution/

```java
class Solution {
    public int reversePairs(int[] nums) {
        return reversePairs(nums, 0, nums.length - 1);
    }
    private int reversePairs(int[] nums, int begin, int end){
        if(begin >= end){
            return 0;
        }
        int mid = begin + (end - begin) / 2;
        int leftPairs = reversePairs(nums, begin, mid);
        int rightPairs = reversePairs(nums, mid + 1, end);
        int[] tmp = new int[end - begin + 1];
        int k = end - begin + 1;
        int ans = leftPairs + rightPairs;
        int i = mid;
        int j = end;
        while(i >= begin || j >= mid + 1){
            if(i < begin){
                tmp[--k] = nums[j--];
            }
            else if(j < mid + 1){
                tmp[--k] = nums[i--];
            }
            else{
                int p1 = nums[i];
                int p2 = nums[j];
                if(p1 > p2){
                    ans += (j - mid);
                    tmp[--k] = nums[i--];
                }
                else{
                    tmp[--k] = nums[j--];
                }
            }
        }
        for(int ii = begin; ii <= end; ++ii){
            nums[ii] = tmp[ii - begin];
        }
        return ans;
    }
}
```

