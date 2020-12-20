输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。

 

为了让您更好地理解问题，以下面的二叉搜索树为例：

 

![img](https://assets.leetcode.com/uploads/2018/10/12/bstdlloriginalbst.png)

 

我们希望将这个二叉搜索树转化为双向循环链表。链表中的每个节点都有一个前驱和后继指针。对于双向循环链表，第一个节点的前驱是最后一个节点，最后一个节点的后继是第一个节点。

下图展示了上面的二叉搜索树转化成的链表。“head” 表示指向链表中有最小元素的节点。

 

![img](https://assets.leetcode.com/uploads/2018/10/12/bstdllreturndll.png)

 

特别地，我们希望可以就地完成转换操作。当转化完成以后，树中节点的左指针需要指向前驱，树中节点的右指针需要指向后继。还需要返回链表中的第一个节点的指针。

<!--more-->

## 1. 递归

还是分为三个部分, 

+ 先把左子树转换为链表, 并且找到左链表的头节点leftFirst和尾节点leftLast
+ 把右子树转换为链表, 并且找到右链表的头节点rightFirst和尾节点rightLast
+ 将root与leftLast连接, 将root与rightFirst连接
+ 返回以头节点为根节点的子树的链表头leftFirst和链表尾rightLast

其中`Node[] getFirstAndLast(Node root)`返回以root为根节点的子树对应的链表的头节点和尾节点

最后别忘了要循环链表. 

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val,Node _left,Node _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/
class Solution {
    public Node treeToDoublyList(Node root) {
        Node[] beginAndEnd = getFirstAndLast(root);
        Node left = beginAndEnd[0];
        Node right = beginAndEnd[1];
        if(left == null && right == null){
            return null;
        }
        left.left = right;
        right.right = left;
        return left;
    }
    private Node[] getFirstAndLast(Node root){
        if(root == null){
            return new Node[]{null, null};
        }
        Node[] left = getFirstAndLast(root.left);
        Node[] right = getFirstAndLast(root.right);
        Node leftFirst = left[0];
        Node leftLast = left[1];
        Node rightFirst = right[0];
        Node rightLast = right[1];


        Node[] ans = new Node[2];
        if(leftFirst != null && leftLast != null){
            leftLast.right = root;
            root.left = leftLast;
            ans[0] = leftFirst;
        }
        else{
            ans[0] = root;
        }
        if(rightFirst != null && rightLast != null){
            rightFirst.left = root;
            root.right = rightFirst;
            ans[1] = rightLast;
        }
        else{
            ans[1] = root;
        }
        return ans;
    }
}
```

