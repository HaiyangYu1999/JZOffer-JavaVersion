请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100"、"5e2"、"-123"、"3.1416"、"-1E-16"、"0123"都表示数值，但"12e"、"1a3.14"、"1.2.3"、"+-5"及"12e+5.4"都不是。

<!--more-->

## 有限状态自动机

和leetcode 65一样。 这题极其繁琐, 错一点都不行

```java
class Solution {
    private static final int S0 = 0;  // begin state
    private static final int S1 = 1;  // digits before 'e' and '.', after head '+/-'
    private static final int S2 = 2; // state to absorb the first "+/-" before 'e'
    private static final int S3 = 3; // state that absorb a "."
    private static final int S4 = 4; // digits after '.'
    private static final int S5 = 5; // state that absorb 'e'
    private static final int S6 = 6; // digits after 'e'
    private static final int S7 = 7; // state that absorb '+/-' after 'e'
    private static final int Serr = -1; //error state
    private Set<Integer> validSet = new HashSet<>();
    private int[][] tran = new int[][]{
            {1,2,3,-1},
            {1,-1,4,5},
            {1,-1,3,-1},
            {4,-1,-1,-1},
            {4,-1,-1,5},
            {6,7,-1,-1},
            {6,-1,-1,-1},
            {6,-1,-1,-1}};
    public Solution()
    {
        validSet.add(S1);
        validSet.add(S4);
        validSet.add(S6);
    }
    private int make(char ch)
    {
        if(Character.isDigit(ch))
            return 0;
        else if(ch == '+' || ch == '-')
            return 1;
        else if(ch == '.')
            return 2;
        else if(ch == 'e' || ch == 'E')
            return 3;
        else
            return -1;
    }
    public boolean isNumber(String s) {
        s = s.trim();
        if(s.isEmpty())
            return false;
        int state = S0;
        for(int i = 0; i < s.length(); ++i)
        {
            char ch = s.charAt(i);
            int curr = make(ch);
            if(curr == -1)
                return false;
            state = tran[state][curr];
            if(state == -1)
                return false;
        }
        return validSet.contains(state);
    }
}
```

