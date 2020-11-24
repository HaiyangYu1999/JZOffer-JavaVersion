在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5

<!--more-->

## 1. 双指针

用begin和end查找某一段val相同的范围[begin,end), 这个范围内的值都相同. 如果范围内的值只有1个, 就什么都不做, 如果大于1个就删掉这个范围的所有节点.

**总是给链表加头节点是一个好事情, 这样可以免去很多的麻烦**

```java
/*
 public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    public ListNode deleteDuplication(ListNode head)
    {
        if(head == null || head.next == null)
            return head;
        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;
        ListNode begin_prev = dummyHead;
        ListNode begin = dummyHead.next;
        ListNode end = dummyHead.next;
        while(begin != null)
        {
            while(end != null && end.val == begin.val)
            {
                end = end.next;
            }
            if(end != begin.next)
            {
                begin_prev.next = end;
                begin = end;
            }
            else{
                begin_prev = begin_prev.next;
                begin = begin.next;
            }
        }
        return dummyHead.next;
    }
}
```

