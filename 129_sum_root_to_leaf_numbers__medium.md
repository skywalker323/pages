# 129 Sum Root to Leaf Numbers – Medium

### Problem:

Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path 1-&gt;2-&gt;3 which represents the number 123.

Find the total sum of all root-to-leaf numbers.

For example,

```
1
```

/ \  
  2   3  
The root-to-leaf path 1-&gt;2 represents the number 12.  
The root-to-leaf path 1-&gt;3 represents the number 13.

Return the sum = 12 + 13 = 25.

### Thoughts:

This is a very basic recursion problem. Binary Tree iteration.

### Solutions:

```java
 int ans = 0;
 void helper(TreeNode* root,int val){

        if(root == nullptr) return;

        //if it is a leaf node then add value till leaf to the answer::
        if(root->left == nullptr && root->right == nullptr){
            ans += (val*10 + root->val);
            return;
        }

        //If not leaf then recursively call for the left and right child
        if(root->left) helper(root->left,val*10 + root->val);
        if(root->right) helper(root->right,val*10 + root->val);
}
```



