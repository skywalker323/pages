# 113 Path Sum II – Medium

### Problem:

Given a binary tree and a sum, find all root-to-leaf paths where each path’s sum equals the given sum.

For example:  
Given the below binary tree and sum = 22,

```
          5
         / \
        4   8
       /   / \
      11  13  4
     /  \    / \
    7    2  5   1
```

return

\[  
   \[5,4,11,2\],  
   \[5,8,4,5\]  
\]

### Thoughts:

The only addition comparing to the version I of this problem is that instead of returning a boolean value, we need to have the exact path of numbers that has the given sum.

Approach will be very similar but need a ArrayList to keep current potential solution.

### Solutions:

```java
vector<vector<int>> res;
    
    void pathSum(TreeNode* root, int ts, vector<int>& temp)
    {
        if( !root) return;
        if(!root->left and !root->right and root->val == ts) { 
            temp.push_back(root->val); 
            res.push_back(temp) ;
            temp.pop_back();
            return;
        }
        temp.push_back(root->val);        
        pathSum(root->left, ts-root->val, temp) ;
        pathSum(root->right, ts-root->val, temp) ;
        temp.pop_back();        
    }
    
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        
        vector<int> temp;
        pathSum(root, targetSum, temp);
        return res;
    }
```



