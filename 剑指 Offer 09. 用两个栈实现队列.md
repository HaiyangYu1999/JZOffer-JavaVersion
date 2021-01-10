用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

 

示例 1：
```
输入：
["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]
```
示例 2：
```
输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]
```
提示：

`1 <= values <= 10000`
最多会对 appendTail、deleteHead 进行 10000 次调用

<!--more-->

## 1. 栈实现队列

等价于leetcode 232

```java
class CQueue {
    Deque<Integer> deq1 = new ArrayDeque<>();
    Deque<Integer> deq2 = new ArrayDeque<>();
    public CQueue() {

    }
    private int size()
    {
        return deq1.size() + deq2.size();
    }
    public void appendTail(int value) {
        deq2.offerFirst(value);
    }
    
    public int deleteHead() {
        if(this.size() == 0)
            return -1;
        if(deq1.size() == 0)
        {
            while(!deq2.isEmpty())
            {
                deq1.offerFirst(deq2.pollFirst());
            }
        }
        return deq1.pollFirst();
    }
}

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue obj = new CQueue();
 * obj.appendTail(value);
 * int param_2 = obj.deleteHead();
 */
```

