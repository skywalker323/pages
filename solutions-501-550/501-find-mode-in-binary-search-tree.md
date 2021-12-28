# 501 Find Mode in Binary Search Tree

### Problem:

Given a binary search tree \(BST\) with duplicates, find all the mode\(s\) \(the most frequently occurred element\) in the given BST.

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than or equal to the node's key.  
The right subtree of a node contains only nodes with keys greater than or equal to the node's key.  
Both the left and right subtrees must also be binary search trees.  
For example:  
Given BST \[1,null,2,2\],

```
   1
    \
     2
    /
   2
```

return \[2\].

Note: If a tree has more than one mode, you can return them in any order.

Follow up: Could you do that without using any extra space? \(Assume that the implicit stack space incurred due to recursion does not count\).

### Solutions:

```java
The basic idea is that the inorder traversal of the BST is in ascending order. For example, the inorder traversal of the BST [2, 1, 3] is [1, 2, 3]. Then we can easily find the most frequently occurred elements by comparing the successive elements.

class Solution {
public:
    int maxFreq = 0, currFreq = 0, precursor = INT_MIN;
    vector<int> res;

    vector<int> findMode(TreeNode *root)
    {
        inorderTraversal(root);
        return res;
    }

    void inorderTraversal(TreeNode *root)
    {
        if (root == NULL) return; // Stop condition
        inorderTraversal(root->left); // Traverse left subtree
        if (precursor == root->val) currFreq++;
        else currFreq = 1;
        if (currFreq > maxFreq)
        {// Current node value has higher frequency than any previous visited
            res.clear();
            maxFreq = currFreq;
            res.push_back(root->val);
        }
        else if (currFreq == maxFreq)
        {// Current node value has a frequency equal to the highest of previous visited
            res.push_back(root->val);
        }
        precursor = root->val; // Update the precursor
        inorderTraversal(root->right); // Traverse right subtree
    }
};
```

```java
Use a dictionary to store the frequency of each interger. Then simply find the largest frequency and return
all the associated keys.
Note we do not use the property of BST in this solution.
from collections import defaultdict
class Solution(object):
    def helper(self, root, cache):
        if root == None:
            return
        cache[root.val] += 1
        self.helper(root.left, cache)
        self.helper(root.right, cache)
        return

    def findMode(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        if root == None:
            return []
        cache = defaultdict(int)
        self.helper(root, cache)
        max_freq = max(cache.values())
        result = [k for k,v in cache.items() if v == max_freq]
        return result
O(N) time and O(1) Space

Write BST Iterator class which gives the next element in_order. Now the problem reduces to finding mode in a sorted array.
Instead of a BST iterator, we can use a recursive inorder traversal and store a class variable pre to indicate the previous integer.
https://discuss.leetcode.com/topic/77308/4ms-java-solution-beats-100-o-1-space-recursion-stack-space-doesn-t-count
Divide and Conquer

Mode lies entirely in left subtree, or in right subtree or the middle element is the mode.
Time would be NlogN at best and space O(1)
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
class Solution {
    int max = 0;
    int curr = Integer.MIN_VALUE;
    int count = 0;
    List<Integer> res = new LinkedList<>();
    public int[] findMode(TreeNode root) {
        countNode(root);

        int[] resArray = new int[res.size()];
        int index = 0;
        for (Integer ele:res) {
            resArray[index++] = ele;
        }
        return resArray;
    }
    private void countNode(TreeNode node) {
        if (node == null) {
            return;
        }
        countNode(node.left);
        if (node.val == curr) {
            count ++;

        }
        else {
            curr = node.val;
            count = 1;
        }
        if (count > max) {
            res.clear();
            max = count;
            res.add(node.val);
        }
        else if (count == max) {
            res.add(node.val);
        }
        countNode(node.right);
    }
}
```



