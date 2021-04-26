给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/15/binarytree.png)

 

示例 1:
```
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出: 3
解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
```
示例 2:
```
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出: 5
解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。
```

说明:
```
所有节点的值都是唯一的。
p、q 为不同节点且均存在于给定的二叉树中。
```

### 后序遍历

我们构造这样的一个函数, `TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q)`

**当以root节点作为根节点的子树存在为p的时候返回p, 存在q的时候返回q,  如果都存在就返回p和q的最近的祖先, 都不存在就返回null**

那么首先定义边界条件, 

```c++
if(root == nullptr){
	return nullptr;
}
if(root == p || root == q){
	return root;
}
```

先查看root左右子树的情况

```c++
TreeNode* left = lowestCommonAncestor(root->left, p, q);
TreeNode* right = lowestCommonAncestor(root->right, p, q);
```

+ 如果left和right都为null, 根据我们的定义, 这时候p和q都不存在root的左右子树上, 函数前面已经判断了root本身不是p或q, 所以root子树肯定不包含p或q, 返回null 
+ 如果left和right都不为null, 那么只有一种可能就是p和q各在一个子树上. 那么root肯定是这两个节点的最近公共节点了, 返回root即可
+ 如果left和right其中有一个为null, 有一个不为null, 这种情况比较复杂, 假设left不为null, right为null (left为null, right不为null同理)
  + 如果left不为null的原因是有一个p在left子树上, 而另外的q在非root子树上, 这时left = p. 这时候根据定义要返回p, 因为left就为p, 所以返回left
  + 如果left不为null的原因是p和q都在left子树上, 这时left = p和q的最近祖先, 根据定义p和q也都在root子树上, 所以也返回left



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
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == nullptr){
            return nullptr;
        }
        if(root == p || root == q){
            return root;
        }
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        if(left == nullptr && right == nullptr){
            return nullptr;
        }
        else if(left != nullptr && right != nullptr){
            return root;
        }
        else if(left != nullptr && right == nullptr){
            return left;
        }
        else{
            return right;
        }
    }
};
```



### hash表

也可以遍历一遍构造出每个节点的父节点, 这样就可以转换成两个相交链表求第一个公共节点的问题了.