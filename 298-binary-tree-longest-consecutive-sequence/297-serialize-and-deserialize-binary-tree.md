# 297 Serialize and Deserialize Binary Tree

### Problem:

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

For example, you may serialize the following tree
```
    1
   / \
  2   3
     / \
    4   5
```
as "[1,2,3,null,null,4,5]", just the same as how LeetCode OJ serializes a binary tree. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.
Note: Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

### Solutions:

BFS

```cpp
class Codec {
public:
    ostringstream res;
    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        if( !root) return res.str();
        queue<TreeNode*> Q;
        Q.push(root);
        TreeNode* cur;

        while(!Q.empty())
        {
                cur = Q.front();
                Q.pop();
                if(cur)
                {
                     if( cur ==root)
                         res <<  cur->val ;
                    else
                        res << "," << cur->val ;
                    Q.push(cur->left);
                    Q.push(cur->right);
                }
                else
                {
                    if( cur ==root)
                        res << "null";
                    else
                        res << ",null";
                }
            }


        return res.str();
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {

      if( data.empty()) return nullptr;
      if( data == "null") return nullptr;

      istringstream iss(data);
      string s;
      vector<int> nos;
      while(std::getline(iss, s, ','))
      {
          if( s == "null")
              nos.push_back(INT_MIN);
          else
              nos.push_back(stoi(s));
      }

       if(nos.empty()) return nullptr;

       int node_no = 1;
       TreeNode* root = new TreeNode(nos[0]);
       TreeNode* cur ;
       queue<TreeNode*> st;
       st.push(root);
       while(!st.empty())
       {
           cur = st.front();
           st.pop();

           if( cur && (node_no*2) < nos.size())
           {
              if(nos[node_no*2-1] == INT_MIN)
                  cur->left = nullptr;
              else
                cur->left = new TreeNode(nos[node_no*2-1]);
              if(nos[node_no*2] == INT_MIN)
                  cur->right = nullptr;
               else
                cur->right = new TreeNode(nos[node_no*2]);

              st.push(cur->left);
              st.push(cur->right);
           }
           node_no++;
       }

      return root;
    }
};
```
