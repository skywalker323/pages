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

Non-Recursion version:

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void flatten(TreeNode root) {
        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode p = new TreeNode(-1);
        TreeNode node = root;
        while (true) {
            if (node == null) {
                if (stack.size() == 0) {
                    break;
                }
                node = stack.pop();
            }
            p.right = node;
            p = node;
            if (node.right != null) {
                stack.push(node.right);
            }
            node = node.left;
            p.left = null;
        }
    }
}
```



