给定一个数组 nums 和滑动窗口的大小 k，请找出所有滑动窗口里的最大值。

示例:
```
输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 

  滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

提示：
```
你可以假设 k 总是有效的，在输入数组不为空的情况下，1 ≤ k ≤ 输入数组的大小。
```





### 队列的最小值

这个题第一次做还真的难想到转化为队列的最大值问题

设计一个可以在队列求最大值的数据结构就能解决. 思路和最小栈一样, 用一个辅助队列来做

辅助队列头部存储当前队列中的最大值

构造方法详见 https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/solution/mian-shi-ti-59-ii-dui-lie-de-zui-da-zhi-by-leetcod/

感觉思路很简单, 但描述起来很难. 这里就直接贴一个链接了. 重点是保持辅助队列递减

```c++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        deque<int> deq;
        vector<int> ans;
        if(nums.empty() || nums.size() < k){
            return ans;
        }
        for(int i = 0; i < k; ++i){
            add(deq, nums[i]);
        }
        ans.push_back(max(deq));
        for(int i = k; i < nums.size(); ++i){
            take(deq,nums[i-k]);
            add(deq,nums[i]);
            ans.push_back(max(deq));
        }
        return ans;
    }
private:
    deque<int>& add(deque<int>& deq, int a){
        if(deq.empty()){
            deq.push_back(a);
        }
        else{
            int top = deq.front();
            while(!deq.empty() && a > deq.back()){
                deq.pop_back();
            }
            deq.push_back(a);
        }
        return deq;
    }
    deque<int>& take(deque<int>& deq, int a){
        if(deq.empty()){
            return deq;
        }
        else{
            int top = deq.front();
            if(a < top){
                return deq;
            }
            else{
                deq.pop_front();
                return deq;
            }
        }
    }

    int max(deque<int>& deq){
        if(deq.empty()){
            throw "NoSuchElementException";
        }
        else{
            return deq.front();
        }
    }
};
```



