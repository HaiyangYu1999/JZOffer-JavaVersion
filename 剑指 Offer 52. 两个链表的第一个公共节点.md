输入两个链表，找出它们的第一个公共节点。




注意：

如果两个链表没有交点，返回 null.
在返回结果后，两个链表仍须保持原有的结构。
可假定整个链表结构中没有循环。
程序尽量满足 O(n) 时间复杂度，且仅用 O(1) 内存。
本题与主站 160 题相同：https://leetcode-cn.com/problems/intersection-of-two-linked-lists/



<!--more-->

### 双指针

很简单的一个题, 一开始还在想如果两个链表不相交怎么办, 后来发现不相交的情况p1和p2在最后一步都为nullptr, 可以跳出循环.

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode *p1 = headA;
        ListNode *p2 = headB;
        while(p1 != p2){
            p1 = (p1 == nullptr) ? headB : p1->next;
            p2 = (p2 == nullptr) ? headA : p2->next;
        }
        return p1;
    }
};
```

