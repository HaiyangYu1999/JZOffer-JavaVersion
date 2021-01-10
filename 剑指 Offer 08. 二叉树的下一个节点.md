给定一棵二叉树和其中的一个节点, 如何找出中序遍历序列的下一个节点? 树中的节点除了有两个分别指向左,右子节点的指针, 还有一个指向父节点的指针.

<!--more-->

## 1.逻辑判断

根据中序遍历的特点, 左子树->根节点->右子树.

如果这个节点的右子树不为空, **一个节点中序遍历的下一个节点就等价于这个节点右子树中序遍历的第一个节点!!**, 也就是这个节点右子树的最左侧节点.

如果这个节点的右子树为空, 并且这个节点是一个左孩子, 那么也根据中序遍历的特点, 下一个遍历的就是它的父节点.

如果这个节点的右子树为空, 并且没有父节点,  那么这个节点应该是中序遍历的最后一个节点了, 返回空即可.

**如果这个节点的右子树为空, 并且这个节点是一个右孩子, 这种情况比较难处理, 我们需要判断父节点是左孩子还是右孩子, 如果是左孩子的话, 那么返回这个父节点的父节点即可. **

**如果父节点仍然是右孩子, 再继续往上找, 直到找到某个祖先A是一个左孩子, 返回祖先A的父节点.** 

```
  1
 /
2    <- 第2步. 2是一个左孩子, 所以直接返回2的父节点1
\
  3  <- 第1步. 查找3的下一个遍历节点, 3的右子树为空并且是一个右孩子,去查看3的父节点2
 /
4
```

```
  0
 /
1          <- 第4步. 1是一个左孩子, 所以1的父节点0是节点6的下一个遍历节点
 \
  2        <- 第3步. 2也是一个右孩子, 继续查找2的父节点1.
 / \
3   4      <- 第2步. 4是一个右孩子, 继续查找4的父节点2.
   /  \
  5    6   <- 第1步. 查找6的下一个遍历节点, 6的右子树为空, 并且是一个右孩子, 查找父节点4
      /
     7
```

```java
/*
public class TreeLinkNode {
    int val;
    TreeLinkNode left = null;
    TreeLinkNode right = null;
    TreeLinkNode next = null;                //这里的next表示当前节点的父节点

    TreeLinkNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    public TreeLinkNode GetNext(TreeLinkNode pNode)
    {
        if(pNode == null)
            return null;
        if(pNode.right != null)
        {
            TreeLinkNode ans = pNode.right;
            while(ans.left != null)
            {
                ans = ans.left;
            }
            return ans;
        }
        TreeLinkNode curr = pNode;
        while(curr.next != null)
        {
            if(curr.next.left == curr)
            {
                return curr.next;
            }
            curr = curr.next;
        }
        return null;
    }
}
```

