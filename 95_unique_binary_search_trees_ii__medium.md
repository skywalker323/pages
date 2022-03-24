# 95 Unique Binary Search Trees II – Medium

### Problem:

Given n, generate all structurally unique BST’s \(binary search trees\) that store values 1…n.

For example,  
Given n = 3, your program should return all 5 unique BST’s shown below.

1         3     3      2      1  
           /     /      /       \  
     3     2     1      1   3      2  
    /     /                        \  
   2     1         2                 3

### Thoughts:

Building BST is harder than count the number comparing to the Version I.

Key idea is for a number n

S\[1, n\] stands for unique binary search trees set from number 1 to n

then use 1 as root, then becomes subproblem\(2, n\)    Empty set + S\[2, n\]

then use 2 as root, then becomes subproblem\(1,1\) \* subproblem\(3,n\)  S\[1\]  + S\[3, n\]

### Solutions:

![](/assets/import.png)

```java

vector<TreeNode*> buildTree(int start, int end) {
    vector<TreeNode*> ans;

    // If start > end, then subtree will be empty so
    /// add NULL in the ans and return it.
    if (start > end) {
        ans.push_back(nullptr);
        return ans;
    }

    // Iterate through all values from start to end to construct
    // left and right subtree recursively
    for (int i = start; i <= end; ++i) {
        // Construct all possible left subtrees
        vector<TreeNode*> leftSubTree = buildTree(start, i - 1);
        // Construct all possible left subtrees
        vector<TreeNode*> rightSubTree = buildTree(i + 1, end); 

        // loop through all left and right subtrees and connect them to ith root
        // leftSubTree.size()* rightSubTree.size() permutations
        for (int j = 0; j < leftSubTree.size(); j++) {
            for (int k = 0; k < rightSubTree.size(); k++) {
                // Create root with value i
                TreeNode* root = new TreeNode(i);
                // Connect left subtree rooted at leftSubTree[j]
                root->left = leftSubTree[j];
                // Connect right subtree rooted at rightSubTree[k]
                root->right = rightSubTree[k]; 
                ans.push_back(root); 
            }
        }
    }

    return ans;
}
```



