# 515 Find Largest Value in Each Tree Row

### Problem

You need to find the largest value in each row of a binary tree.

Example:

```
Input: 

          1
         / \
        3   2
       / \   \  
      5   3   9 

Output: [1, 3, 9]
```

### Solutions

```cpp
class Solution {
public:
    vector<int> largestValues(TreeNode* root) {
        vector<int> maxs;
        find(root, 0, maxs);
        return maxs;
    }

private:
    void find(TreeNode* node, int row, vector<int>& maxs) {
        if (!node) {
            return;
        }

        if (row >= maxs.size()) {
            maxs.push_back(node->val);
        }
        else {
            maxs[row] = max(maxs[row], node->val);
        }

        find(node->left, row + 1, maxs);
        find(node->right, row + 1, maxs);
    }
};
Post Order
In preorder solution the vector have been constantly resized, and each time add 1 because we don't know how deep the tree is. If change to post order, we can resize the vector only at leaf node, this should improve the performance.

class Solution {
public:
    vector<int> largestValues(TreeNode* root) {
        vector<int> res;
        dfs(root, 1, res);
        return res;
    }

private:
    void dfs(TreeNode* node, int depth, vector<int>& res) {
        if (!node) {
            return;
        }
        dfs(node->left, depth + 1, res);
        dfs(node->right, depth + 1, res);
        if (depth > res.size()) {
            res.resize(depth, INT_MIN);
        }
        res[depth - 1] = max(res[depth - 1], node->val);
    }
};
```



