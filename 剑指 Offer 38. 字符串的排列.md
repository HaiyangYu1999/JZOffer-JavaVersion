输入一个字符串，打印出该字符串中字符的所有排列。

 

你可以以任意顺序返回这个字符串数组，但里面不能有重复元素。

 

示例:
```
输入：s = "abc"
输出：["abc","acb","bac","bca","cab","cba"]
```

限制：

`1 <= s 的长度 <= 8`



<!--more-->

## 1. DFS

直接DFS, 不剪枝, 比较慢. 

如果剪枝的话, 就要判断当前元素chars[i]的前面是否存在相同的元素chars[j], (j < i), 并且isTraversed[j]为false. 如果这些条件成立, 可以进行剪枝, 即跳过当前元素chars[i]. 

但是剪枝也会消耗一定的复杂度. 如果能确定几乎没有重复, 还是不剪枝快一些. 下面同时放上剪枝和非剪枝的代码.

```java
class Solution {
    public String[] permutation(String s) {
        if(s == null){
            throw new NullPointerException();
        }
        if("".equals(s)){
            return new String[0];
        }
        Set<String> ans = new HashSet<>();
        char[] chars = s.toCharArray();
        boolean[] isTraversed = new boolean[s.length()];
        char[] curr = new char[s.length()];
        int currLen = 0;
        dfs(ans, curr, 0, chars, isTraversed);
        return ans.toArray(new String[0]);
    }

    private void dfs(Set<String> ans, char[] curr, int currLen, char[] chars, boolean[] isTraversed){
        if(currLen == curr.length){
            ans.add(new String(curr));
            return;
        }
        else{
            for(int i = 0; i < curr.length; ++i){
                if(!isTraversed[i]){
                    isTraversed[i] = true;
                    curr[currLen] = chars[i];
                    dfs(ans, curr, currLen + 1, chars, isTraversed);
                    isTraversed[i] = false;
                }
            }
        }
    }
}
```



```java
class Solution {
    public String[] permutation(String s) {
        if(s == null){
            throw new NullPointerException();
        }
        if("".equals(s)){
            return new String[0];
        }
        Set<String> ans = new HashSet<>();
        char[] chars = s.toCharArray();
        boolean[] isTraversed = new boolean[s.length()];
        char[] curr = new char[s.length()];
        int currLen = 0;
        dfs(ans, curr, 0, chars, isTraversed);
        return ans.toArray(new String[0]);
    }

    private void dfs(Set<String> ans, char[] curr, int currLen, char[] chars, boolean[] isTraversed){
        if(currLen == curr.length){
            ans.add(new String(curr));
            return;
        }
        else{
            for(int i = 0; i < curr.length; ++i){
                if(!isTraversed[i]){

                    boolean flag = false;
                    for(int k = 0; k < i; ++k){
                        if(chars[k] == chars[i] && !isTraversed[k]){
                            flag = true;
                        }
                    }
                    if(flag){
                        continue;
                    }
                    
                    isTraversed[i] = true;
                    curr[currLen] = chars[i];
                    dfs(ans, curr, currLen + 1, chars, isTraversed);
                    isTraversed[i] = false;
                }
            }
        }
    }
}
```

