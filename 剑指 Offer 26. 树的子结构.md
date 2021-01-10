输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

例如:
给定的树 A:
```
     3
    / \
   4   5
  / \
 1   2
```
给定的树 B：
```
   4 
  /
 1
```
返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。

示例 1：
```
输入：A = [1,2,3], B = [3,1]
输出：false
```
示例 2：
```
输入：A = [3,4,5,1,2], B = [4,1]
输出：true
```
限制：

`0 <= 节点个数 <= 10000`

<!--more-->

## 递归

首先要确定的是我们要完成两件事情, 一件事情是判断某个树A是不是含有树B的子结构, 由于树B的根节点不一定是树A的根节点, 可能是从A的某个子节点开始的, 所以需要另外一个方法`doesBeginAtA(someNodeInA, B)`. 

这个方法是给定两个子树, 判断B是不是包含在A中, 并且是从根节点就开始匹配的. 

例如

```
A: 4              B:  4
  / \                / \
 8   9              8   9    
    /
   3
这样我们就可以说明B是在A的头部. 即 doesBeginAtA(A, B) == true, 也就是节点A可以作为B的根节点
```

然后对于A中的每个节点, 检查这个节点是不是可以作为B的根节点, 如果至少有1个节点可以作为B的根节点, 就返回true



即思路是这样的.  首先`isSubStructure` 方法先序地遍历A中所有的节点, 对于每一个节点, 检查这个节点可不可以作为B的根节点, 如果可以, 就返回true, 如果不行, 在检查当前节点的两个子节点是不是可以作为B的根节点.

复杂度O(m*n). 因为最坏情况下每个节点都要检查一遍可不可以作为B的根节点, 每一次检查最坏的情况就是检查n次, 其中n为B的节点数量

**写到这里想到这个题的算法可以用类似KMP算法的思想改进.** 

试想这么一个例子, 所有的节点都只有左孩子, 那么树就退化成单链表了, 我们把它想象成字符串. 树A含有树B, 就等价于字符串A含有字符串B, 这时候我们可以用KMP.

对于一般的树也一样, 例如

```
A:  1
   / \
  1   1
B:  2
   / \
  3   4
我的想法是和kmp中的next数组一样, 通过一个数据结构来保存b的特征.
在上面的A, B中, 首先A根节点和B根节点不匹配(1 != 2), 我们按照普通的递归算法应该比较A的两个孩子是不是和B的根节点匹配. 
但是A的两个孩子也都是1, 肯定是不匹配的. 如果能记录之前的匹配信息(1 != 2), 就可以免除这两次比较
```

**但是这种优化方法感觉实现起来很难, kmp的next数组让我手写都不一定写出来, 别说更复杂的树结构了. 只是作为一个思路吧.**

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isSubStructure(TreeNode A, TreeNode B) {
        if(B == null){
            return false; //这里我感觉应该是空树B肯定是包含在任意的树A中的. 不知道为什么leetcode-cn官网上认定的是返回false
        }
        if(A == null){   //空树不可能包含任意一个非空的树
            return B == null;
        }
        if(doesBeginAtA(A, B)){
            return true;
        }
        else{
            return isSubStructure(A.left, B) || isSubStructure(A.right, B);
        }
    }

    private boolean doesBeginAtA(TreeNode A, TreeNode B){
        if(B == null){
            return true;
        }
        if(A == null){
            return B == null;
        }
        if(A.val == B.val){
            return doesBeginAtA(A.left, B.left) && doesBeginAtA(A.right, B.right);
        }
        return false;
    }
}
```

