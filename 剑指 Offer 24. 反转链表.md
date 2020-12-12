定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

 

示例:
```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

限制：
```<>
0 <= 节点个数 <= 5000
```

<!--more-->

## 翻转链表

也是很经典的一个问题了, 就不再讲思路了 和leetcode 206相同

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        ListNode dummyHead = new ListNode(-1);
        dummyHead.next = head;
        ListNode p1 = head;
        ListNode p2 = head.next;
        while(p2 != null){
            ListNode tmp = p2.next;
            p2.next = p1;

            p1 = p2;
            p2 = tmp;
        }

        dummyHead.next.next = null;
        dummyHead.next = p1;

        return dummyHead.next;
    }
}
```

