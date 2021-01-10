请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。
```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```
但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:
```
    1
   / \
  2   2
   \   \
   3    3
```


示例 1：
```
输入：root = [1,2,2,3,4,4,3]
输出：true
```
示例 2：
```
输入：root = [1,2,2,null,3,null,3]
输出：false
```

限制：

`0 <= 节点个数 <= 1000`

<!--more-->

## 递归

两棵子树是对称的 <==> 子树A的左孩子和子树B的右孩子对称， 并且子树A的右孩子和子树B的左孩子对称

而一棵树是对称的又等价于这棵树的两个子树是对称的.

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
    public boolean isSymmetric(TreeNode root) {
        if(root == null){
            return true;
        }
        return isSymmetric(root.left, root.right);
    }

    private boolean isSymmetric(TreeNode t1, TreeNode t2){
        if(t1 == null && t2 == null){
            return true;
        }
        if(t1 != null && t2 == null || t1 == null && t2 != null){
            return false;
        }
        if(t1.val != t2.val){
            return false;
        }
        return isSymmetric(t1.left, t2.right) && isSymmetric(t1.right, t2.left);
    }
}
```

