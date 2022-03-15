# 150 Evaluate Reverse Polish Notation – Medium

### Problem:

Evaluate the value of an arithmetic expression in Reverse Polish Notation.

Valid operators are +, -, \*, /. Each operand may be an integer or another expression.

Some examples:

\["2", "1", "+", "3", "\*"\] -&gt; \(\(2 + 1\) \* 3\) -&gt; 9  
  \["4", "13", "5", "/", "+"\] -&gt; \(4 + \(13 / 5\)\) -&gt; 6

### Thoughts:

This is a very straight forward problem using a stack.

### Solutions:

```cpp
class Solution {
public:
    int operate( int a , int b , string op)
    {
        if(op =="+") return a+b;
        if(op == "-") return a-b;
        if(op == "*") return a*b;
        return a/b;
    }
     bool isop(string it){
     return( it =="+" or it =="-" or it =="*" or it=="/") ;
     }
    int evalRPN(vector<string>& toks) {
        stack<int> s ;
         for(int i=0 ;i<toks.size() ;i++)
         {
             if(isop(toks[i]))
             {
               int a=s.top() ; s.pop() ;
               int b =s.top() ;s.pop() ;
               s.push(operate(b ,a ,toks[i])) ; 
             }
             else
             {
                 s.push(stoi(toks[i])) ;
             }
         }
         return s.top() ;
    }
};
```



