从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。

 

例如:
给定二叉树: [3,9,20,null,null,15,7],
```
    3
   / \
  9  20
    /  \
   15   7
```
返回：

`[3,9,20,15,7]`


提示：

`节点总数 <= 1000`

<!--more-->



## BFS

直接BFS按层遍历即可

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
    public int[] levelOrder(TreeNode root) {
        if(root == null){
            return new int[]{};
        }
        Queue<TreeNode> q = new ArrayDeque<>();
        List<Integer> ans = new ArrayList<>();
        q.offer(root);
        while(!q.isEmpty()){
            TreeNode curr = q.poll();
            ans.add(curr.val);
            if(curr.left != null){
                q.offer(curr.left);
            }
            if(curr.right != null){
                q.offer(curr.right);
            }
        }
        Stream<Integer> integerStream = ans.stream();
        IntStream intStream = integerStream.mapToInt(x -> x);
        int[] answer = intStream.toArray();
        return answer;
    }
}
```

