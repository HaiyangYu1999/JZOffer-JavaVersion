给定单向链表的头指针和一个要删除的节点，定义一个函数删除该节点。

<!--more-->

## 1. 分类讨论

这题leetcode有类似的题, 给定一个指向非尾节点的指针, 用O(1)的时间删除这个节点. 方法就是复制下一个节点的值到这一个节点上， 然后删除下一个节点.

这个题也一样, 如果被删除的节点不是最后一个, 就按照上面的方法来判断. 如果是最后一个, 那么只能遍历一遍链表来删除了

一定要注意2种特=特殊情况

+ 给的链表或被删除的节点为null
+ 被删除的节点是链表的头节点, 并且链表只有1个元素

```java
class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
    @Override
    public String toString(){
        StringBuilder sb = new StringBuilder();
        ListNode curr = this;
        while (curr != null)
        {
            sb.append(curr.val);
            sb.append(" ");
            curr = curr.next;
        }
        return sb.toString();
    }
  }
class Solution {
    public ListNode deleteNode(ListNode head, ListNode toBeDeleted) {
        if(head == null || toBeDeleted == null)
            throw new NullPointerException("input is null");
        if(toBeDeleted.next == null)
        {
            if(head == toBeDeleted){      //特别要注意这一行
                return null;
            }
            ListNode curr = head;
            while(curr != null)
            {
                if(curr.next == toBeDeleted)
                {
                    curr.next = curr.next.next;
                }
                curr = curr.next;
            }
            return head;
        }
        else
        {
            toBeDeleted.val = toBeDeleted.next.val;
            toBeDeleted.next = toBeDeleted.next.next;
            return head;
        }
    }

    public static void main(String[] args) {
        ListNode a1 = new ListNode(1);
        ListNode a2 = new ListNode(2);
        ListNode a3 = new ListNode(3);
        ListNode a4 = new ListNode(4);
        ListNode a5 = new ListNode(5);
        ListNode a6 = new ListNode(6);
        a1.next = a2;
        a2.next = a3;
        a3.next = a4;
        a4.next = a5;
        a5.next = a6;
        ListNode single = new ListNode(-1);
        Solution s = new Solution();
        //test null LinkedList, one element LinkedList, regular LinkedList
        System.out.println(s.deleteNode(a1, a4));
        System.out.println(s.deleteNode(single, single));
        System.out.println(s.deleteNode(null, null));
    }
}
```



