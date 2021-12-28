# 545 Boundary of Binary Tree

### Problem:

Given a binary tree, return the values of its boundary in anti-clockwise direction starting from root. Boundary includes left boundary, leaves, and right boundary in order without duplicate nodes.

Left boundary is defined as the path from root to the left-most node. Right boundary is defined as the path from root to the right-most node. If the root doesn't have left subtree or right subtree, then the root itself is left boundary or right boundary. Note this definition only applies to the input binary tree, and not applies to any subtrees.

The left-most node is defined as a leaf node you could reach when you always firstly travel to the left subtree if exists. If not, travel to the right subtree. Repeat until you reach a leaf node.

The right-most node is also defined by the same way with left and right exchanged.

Example 1

```
Input:
  1
   \
    2
   / \
  3   4

Ouput:
[1, 3, 4, 2]

Explanation:
The root doesn't have left subtree, so the root itself is left boundary.
The leaves are node 3 and 4.
The right boundary are node 1,2,4. Note the anti-clockwise direction means you should output reversed right boundary.
So order them in anti-clockwise without duplicates and we have [1,3,4,2].
```

Example 2

```
Input:
    ____1_____
   /          \
  2            3
 / \          / 
4   5        6   
   / \      / \
  7   8    9  10  

Ouput:
[1,2,4,7,8,9,10,6,3]

Explanation:
The left boundary are node 1,2,4. (4 is the left-most node according to definition)
The leaves are node 4,7,8,9,10.
The right boundary are node 1,3,6,10. (10 is the right-most node).
So order them in anti-clockwise without duplicate nodes we have [1,2,4,7,8,9,10,6,3].
```

### Solutions:

```java
The boundary of tree can be split into 4 parts:

1) left boudnary (do not count leaves here);
2) leaves of left subtree;
3) leaves of right subtree;
4) right boundary (do not count leaves here).
public:
    vector<int> boundaryOfBinaryTree(TreeNode* r) {
      if (!r) return boundary;
      boundary.push_back(r->val);
      sideBound(r->left, true), leaves(r->left), leaves(r->right), sideBound(r->right, false);
      return boundary;
    }
  
private:
  vector<int> boundary;
  
  void sideBound(TreeNode* r, bool isLeft) { // add side boundary
    if (!r || !r->left && !r->right) return; // do not add leaves
    if (isLeft) boundary.push_back(r->val), sideBound(r->left? r->left : r->right, isLeft);
    else sideBound(r->right? r->right : r->left, isLeft), boundary.push_back(r->val);
  } 
  
  void leaves(TreeNode* r) { // add leaves
    if (!r) return;
    if (!r->left && !r->right) boundary.push_back(r->val);
    leaves(r->left), leaves(r->right);
  }
```

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
    public List<Integer> boundaryOfBinaryTree(TreeNode root) {
        List<Integer> res = new LinkedList<Integer>();
        if (root == null) {
            return res;
        }
        res.add(root.val);
        left(root.left, res);
        right(root.right, res);
        return res;
    }
    private void left(TreeNode node, List<Integer> res) {
        if (node == null) {
            return;
        }
        res.add(node.val);
        if (node.left != null) {
            left(node.left, res);
            inorder(node.right, res);
        }
        else {
            left(node.right, res);
        }
    }
    private void right(TreeNode node, List<Integer> res) {
        if (node == null) {
            return;
        }
        if (node.right != null) {
            inorder(node.left, res);
            right(node.right, res);
        }
        else {
            right(node.left, res);
        }
        res.add(node.val);
    }
    private void inorder(TreeNode node, List<Integer> res) {
        if (node == null) {
            return;
        }
        if (node.left == null && node.right == null) {
            res.add(node.val);
        }
        else {
            inorder(node.left, res);
            inorder(node.right, res);
        }
    }
}
```



