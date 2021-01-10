请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。

 

例如:
给定二叉树: [3,9,20,null,null,15,7],
```
    3
   / \
  9  20
    /  \
   15   7
```
返回其层次遍历结果：
```
[
  [3],
  [20,9],
  [15,7]
]
```

提示：

`节点总数 <= 1000`

<!--more-->

## BFS

这次要新建一个变量`doesPrintReversed`了, 表示是否倒着打印. 

然后每一层的list我们都使用LinkedList. 因为LinkedList可以头插也可以尾插.

当需要倒着打印时, 就头插这一层的每个元素, 反之尾插

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
    public List<List<Integer>> levelOrder(TreeNode root) {
        if(root == null){
            return new ArrayList<>();
        }
        List<List<Integer>> ans = new ArrayList<>();
        Queue<TreeNode> q = new ArrayDeque<>();
        boolean doesPrintReversed = false;
        q.offer(root);
        while(!q.isEmpty()){
            int size = q.size();
            LinkedList<Integer> layerList = new LinkedList();
            while(size-- != 0){
                TreeNode curr = q.poll();
                if(doesPrintReversed){
                    layerList.addFirst(curr.val);
                }
                else{
                    layerList.addLast(curr.val);
                }
                if(curr.left != null){
                    q.offer(curr.left);
                }
                if(curr.right != null){
                    q.offer(curr.right);
                }
            }
            doesPrintReversed = !doesPrintReversed;
            ans.add(layerList);
        }
        return ans;
    }
}
```

