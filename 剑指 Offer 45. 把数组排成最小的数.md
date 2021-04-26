输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。

 

示例 1:
```
输入: [10,2]
输出: "102"
```
示例 2:
```
输入: [3,30,34,5,9]
输出: "3033459"
```

提示:
```
0 < nums.length <= 100
```
说明:
```
输出结果可能非常大，所以你需要返回一个字符串而不是整数
拼接起来的数字可能会有前导 0，最后结果不需要去掉前导 0
```

<!--more-->



### 字符串比较

考虑两个数字a, b拼接, 

如果ab < ba, 就把a放在前面. 这样可以保证顺序正确,

例如[3,30]. 显然303 < 330. 所以30要放在3的前面. 按照这样的顺序排序后的数组从左到右串起来就是最小的. 

这里还利用了一个特点, 当长度相等的时候字符串比较和整数比较的结果是一样的. 即 "303" < "330" 等价于 int(303) < int(330)

```c++
class Solution {
public:
    string minNumber(vector<int>& nums) {
        vector<string> strings;
        for(int i : nums){
            strings.push_back(std::to_string(i));
        }
        std::sort(strings.begin(), strings.end(), [](const string& a, const string& b){
            return a + b < b + a;
        });
        string ans = "";
        for(string& str : strings){
            ans += str;
        }
        return ans;
    }
};
```

