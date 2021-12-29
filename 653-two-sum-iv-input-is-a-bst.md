# 653 Two Sum IV - Input is a BST

### Problem

Given a Binary Search Tree and a target number, return true if there exist two elements in the BST such that their sum is equal to the given target.

Example 1:

```
Input: 
    5
   / \
  3   6
 / \   \
2   4   7

Target = 9
Output: True
```

Example 2:

```
Input: 
    5
   / \
  3   6
 / \   \
2   4   7

Target = 28

Output: False
```

### Solutions:

```java
First Solution - Keep Values in Set:
We keep a set to remember the previous elements in the tree.
While traversing the tree, if we saw already k - root->val, then we have a two sum, return true.
Otherwise, insert val into set and keep traversing.

class Solution {
public:
    bool findTarget(TreeNode* root, int k) {
        if (!root) return false;
        if (s.count(k - root->val)) return true;
        s.insert(root->val);
        return findTarget(root->left, k) || findTarget(root->right, k);
    }
    
private:
    unordered_set<int> s;
};
Second Solution - Inorder Traversal + Two Pointers
Use inorder traversal to get a sorted vector of all the tree's values.
Then use two-pointer approach to find a two sum.

class Solution {
public:
    void inorder(TreeNode* root) {
        if (!root) return;
        inorder(root->left);
        vec.push_back(root->val);
        inorder(root->right);
    }
    
    bool findTarget(TreeNode* root, int k) {
        inorder(root);
        int l = 0, r = vec.size()-1;
        while (l < r) {
            if (vec[l] + vec[r] == k) return true;
            else {
                if (vec[l] + vec[r] < k) l++;
                else r--;
            }
        }
        return false;
    }
    
private:
    vector<int> vec;
};
```



