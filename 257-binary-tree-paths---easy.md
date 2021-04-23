# 257 Binary Tree Paths - Easy

### Problem
Given a binary tree, return all root-to-leaf paths.

For example, given the following binary tree:

```
   1
 /   \
2     3
 \
  5
```

All root-to-leaf paths are:

["1->2->5", "1->3"]

### Solutions:

```
class Solution {
public:
    vector<string> res;

    void btp(TreeNode* r, string str)
    {
        if(!r) return;

        if( str != "")
            str = str + "->" + to_string(r->val);
        else
            str = to_string(r->val);

        if( r->left == nullptr  && r->right ==  nullptr)
        {
            res.push_back(str);
            return;
        }
        btp(r->left, str);
        btp(r->right, str);
    }
    vector<string> binaryTreePaths(TreeNode* root) {
        btp(root, "");
        return res;
    }
};
```
