# 314 Binary Tree Vertical Order Traversal

### Problem:

Given a binary tree, return the vertical order traversal of its nodes' values. \(ie, from top to bottom, column by column\).

If two nodes are in the same row and column, the order should be from left to right.

Examples:

Given binary tree \[3,9,20,null,null,15,7\],

```
   3
  /\
 /  \
 9  20
    /\
   /  \
  15   7
```

return its vertical order traversal as:

```
[
  [9],
  [3,15],
  [20],
  [7]
]
```

Given binary tree \[3,9,8,4,0,1,7\],

```
     3
    /\
   /  \
   9   8
  /\  /\
 /  \/  \
 4  01   7
```

return its vertical order traversal as:

```
[
  [4],
  [9],
  [3,0,1],
  [8],
  [7]
]
```

Given binary tree \[3,9,8,4,0,1,7,null,null,null,2,5\] \(0's right child is 2 and 1's left child is 5\),

```
     3
    /\
   /  \
   9   8
  /\  /\
 /  \/  \
 4  01   7
    /\
   /  \
   5   2
```

return its vertical order traversal as:

```
[
  [4],
  [9,5],
  [3,0,1],
  [8,2],
  [7]
]
```

### Solutions:

```java
 public:
  // BFS
  vector<vector<int>> verticalOrder(TreeNode *root) {
    queue<pair<TreeNode *, int>> q;  // second int keeps track of column offset
    q.push({root, 0});
    while (!q.empty()) {
      auto currPair = q.front();
      q.pop();
      auto currNode = currPair.first;
      auto currColumn = currPair.second;
      if (!currNode) {
        continue;
      }
      columnTable[currColumn].push(currNode->val);
      q.push({currNode->left, currColumn - 1});
      q.push({currNode->right, currColumn + 1});
    }
    buildAnswer();
    return ans;
  }

 private:
  // map key: column offset (-1, +2, etc.); map value: vector of nodes
  // in that column. The values are guaranteed to be sorted by row
  // order, because we are using BFS.
  map<int, vector<int>> columnTable;
  vector<vector<int>> ans;
  void buildAnswer() {
    ans.reserve(columnTable.size());
    for (auto &[col, vec] : columnTable) {
      ans.push_back(vec);
    }
  }
};
```

```cpp
class Solution {
public:
    vector<vector<int>> verticalOrder(TreeNode* root) {
        if(!root) return {};
        map<int,vector<int>> m;
        queue<pair<int,TreeNode*>> q;
        q.push({0,root});
        
        while(!q.empty()) {
            auto cur = q.front();
            q.pop();
            int val = cur.first;
            TreeNode* node = cur.second;
            m[val].push_back(node->val);
            if(node->left)q.push({val - 1, node->left});
            if(node->right)q.push({val + 1,node->right});
        }
        
        vector<vector<int>> ans;
        for(auto x : m) {
            ans.push_back(x.second);
        }
        return ans;
    }
};
```



