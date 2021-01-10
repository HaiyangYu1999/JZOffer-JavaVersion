输入整数数组 arr ，找出其中最小的 k 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

 

示例 1：
```
输入：arr = [3,2,1], k = 2
输出：[1,2] 或者 [2,1]
```
示例 2：
```
输入：arr = [0,1,2,1], k = 1
输出：[0]
```

限制：
```
0 <= k <= arr.length <= 10000
0 <= arr[i] <= 10000
```



<!--more-->

## 分治

同样是用快速排序的思想, 先找到一个哨兵cursor, 这里我们就定义为数组中的第0个元素.

和快速排序一样, 把比cursor小的移到数组左边, 比cursor大的移到右边. 记录cursor的位置.

现在, 令size = cursor_index - begin, size 就是所有比cursor小的元素的个数了.

如果k == size, 那么从begin到cursor_index-1的这size个数即为所求.

如果k == size+1, 那么从begin到cursor_index的这size+1个数即为所求.

如果k > size + 1, 那么先把从begin到cursor_index的这size+1个数放到结果集中, 然后再从cursor_index+1到end中找top k - size - 1个最小的数即可

如果k < size, 那么k个最小的数一定在begin到cursor_index-1这个范围中, 再次从这个范围中寻找k个最小的数即可

这个方法时间复杂度为O(n)

而优先队列(二叉堆)的复杂度为O(n*logk), 并且占用O(k)的额外空间. 所以一般情况下这种方法更优. 

```java
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        if(arr == null){
            throw new NullPointerException();
        }
        List<Integer> list = new ArrayList<>();
        getLeastNumbers(arr, 0, arr.length - 1, k, list);

        int[] res = new int[list.size()];
        int s = 0;
        for(int i : list){
            res[s++] = i;
        }
        return res;
    }

    private void getLeastNumbers(int[] arr, int begin, int end, int k, List<Integer> list){
        if(k < 0 || end - begin + 1 < k){
            throw new IllegalArgumentException();
        }
        if(k == 0){
            return;
        }

        int cursor = arr[begin];
        int i = begin;
        int j = end;
        while(i < j){
            while(i < end && arr[i] <= cursor){
                ++i;
            }
            while(j > begin && arr[j] >= cursor){
                --j;
            }
            if(i >= j){
                break;
            }
            int tmp = arr[i];
            arr[i] = arr[j];
            arr[j] = tmp;
        }
        int tmp = arr[j];
        arr[j] = arr[begin];
        arr[begin] = tmp;

        int size = j - begin;     // count that arr[i] < cursor
        if(size == k){
            for(int s = begin; s < j; ++s){
                list.add(arr[s]);
            }
        }
        else if(size + 1 == k){
            for(int s = begin; s <= j; ++s){
                list.add(arr[s]);
            }
        }
        else if(k > size + 1){
            for(int s = begin; s <= j; ++s){
                list.add(arr[s]);
            }
            getLeastNumbers(arr, j + 1, end, k - size - 1, list);
        }
        else{
            getLeastNumbers(arr, begin, j - 1, k, list);
        }
    }
}
```

