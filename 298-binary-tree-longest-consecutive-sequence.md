# 298. Binary Tree Longest Consecutive Sequence

### Problem:

Given a binary tree, find the length of the longest consecutive sequence path.

The path refers to any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The longest consecutive path need to be from parent to child (cannot be the reverse).

For example,
```
   1
    \
     3
    / \
   2   4
        \
         5
```
Longest consecutive sequence path is 3-4-5, so return 3.
```
   2
    \
     3
    /
   2
  /
 1
```
Longest consecutive sequence path is 2-3,not3-2-1, so return 2.

### Solutions:

```java
class Solution {
public:
    int longestConsecutive(TreeNode* root) {
        int res = 0;
        dfs(root, res); // recursively find the answer
        return res;
    }

private:
    int dfs(TreeNode* root, int& res)
    {
        if (!root)             return 0; // null pointer

        int len = 1; // longest length of path starting from current node
        int left = dfs(root->left, res); // longest path starting from left child
        int right = dfs(root->right, res); // longest path starting from right child

        if (root->left && root->val + 1 == root->left->val)
        {
            len = max(len, left + 1); // if current node and left child are consecutive, update the longest path starting from current node
        }

        if (root->right && root->val + 1 == root->right->val)
        {
            len = max(len, right + 1); // if current node and right child are consecutive, update the longest path starting from current node
        }

        res = max(res, len); // update the overall longest path
        return len; // return the longest path starting from current node
    }
};
```
