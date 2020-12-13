定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。

 

示例:
```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.min();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.min();   --> 返回 -2.
```

提示：

`各函数的调用总次数不超过 20000 次`

<!--more-->



## 1. 辅助栈

使用额外的空间, 这样就变得很简单了. 

辅助栈存储当前栈里的所有元素的最小值.

例如, 当push(-2)的时候, 由于栈和辅助栈都为空, 所以直接push进去即可`stk = [-2], stkAux = [-2]`

当push(0)的时候, 由于当前栈中所有元素的最小值为`stkAux.top() == -2 < 0`, 所以stkAux仍然push进去-2. `stk = [-2,0], stkAux = [-2,-2]`

当push(-3)的时候, 由于当前栈中所有元素的最小值为`stkAux.top() == -2 > -3`, 所以stkAux要push进去-3, 表示stk中最小的元素现在变为了-3. `stk = [-2,0,-3], stkAux = [-2,-2,-3]`

pop的时候直接pop掉stk和stkAux即可

top直接取stk的top

min直接取stkAux的top

```java
class MinStack {
    Deque<Integer> stk = new ArrayDeque<>();
    Deque<Integer> stkAux = new ArrayDeque<>();
    /** initialize your data structure here. */
    public MinStack() {

    }
    
    public void push(int x) {
        stk.push(x);
        int min = (stkAux.isEmpty() || stkAux.peek() > x) ? x : stkAux.peek();
        stkAux.push(min);
    }
    
    public void pop() {
        stk.pop();
        stkAux.pop();
    }
    
    public int top() {
        if(stk.isEmpty()){
            throw new java.util.NoSuchElementException();
        }
        return stk.peek();
    }
    
    public int min() {
        if(stk.isEmpty()){
            throw new java.util.NoSuchElementException();
        }
        return stkAux.peek();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.min();
 */
```

## 2. O(1)额外空间

只是用一个int类型来保存当前栈中的最小值

这次要改变一下思路, stack不储存各个元素了, 而是存储各个元素与min的差值.

参考自https://zhuanlan.zhihu.com/p/49854919, 具体如下: 

1. 第一次push的时候，把该元素作为最小元素min。
2. 在后面的push操作中，首先判断当前元素`num`是否小于`min`,如果不小于`min`，就向栈中存入元素值`data = num-min`(这个值肯定大于0，因为num大于min)；如果num小于min，也向栈中存入`data = num-min`(`data`小于0)，同时记得更新`min`值。
3. pop的时候，首先判断栈顶的元素data是否大于0，如果大于0，则pop的值应该是`num=data +min`（因为存的时候是`data = num-min`);如果小于0，则pop的时候应该是`min`，同时要更新min，`min = min- data`。
4. 同时get_min时直接返回min的值就是整个栈元素中的最小值

```java
class MinStack {
    Deque<Long> stk = new ArrayDeque<>();
    long min = 0;
    /** initialize your data structure here. */
    public MinStack() {

    }
    
    public void push(long x) {
        if(stk.isEmpty()){
            min = x;
            stk.push((long) 0);
        }
        else{
            if(x >= min){
                stk.push(x - min);
            }
            else{
                stk.push(x - min);
                min = x;
            }
        }
    }
    
    public void pop() {
        long peekValue = stk.peek();
        if(peekValue >= 0){
            stk.pop();
        }
        else{
            min = min - peekValue;
            stk.pop();
        }
    }
    
    public long top() {
        if(stk.peek() >= 0){
            return stk.peek() + min;
        }
        else{
            return min;
        }
    }
    
    public long min() {
        return min;
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.min();
 */
```

