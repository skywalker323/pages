# 543 Diameter of Binary Tree

### Problem:

Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

Example:  
Given a binary tree

```
          1
         / \
        2   3
       / \     
      4   5
```

Return 3, which is the length of the path \[4,2,1,3\] or \[5,2,1,3\].

Note: The length of path between two nodes is represented by the number of edges between them.

### Solutions:

```java
Approach:-
Please draw a sample tree first. You will see that a diameter is nothing but maximum( left height +right height+1) for a node. Keep updating this value for every node.

You can calculate height of the tree for every node and then compare it with the value (in this case "ans" ). The return value is the height of the tree for a node.

It will take O( N^2 ) if we write a separate height function, as we are visiting each node twice. Why not, visit the tree once and calculate height in the same recursion keep a check on "ans".

Time Comp - O(N)

class Solution {
	public:
int ans=0;

int height(TreeNode* root)
{
    if(!root) return 0;
    
    int lHeight = height(root->left);
    int rHeight = height(root->right);
    
    ans= max(ans, 1 + lHeight + rHeight);
    return 1+ max( lHeight , rHeight);

}

int diameterOfBinaryTree(TreeNode* root) {
    if(!root) return 0;
    height(root);
    return ans-1;
}
};
```



