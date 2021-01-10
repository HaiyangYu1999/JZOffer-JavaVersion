输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

 

例如，给出

前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
返回如下的二叉树：
```
    3
   / \
  9  20
    /  \
   15   7
```

限制：

`0 <= 节点个数 <= 5000`

<!--more-->

## 1.递归

和leetcode 105一样. 在前序遍历中找到根节点, 然后中序遍历中查找根节点的位置, 得到左子树元素个数和右子树元素个数. 然后再递归的构造.

**注意, 要先给中序遍历数组构建一个hashmap, 加快查找速度, 复杂度为O(n).** 否则, 时间复杂度会退化到O(nlogn). 

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
    Map<Integer, Integer> map = new HashMap<>();
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        for(int i = 0; i < inorder.length; ++i)
        {
            map.put(inorder[i], i);
        }
        return buildTree(preorder,inorder, 0, preorder.length -1, 0, inorder.length - 1); 
    }
    private TreeNode buildTree(int[] preorder, int[] inorder, int preorderBegin, int preorderEnd, int inorderBegin, int inorderEnd)
    {
        if(preorderBegin > preorderEnd || inorderBegin > inorderEnd)
            return null;
        if(preorderBegin == preorderEnd)
        {
            if(inorderBegin == inorderEnd)
                return new TreeNode(preorder[preorderBegin]);
            else
                throw new RuntimeException("traverse size not match");
        }
        int root = preorder[preorderBegin];
        int rootIndex = map.get(root) - inorderBegin;
        TreeNode ans = new TreeNode(root);
        ans.left = buildTree(preorder, inorder, preorderBegin + 1, preorderBegin + rootIndex, inorderBegin, inorderBegin + rootIndex - 1);
        ans.right = buildTree(preorder, inorder, preorderBegin + rootIndex + 1, preorderEnd, inorderBegin + rootIndex + 1, inorderEnd);
        return ans;
    }
}
```

