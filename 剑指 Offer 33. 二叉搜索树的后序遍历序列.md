输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 true，否则返回 false。假设输入的数组的任意两个数字都互不相同。

 

参考以下这颗二叉搜索树：
```
     5
    / \
   2   6
  / \
 1   3
```
示例 1：
```
输入: [1,6,3,2,5]
输出: false
```
示例 2：
```
输入: [1,3,2,6,5]
输出: true
```

提示：

`数组长度 <= 1000`

<!--more-->

## 递归

这题我第一次做竟然没做出来, 没往递归那里想, 只想着对数组进行一些运算得到答案. 看来以后碰到二叉树就要递归

首先拿到数组之后, 最后一个元素肯定是根节点, 然后从前往后找出所有的比nums[end]小的元素, 这些元素就是左子树了, 剩下的元素就是右子树. **这里还要对右子树检查, 检查是不是确实每个值都大于nums[end], 一开始做的时候忘了检查了, 怎么做都错. 错误示例[1,2,5,10,6,9,4,3]** 

然后递归地检查左子树和右子树是不是二叉查找树

```java
class Solution {
    public boolean verifyPostorder(int[] postorder) {
        return verifyPostorder(postorder, 0, postorder.length - 1);
    }
    private boolean verifyPostorder(int[] postorder, int begin, int end){
        if(postorder == null){
            throw new NullPointerException();
        }
        int length = end - begin + 1;
        if(length <= 2){
            return true;
        }
        int endValue = postorder[end];
        int threshold = begin;
        while(postorder[threshold] < endValue){
            ++threshold;
        }
        for(int i = threshold; i < end; ++i){
            if(postorder[i] < endValue){
                return false;
            }
        }
        return verifyPostorder(postorder, begin, threshold - 1) && verifyPostorder(postorder, threshold, end - 1);
    }
}
```

