# 366 Find Leaves of Binary Tree

### Problem:

Given a binary tree, collect a tree's nodes as if you were doing this: Collect and remove all leaves, repeat until the tree is empty.

Example:
Given binary tree 
<pre>
          1
         / \
        2   3
       / \     
      4   5    
</pre>

Returns [4, 5, 3], [2], [1].

Explanation:
1 Removing the leaves [4, 5, 3] would result in this tree:
<pre>
          1
         / 
        2     
</pre>
     
2 Now removing the leaf [2] would result in this tree:
<pre>
          1          
</pre>

3 Now removing the leaf [1] would result in the empty tree:
<pre>
          []         
</pre>


Returns [4, 5, 3], [2], [1].

### Solutions:

```java
class Solution {
public:
    vector<vector<int>> findLeaves(TreeNode* root) {
        vector<vector<int>> res;
        vector<int> t;
        while(root){
            root = dfs(root, t);
            res.push_back(t);
            t.clear();
        }
        return res;
    }
    TreeNode* dfs(TreeNode* node, vector<int> &v){
        if(!node) return NULL;
        if(!node->left && !node->right){
            v.push_back(node->val);
            node = NULL;
            return NULL;
        }
        node->left = dfs(node->left, v);
        node->right = dfs(node->right, v);
        return node;
    }
};
```
