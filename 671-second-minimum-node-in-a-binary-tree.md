# 671 Second Minimum Node In a Binary Tree

### Problem

Given a non-empty special binary tree consisting of nodes with the non-negative value, where each node in this tree has exactly two or zero sub-node. If the node has two sub-nodes, then this node's value is the smaller value among its two sub-nodes.

Given such a binary tree, you need to output the second minimum value in the set made of all the nodes' value in the whole tree.

If no such second minimum value exists, output -1 instead.

Example 1:

```
Input: 
    2
   / \
  2   5
     / \
    5   7

Output: 5
Explanation: The smallest value is 2, the second smallest value is 5.
```

Example 2:

```
Input: 
    2
   / \
  2   2

Output: -1
Explanation: The smallest value is 2, but there isn't any second smallest value.
```

### Solutions

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
class Solution {
    public int findSecondMinimumValue(TreeNode root) {
        if (root == null) {
            return -1;
        }
        int[] res = new int[1];
        res[0] = -1;
        preorder(root, root.val, res);
        return res[0];
    }
    private void preorder(TreeNode node, int min, int[] res) {
        if (node == null) {
            return;
        }
        if (node.val > min && (res[0] == -1 || node.val < res[0])) {
            res[0] = node.val;
        }
        preorder(node.left, min, res);
        preorder(node.right, min, res);
    }
}
```

```java
This question is very similar to searching for minimum value in the Binary Tree.
The only requirement is that this value must be different from the root value, k.

If the root value of a subtree == k, 
         keep searching its children.
else, 
         return the root value because it is the minimum of current subtree.
         
class Solution {
public:
    int findSecondMinimumValue(TreeNode* root) {
        if (!root) return -1;
        int ans = minval(root, root->val);
        return ans;
    }
private:
    int minval(TreeNode* p, int first) {
        if (p == nullptr) return -1;
        if (p->val != first) return p->val;
        int left = minval(p->left, first), right = minval(p->right, first);
        // if all nodes of a subtree = root->val, 
        // there is no second minimum value, return -1
        if (left == -1) return right;
        if (right == -1) return left;
        return min(left, right);
    }
};
```



