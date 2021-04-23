# 255. Verify Preorder Sequence in Binary Search Tree -  Medium

### Problem:

<pre>
Given an array of numbers, verify whether it is the correct preorder traversal sequence of a binary search tree.

You may assume each number in the sequence is unique.

Follow up:
Could you do it using only constant space complexity?
</pre>

### Solutions:
A preorder sequence consist of nested structure of {Node, Left subtree, Right Subtree} triplet.

To verify a preorder sequence, we only need to verify if the sequence has the characteristics of BST, which is, For every given node,:

it's the lower bound to its right subtree
it's the upper bound to its left subtree.
We use the first characteristic to implement the solution:

use a "lower" to keep track of current lower bound. Initially lower = INT_MIN, which means no lower bound at first.

use a stack to store cadidates of next lower bound. stack would be kept in decreasing order.

when loop through the sequence, preorder[i] should be greater than "lower", otherwise return false.

If preorder[i] is greater than the stack top, which indicate preorder[i] is the first node of a right subtree, and we have to update "lower" to its parent node for validating rest of the node in this right subtree.
We updae "lower" when removing stack top.
For each remove operation, it's like moving up to upper level in BST, when the stack top is greater than preorder[i], the last removed item is its parent node.

Push preorder[i] into stack as it could be parent node for nodes that haven't been visited yet.

Here is the first solution with O(N) space conplexity

```
bool verifyPreorder(vector<int>& preorder) {
        int mini = INT_MIN;
        vector<int> stack;

        for(auto val:preorder) {
            if(val < mini)
                return false;
            while(stack.size() && stack.back() < val) {
                mini = stack.back();
                stack.pop_back();
            }
            stack.push_back(val);
        }
        return true;
    }
```
