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
    public int[] findMode(TreeNode root) {
        if (root == null) {
            return new int[0];
        }
        HashMap<Integer, Integer> count = new HashMap<Integer, Integer>();
        process(root, count);
        int max = 0;
        int length = 0;
        for (Integer val:count.keySet()) {
            if (count.get(val) == max) {
                length ++;
            }
            if (count.get(val) > max) {
                length = 1;
                max = count.get(val);
            }
        }
        int[] result = new int[length];
        int k = 0;
        for (Integer val:count.keySet()) {
            if (count.get(val) == max) {
                result[k++] = val;
            }
        }
        return result;
    }
    private void process(TreeNode root, HashMap<Integer, Integer> count) {
        if (root == null) {
            return;
        }
        if (!count.containsKey(root.val)) {
            count.put(root.val, 1);
        }
        else {
            count.put(root.val, count.get(root.val) + 1);
        }
        process(root.left, count);
        process(root.right, count);
    }
}
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



