# 404 Sum of Left Leaves

### Problem

Find the sum of all left leaves in a given binary tree.

Example:

```
    3
   / \
  9  20
    /  \
   15   7
```

There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.

### Solutions

```cpp
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    void dfs(TreeNode* r, bool left)
    {
        if(!r) return;

        if(r and !r->left and !r->right and left)
            res += r->val;

        dfs(r->left, true);
        dfs(r->right, false);
    }

     int sumOfLeftLeaves(TreeNode* root) {
        dfs(root, false); 
        return res;
    }
}
```



