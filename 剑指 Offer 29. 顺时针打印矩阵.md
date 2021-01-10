输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

 

示例 1：
```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```
示例 2：
```
输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]
```

限制：

`0 <= matrix.length <= 100`
`0 <= matrix[i].length <= 100`

<!--more-->

## 1. 模拟

可以模拟顺时针的过程, 但是要新建一个矩阵储存哪些元素被遍历过了哪些没有.

规则是: 

对于某个location{i,j}, 分别检查左边,右边, 上边,下边的元素是不是还没有遍历过并且没有超出边界.

  [ 1,  2,  **3**,  4],
  [ 5,  6,  7,  8],
  [ **9**, 10,11,**12**],
  [13,**14**,15,16]


1. 如果一个元素的右边和下边都没有遍历 接下来要先遍历右边 对于上图元素3,接下来要遍历4
2. 如果一个元素的下边和左边都没有遍历 接下来要先遍历下边 对于上图元素12,接下来要遍历16
3. 如果一个元素的左边和上边都没有遍历 接下来要先遍历左边 对于上图元素14, 接下来要遍历13
4. 如果一个元素的上边和右边都没有遍历 接下来要先遍历上边 对于上图元素9, 接下来要遍历5
5. 其他情况, 对应角上的元素, 例如4,16,13 只有一个方向没有遍历过. (4只能往下走, 16只能往左走, 13只能往上走) 那就遍历这个方向.

这种方法虽然直观, 但是代码实现很复杂.

```c++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> vc;
        if(matrix.empty() || matrix.front().empty())
            return vc;
        
        pair<int,int> pr = {0,0};
        vc.push_back(matrix[0][0]);
        matrix[0][0] = INT_MIN;
        
        while(hasNextPair(matrix,pr))
        {
            getNextPair(matrix,pr);
            vc.push_back(matrix[pr.first][pr.second]);
            matrix[pr.first][pr.second] = INT_MIN;
        }
        return vc;
    }
private:
    bool hasNextPair(const vector<vector<int>>& m, const pair<int,int>& pr)
    {
        return isRightAvailable(m,pr) || isDownAvailable(m,pr) || isLeftAvailable(m,pr) || isUpAvailable(m,pr);
    }
    
    void getNextPair(const vector<vector<int>>& m, pair<int,int>& pr)
    {
        if(isRightAvailable(m,pr) && isDownAvailable(m,pr))
        {
            ++pr.second;
        }
        else if(isDownAvailable(m,pr) && isLeftAvailable(m,pr))
        {
            ++pr.first;
        }
        else if(isLeftAvailable(m,pr) && isUpAvailable(m,pr))
        {
            --pr.second;
        }
        else if(isUpAvailable(m,pr) && isRightAvailable(m,pr))
        {
            --pr.first;
        }
        else if(isRightAvailable(m,pr))
        {
            ++pr.second;
        }
        else if(isDownAvailable(m,pr))
        {
            ++pr.first;
        }
        else if(isLeftAvailable(m,pr))
        {
            --pr.second;
        }
        else if(isUpAvailable(m,pr))
        {
            --pr.first;
        }
    }
    
    bool isRightAvailable(const vector<vector<int>>& m, const pair<int,int>& pr)
    {
        int i = pr.first;
        int j = pr.second;
        return (j + 1 < m.front().size()) && (m[i][j+1] != INT_MIN);
    }
    bool isDownAvailable(const vector<vector<int>>& m, const pair<int,int>& pr)
    {
        int i = pr.first;
        int j = pr.second;
        return (i + 1 < m.size()) && (m[i+1][j] != INT_MIN);
    }
    bool isLeftAvailable(const vector<vector<int>>& m, const pair<int,int>& pr)
    {
        int i = pr.first;
        int j = pr.second;
        return (j - 1  >= 0) && (m[i][j-1] != INT_MIN);
    }
    bool isUpAvailable(const vector<vector<int>>& m, const pair<int,int>& pr)
    {
        int i = pr.first;
        int j = pr.second;
        return (i - 1 >= 0) && (m[i-1][j] != INT_MIN);
    }
};
```

## 2. 按圈遍历

一圈一圈地遍历, 从最外圈到最内圈

先用4个变量标记位置信息, 

```java
int beginRow = 0;
int endRow = m - 1;
int beginColumn = 0;
int endColumn = n - 1;
```

这4个变量可以唯一确定每个圈地位置.

遍历的顺序就是先遍历上面的

```java
for(int j = beginColumn; j <= endColumn; ++j){
	ans[k++] = matrix[beginRow][j];
}
```

再遍历右边的

```java
for(int i = beginRow + 1; i <= endRow; ++i){
	ans[k++] = matrix[i][endColumn];
}
```

再遍历下边的(注意边界条件, 如果当前的圈只有一行, 即`beginRow == endRow`, 就不要遍历了, 要不然就相当于这一行遍历了2遍)

```java
if(beginRow < endRow){
    for(int j = endColumn - 1; j >= beginColumn; --j){
        ans[k++] = matrix[endRow][j];
    }
}
```

再遍历左边的(注意边界条件, 如果当前的圈只有一列, 即`beginColumn == endColumn`, 就不要遍历了, 要不然就相当于这一列遍历了2遍)

```java
if(beginColumn < endColumn){
    for(int i = endRow - 1; i > beginRow; --i){
        ans[k++] = matrix[i][beginColumn];
    }
}
```

同时还要注意四个角的元素, 因为这些元素在两个边的相交处, 所以要小心地处理, 保证只被遍历一次

最后递增`beginRow, beginColumn`, 递减`endRow, endColumn`, 直到不符合条件`beginRow <= endRow && beginColumn <= endColumn`退出while

```java
class Solution {
    public int[] spiralOrder(int[][] matrix) {
        if(matrix == null || matrix.length == 0 || matrix[0] == null || matrix[0].length == 0){
            return new int[]{};
        }
        int m = matrix.length;
        int n = matrix[0].length;
        int size = m * n;
        int[] ans = new int[size];
        int k = 0;
        int beginRow = 0;
        int endRow = m - 1;
        int beginColumn = 0;
        int endColumn = n - 1;
        while(beginRow <= endRow && beginColumn <= endColumn){
            for(int j = beginColumn; j <= endColumn; ++j){
                ans[k++] = matrix[beginRow][j];
            }
            for(int i = beginRow + 1; i <= endRow; ++i){
                ans[k++] = matrix[i][endColumn];
            }
            if(beginRow < endRow){
                for(int j = endColumn - 1; j >= beginColumn; --j){
                    ans[k++] = matrix[endRow][j];
                }
            }
            if(beginColumn < endColumn){
                for(int i = endRow - 1; i > beginRow; --i){
                    ans[k++] = matrix[i][beginColumn];
                }
            }
            ++beginRow;
            --endRow;
            ++beginColumn;
            --endColumn;
        }
        return ans;
    }

}
```

