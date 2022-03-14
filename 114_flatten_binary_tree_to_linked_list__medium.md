# 114 Flatten Binary Tree to Linked List – Medium

### Problem:

Given a binary tree, flatten it to a linked list in-place.

For example,  
Given

```
     1
    / \
   2   5
  / \   \
 3   4   6
```

The flattened tree should look like:

1  
    \  
     2  
      \  
       3  
        \  
         4  
          \  
           5  
            \  
             6

### Thoughts:

Idea is to use a modified preorder walk of the tree. Root, left, right.

Have a global pointer that keeps the node that needs a new right child to insert.

### Solutions:

```java
public class Solution {
     TreeNode* t;

    void ft(TreeNode* r)
    {
       if(!r) return;          
       t->right =  r;
       t = t->right;
       auto l = r->left;
       auto rr = r->right;
       r->left = nullptr;
       r->right = nullptr;
       ft(l);
       ft(rr);        
    }

    void flatten(TreeNode* root) {
      t = new TreeNode();
      ft(root);
    }
}
```



