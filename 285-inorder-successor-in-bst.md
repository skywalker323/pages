# 285 Inorder Successor in BST

### Problem:

Given a binary search tree and a node in it, find the in-order successor of that node in the BST.

Note: If the given node has no in-order successor in the tree, return null.

### Solutions:

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
    TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
        stack<TreeNode*> st;
        bool b=false;

        while(true){
            while(root){
                st.push(root);
                root=root->left;
            }
            if(st.empty()) return NULL;
            TreeNode *t=st.top();
            st.pop();
            if(b) return t;
            if(t==p)  b=true;
            root=t->right;
        }
    }

}
```
