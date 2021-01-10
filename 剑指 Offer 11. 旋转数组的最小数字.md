把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1。  

示例 1：
```
输入：[3,4,5,1,2]
输出：1
```
示例 2：
```
输入：[2,2,2,0,1]
输出：0
```

<!--more-->

## 1.一次失败的尝试

这题思路很简单, 就是求出mid之后然后和两端元素比较, 判断最小值在mid左边还是右边.

重点是处理重复的元素. 如果`numbers[mid] == numbers[begin]`或`numbers[end]`会怎样.

我一开始是同时用了三个元素来判断, `numbers[mid], numbers[begin], numbers[end]`

+ 如果`numbers[begin] == numbers[mid]`, 就将begin右移直到不等于`numbers[mid]`
+ 如果`numbers[end] == numbers[mid]`, 就将end左移直到不等于`numbers[mid]`

+ 如果`numbers[mid] > numbers[begin] `就说明最小值在mid到end中
+ 如果`numbers[mid] < numbers[end]`就说明最小值在begin到mid中.

后来发现这三个元素是不必要的. 只需要判断`numbers[mid], numbers[end]`即可. 因为比`numbers[end]`大并且比`numbers[begin]`小的元素是不存在的. 只要比`numbers[end]`大就可以说明最小值在`mid`到`end`中了.

而且, 如果碰到了`numbers[end] == numbers[mid]`的情况, 只需要end左移一次就好, 下一次迭代重新计算mid和end就好, 不用一直移动end直到`numbers[end] != numbers[mid]`. 时间上不会有任何提升而且会使得逻辑错误.

```java
//失败代码, 最后被numbers[mid] == numbers[end]和numbers[mid] == numbers[begin]这种情况搞得一团糟
//并且会得到错误的解! 反例[10,1,10,10,10]
//第一次有begin = 0, mid = 2, end = 4
//然后因为numbers[mid] == numbers[end]和numbers[mid] == numbers[begin]不断地右移begin和左移end
//得到begin = 1, mid = 2, end = 2
//然后因为有判断numbers[mid] > numbers[begin]成立, 进入mid到end中查找最小值, 得到10. 但是事实上错过了最小值1
class Solution {
    public int minArray(int[] numbers) {
        if(numbers == null || numbers.length == 0)
            throw new RuntimeException("InvalidInputException");
        int begin = 0;
        int end = numbers.length - 1;
        if(numbers[end] > numbers[begin])
            return numbers[begin];
        while(begin < end)
        {
            int mid = begin + (end - begin) / 2;
            while(numbers[mid] == numbers[begin] && begin < mid)
            {
                ++begin;
            }
            while(numbers[mid] == numbers[end] && end > mid)
            {
                --end;
            }
            if(numbers[mid] > numbers[begin])
            {
                begin = mid;
            }
            else if(numbers[mid] < numbers[end])
            {
                end = mid;
            }
        }
        return Math.min(numbers[begin], numbers[end]);
    }
}
```

## 2. 正确解法

只需要判断`numbers[mid], numbers[end]`即可. 因为比`numbers[end]`大并且比`numbers[begin]`小的元素是不存在的. 只要比`numbers[end]`大就可以说明最小值在`mid`到`end`中了.

而且, 如果碰到了`numbers[end] == numbers[mid]`的情况, 只需要end左移一次就好, 下一次迭代重新计算mid和end就好, 不用一直移动end直到`numbers[end] != numbers[mid]`. 时间上不会有任何提升而且会使得逻辑错误.

```java
class Solution {
    public int minArray(int[] numbers) {
        if(numbers == null || numbers.length == 0)
            throw new RuntimeException("InvalidInputException");
        int begin = 0;
        int end = numbers.length - 1;
        if(numbers[end] > numbers[begin])
            return numbers[begin];
        while(begin < end)
        {
            int mid = begin + (end - begin) / 2;
            if(numbers[mid] > numbers[end])
            {
                begin = mid + 1;
            }
            else if(numbers[mid] < numbers[end])
            {
                end = mid;
            }
            else{
                --end;
            }
        }
        return numbers[begin];
    }
}
```



