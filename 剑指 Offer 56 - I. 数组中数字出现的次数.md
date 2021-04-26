一个整型数组 nums 里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是O(n)，空间复杂度是O(1)。

 

示例 1：

输入：nums = [4,1,4,6]
输出：[1,6] 或 [6,1]
示例 2：

输入：nums = [1,2,10,4,1,4,3,3]
输出：[2,10] 或 [10,2]


限制：

2 <= nums.length <= 10000





### 位运算

假设数组中只有a和b出现了一次, 那么数组所有元素异或的结果为`a^b`

例如数组`[1,1,2,2,3,3,4,4,5,5,9,15]`

最后异或出来的结果就为`9^15 = 0x110`

异或结果最后一位不为0的是从右向左第二位. `0x10` 记这个结果为`lastDigit`

根据异或的定义, **a或b必有一个与`lastDigit`进行`&`操作结果为0, 另外一个与`lastDigit`进行`&`操作结果不为0**

这样可以把数组分为两部分, 和`lastDigit`进行`&`操作结果为0是一部分, 不为0的是另一部分, 

`[1,1,2,2,3,3,4,4,5,5,9,15] => [1,1,4,4,9], [2,2,3,3,15]`

这样就可以分别求出a和b了

```c++
class Solution {
public:
    vector<int> singleNumbers(vector<int>& nums) {
        int n = 0;
        for(int i : nums){
            n ^= i;
        }
        int lastDigit = 1;
        while(!(lastDigit & n)){
            lastDigit <<= 1;
        }
        int a = 0;
        int b = 0;
        for(int i : nums){
            if(i & lastDigit){
                a ^= i;
            }
            else{
                b ^= i;
            }
        }
        return vector<int>({a,b});
    }
};
```

