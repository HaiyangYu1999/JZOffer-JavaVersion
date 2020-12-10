## 题目描述

给一个链表，若其中包含环，请找出该链表的环的入口结点，否则，输出null。



## 快慢指针

快慢指针，快指针每次前进两个长度，慢指针每次前进一个长度，若存在环，则一定会在环内相遇。
假设起点到环入口长度a，环入口到相遇点长度为b，相遇点到环入口长度为c，环长度为r，则有如下关系:
2(a+b) = a+b+nr   =>  a+b = nr
b + c = r  ==> a+r-c = nr

a = c+(n-1)r

所以当存在环时，令两个指针分别从相遇点和头出发，相遇点即为环入口

因为如果快指针从相遇点开始走, 慢指针从头开始走, 当慢指针走到环入口的时候, 即走了a, 这次是快慢指针每一次都走一步. 所以快指针也走了a, 根据上面的推导, `a = c+(n-1)r`, 所以快指针从相遇点走了c+(n-1)r, 即也是正好走到环的入口节点.

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

    public ListNode EntryNodeOfLoop(ListNode pHead)
    {
        if(pHead == null || pHead.next == null){
            return null;
        }
        ListNode fast = pHead.next.next;
        ListNode slow = pHead.next;
        while(fast != slow){
            slow = slow.next;
            if(fast == null || fast.next == null){
                return null;
            }
            fast = fast.next.next;
        }
        slow = pHead;
        while(slow != fast){
            slow = slow.next;
            fast = fast.next;
        }
        return slow;
    }
}
```

