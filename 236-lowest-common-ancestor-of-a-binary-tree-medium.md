# 236 Lowest Common Ancestor of a Binary Tree – Medium


### Problem:
Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants (where we allow a node to be a descendant of itself).”
```
        _______3______
       /              \
    ___5__          ___1__
   /      \        /      \
   6      _2       0       8
         /  \
         7   4
```
For example, the lowest common ancestor (LCA) of nodes 5 and 1 is 3. Another example is LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.

### Thoughts:
This is similar to the earlier problem, find Lowest Common Ancestor of a Binary Search Tree. The different is that now it is not a Binary Search Tree, there is no easy way to tell if a node is in the left subtree or in the right subtree.
Now we have to iterate the nodes of the tree to determine if a subtree contains a node.
The trick here is how to iterate effectively the tree.

We iterate from root to bottom, if current dealing node is either p or q, then that’s the lowest common ancestor. Otherwise, we continue to try with curr.left and curr.right.

### Solutions:

```java
class Solution {
public:
    TreeNode* res;


    bool lca(TreeNode* root, TreeNode* p, TreeNode* q)
    {



      if(!root) return false;
      auto m = (root == p || root == q)? 1 : 0;
      auto l = lca(root->left, p, q) ? 1 : 0;
      auto r = lca(root->right, p, q) ? 1 : 0;

      if( ( m + l + r ) == 2)
      {
           res = root;
           return true;
      }

      return (m + l + r)  > 0  ;

    }

    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        res = nullptr;
        lca(root, p, q);
        return res;
    }
};
```
