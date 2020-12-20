请实现 copyRandomList 函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个 next 指针指向下一个节点，还有一个 random 指针指向链表中的任意节点或者 null。

 

示例 1：


```
输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]
```
示例 2：


```
输入：head = [[1,1],[2,1]]
输出：[[1,1],[2,1]]
```
示例 3：


```
输入：head = [[3,null],[3,0],[3,null]]
输出：[[3,null],[3,0],[3,null]]
```
示例 4：
```
输入：head = []
输出：[]
解释：给定的链表为空（空指针），因此返回 null。
```

提示：
```
-10000 <= Node.val <= 10000
Node.random 为空（null）或指向链表中的节点。
节点数目不超过 1000 。
```

<!--more-->

## 利用链表作为映射

这方法太巧妙了(日常感叹答案的巧妙哈哈哈). 

对于原链表`a->b->c`, 我们仍然需要原链表某一个节点到对应链表的节点的映射.

这时, 把新链表的那个节点都插入到原链表对应节点的后面, 变成`a->a'->b->b'->c->c'`

首先要重新赋值新链表的random指针, 

> 如果原链表的random指针为`a.ramdom == c`, 那么根据我们的映射, 就能得到`a.next.random == c.next`!

然后再将两条链表分离出来即可.

```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/
class Solution {
    public Node copyRandomList(Node head) {
        if(head == null){
            return null;
        }
        for(Node i = head; i != null; i = i.next.next){
            Node copyI = new Node(i.val);
            Node tmp = i.next;
            i.next = copyI;
            copyI.next = tmp;
        }

        for(Node i = head; i != null; i = i.next.next){
            Node copyI = i.next;
            Node j = i.random;
            if(j == null){
                copyI.random = null;
            }
            else{
                Node copyJ = j.next;
                copyI.random = copyJ;
            }
        }

        Node cloneListHead = new Node(-1);
        Node cloneListTail = cloneListHead;

        for(Node i = head; i != null; i = i.next){
            Node copyI = i.next;
            i.next = i.next.next;
            cloneListTail.next = copyI;
            cloneListTail = cloneListTail.next;
        }

        return cloneListHead.next;
    }
}
```

