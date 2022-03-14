# 116 Populating Next Right Pointers in Each Node – Medium

### Problem:

Given a binary tree

```
struct TreeLinkNode {
  TreeLinkNode *left;
  TreeLinkNode *right;
  TreeLinkNode *next;
}
```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

Note:

You may only use constant extra space.  
You may assume that it is a perfect binary tree \(ie, all leaves are at the same level, and every parent has two children\).  
For example,  
Given the following perfect binary tree,

```
     1
   /  \
  2    3
 / \  / \
4  5  6  7
```

After calling your function, the tree should look like:

```
     1 -> NULL
   /  \
  2 -> 3 -> NULL
 / \  / \
4->5->6->7 -> NULL
```

### Thoughts:

Since the requirement says we are talking about a perfect tree so it makes a lot of things easier.

Now here is how to tackle this problem:

For every node,

1 the left child should point next to the right child.

2 the right child should point next to current node’s next node’s left child.

If current node doesn’t have a next, right child should point next to null.

### Solutions:

Recursion:

```

Node* connect(Node* root) {
    if (!root) return root;
    queue<Node*> Q;
    Q.push(root);
    while (!Q.empty())
    {
        auto sz = Q.size();
        for (auto i = 0; i < sz; i++)
        {
            auto r = Q.front(); Q.pop();
            if (i == sz - 1) r->next = nullptr;
            else r->next = Q.empty() ? nullptr : Q.front();
            if (r->left) Q.push(r->left);
            if (r->right) Q.push(r->right);
        }
    }
    return root;
}
```



