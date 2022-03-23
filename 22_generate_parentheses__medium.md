# 22 Generate Parentheses – Medium

### Problem:

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given n = 3, a solution set is:

"\(\(\(\)\)\)", "\(\(\)\(\)\)", "\(\(\)\)\(\)", "\(\)\(\(\)\)", "\(\)\(\)\(\)"

### Thoughts:

Key idea is that, if you still have left parantheses left, you have two choice : insert a parantheses or a right parenthesis. But the condition for inserting right parenthesis is that used left ones is more than used right ones.

### Solutions:

```java
void solve(string&curr, int n, int pt){
        // if no more remaining characters
        // push the string into the solution
        if(n == 0){
            sol.push_back(curr);
            return;
        }
        
        // if n > pt
        // this means we still have enough remaining
        // characters to close all open
        // parenthesis
        if(n > pt){
            curr.push_back('(');
            solve(curr,n-1,pt+1);
            curr.pop_back();
        }
        
        // push ) in those cases
        // where pt > 0
        // i.e there is some open brackets before
        // ensures balanced expression is formed
        if(pt > 0){
            curr.push_back(')');
            solve(curr,n-1,pt-1);
            curr.pop_back();
        }
    }
```



