# 117 Populating Next Right Pointers in Each Node II

### Problem

Follow up for problem "Populating Next Right Pointers in Each Node".

What if the given tree could be any binary tree? Would your previous solution still work?

Note:

You may only use constant extra space.  
For example,  
Given the following binary tree,

```
         1
       /  \
      2    3
     / \    \
    4   5    7
```

After calling your function, the tree should look like:

```
         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \    \
    4-> 5 -> 7 -> NULL
```

### Solutions

```java
 Node* connect(Node* root) { 
        if(!root) return root;
        queue<Node*> Q;
        Q.push(root);
        while(!Q.empty())
        {
            auto sz = Q.size();
            for(auto i=0; i < sz; i++)
            {
                auto r = Q.front(); Q.pop();
                if(i == sz-1) r->next = nullptr;
                else r->next = Q.empty()? nullptr : Q.front();
                if(r->left)   Q.push(r->left);
                if(r->right)   Q.push(r->right);
            }            
        }
        return root;
       
    }
```

```java

```



