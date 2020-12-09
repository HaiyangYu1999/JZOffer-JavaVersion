输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。例如，一个链表有6个节点，从头节点开始，它们的值依次是1、2、3、4、5、6。这个链表的倒数第3个节点是值为4的节点。

 

示例：
```
给定一个链表: 1->2->3->4->5, 和 k = 2.

返回链表 4->5.
```

<!--more-->

## 两次遍历

这个题主要是判断特殊条件, 如果传入的head为null如何处理, 如果k <= 0如何处理, 如果k大于链表长度如何处理. 这些都需要用if逐个判断. 判断后再写出程序就很简单了. 因为保证了head不为空, 并且0 < k <= len

第一次遍历是找出链表长度, 第二次遍历找出倒数第k个节点

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
    public ListNode getKthFromEnd(ListNode head, int k) {
        if(head == null){
            throw new NullPointerException();
        }
        int len = 0;
        for(ListNode i = head; i != null; i = i.next){
            ++len;
        }
        if(k <= 0 || k > len){
            throw new IllegalArgumentException();
        }
        int rank = len - k;
        ListNode ans = head;
        for(int i = 0; i < rank; ++i){
            ans = ans.next;
        }
        return ans;
    }
}
```

## 一次遍历

上面方法的改进版, 用两个指针, 快指针走k步之后慢指针再走. 快指针走到链表尾部的时候慢指针刚好走到倒数第k个节点.

同样要注意k大于链表长度的情况, 这时快指针走完慢指针还没开始走

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
    public ListNode getKthFromEnd(ListNode head, int k) {
        if(head == null){
            throw new NullPointerException();
        }
        if(k <= 0){
            throw new IllegalArgumentException("k <= 0");
        }
        ListNode fast = head;
        ListNode slow = head;
        boolean flag = false;
        for(int i = 0; i < k; ++i){
            if(fast == null){
                flag = true;
                break;
            }
            fast = fast.next;
        }
        if(flag){
            throw new IllegalArgumentException("k > length of list");
        }
        while(fast != null){
            fast = fast.next;
            slow = slow.next;
        }
        return slow;
    }
}
```

