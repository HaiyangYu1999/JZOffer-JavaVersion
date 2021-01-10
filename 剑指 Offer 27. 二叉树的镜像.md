请完成一个函数，输入一个二叉树，该函数输出它的镜像。

例如输入：
```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```
镜像输出：
```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

 

示例 1：
```
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```

限制：

`0 <= 节点个数 <= 1000`

<!--more-->

## 递归

翻转root  <=> 翻转root.left和翻转root.right并且把翻转后的root.left放到root的右孩子上, 把翻转后的root.right放到root的左孩子上.

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
    public TreeNode mirrorTree(TreeNode root) {
        if(root == null){
            return root;
        }
        TreeNode left = root.left;
        TreeNode right = root.right;
        root.right = mirrorTree(left);
        root.left = mirrorTree(right);
        return root;
    }
}
```

