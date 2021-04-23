# 235 Lowest Common Ancestor of a Binary Search Tree – Easy

### Problem:
Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants (where we allow a node to be a descendant of itself).”
```
        _______6______
       /              \
    ___2__          ___8__
   /      \        /      \
   0      _4       7       9
         /  \
         3   5
```
For example, the lowest common ancestor (LCA) of nodes 2 and 8 is 6. Another example is LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.


### Thoughts:

This is very straightforward by using recursion, because the lowest Common Ancestor must be the one element that’s in between the values of the two given nodes.

This can also be done in iterative way by using a variable to keep track of current node to deal with.

### Solutions:

```java
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {

        if( q->val < p->val)
            swap(p, q);

        while(root!=nullptr)
        {

            if( p->val < root->val && q->val < root->val)
            {
                root  = root->left;
            }
            else
            {
                if( p->val <= root->val && q->val >= root->val)
                    return root;
                else
                {
                    root= root->right;
                }
            }
        }

        return  nullptr;
    }
};
```
