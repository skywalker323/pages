# 241 Different Ways to Add Parentheses – Medium

### Problem:
Given a string of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. The valid operators are +, - and *.
Example 1

Input: "2-1-1".
```
((2-1)-1) = 0
(2-(1-1)) = 2
```
Output: [0, 2]
Example 2

Input: "2*3-4*5"
```
(2*(3-(4*5))) = -34
((2*3)-(4*5)) = -14
((2*(3-4))*5) = -10
(2*((3-4)*5)) = -10
(((2*3)-4)*5) = 10
```
Output: [-34, -14, -10, -10, 10]

### Thoughts:
The problem doesn’t say if each original number is positive. But based on the solution below can pass the test, so this is one of the assumption. If the original number can be negative, we just need some more operation to extract number.

The key here is that we each continuous two elements can be calculated first.
For the e.g above, 23 can be calculated first, or 3 – 4 can be calculated first, then for these two cases the problem becomes 6-45 and 2(-1)5.
So it will become two sub problem.
It is very promising to use a divide and conquer.
If we use [23-45] to represent a problem of such, it can be broken into 2[3-45], [23]-[45] and [23-4]5 sub problems.

In the first solution below, it shows an idea of such. It uses a regular expression to help extract nums and operators. While is not necessary to use though.

In the second solution, it is not using regular expression and the code is less.

If performance is a concern, then we can use memorization to improve performance. E.g use a HashMap data to keep the return value for a given string. So that we avoid redo a problem that’s already been processed.

### Solutions:

```java
class Solution {
public:
    int s(int n1, int n2, int c)
    {
        if( c == '-') return n1-n2;
        if( c== '+') return n1+n2;
        if( c== '*') return n1*n2;
        return 0;
    }

    void add2(vector<int>& res, vector<int>& l, vector<int>& r, char ch)
    {
        for(int i =0; i < l.size(); i++)
            for(int j=0; j< r.size(); j++)
               res.push_back(s(l[i], r[j], ch));
    }
    vector<int> diffWaysToCompute(string expression) {
        vector<int> res;
        for(int i =0; i < expression.size(); i++)
        {
            if(expression[i]== '-' || expression[i]== '+' || expression[i]== '*')
            {
                auto l = diffWaysToCompute(expression.substr(0, i));
                auto r = diffWaysToCompute(expression.substr(i+1));
                add2(res, l, r, expression[i]);
            }
        }

        if (res.size() == 0) res.push_back(stoi(expression));
        return res;
    }
};
```
