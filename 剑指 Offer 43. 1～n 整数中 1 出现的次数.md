输入一个整数 n ，求1～n这n个整数的十进制表示中1出现的次数。

例如，输入12，1～12这些整数中包含1 的数字有1、10、11和12，1一共出现了5次。

 

示例 1：
```
输入：n = 12
输出：5
```
示例 2：
```
输入：n = 13
输出：6
```

限制：
```
1 <= n < 2^31
```

<!--more-->

### 按位累计

先统计从1-n的个位数的1的总和, 再统计10位, 直至统计完所有位.

懒得写题解了, 贴一个吧. 思路和我写的还挺像的

`https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/solution/mian-shi-ti-43-1n-zheng-shu-zhong-1-chu-xian-de-2/`

```c++
class Solution {
public:
    int countDigitOne(int n) {
        long digit = 1;
        int count1 = 0;
        while(n / digit){
            long base = digit * 10;
            int low = n % base;
            int high = n / base;
            int currentDigit = low / digit;
            int res = 0;
            if(currentDigit == 1){
                res = low + 1 - digit;
            }
            else if(currentDigit > 1){
                res = digit;
            }
            else{
                res = 0;
            }
            count1 += high * digit + res;
            // cout<< high << " " << digit << " " << res <<endl;
            digit *= 10;
        }
        return count1;
    }
};
```

