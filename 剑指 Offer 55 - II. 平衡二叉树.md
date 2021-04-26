输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。

 

示例 1:

给定二叉树 [3,9,20,null,null,15,7]
```
    3
   / \
  9  20
    /  \
   15   7
```
返回 true 。

示例 2:

给定二叉树 [1,2,2,3,3,null,null,4,4]
```
       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
```
返回 false 。

 

限制：

0 <= 树的结点个数 <= 10000





### 自底向上递归

一开始可能容易想到自顶向下, 但是时间复杂度较高.

```
先验证当前节点是不是平衡, 再验证左右子节点是不是平衡. 这样在验证当前节点的平衡时需要求左右子树的高度, 验证左右子节点的时候也需要求左右子树的高度, 就产生重复计算了

可以先验证左右子树是不是平衡, 如果不平衡返回-1, 平衡就返回高度. 然后利用返回的高度验证当前节点的平衡性
```





```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        return isBalancedOrGetDepth(root) != -1;
    }
private:
    int isBalancedOrGetDepth(TreeNode* root){
        if(root == nullptr){
            return 0;
        }
        int left;
        int right;
        if((left = isBalancedOrGetDepth(root->left)) != -1 
            && (right = isBalancedOrGetDepth(root->right)) != -1 
            && std::abs(left - right) <= 1){
            return std::max(left, right) + 1;
        }
        else{
            return -1;
        }
    }
};
```

