输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

 

示例 1：

```
输入：head = [1,3,2]
输出：[2,3,1]
```

限制：

`0 <= 链表长度 <= 10000`

<!--more-->

## 1. 链表反转

如果可以改变链表结构, 直接翻转过来再打印即可

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
    public int[] reversePrint(ListNode head) {
        if(head == null || head.next == null)
        {
            return head == null ? new int[0] : new int[]{head.val};
        }
        int[] ans = new int[getLength(head)];
        ListNode newHead = reverseLinkedList(head);
        int k = 0;
        for(ListNode i = newHead; i != null; i= i.next)
        {
            ans[k++] = i.val;
        }
        return ans;
    }
    private int getLength(ListNode head)
    {
        int len = 0;
        for(ListNode i = head; i != null; i = i.next)
        {
            ++len;
        }
        return len;
    }
    private ListNode reverseLinkedList(ListNode head)
    {
        ListNode p1 = head;
        ListNode p2 = head.next;
        while(p2 != null)
        {
            ListNode tmp = p2.next;
            p2.next = p1;
            p1 = p2;
            p2 = tmp;
        }
        head.next = null;
        return p1;
    }
}
```

## 2. 直接打印

如果不要求链表可以翻转, 只能先用个数组存储, 然后翻转即可. 但是栈可以更简洁的做到这个操作.

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
    public int[] reversePrint(ListNode head) {
        Deque<Integer> dq = new ArrayDeque<>();
        for(ListNode i = head; i != null; i = i.next)
        {
            dq.offerFirst(i.val);
        }
        int[] ans = new int[dq.size()];
        int k = 0;
        while(!dq.isEmpty())
        {
            ans[k++] = dq.pollFirst();
        }
        return ans;
    }
}
```

