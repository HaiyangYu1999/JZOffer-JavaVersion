给定一棵二叉搜索树，请找出其中第k大的节点。

<!--more-->

### 中序遍历 + 提前返回

设置一个变量isFound, 找到了就把这个变量设为True. 并且直接返回, 不继续找了

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int kthLargest(TreeNode* root, int k) {
        int ans = 0;
        bool isFound = false;
        nthLargest(root, k, ans, isFound);
        return ans;
    }
private:
    void nthLargest(TreeNode *root, int& k, int& ans, bool& isFound){
        if(isFound || root == nullptr){
            return;
        }
        nthLargest(root->right, k, ans, isFound);
        if(isFound){
            return;
        }
        if(k == 1){
            ans = root->val;
            isFound = true;
            return;
        }
        --k;
        nthLargest(root->left, k, ans, isFound);
    }
};
```

