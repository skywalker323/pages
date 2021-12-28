# 549 Binary Tree Longest Consecutive Sequence II

### Problem:

Given a binary tree, you need to find the length of Longest Consecutive Path in Binary Tree.

Especially, this path can be either increasing or decreasing. For example, \[1,2,3,4\] and \[4,3,2,1\] are both considered valid, but the path \[1,2,4,3\] is not valid. On the other hand, the path can be in the child-Parent-child order, where not necessarily be parent-child order.

Example 1:

```
Input:
        1
       / \
      2   3
Output: 2
Explanation: The longest consecutive path is [1, 2] or [2, 1].
```

Example 2:

```
Input:
        2
       / \
      1   3
Output: 3
Explanation: The longest consecutive path is [1, 2, 3] or [3, 2, 1].
```

Note: All the values of tree nodes are in the range of \[-1e7, 1e7\].

### Solutions:

```java
class Solution {
public:
    // helper returns {incrLen, decrLen, val} from current Node, at the same time dynamically calculate the result
    vector<int> helper(TreeNode* root, int& res) {
        if (!root) return {0, 0, 0};
        
        auto l = helper(root->left, res);
        auto r = helper(root->right, res);

        int incrLen = 0, decrLen = 0;
		// we have returns from both sub trees, need to find out the longer increasing length, 
		// if none of them are consecutively increasing plus root, then the lenght is 1, which is the current node itself
        incrLen = max(incrLen, l[0] && l[2] + 1 == root->val ? l[0] + 1 : 1);
        incrLen = max(incrLen, r[0] && r[2] + 1 == root->val ? r[0] + 1 : 1);
        
		// similar way to calculate consecutively decreasing length.
        decrLen = max(decrLen, l[1] && l[2] - 1 == root->val ? l[1] + 1 : 1);
        decrLen = max(decrLen, r[1] && r[2] - 1 == root->val ? r[1] + 1 : 1);

		// target result could be from current node's left side or right side
        res = max(res, max(incrLen, decrLen));

        // target result could also be gotten if we can combine left subtree and right subtree together
        if (l[0] && r[1] && l[2] + 1 == root->val && root->val + 1 == r[2]) {
            res = max(res, l[0] + r[1] + 1);
        }
        
        if (l[1] && r[0] && l[2] - 1 == root->val && root->val - 1 == r[2]) {
            res = max(res, l[1] + r[0] + 1);
        }
        
        return {incrLen, decrLen, root->val};
    }
    
    int longestConsecutive(TreeNode* root) {
        int res = 0;
        helper(root, res);
        return res;
    }
};
```



