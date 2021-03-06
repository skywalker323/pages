# 333 Largest BST Subtree

### Problem:

<pre>
Given a binary tree, find the largest subtree which is a Binary Search Tree (BST), where largest means subtree with largest number of nodes in it.

Note:
A subtree must include all of its descendants.
Here's an example:
    10
    / \
   5  15
  / \   \ 
 1   8   7
The Largest BST Subtree in this case is the highlighted one. 
The return value is the subtree's size, which is 3.
Hint:

You can recursively use algorithm similar to 98. Validate Binary Search Tree at each node of the tree, which will result in O(nlogn) time complexity.
Follow up:
Can you figure out ways to solve it with O(n) time complexity?
</pre>

### Solutions:

```java
class Solution {
public:
    int largestBSTSubtree(TreeNode* root) {
        return get<2>(traversal(root));
    }
    tuple<int,int,int> traversal(TreeNode* cur) {        
        if(!cur) return {INT_MIN, INT_MAX, 0};

        auto [lmax, lmin, lval] = traversal(cur->left);
        auto [rmax, rmin, rval] = traversal(cur->right);
        
        if(lmax >= cur->val || rmin <= cur->val) return {INT_MAX,INT_MIN,max(lval, rval)};

        return {max(rmax, cur->val), min(lmin, cur->val), rval+lval+1};
    }
};
```