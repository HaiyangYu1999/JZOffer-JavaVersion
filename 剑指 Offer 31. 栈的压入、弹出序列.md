输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。

 

示例 1：
```
输入：pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
输出：true
解释：我们可以按以下顺序执行：
push(1), push(2), push(3), push(4), pop() -> 4,
push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
```
示例 2：
```
输入：pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
输出：false
解释：1 不能在 2 之前弹出。
```

提示：
```
0 <= pushed.length == popped.length <= 1000
0 <= pushed[i], popped[i] < 1000
pushed 是 popped 的排列。
```



<!--more-->

## 模拟

这题我一开始没想到模拟的方法, 想用动态规划做, 通过前m个pushed是否可以弹出为前n个popped来做. 后来怎么想都找不到解决办法. 

后来想不下去了, 看了剑指offer上面的答案, 才发现模拟一个stack就能做到......**这都没想到我是🐖么**

首先定义两个指针i和j, i表示当前push到哪里了, j表示pop到哪里了

当j != length(popped)时, 我们检查栈顶元素是不是popped[j]

+ 如果是, 就弹出这个元素, j++, 执行下一轮

+ 如果不是, 或者栈为空, 就要一直按照顺序push, 直到栈顶等于popped[j]. push的过程中如果pushed中的元素用完了, 就返回false

```java
class Solution {
    public boolean validateStackSequences(int[] pushed, int[] popped) {
        if(pushed == null || popped == null){
            throw new NullPointerException();
        }
        Deque<Integer> stack = new ArrayDeque<>();
        int i = 0;
        int j = 0;
        while(j != popped.length){
            while(stack.isEmpty() || stack.peek() != popped[j]){
                if(i == pushed.length){
                    return false;
                }
                stack.push(pushed[i++]);
            }
            stack.pop();
            ++j;
        }
        return true;
    }
}
```

