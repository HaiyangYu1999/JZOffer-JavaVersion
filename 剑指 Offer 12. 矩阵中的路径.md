请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）。
```
[["a","b","c","e"],
["s","f","c","s"],
["a","d","e","e"]]
```
但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。

 

示例 1：
```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
```
示例 2：
```
输入：board = [["a","b"],["c","d"]], word = "abcd"
输出：false
```
提示：

`1 <= board.length <= 200`
`1 <= board[i].length <= 200`



## 1. DFS

普通的dfs做法. 创建一个boolean二维数组, 储存某个元素是否遍历过.

```java
class Solution {
    public boolean exist(char[][] board, String word) {
        if(board == null || board.length == 0 || board[0].length == 0)
            return false;
        final int m = board.length;
        final int n = board[0].length;
        boolean[][] isTraversed = new boolean[m][n];
        for(int i = 0; i < m; ++i)
        {
            for(int j = 0; j < n; ++j)
            {
                if(exists(board, i, j, word, 0, isTraversed))
                    return true;
            }
        }
        return false;
    }
    private boolean exists(char[][] board, int i, int j, String word, int stringIndex, boolean[][] isTraversed)
    {
        if(stringIndex == word.length())
            return true;
        if(!isValid(board, i, j) || board[i][j] != word.charAt(stringIndex) || isTraversed[i][j])
        {
            return false;
        }
        isTraversed[i][j] = true;
        boolean ans = exists(board, i+1, j, word, stringIndex+1, isTraversed) || exists(board, i-1, j, word, stringIndex+1, isTraversed) || exists(board, i, j+1, word, stringIndex+1, isTraversed) || exists(board, i, j-1, word, stringIndex+1, isTraversed);
        isTraversed[i][j] = false;
        return ans;

    }
    private boolean isValid(char[][] board, int i, int j)
    {
        return i >= 0 && i < board.length && j >= 0 && j < board[i].length;
    }
}
```

## 2. 空间复杂度为O(1)的算法

这次不用多余的数组存储某个元素有没有被遍历过了.

我们直接用`'\0'`来表示一个字符被遍历过. 完成dfs之后再把这个位置的`'\0'`改回原来的字符

注意, 必须保证原来的矩阵中没有`'\0'`. 否则会出错

```java
class Solution {
    public boolean exist(char[][] board, String word) {
        if(board == null || board.length == 0 || board[0].length == 0)
            return false;
        final int m = board.length;
        final int n = board[0].length;
        boolean[][] isTraversed = new boolean[m][n];
        for(int i = 0; i < m; ++i)
        {
            for(int j = 0; j < n; ++j)
            {
                if(exists(board, i, j, word, 0))
                    return true;
            }
        }
        return false;
    }
    private boolean exists(char[][] board, int i, int j, String word, int stringIndex)
    {
        if(stringIndex == word.length())
            return true;
        if(!isValid(board, i, j) || board[i][j] != word.charAt(stringIndex))
        {
            return false;
        }

        char tmp = board[i][j];
        board[i][j] = '\0';       //equals isTraversed[i][j] = true;
        
        boolean ans = exists(board, i+1, j, word, stringIndex+1) || exists(board, i-1, j, word, stringIndex+1) || exists(board, i, j+1, word, stringIndex+1) || exists(board, i, j-1, word, stringIndex+1);
        
        board[i][j] = tmp;       //equals isTraversed[i][j] = false;
        
        return ans;

    }
    private boolean isValid(char[][] board, int i, int j)
    {
        return i >= 0 && i < board.length && j >= 0 && j < board[i].length;
    }
}
```

