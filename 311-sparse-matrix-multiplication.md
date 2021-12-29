# 311 Sparse Matrix Multiplication

### Problem:

<pre>
Given two sparse matrices A and B, return the result of AB.

You may assume that A's column number is equal to B's row number.

Example:

A = [
  [ 1, 0, 0],
  [-1, 0, 3]
]

B = [
  [ 7, 0, 0 ],
  [ 0, 0, 0 ],
  [ 0, 0, 1 ]
]


     |  1 0 0 |   | 7 0 0 |   |  7 0 0 |
AB = | -1 0 3 | x | 0 0 0 | = | -7 0 3 |
                  | 0 0 1 |
            
</pre>

### Solutions:

```java
// Normal matrix multiplication

vector<vector<int>> multiply(vector<vector<int>>& mat1, vector<vector<int>>& mat2) {
        int m = mat1.size(), n = mat1[0].size(), q = mat2[0].size();
        vector<vector<int>> ans(m, vector<int>(q, 0));
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < q; j++) {
                for (int k = 0; k < n; k++) {
                    ans[i][j] += mat1[i][k] * mat2[k][j];
                }
            }
        }
        return ans;
    }
// Sparse matrix multiplication

class Solution {
public:
    vector<vector<int>> multiply(vector<vector<int>>& mat1, vector<vector<int>>& mat2) {
        int m = mat1.size(), n = mat1[0].size(), q = mat2[0].size();
        vector<vector<int>> ans(m, vector<int>(q, 0));
        unordered_map<int, unordered_map<int, int>> m1;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (mat1[i][j] != 0) {
                    m1[i][j] = mat1[i][j];
                }
            }
        }
        for (auto [r, rows] : m1) {
            for (int j = 0; j < q; j++) {
                for (auto [c, v] : rows) {
                    ans[r][j] += v * mat2[c][j];
                }
            }
        }
        return ans;
    }
};
```