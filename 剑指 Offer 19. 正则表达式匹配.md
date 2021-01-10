请实现一个函数用来匹配包含'. '和'*'的正则表达式。模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（含0次）。在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但与"aa.a"和"ab*a"均不匹配。

示例 1:

输入:
```
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。
```
示例 2:
```
输入:
s = "aa"
p = "a*"
输出: true
解释: 因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。
```
示例 3:
```
输入:
s = "ab"
p = ".*"
输出: true
解释: ".*" 表示可匹配零个或多个（'*'）任意字符（'.'）。
```
示例 4:
```
输入:
s = "aab"
p = "c*a*b"
输出: true
解释: 因为 '*' 表示零个或多个，这里 'c' 为 0 个, 'a' 被重复一次。因此可以匹配字符串 "aab"。
```
示例 5:
```
输入:
s = "mississippi"
p = "mis*is*p*."
输出: false
```
s 可能为空，且只包含从 a-z 的小写字母。
p 可能为空，且只包含从 a-z 的小写字母以及字符 . 和无连续的 '*'。

<!--more-->

## 1. 动态规划

还是动态规划好理解

`dp[i][j]`表示字符串`s`的字串`[0, ..., i-1]`是不是与正则表达式`p`的字串`[0, ..., j-1]`是不是匹配.

注意这里代表的是不包含第`i`个元素和第`j`个元素的字串.

所以`dp[i][j]`为true的条件就是`s`的字串`s[0, ..., i-1]`和`p`的字串`p[0, ..., j-1]`匹配

显然p字串的最后一个元素`p[j-1]`很关键. 

+ 如果`p[j-1] == '.'`, 那么这个`'.'`必然消耗s的一个字符, 并且是什么字符都行. 所以只要`s[0, ..., i-2]`和`p[0, ..., j-2]`匹配即可. 即只要`dp[i][j] = dp[i-1][j-1]`
+ 如果`p[j-1]`为一个普通字符, 普通字符也必须消耗s中的一个字符, 并且消耗的字符必须和这个字符相等. 所以需要`s[0, ..., i-2]`和`p[0, ..., j-2]`匹配, 并且`s[i-1] == p[j-1]`. 即`dp[i][j] = dp[i-1][j-1] && s[i-1] == p[j-1]`

+ 如果`p[j-1] == '*'`, 那么我们需要找到星号前的字符`charBeforeStar`. 并且星号也可以匹配0个字符或多个字符.
  + 如果星号匹配0个字符, 那么就要保证`dp[i][j-2]`为true. 例如`s = "123"`, `p = ".234*"`. 判断`.234*`能不能匹配`123`只需要判断`.23`能不能匹配`123`.
  + 如果星号匹配多个字符, 那么就要保证星号代表的字符和s里面的字符相等, 即`charBeforeStar == '.' || charBeforeStar == s.charAt(i-1)`. 并且要保证`dp[i-1][j]`为true. 例如`s = "123"`, `p = "123*"` 这时`charBeforeStar`为'3', 并且s的最后一个字符也为3, 所以只需要保证`"123*"`能不能匹配`"12"`即可

同时还要先求出边界条件. 显然空正则表达式可以匹配空字符串`dp[0][0] = true`

空的正则表达式不能匹配任何非空字符串. `dp[i][0] = false (i > 0)`

普通的正则表达式只有在特殊情况下才能匹配空字符串. 特殊情况即为一个普通字符一个星号组成的正则表达式. 例如`"1*.*2*3*a*"`这样的. 并且所有的星号都按0次匹配来计算. 所以`dp[0][j] = (p.charAt(j-1) == '*') && dp[0][j-2]`

```java
class Solution {
    public boolean isMatch(String s, String p) {
        if(s == null || p == null){
            throw new NullPointerException();
        }
        int sLen = s.length();
        int pLen = p.length();
        boolean[][] dp = new boolean[sLen+1][pLen+1];
        dp[0][0] = true;
        for(int i = 1; i <= sLen; ++i){
            dp[i][0] = false;
        }
        for(int j = 1; j <= pLen; ++j){
            if(p.charAt(j-1) == '*'){
                if(j - 2 < 0){
                    throw new IllegalArgumentException("Invalid REGEX that starts with '*'.");
                }
                dp[0][j] = dp[0][j-2];
            }
            else{
                dp[0][j] = false;
            }
        }
        for(int i = 1; i <= sLen; ++i){
            for(int j = 1; j <= pLen; ++j){
                if(p.charAt(j - 1) == '*'){
                    if(j - 2 < 0){
                        throw new IllegalArgumentException("Invalid REGEX that starts with '*'.");
                    }
                    int charBeforeStar = p.charAt(j-2);
                    boolean notUseStar = dp[i][j-2];
                    boolean useStar = (charBeforeStar == '.' || charBeforeStar == s.charAt(i-1)) && dp[i-1][j];
                    dp[i][j] = notUseStar || useStar;
                }
                else if(p.charAt(j-1) == '.'){
                    dp[i][j] = dp[i-1][j-1];
                }
                else{
                    dp[i][j] = dp[i-1][j-1] && (s.charAt(i-1) == p.charAt(j-1));
                }
            }
        }
        return dp[sLen][pLen];
    }
}
```

## 2. 递归

根据上面的分析, 写出递归的程序就很简单了.

但是速度相比就慢很多了.**(一顿操作猛如虎, 一看击败百分五)** 个人推测应该是因为星号变多的时候会发生指数爆炸.

例如 `s = "aaaaaaaaaaaaab", p = "a*a*a*a*a*a*a*"`

每一次都走`return isMatch(s.substring(1), p) || isMatch(s, p.substring(2));`这一条逻辑分支. 最坏情况时复杂度将会为O(2^n)!

同时, 每一次传递参数都会经过string的构造和复制, 也对速度造成了很大的影响. 

**怀念C的指针, 虽然出了错很难调试, 但是确实快啊**

```java
class Solution {
    public boolean isMatch(String s, String p) {
        if(s == null || p == null){
            throw new NullPointerException();
        }
        if("".equals(p)){
            return "".equals(s);
        }
        if("".equals(s)){
            return isMatchEmptyString(p);
        }
        if(p.startsWith("*")){
            throw new IllegalArgumentException("Invalid REGEX that starts with '*'.");
        }
        if(p.length() > 1 && p.charAt(1) == '*'){
            if(p.charAt(0) == '.' || p.charAt(0) == s.charAt(0)){
                return isMatch(s.substring(1), p) || isMatch(s, p.substring(2));
            }
            else{
                return isMatch(s, p.substring(2));
            }
        }
        else if(p.charAt(0) == '.'){
            return isMatch(s.substring(1), p.substring(1));
        }
        else{
            return (s.charAt(0) == p.charAt(0)) && isMatch(s.substring(1), p.substring(1));
        }
    }
    private boolean isMatchEmptyString(String p){
        if(p == null){
            throw new NullPointerException();
        }
        if(p.length() == 0){
            return true;
        }
        if(p.length() == 1){
            return false;
        }
        if(p.length() > 1 && p.charAt(1) == '*'){
            return isMatchEmptyString(p.substring(2));
        }
        return false;
    }
}
```

