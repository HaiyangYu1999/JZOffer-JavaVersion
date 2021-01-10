输入数字 n，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。

示例 1:
```
输入: n = 1
输出: [1,2,3,4,5,6,7,8,9]
```

说明：

`用返回一个整数列表来代替打印`
`n 为正整数`



## 1. 大数运算

这题要是按照leetcode来, 肯定不行. leetcode上面的函数是`public int[] printNumbers(int n)`. 因为返回值是`int[]`, 自然不需要考虑int溢出的问题. 没法达到使用大数的效果. 

还是自己写一个吧. 这题的关键在于

+ 是否判断n<=0的情况
+ 字符串表示的数的加1操作

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<String> printNumbers(int n) {
        if(n <= 0){
            return new ArrayList<>();
        }
        String begin = "1";
        List<String> list = new ArrayList<>();
        while (begin.length() <= n)
        {
            list.add(begin);
            begin = stringIncrement(begin);
        }
        return list;
    }

    private String stringIncrement(String s){
        char[] chars = s.toCharArray();
        int i = chars.length - 1;
        while(i >= 0)
        {
            if (chars[i] != '9')
                break;
            --i;
        }
        if(i == -1)
        {
            char[] ans = new char[chars.length + 1];
            ans[0] = '1';
            for(int k = 1; k < ans.length; ++k)
            {
                ans[k] = '0';
            }
            return  String.valueOf(ans);
        }
        else
        {
            chars[i] += 1;
            for(int j = i + 1; j < chars.length; ++j)
            {
                chars[j] = '0';
            }
            return String.valueOf(chars);
        }
    }

    public static void main(String[] args) {
        //test cases
        // n = 0, -1, 1, 2, 3
        Solution s = new Solution();
        System.out.println(s.printNumbers(2));
    }
}
```

