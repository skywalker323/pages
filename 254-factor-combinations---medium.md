# 254 Factor Combinations - Medium

### Problem:
<pre>
Numbers can be regarded as product of its factors. For example,

8 = 2 x 2 x 2;
  = 2 x 4.

Write a function that takes an integer n and return all possible combinations of its factors.

Note:
You may assume that n is always positive.
Factors should be greater than 1 and less than n.

Examples:
input: 1
output:
[]
input: 37
output:
[]
input: 12
output:
  [
    [2, 6],
    [2, 2, 3],
    [3, 4]
  ]
input: 32
output:
[
  [2, 16],
  [2, 2, 8],
  [2, 2, 2, 4],
  [2, 2, 2, 2, 2],
  [2, 4, 4],
  [4, 8]
]
</pre>

# Solutions:

```
class Solution {
public:
    vector<vector<int>> factors;
    vector<int> t1;
    void dfs(int n, int index=2)
    {
        if( n==0)
        {
            return;
        }

        for(int i = index; i*i <= n; i++)
        {
            if( n%i ==0)
            {
                t1.push_back(i);
                t1.push_back(n/i);
                factors.push_back(t1);
                t1.pop_back();
                dfs(n/i, i);
                t1.pop_back();
            }
        }
    }
    vector<vector<int>> getFactors(int n) {
        dfs(n);
        return factors;
    }

};
```
