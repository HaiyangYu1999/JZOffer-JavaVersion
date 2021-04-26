给定一个数组 A[0,1,…,n-1]，请构建一个数组 B[0,1,…,n-1]，其中 B[i] 的值是数组 A 中除了下标 i 以外的元素的积, 即 B[i]=A[0]×A[1]×…×A[i-1]×A[i+1]×…×A[n-1]。不能使用除法。

 

示例:
```
输入: [1,2,3,4,5]
输出: [120,60,40,30,24]
```

提示：

所有元素乘积之和不会溢出 32 位整数
`a.length <= 100000`

### 构建辅助数组

设立辅助数组left和right, left[i]为a中0到i的乘积, right[i]为a中i到a.size()-1的乘积.

所以`b[i] = left[i-1] * right[i+1]`


```c++
class Solution {
public:
    vector<int> constructArr(vector<int>& a) {
        vector<int> left;
        int tmp = 1;
        for(int i : a){
            tmp *= i;
            left.push_back(tmp);
        }
        tmp = 1;
        deque<int> right;
        for(int i = a.size() - 1; i >= 0; --i){
            tmp *= a[i];
            right.push_front(tmp);
        }
        vector<int> b;
        for(int i = 0; i < a.size(); ++i){
            int leftProduct = (i == 0) ? 1 : left[i-1];
            int rightProduct = (i == a.size() - 1) ? 1 : right[i+1];
            b.push_back(leftProduct * rightProduct);
        }
        return b;
    }
};
```

