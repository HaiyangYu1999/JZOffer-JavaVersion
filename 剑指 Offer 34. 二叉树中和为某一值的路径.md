输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。

 

示例:
给定如下二叉树，以及目标和 sum = 22，
```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
```
返回:
```
[
   [5,4,11,2],
   [5,8,4,5]
]
```

提示：

`节点总数 <= 10000`

<!--more-->

## DFS

这题按照一般的套路还不行, 我一开始想用root == null作为终止的条件,但是碰见那种只有一个节点的树就失灵了

例如

```
     1
    / \
   2   null
  / \
null null     
```

如果是root == null这一个终止条件, 那么这就有3条路, `1 -> null,1 -> 2 -> leftNull, 1->2->RightNull `

如果target = 1, 原来的树没有这样的路径, 但是这样会找出来一条路径`1->null`

如果target = 3, 则会找出2条相同的路径来, `1->2`和`1->2`

所以DFS终止条件是`root.left == null && root.right == null`

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
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> ans = new ArrayList<>();
        if(root == null){
            return ans;
        }
        List<Integer> curr = new ArrayList<>();
        int currSum = 0;
        DFS(root, sum, ans, curr, currSum);
        return ans;
    }
    private void DFS(TreeNode root, int sum, List<List<Integer>> ans, List<Integer> curr, int currSum){
        if(root.left == null && root.right == null){
            currSum += root.val;
            if(currSum == sum){
                curr.add(root.val);
                ans.add(new ArrayList<>(curr));
                curr.remove(curr.size() - 1);
            }
            return;
        }
        else{
            currSum += root.val;
            curr.add(root.val);
            if(root.left != null)
                DFS(root.left, sum, ans, curr, currSum);
            if(root.right != null)
                DFS(root.right, sum, ans, curr, currSum);
            currSum -= root.val;
            curr.remove(curr.size() - 1);
        }
    }
}
```

