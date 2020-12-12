输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。

示例1：
```
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```
限制：
```
0 <= 链表长度 <= 1000
```

<!--more-->

## 归并

这题和归并排序的思想一样, 直接模拟即可.

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
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode head = new ListNode(-1);
        ListNode tail = head;
        ListNode i = l1;
        ListNode j = l2;
        while(i != null || j != null){
            if(i == null || (i != null && j != null && i.val > j.val)){
                ListNode moveNode = j;
                j = j.next;
                tail.next = moveNode;
                tail = tail.next;
                tail.next = null;
            }
            else{
                ListNode moveNode = i;
                i = i.next;
                tail.next = moveNode;
                tail = tail.next;
                tail.next = null;
            }
        }

        return head.next;
    }
}
```

