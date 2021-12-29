# 301 Remove Invalid Parentheses

### Problem

Remove the minimum number of invalid parentheses in order to make the input string valid. Return all possible results.

Note: The input string may contain letters other than the parentheses \( and \).

Examples:

```
"()())()" -> ["()()()", "(())()"]
"(a)())()" -> ["(a)()()", "(a())()"]
")(" -> [""]
```

### Solutions

BFS

```java
BFS Solutions

BFS guarantees shortest path. Since the problem asks to remove minimum parenthesis, it is natural think of BFS. 
A straightforward approach is to remove a parenthesis from the current string until we get a valid string. It 
generates both duplicate and invalid strings. We can use a hash table to remove duplicates and check each string for 
validity. 

    vector<string> removeInvalidParentheses(string s) {
        queue<string> q;
        unordered_set<string> ht;
        q.push(s);
        vector<string> res;
        while(!q.empty()) {
            string ss = q.front();
            q.pop();
            if(ht.count(ss)) continue;
            ht.insert(ss);
            if(isValid(ss)) res.push_back(ss);
            else if (res.empty()) 
                for(int i=0;i<ss.size();i++) 
                    if(ss[i]==')'|| ss[i]=='(') q.push(ss.substr(0,i)+ss.substr(i+1));
        }
        return res;
    }
    bool isValid(string &s) {
        int count=0;
        for(auto c:s) {
            if(c=='(') count++;
            if(c==')')
                if(count>0) count--;
                else return false;
        }
        return !count;
    }
```

DFS

```java
A naive DFS is to generate all the 2^n substr. We use hash table to remove duplicates. and then 
return the longest ones. It is less efficient than BFS because DFS does not guarantee shortest path. So we cannot 
stop after the first valid strings as in BFS.

vector<string> removeInvalidParentheses(string s) 
{
        unordered_set<string> ht;
        string cur;
        dfs(0,cur,s,ht);
        size_t ml = 0;
        for(auto& str:ht) ml = max(ml,str.size());
        vector<string> res;
        for(auto& str:ht) if(str.size()==ml) res.push_back(str);
        return res;
}
void dfs(int p, string& cur, string& s, unordered_set<string>& res) 
{
        if(p==s.size()) {
            if(isValid(cur)) res.insert(cur);
            return;
        }
        cur+=s[p];
        dfs(p+1,cur,s,res);
        cur.pop_back();
        if(s[p]=='('||s[p]==')') dfs(p+1,cur,s,res); 
}
```



