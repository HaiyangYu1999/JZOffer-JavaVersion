如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

例如，

[2,3,4] 的中位数是 3

[2,3] 的中位数是 (2 + 3) / 2 = 2.5

设计一个支持以下两种操作的数据结构：

void addNum(int num) - 从数据流中添加一个整数到数据结构中。
double findMedian() - 返回目前所有元素的中位数。
示例 1：
```
输入：
["MedianFinder","addNum","addNum","findMedian","addNum","findMedian"]
[[],[1],[2],[],[3],[]]
输出：[null,null,null,1.50000,null,2.00000]
示例 2：
```
```
输入：
["MedianFinder","addNum","findMedian","addNum","findMedian"]
[[],[2],[],[3],[]]
输出：[null,null,2.00000,null,2.50000]
```

限制：

`最多会对 addNum、findMedian 进行 50000 次调用。`

<!--more-->



## 1. 数据存储

把每一次的数据都放到一个排序数组中, 每插入一次消耗的时间为O(n). 类似于插入排序

然后取中位数时就只需要求`(list[n/2] + list[(n-1)/2]) / 2`即可

但是这种方法又慢空间占用也大.

```java
class MedianFinder {
    ArrayList<Integer> list;
    /** initialize your data structure here. */
    public MedianFinder() {
        list = new ArrayList<>();
    }
    
    public void addNum(int num) {
        int i = 0;
        while(i < list.size()){
            if(list.get(i) < num){
                ++i;
            }
            else{
                break;
            }
        }
        list.add(i, num);
    }
    
    public double findMedian() {
        int len = list.size();
        return (list.get(len / 2) + list.get((len - 1) / 2)) / 2.0;
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```

### 2. 2个优先队列

建立一个 **小根堆** pq1 和 **大根堆** pq2，各保存列表的一半元素. **pq1保存半部分较小的元素, pq2保存半部分较大的元素**

如果总元素是奇数(2k+1), 那么规定pq1保存k+1个元素, pq2保存k个元素

如果N是偶数, 那么pq1和pq2的根元素的平均值即为中位数

如果N是奇数, 那么pq1的根元素为中位数

下面要考虑怎样在插入num的过程中维护pq1和pq2.

分类讨论如下, 

+ 如果当前pq1和pq2元素一样多, 那么插入一个元素后要保持pq1的个数比pq2多一个. 这时num不能直接插入到pq1中, 因为如果num很大的话, 这就与**pq1保存半部分较小的元素**这个规定矛盾了. 所以要比较num和pq1.peek()的值. 如果num <= pq2.peek(), 就直接把num插到pq1中. 反之要把pq2的根元素放到pq1中. 再把num放到pq2中. 总体来说还是使得插入后pq1的元素个数比pq2多一个. 对于`num <= pq2.peek()`这个条件的理解, 可以参考插入[40, 12, 16]这三个数的过程
+ 如果当前pq1的元素比pq2多一个, 那么插入后要保持pq1和pq2元素个数相等. 还是要比较num和pq1.peek()的大小关系, 大于等于pq1.peek()就直接插入到pq2中, **插入完之后保持pq2所有元素都比pq1大**. 如果小于pq1.peek(), 要先把pq1的根元素放到pq2中, 然后再把num放到pq1中. 这样操作之后仍然**保持pq2所有元素都比pq1大, 并且pq1和pq2元素个数相等**

```java
class MedianFinder {
    PriorityQueue<Integer> pq1;
    PriorityQueue<Integer> pq2;
    /** initialize your data structure here. */
    public MedianFinder() {
        pq1 = new PriorityQueue<Integer>((a, b) -> {return b - a;});
        pq2 = new PriorityQueue<Integer>();
    }
    
    public void addNum(int num) {
        int size1 = pq1.size();
        int size2 = pq2.size();
        if(size1 > size2){
            if(num <= pq1.peek()){
                int tmp = pq1.poll();
                pq1.offer(num);
                pq2.offer(tmp);
            }
            else{
                pq2.offer(num);
            }
        }
        else{
            if(pq1.isEmpty()){
                pq1.offer(num);
            }
            else{
                if(num <= pq2.peek()){
                    pq1.offer(num);
                }
                else{
                    int tmp = pq2.poll();
                    pq2.offer(num);
                    pq1.offer(tmp);
                }
            }
        }
    }
    
    public double findMedian() {
        return (pq1.size() == pq2.size()) ? (pq1.peek() + pq2.peek()) / 2.0 : pq1.peek();
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```

